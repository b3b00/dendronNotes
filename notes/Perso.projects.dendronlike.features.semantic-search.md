---
id: Perso.projects.dendronlike.features.semantic-search
title: Perso.projects.dendronlike.features.semantic-search
desc: Semantic search
updated: 1791907284564
created: 1791907254686
---

# Offline Global Semantic Search Architecture & Implementation Plan

## 1. Executive Summary & Objectives

The goal of this feature is to introduce a **global semantic search engine** into Dendr-Online. The search engine allows users to discover conceptually related materials across all data types stored in a PKM collection:
- **Dendron Notes** (`tree_notes`)
- **Stash Notes & Categories** (`stash_notes`, `stash_categories`)
- **Bookmarks** (`bookmark_bookmarks`)
- **Cached Articles** (`bookmark_articles`)

### Core Requirements & Constraints
1. **100% Offline Execution**: All model inference, vector storage, and similarity calculations must execute entirely in the browser without sending data to any external server.
2. **Dual-Tier Multi-Device Access**:
   - **Dedicated Search View (`#/search`)**: First-class full-page search available across **all devices (computers, tablets, and mobile phones)**. On computers, it provides a widescreen master-detail layout with an instant live preview pane, relevance sliders, and entity filter tabs. On mobile, it adapts into a touch-optimized responsive card layout.
   - **Command Palette Overlay (`Ctrl+K`)**: Lightweight floating modal on desktop computers for rapid in-context keyboard navigation without leaving the current note or stash.
3. **Speed & Low Latency**: 
   - Search query latency must be sub-second (< 100ms for similarity ranking, < 150ms for query embedding).
   - Background indexing must not cause frame drops, stutter, or UI input lag.
4. **Space & Memory Optimization**:
   - IndexedDB footprint must remain compact (< 10 MB for a typical collection of thousands of notes/bookmarks).
   - RAM usage must be bounded; raw content bodies should not be kept permanently resident in RAM.
5. **Resilience & Collection Isolation**:
   - Embeddings must belong to the active collection database (`collection_<normalized_name>`) and cleanly switch or clear when collections change.
   - App updates must not evict the downloaded ONNX model cache in the Service Worker.

---

## 2. Technical Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                             MAIN THREAD (UI)                                │
│                                                                             │
│  ┌───────────────────────┐             ┌─────────────────────────────────┐  │
│  │ CommandPalette.svelte │ ◄─────────► │    SemanticSearchService.ts     │  │
│  │ (Query Input, Toggle, │             │  (Coordinates Worker & IDB,     │  │
│  │  Unified Result List) │             │   Listens to Collection Stores) │  │
│  └───────────────────────┘             └────────────────┬────────────────┘  │
└─────────────────────────────────────────────────────────┼───────────────────┘
                                                          │ postMessage
                                                          ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                    DEDICATED WEB WORKER (Inference & Index)                 │
│                                                                             │
│  ┌─────────────────────────┐             ┌───────────────────────────────┐  │
│  │   Transformers.js v2/v3 │             │      Vector Search Engine     │  │
│  │   (ONNX Runtime Web -   │             │   - Contiguous Float32Array   │  │
│  │    WASM / SIMD / WebGPU)│             │   - Fast Dot Product Cosine   │  │
│  │   all-MiniLM-L6-v2 (q8) │             │   - Top-K Heap / Threshold   │  │
│  └────────────┬────────────┘             └──────────────┬────────────────┘  │
└───────────────┼─────────────────────────────────────────┼───────────────────┘
                │ Cache API                               │ Read / Write
                ▼                                         ▼
┌─────────────────────────────┐             ┌─────────────────────────────────┐
│     Cache API (Browser)     │             │    IndexedDB (Collection DB)    │
│  'transformers-cache'       │             │  - 'semantic_vectors'           │
│  (ONNX model weights ~23MB) │             │  - 'semantic_meta'              │
└─────────────────────────────┘             └─────────────────────────────────┘
```

---

## 3. Technology Selection & Offline Inference Pipeline

### 3.1 Embedding Model
- **Primary Recommendation**: `@xenova/transformers` with model **`Xenova/all-MiniLM-L6-v2`** (quantized `q8` ONNX).
  - **Model Size**: ~23 MB download.
  - **Dimensions ($D$)**: 384 dimensions.
  - **Context Window**: 256–512 tokens (ideal for paragraphs and note sections).
  - **Inference Speed**: ~15–30 ms per chunk on modern devices via WebAssembly SIMD or WebGPU.
  - **Quality**: Proven benchmark leader for sentence similarity and local PKM embeddings.
- **Multilingual Consideration**: If French/English bilingual collections require dense French sentence matching, the architecture must support an optional model flag for **`Xenova/paraphrase-multilingual-MiniLM-L12-v2`** (~45 MB, 384 dim) or **`Xenova/bge-small-en-v1.5`**.

### 3.2 Offline Model Storage & Service Worker Protection
1. **Initial Model Download**:
   - Model weights are fetched on first initialization and automatically cached in the browser `Cache` storage (`transformers-cache`).
   - A settings screen / prompt allows the user to download the model explicitly, or triggers lazy background download when semantic search is first toggled.
2. **Critical Service Worker Preservation (`sw.template.js`)**:
   - Currently, `sw.template.js` contains:
     ```javascript
     caches.keys().then(keys => Promise.all(
       keys.filter(key => key !== CACHE_NAME).map(key => caches.delete(key))
     ))
     ```
   - **Mandatory Update**: `sw.template.js` must whitelist model caches:
     ```javascript
     keys
       .filter(key => key !== CACHE_NAME && !key.startsWith('transformers-') && !key.startsWith('onnx-'))
       .map(key => caches.delete(key))
     ```
   - This prevents evicting the ~23 MB ONNX model on every application version bump.

---

## 4. Multi-Entity Data Ingestion & Chunking Strategy

To search across notes, stashes, bookmarks, and articles, raw content must be normalized into uniform searchable chunks with contextual metadata prefixes.

```
Entity Source               Context Injection                     Chunking Unit
──────────────────────────────────────────────────────────────────────────────────────
Dendron Note        ──►  [Note: {name} | {title}]           ──►  H1/H2 sections or
                          {desc} ...content...                    ~400-char windows (100 overlap)

Stash Note          ──►  [Stash: {category} | {title}]      ──►  Entire note (if < 500 chars)
                          ...content...                           or split into paragraphs

Bookmark            ──►  [Bookmark: {title}]                ──►  Single chunk
                          {desc} Tags: {tags} {url}

Bookmark Article    ──►  [Article: {bookmarkTitle}]         ──►  Paragraph-based chunks
                          ...plain text from HTML...              (~500 chars with 100 overlap)
```

### 4.1 Content Extraction Rules
1. **Dendron Notes (`tree_notes`)**:
   - Extract `header.name`, `header.title`, `header.description`, and markdown `body`.
   - Strip front matter and HTML tags.
   - Split long bodies by markdown headings (`## `, `### `) or into ~400-character blocks with 80-character overlap.
   - Prepend note title and description to each chunk so isolated paragraphs retain document context.
2. **Stash Notes (`stash_notes`)**:
   - Match each note to its parent category in `stash_categories` to retrieve category `title`.
   - Use note `title` and markdown `content`.
   - Typically 1 chunk per stash note. If `content.length > 500`, split by paragraph.
3. **Bookmarks (`bookmark_bookmarks`)**:
   - Concatenate `title`, `description`, tags (`tags.join(', ')`), and hostname from `link`.
   - Produces 1 chunk per bookmark.
4. **Bookmark Articles (`bookmark_articles`)**:
   - Stored as raw HTML strings keyed by `b64Url`.
   - Use a lightweight HTML-to-text parser (DOMParser on main thread or regex/linkedom in worker):
     - Strip `<script>`, `<style>`, `<nav>`, `<header>`, `<footer>`.
     - Extract clean text from `#dendron-article-content` or standard paragraphs (`<p>`).
     - Split long article text into ~500-character chunks with 100-character overlap.
   - Look up the associated bookmark title via `article_bookmarks` or matching URL to prepend document context.

---

## 5. Storage & IndexedDB Schema

To maintain collection isolation, semantic embeddings are stored directly in the respective collection database (`collection_<normalized_name>`).

### 5.1 New Object Stores in Collection Database
Add two stores to `COLLECTION_CONTENT_STORES` in `wwwroot/scripts/constants.ts`:
1. `SEMANTIC_VECTORS = 'semantic_vectors'`
2. `SEMANTIC_META = 'semantic_meta'`

Because `initDatabase` in `wwwroot/scripts/indexeddb.ts` automatically discovers and adds missing stores, existing collection databases will upgrade seamlessly.

### 5.2 Store Record Formats

#### A. `semantic_vectors` (Key: `id`)
```typescript
export interface SemanticVectorRecord {
  id: string;                         // e.g. "note:Perso.csly:0", "stash:<uuid>:0", "art:<b64>:2"
  sourceType: 'note' | 'stash' | 'bookmark' | 'article';
  sourceId: string;                   // Logical identifier for cascading updates/deletes
  chunkIndex: number;
  title: string;                      // Display title for UI
  subtitle?: string;                  // Note hierarchy or Stash category name
  snippet: string;                    // 150-250 char excerpt for snippet preview
  route: string;                      // Navigation route (e.g. "/view/:id", "/stashes", "/read/:b64/false")
  vector: Float32Array;               // 384-element float vector (L2 normalized)
  contentHash: string;                // SHA-1 / fnv hash of the source chunk text
  updatedTS: number;
}
```

#### B. `semantic_meta` (Key-value store)
- `key = "indexing_state"`:
  ```typescript
  interface SemanticMetaState {
    model: string;                    // "Xenova/all-MiniLM-L6-v2"
    version: number;                  // Schema/chunking version
    lastIndexedTS: number;
    totalChunks: number;
    entityHashes: Record<string, { hash: string; chunkCount: number }>;
  }
  ```

### 5.3 Storage Footprint Evaluation
- For 384 dimensions, each `Float32Array` consumes:
  $$384 \times 4 \text{ bytes} = 1,536 \text{ bytes} \approx 1.5 \text{ KB}$$
- For a representative collection:
  - 500 Dendron notes $\times 2$ chunks $= 1,000$ chunks
  - 200 Stash notes $\times 1$ chunk $= 200$ chunks
  - 300 Bookmarks $\times 1$ chunk $= 300$ chunks
  - 100 Articles $\times 5$ chunks $= 500$ chunks
  - **Total**: $2,000$ chunks.
  - **Vectors**: $2,000 \times 1.5 \text{ KB} = 3.0 \text{ MB}$.
  - **Text Metadata & Snippets**: $\approx 2.0 \text{ MB}$.
  - **Total IndexedDB space**: $\approx 5.0 \text{ MB}$.
- **Conclusion**: The footprint is negligible for modern browsers (which grant 50 MB to several GB per origin).

---

## 6. Incremental & Differential Indexing Pipeline

Generating embeddings for 2,000 chunks takes approximately 30–50 seconds. Therefore:
- The system must **never recompute embeddings unnecessarily**.
- Indexing must occur **incrementally in small batches**.

```
                           INCREMENTAL INDEX FLOW
                           
  [Active Collection Change / Note Edit / Bookmark Add / Sync / Manual Refresh]
                                      │
                                      ▼
                        Read Entity Hashes from IDB
                                      │
                        Compare Current Content Hash
                       ┌──────────────┴──────────────┐
                   Identical                      Changed or New
                       │                             │
                     [SKIP]              Remove Old Chunks for Entity
                                                     │
                                         Extract Text Chunks
                                                     │
                                        Compute Embeddings in Worker
                                          (Batches of 10 chunks)
                                                     │
                                        Persist Vectors to IDB
                                                     │
                                          Update Entity Hash
```

### 6.1 Change Detection
1. Compute a fast hash (`hash-wasm` sha1 or deterministic numeric hash) for each entity's complete text.
2. If `entityHashes[entityId].hash === currentHash`, skip immediately.
3. If modified or deleted:
   - Identify obsolete chunks (`sourceId === entityId`) and purge from `semantic_vectors`.
   - For modified items, generate embeddings only for the newly formed chunks.
4. For single-note edits in `Edit.svelte` or `NoteEditor.svelte`:
   - Debounce update by 2 seconds or trigger on explicit save (`Ctrl+S`).
   - Re-indexing takes < 50ms for a single note, running completely unnoticeably in the background.

### 6.2 Background Worker Throttling
- When indexing multiple entities (e.g., initial index or after GitHub pull / ZIP import):
  - Worker processes chunks in batches of 10.
  - Between batches, the worker pauses for 16ms (`setTimeout(nextBatch, 16)`) or uses `requestIdleCallback` to yield CPU cycles.
  - Emits progress events to the main thread:
    ```typescript
    { type: 'PROGRESS', current: 140, total: 500, label: 'Perso.projects.csly' }
    ```

---

## 7. Vector Search Engine (In-Memory contiguous buffer & Dot Product)

### 7.1 Fast Cosine Similarity
Because all chunk vectors are L2-normalized upon creation:
$$\|\mathbf{v}\| = \sqrt{\sum v_i^2} = 1$$
The cosine similarity between query vector $\mathbf{q}$ and chunk vector $\mathbf{v}$ simplifies to the standard **dot product**:
$$\text{similarity}(\mathbf{q}, \mathbf{v}) = \sum_{i=0}^{383} q_i \cdot v_i$$

### 7.2 Memory Optimization: Flat Contiguous Buffer
Instead of maintaining 2,000 separate JavaScript arrays or objects in RAM:
- The worker allocates a single contiguous typed buffer:
  ```typescript
  const vectorBuffer = new Float32Array(totalChunks * 384);
  ```
- Parallel array `metadata[chunkIndex]` stores only lightweight pointers:
  ```typescript
  interface ChunkMeta {
    id: string;
    sourceType: 'note' | 'stash' | 'bookmark' | 'article';
    title: string;
    subtitle?: string;
    snippet: string;
    route: string;
  }
  ```
- **SIMD / Unrolled Dot Product**:
  Scanning 2,000 vectors (768,000 float multiplications) takes **1.5 to 3.5 milliseconds** in V8/Chromium.
- **Top-K Selection**: Maintain a min-heap of size $K = 20$.
- **Score Threshold**: Discard matches with $\text{score} < 0.35$ to eliminate low-relevance noise.

---

## 8. UI/UX Design & User Workflows

### 8.1 Dedicated Search View on Desktop / Computers (`Search.svelte` / `#/search`)

The dedicated search view is available directly on computers via the top navigation bar (`#/search`) or keyboard shortcut (`Ctrl+Alt+F` / `Ctrl+Shift+F`). It provides a spacious, widescreen **master-detail two-column layout** with an interactive live document preview panel:

![Dedicated Desktop Search View UI Mockup](semantic-search-desktop-view.png)

#### Desktop Dedicated View Wireframe
```
┌────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ Dendr-Online v2.4    [Collections]  [Tree]  [Stashes]  [Bookmarks]  [System]  [🔍 Search]   [perso-pkm] │
├────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│  ┌────────────────────────────────────────────────────────────────┐[ x ]  [✨ Semantic] [🔤 Keyword]   │
│  │ 🔍 compiler lexer ast parser                                   │       Threshold: 75% [──────●──]   │
│  └────────────────────────────────────────────────────────────────┘                                    │
│  ( All: 14 )  ( 🌲 Notes: 6 )  ( 📝 Stashes: 3 )  ( 🔖 Bookmarks: 3 )  ( 📄 Articles: 2 )              │
│  Found 14 relevant items across collection (⚡ 1,842 chunks indexed)                                   │
├────────────────────────────────────────────────────┬───────────────────────────────────────────────────┤
│  RESULTS LIST (42% width)                          │  LIVE PREVIEW PANE (58% width)                    │
│                                                    │                                                   │
│  ┌──────────────────────────────────────────────┐  │  Note Preview: Perso.projects.CSLY.Lexer...       │
│  │ 🌲 Note (Active)                 [94% match] │  │  [ ✏️ Edit Note ]  [ ↗ Open Full ]  [ Copy Link ] │
│  │ Perso.projects.CSLY.Lexer.extensions         │  │  ─────────────────────────────────────────────  │
│  │ Perso > projects > CSLY                      │  │  Created: 2022-03-05    Updated: 2024-03-05       │
│  │ ...generic lexer extension support and...    │  │  [ 94% Semantic Relevance Match ]                 │
│  └──────────────────────────────────────────────┘  │                                                   │
│  ┌──────────────────────────────────────────────┐  │  ### Lexer Extension Pipelines                    │
│  │ 📝 Stash                         [88% match] │  │                                                   │
│  │ Git Commits & Tree Refs                      │  │  Token streaming is configured via custom         │
│  │ Devops / Git                                 │  │  lexer extensions. The tokenizer pipeline         │
│  └──────────────────────────────────────────────┘  │  streams tokens dynamically with zero memory      │
│  ┌──────────────────────────────────────────────┐  │  allocation...                                    │
│  │ 🔖 Bookmark                      [82% match] │  │                                                   │
│  │ Chumsky - Parser Combinator Library          │  │  [Highlighted semantic passage in amber/violet]   │
│  └──────────────────────────────────────────────┘  │                                                   │
│  ┌──────────────────────────────────────────────┐  │                                                   │
│  │ 📄 Article                       [78% match] │  │                                                   │
│  │ Building High Performance Lexers             │  │                                                   │
│  └──────────────────────────────────────────────┘  │                                                   │
└────────────────────────────────────────────────────┴───────────────────────────────────────────────────┘
```

#### Desktop Dedicated View Features
1. **Top Desktop Navbar Entry**:
   - In `App.svelte`, the `<nav>` includes `<li><a href="#/search"><Fa icon="{faSearch}"/><span style="margin-left: 5px">Search</span></a></li>` alongside `Tree`, `Stashes`, `Bookmarks`, and `System`.
2. **Master-Detail Two-Column Architecture**:
   - **Left Column (~42% width)**: Scrollable list of ranked result cards with keyboard arrow navigation (`↑` / `↓`).
   - **Right Column (~58% width)**: Live interactive preview panel that displays the selected note, stash markdown, bookmark metadata, or parsed article in-place without navigating away from the search results.
3. **In-Context Passage Highlighting**:
   - The specific chunk or sentence responsible for the high semantic similarity match is highlighted in the preview pane.
4. **Quick Action Controls**:
   - Buttons directly in the preview pane to `[ ✏️ Edit Note ]`, `[ ↗ Open Full View ]`, or copy links.
5. **Relevance Threshold Filtering**:
   - Interactive threshold slider (e.g. 50% to 90%) to tune precision vs recall in real time.

---

### 8.2 Dedicated Search View for Mobile & Tablets (`Search.svelte` / `#/search`)

Because smartphones and tablets lack hardware `Ctrl+K` keyboard shortcuts, and full-screen browsing is essential for mobile ergonomics, the same dedicated route (`#/search`) automatically renders as a responsive, touch-optimized single-column interface:

![Dedicated Mobile Search UI Mockup](semantic-search-mobile-ui.png)

#### Mobile & Tablet Wireframe
```
┌──────────────────────────────────────────────────────────┐
│  Dendr-Online v2.4    [perso-pkm]      [🔍 Search] [ ☰ ] │
├──────────────────────────────────────────────────────────┤
│  ┌─────────────────────────────────────────────────┐[ x ]│
│  │ 🔍 distributed consensus raft paxos             │     │
│  └─────────────────────────────────────────────────┘     │
│  [ ✨ Semantic Search ]       [ 🔤 Keyword Search ]      │
├──────────────────────────────────────────────────────────┤
│  FILTER CHIPS:                                           │
│  ( All: 12 )  ( 🌲 Notes: 5 )  ( 📝 Stash: 3 )  ( 🔖 2 ) │
├──────────────────────────────────────────────────────────┤
│  SEARCH RESULTS:                                         │
│                                                          │
│  ┌────────────────────────────────────────────────────┐  │
│  │ 🌲 Note                                [ 95% match]│  │
│  │ Distributed Consensus: Raft vs Paxos               │  │
│  │ Architecture > Distributed                         │  │
│  │ ...leader election, log replication, and quorum... │  │
│  └────────────────────────────────────────────────────┘  │
│                                                          │
│  ┌────────────────────────────────────────────────────┐  │
│  │ 📄 Article                             [ 89% match]│  │
│  │ The Raft Consensus Algorithm Illustrated           │  │
│  │ ...interactive visualization of leader heartbeat...│  │
│  └────────────────────────────────────────────────────┘  │
│                                                          │
│  ┌────────────────────────────────────────────────────┐  │
│  │ 📝 Stash                               [ 84% match]│  │
│  │ Quorum Math & Split Vote Cheat Sheet               │  │
│  │ Distributed Systems                                │  │
│  │ ...majority formula (N/2 + 1) and partition tips...│  │
│  └────────────────────────────────────────────────────┘  │
│                                                          │
│  ┌────────────────────────────────────────────────────┐  │
│  │ 🔖 Bookmark                            [ 81% match]│  │
│  │ raft.github.io - Raft Interactive Simulator        │  │
│  │ [distributed] [raft] [visualization]               │  │
│  └────────────────────────────────────────────────────┘  │
├──────────────────────────────────────────────────────────┤
│  ⚡ Offline Semantic Index Ready (1,842 chunks)          │
└──────────────────────────────────────────────────────────┘
```

#### Mobile Touch & UX Capabilities
1. **Direct Header & Navigation Drawer Access**:
   - In `App.svelte` header: A touch-friendly search icon button (`<a href="#/search"><Fa icon={faSearch}/></a>`) sits directly beside the burger button for 1-tap search initiation.
   - In `<nav>` drawer: Search item placed at the top of collection-level navigation.
2. **Segmented Mode Toggle**:
   - Large touch pills: `[ ✨ Semantic Search ]` (active glowing indigo) and `[ 🔤 Keyword Search ]`.
3. **Horizontally Scrollable Filter Chips**:
   - Filter results instantly without re-embedding: `All`, `Notes`, `Stashes`, `Bookmarks`, `Articles`, with live count badges.
4. **Touch Target Sizing**:
   - Result cards have minimum 48px touch heights with generous padding and tap feedback.
   - Tapping any card navigates smoothly to the target (`#/view/:id`, `#/stashes`, `#/bookmarks`, or `#/read/:b64/false`).

---

### 8.3 Desktop Quick-Access Command Palette (`CommandPalette.svelte` / `Ctrl+K`)

For rapid in-context keyboard navigation without leaving the current note or stash, desktop users can press `Ctrl+K` to open the lightweight floating modal:

![Command Palette Desktop Overlay Mockup](semantic-search-ui.png)

1. **Search Mode Toggle**:
   - Add a toggle icon in the palette header:
     - Standard Keyword Search (`faSearch`)
     - Semantic Search (`faBrain` or `faWandMagicSparkles`)
   - Or allow prefix `?` for immediate semantic query (e.g. `?lexer extension`).
2. **Unified Navigation Target Handling**:
   - When a result is selected (`Enter` or click):
     - `sourceType === 'note'`: Navigates to `#/view/{sourceId}`.
     - `sourceType === 'stash'`: Navigates to `#/stashes`, automatically expanding the relevant category and focusing the note.
     - `sourceType === 'bookmark'`: Navigates to `#/bookmarks`, setting `pendingScrollToBookmark` to scroll to the target card.
     - `sourceType === 'article'`: Navigates to `#/read/{b64Url}/false` to open the full reader view.
3. **Indexing Status Pill**:
   - Palette footer displays current status:
     - `Ready (2,415 items)`
     - `Indexing 42% (Note 14/80)...`
     - `Model download required (Click to download ~23MB)`

### 8.4 System Page Management (`System.svelte`)
In the System view, add a dedicated **Semantic Search** card:
- Model download status and cache size.
- Button to trigger full index rebuild (`Re-index All`).
- Button to clear semantic index cache.
- Toggle for automatic background indexing.

---

## 9. Rollup & Build System Configuration

### 9.1 Worker Bundling
In `wwwroot/rollup.config.mjs`:
- Configure Rollup to support Web Workers with ES module output, or add a dedicated rollup entry for `scripts/workers/semanticSearch.worker.ts`.
- Ensure output files match the naming scheme:
  ```javascript
  // scripts/workers/semanticSearch.worker.ts -> dist/scripts/semantic-worker-[hash].js
  ```
- Alternatively, load via standard Vite/Rollup worker syntax:
  ```typescript
  const worker = new Worker(
    new URL('./workers/semanticSearch.worker.ts', import.meta.url),
    { type: 'module' }
  );
  ```

### 9.2 Dependency Additions (`wwwroot/package.json`)
- `@xenova/transformers`: `^2.17.2` (or modern `@huggingface/transformers` v3 if compatible).
- Verify browser compatibility: Chromium, Safari, Firefox all support ONNX Runtime Web WASM backend.

---

## 10. Step-by-Step Implementation Roadmap

### Phase 1: Core Storage & Build System Setup
- [ ] Add `@xenova/transformers` to `wwwroot/package.json`.
- [ ] Update `wwwroot/sw.template.js` cache cleanup to preserve `transformers-*` and `onnx-*` cache keys.
- [ ] Update `wwwroot/scripts/constants.ts` with `SEMANTIC_VECTORS` and `SEMANTIC_META` store names.
- [ ] Register new stores in `COLLECTION_CONTENT_STORES` so `initDatabase` creates them automatically.
- [ ] Add storage helper functions in `wwwroot/scripts/database.ts`:
  - `getSemanticVectors(collectionName)`
  - `putSemanticVectors(collectionName, records)`
  - `deleteSemanticVectorsBySource(collectionName, sourceId)`
  - `clearSemanticIndex(collectionName)`
  - `getSemanticMeta(collectionName)` / `setSemanticMeta(collectionName, meta)`

### Phase 2: Web Worker & Inference Engine
- [ ] Create `wwwroot/scripts/workers/semanticSearch.worker.ts`:
  - Initialize pipeline: `pipeline('feature-extraction', 'Xenova/all-MiniLM-L6-v2', { quantized: true })`.
  - Handle messages: `INIT_MODEL`, `EMBED_QUERY`, `INDEX_BATCH`, `BUILD_IN_MEMORY_INDEX`, `SEARCH`.
  - Implement L2 vector normalization and SIMD/tight-loop dot product.
  - Implement top-$K$ priority queue/heap with score cutoff.
- [ ] Configure Rollup worker bundling in `wwwroot/rollup.config.mjs`.

### Phase 3: Extraction & Chunking Services
- [ ] Create `wwwroot/scripts/semantic/chunker.ts`:
  - Pure functions for chunking Dendron markdown notes, Stash notes, Bookmarks, and HTML articles.
  - Implement context prefix formatting (`[Note: ...]`, `[Stash: ...]`, etc.).
  - Implement fast text hashing for differential detection.
- [ ] Create `wwwroot/scripts/semantic/semanticSearchService.ts`:
  - Worker lifecycle management and message RPC abstraction.
  - Methods: `init()`, `search(query, collectionName)`, `syncCollection(collectionName, progressCallback)`.
  - Incremental single-item updates: `updateNoteIndex()`, `updateStashIndex()`, `updateBookmarkIndex()`.

### Phase 4: UI Integration (Desktop CommandPalette & Dedicated Mobile View)
- [ ] Create `wwwroot/components/Search.svelte`:
  - Full-page responsive dedicated search view for mobile phones, tablets, and full-screen desktop use.
  - Large touch search input with debouncing and clear button.
  - Segmented touch toggle: `[ ✨ Semantic Search ]` vs `[ 🔤 Keyword Search ]`.
  - Horizontal filter chips carousel: `All`, `Notes`, `Stashes`, `Bookmarks`, `Articles` with real-time match counters.
  - Touch-optimized result cards with badges, relevance score pills, snippet previews, and direct navigation links.
  - Offline index readiness status banner.
- [ ] Update `wwwroot/App.svelte`:
  - Register route `'/search': wrap({ asyncComponent: () => import('./components/Search.svelte') })`.
  - Add search button icon in the top header bar next to burger menu for 1-tap mobile access.
  - Add "Search" navigation entry in the slide-out drawer menu.
- [ ] Update `wwwroot/components/CommandPalette.svelte` (Desktop Quick Access):
  - Add search mode toggle (Keyword / Semantic) and optional `?` query prefix.
  - Update `searchNotesBackend` to query `semanticSearchService.search` when in semantic mode.
  - Enhance result template slot to render distinct badges for Note, Stash, Bookmark, and Article.
  - Add indexing status banner/indicator.
- [ ] Implement cross-entity navigation in `selectItem`:
  - Support routing to `/view/:id`, `/stashes`, `/bookmarks`, and `/read/:url/false`.

### Phase 5: System Management & Auto-Sync
- [ ] Update `wwwroot/components/bookmarks/System.svelte` with a "Semantic Search" management panel.
- [ ] Hook background sync on collection switch in `App.svelte` and `dendronStore.ts`.
- [ ] Hook incremental index updates on note save (`Edit.svelte`), stash save (`NoteEditor.svelte`), and bookmark create (`bookmarks.svelte`).

### Phase 6: Testing, Optimization & Documentation
- [ ] Add Jest unit tests in `wwwroot/tests/`:
  - `chunker.test.ts`: Chunking correctness, heading preservation, overlap bounds.
  - `vectorSearch.test.ts`: Dot product accuracy, top-$K$ ordering, score thresholding.
  - `differentialIndex.test.ts`: Hashing and change detection verification.
- [ ] Verify offline mode: disconnect network, reload page, ensure semantic search continues to function seamlessly.
- [ ] Verify Service Worker updates: ensure application version updates do not purge downloaded model weights.

---

## 11. Edge Cases & Risk Mitigation

| Risk / Edge Case | Likelihood | Impact | Mitigation Strategy |
| :--- | :--- | :--- | :--- |
| **Model download fails on slow connection** | Medium | Medium | Wrap download in retry logic with explicit progress bar. If offline and model is not yet cached, fallback gracefully to keyword search with a clear notice. |
| **Service Worker cache wipe on deploy** | High | High | Explicitly protect `transformers-*` and `onnx-*` in `sw.template.js` activation event handler. |
| **Large collection indexing freezes UI** | Low | High | Enforce 100% execution in Web Worker; chunk in batches of 10 with yielding pauses; run during browser idle periods. |
| **HTML articles contain messy scripts/ads** | Medium | Low | Strip script, style, SVG, and boilerplate before chunking; prioritize text inside `#dendron-article-content`. |
| **Deleted items remaining in index** | Medium | Medium | Use `sourceId` foreign key pattern in `semantic_vectors`; always remove all previous chunks for a given `sourceId` during re-index or deletion. |
| **Memory usage in low-end mobile devices** | Low | Medium | Contiguous `Float32Array` ensures compact memory (< 5 MB RAM for 2,000 vectors). Only top-$K$ metadata objects are dispatched to main thread. |
