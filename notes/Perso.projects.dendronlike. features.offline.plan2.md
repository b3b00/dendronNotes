---
id: Perso.projects.dendronlike. features.offline.plan2
title: Perso.projects.dendronlike. features.offline.plan2
desc: Plan #2
updated: 0
created: 0
---
# Use dendr-Online Offline

This plan outlines the architecture and implementation steps to enable offline capabilities for dendr-Online. Users will be able to browse and add tree notes, stash notes, and bookmarks while offline, with changes synchronizing to the server (and GitHub) upon reconnecting.

## User Review Required

> [!WARNING]
> **CRDT Integration**: To achieve automatic conflict resolution (CRDT approach), we will need to integrate a library capable of handling text merges or use an existing CRDT library like `Yjs` or `Automerge`, or fallback to a standard 3-way text merge algorithm (like Google's `diff-match-patch`) to handle concurrent edits on the same note. Using a lightweight 3-way merge on sync is often sufficient for single-user cross-device text conflicts without the full overhead of a CRDT event log. Please confirm if a 3-way merge is acceptable for this, or if strict CRDT data structures are preferred.

## Proposed Changes

### 1. Data Availability & Caching (Read)

To support full offline availability, all user data will be cached locally using IndexedDB (to avoid `localStorage` size limits).

#### [MODIFY] `wwwroot/scripts/stashCache.ts`
- Migrate storage from `localStorage` to `IndexedDB`.
- Remove the 5-minute `CACHE_TTL` to rely on explicit sync mechanisms.
- Ensure *all* stash notes are fetched and stored in the IndexedDB cache upon initial load or sync.

#### [NEW] `wwwroot/scripts/treeCache.ts`
- Implement an IndexedDB-based cache to store the complete tree structure and the full content of all tree notes for offline access.

#### [MODIFY] `wwwroot/scripts/bookmarkArticleCache.ts` (or create `bookmarksCache.ts`)
- Extend caching into IndexedDB to store the *list* of bookmarks and their metadata, in addition to the HTML content of TODO articles.

### 2. Offline Login & Authentication

- **No Login Required Offline**: When the app detects it is offline (`!navigator.onLine` or fetch to backend fails), it will skip the GitHub OAuth flow entirely. 
- The app will securely assume that any data present in the local IndexedDB cache is legitimately owned by the current user and allow full access to the offline app interface.

### 3. Offline Action Queue (Write)

Mutations made while offline must be saved locally and applied optimistically.

#### [NEW] `wwwroot/scripts/offlineSyncQueue.ts`
- Create an IndexedDB store to queue outgoing actions (e.g., `ADD_NOTE`, `EDIT_NOTE`, `ADD_BOOKMARK`).
- Structure: `{ id: string, type: string, payload: any, timestamp: number }`.

#### [MODIFY] `wwwroot/scripts/dendronClient.ts` & `stashApi.ts`
- Intercept all mutating API calls.
- If offline, push the action to `offlineSyncQueue`, update the local IndexedDB cache (Optimistic UI), and return a simulated success response to the UI.

### 4. Bookmarks Offline Specifics

#### [MODIFY] `wwwroot/components/bookmarks/bookmarks.svelte` & `Bookmark.svelte`
- **Adding Offline**: When adding a bookmark offline, prompt the user for a short description via a modal/dialog.
- Create the local bookmark object with a flag: `incomplete: true` (or a specific tag).
- **Styling**: Add a visual indicator (e.g., a violet or red border) for incomplete bookmarks.
- **Reloading TODOs**: During synchronization, the system will automatically fetch the full HTML content for any TODO bookmarks added offline once the network returns.

### 5. Synchronization & Automatic Conflict Resolution

Synchronization will be triggered both automatically (when `navigator.onLine` becomes true) and manually (via a "Sync Now" button in the UI).

#### [NEW] `wwwroot/scripts/syncManager.ts`
- **Dual Triggers**: Listens to the `window.addEventListener('online')` event and exposes a manual `triggerSync()` method for the UI.
- Iterates through the `offlineSyncQueue` and replays actions to the backend.

#### [MODIFY] Backend & Sync Logic (CRDT / Auto-Merge)
- **Automatic Resolution**: To resolve conflicts automatically without prompting the user, we will implement a text merging strategy for concurrent edits (e.g. Phone edits v1 -> v2.phone, Laptop edits v1 -> v2.laptop).
- When the offline sync pushes an edit to the server, the server (or client during sync) will check the base version.
- If a conflict is detected, it will apply a CRDT-based or 3-way merge algorithm (like `diff-match-patch`) to seamlessly combine the changes, resulting in a merged v3, without requiring user intervention.

## Verification Plan

### Automated/Manual Testing
- **Read Offline**: Disconnect network, bypass login, verify tree notes, stash notes, and bookmarks are visible and readable from IndexedDB.
- **Write Offline**: Disconnect network, add a stash note, edit a tree note, and add a bookmark (verify prompt for description and incomplete styling).
- **Sync Triggers**: Reconnect network, verify background sync fires. Also verify the manual "Sync Now" button triggers synchronization.
- **Automatic Conflict Resolution**: Simulate a conflict by editing a note on GitHub/Laptop while the phone is offline, edit the same note offline on the phone, go online, and verify the edits are automatically merged cleanly without a manual resolution prompt.

