---
id: Perso.projects.dendronlike.features.bookmarks.reader.bookmark
title: Perso.projects.dendronlike.features.bookmarks.reader.bookmark
desc: Reading bookmark
updated: 1785599577786
created: 0
---

# add a reading bookmark 

When reading and article user might want to add a bookmark to remember read position. 

[Conversation gemini](https://share.gemini.google/VXHd0BN572ZY)

```
<script>
  import { onMount, tick } from 'svelte';

  // Props
  export let articleId;
  export let articleContent; // SmartReader HTML string
  export let savedBookmarkNodeId = null; // e.g. "read-node-14"
  export let onSaveBookmark = async (bookmarkData) => {}; 

  let articleContainer;
  let activeBookmarkId = savedBookmarkNodeId;

  // Prepare nodes and attach handlers after DOM mounts or content changes
  onMount(async () => {
    if (!articleContainer) return;

    prepareBookmarkableNodes();
    
    // Wait for layout/images to settle, then scroll to saved position
    await tick();
    if (activeBookmarkId) {
      scrollToBookmark(activeBookmarkId);
    }
  });

  function prepareBookmarkableNodes() {
    const blocks = articleContainer.querySelectorAll('p, h2, h3, ul, ol, figure');

    blocks.forEach((node, index) => {
      const nodeId = `read-node-${index}`;
      node.setAttribute('id', nodeId);
      node.classList.add('bookmarkable-block');

      // Check if this node is currently active
      if (nodeId === activeBookmarkId) {
        node.classList.add('is-bookmarked');
      }

      // Create bookmark button
      const btn = document.createElement('button');
      btn.className = `bookmark-btn ${nodeId === activeBookmarkId ? 'active' : ''}`;
      btn.setAttribute('aria-label', 'Bookmark section');
      btn.innerHTML = '🔖';

      // Attach Svelte-friendly handler
      btn.onclick = (e) => {
        e.stopPropagation();
        toggleBookmark(nodeId, node);
      };

      node.prepend(btn);
    });
  }

  async function toggleBookmark(nodeId, node) {
    const isCurrentlyActive = activeBookmarkId === nodeId;

    // Clear previous highlights across DOM
    articleContainer.querySelectorAll('.bookmark-btn.active').forEach(b => b.classList.remove('active'));
    articleContainer.querySelectorAll('.is-bookmarked').forEach(el => el.classList.remove('is-bookmarked'));

    if (isCurrentlyActive) {
      // Toggle OFF
      activeBookmarkId = null;
      await onSaveBookmark(null);
    } else {
      // Toggle ON
      activeBookmarkId = nodeId;
      node.classList.add('is-bookmarked');
      
      const btn = node.querySelector('.bookmark-btn');
      if (btn) btn.classList.add('active');

      const snippet = node.textContent.replace('🔖', '').trim().substring(0, 60) + '...';
      
      await onSaveBookmark({
        nodeId: nodeId,
        snippet: snippet,
        updatedAt: Date.now()
      });
    }
  }

  function scrollToBookmark(nodeId) {
    const targetNode = articleContainer.querySelector(`#${CSS.escape(nodeId)}`);
    if (targetNode) {
      targetNode.scrollIntoView({ behavior: 'smooth', block: 'center' });
    }
  }
</script>

<div class="reader-view">
  {#if activeBookmarkId}
    <button class="jump-pill" on:click={() => scrollToBookmark(activeBookmarkId)}>
      📍 Jump to bookmark
    </button>
  {/if}

  <!-- Render SmartReader HTML -->
  <article bind:this={articleContainer} class="prose">
    {@html articleContent}
  </article>
</div>

<style>
  .reader-view {
    position: relative;
    max-width: 68ch;
    margin: 0 auto;
    padding: 2rem 1rem;
  }

  /* Target dynamically injected content */
  :global(.bookmarkable-block) {
    position: relative;
  }

  :global(.bookmark-btn) {
    position: absolute;
    left: -2.2rem;
    top: 0.1rem;
    opacity: 0.15;
    background: transparent;
    border: none;
    cursor: pointer;
    font-size: 1rem;
    padding: 0.2rem;
    transition: opacity 0.2s, transform 0.15s;
  }

  :global(.bookmarkable-block:hover .bookmark-btn) {
    opacity: 0.7;
  }

  :global(.bookmark-btn:hover) {
    transform: scale(1.2);
    opacity: 1 !important;
  }

  :global(.bookmark-btn.active) {
    opacity: 1;
  }

  :global(.bookmarkable-block.is-bookmarked) {
    border-left: 3px solid #e63946;
    padding-left: 0.8rem;
    transition: border-color 0.3s ease;
  }

  /* Floating button to quickly jump back to bookmark */
  .jump-pill {
    position: fixed;
    bottom: 1.5rem;
    right: 1.5rem;
    background: #1e293b;
    color: #fff;
    border: none;
    padding: 0.6rem 1rem;
    border-radius: 9999px;
    font-size: 0.875rem;
    cursor: pointer;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    z-index: 50;
    transition: transform 0.2s;
  }

  .jump-pill:hover {
    transform: translateY(-2px);
  }
</style>
```
