---
id: Perso.projects.dendronlike. features.offline.plan
title: Perso.projects.dendronlike. features.offline.plan
desc: Plan #1
updated: 1784720768753
created: 0
---
# Use dendr-Online Offline

This plan outlines the architecture and implementation steps to enable offline capabilities for dendr-Online. Users will be able to browse and add tree notes, stash notes, and bookmarks while offline, with changes synchronizing to the server (and GitHub) upon reconnecting.

## User Review Required

> [!WARNING]
> **Conflict Resolution Strategy**: The plan currently assumes a manual conflict resolution approach where the user is prompted if a conflict occurs during sync (e.g., the server has a newer version of a note edited offline). Is this acceptable, or would you prefer an automatic approach (like last-write-wins)?
> 
> **Data Caching Scope**: The plan suggests caching *all* tree notes, stash notes, and bookmarks locally to ensure full offline availability. Depending on the size of the vault, this could take up significant local storage. Is it acceptable to sync the entire vault, or should it be limited?

## Open Questions

> [!IMPORTANT]
> 1. **Offline Login**: If a user opens the app while completely offline (e.g., closed the tab and re-opened it), the standard GitHub OAuth flow cannot happen. Should we store a persistent session token/flag in `localStorage` to bypass the login screen when offline?
> 2. **Sync Trigger**: Should synchronization happen automatically in the background as soon as `navigator.onLine` becomes true, or should there be an explicit "Sync Now" button in the UI?
> 3. **Conflict UI**: For conflict resolution, do we want a full side-by-side diff viewer, or simply a prompt asking "Keep Local" or "Keep Server"?

## Proposed Changes

### 1. Data Availability & Caching (Read)

To support browsing while offline, we will augment the existing caching mechanisms.

#### [MODIFY] `wwwroot/scripts/stashCache.ts`
- Remove the 5-minute `CACHE_TTL`.
- Rely on synchronization to update the cache rather than time-based expiration.

#### [NEW] `wwwroot/scripts/treeCache.ts`
- Implement an IndexedDB-based cache (using `idb` or similar) to store the tree structure and the full content of all tree notes.

#### [MODIFY] `wwwroot/scripts/bookmarkArticleCache.ts` (or create `bookmarksCache.ts`)
- Extend the caching to store the *list* of bookmarks and their metadata, not just the HTML content of TODO articles.

### 2. Offline Action Queue (Write)

When the app is offline, mutations must be saved locally and applied optimistically.

#### [NEW] `wwwroot/scripts/offlineSyncQueue.ts`
- Create an IndexedDB store to queue outgoing actions (e.g., `ADD_NOTE`, `EDIT_NOTE`, `ADD_BOOKMARK`).
- Structure: `{ id: string, type: string, payload: any, timestamp: number }`.

#### [MODIFY] `wwwroot/scripts/dendronClient.ts` & `stashApi.ts`
- Intercept all mutating API calls.
- If `!navigator.onLine`, push the action to `offlineSyncQueue`, update the local cache (Optimistic UI), and return a simulated success response to the UI.

### 3. Bookmarks Offline Specifics

#### [MODIFY] `wwwroot/components/bookmarks/bookmarks.svelte` & `Bookmark.svelte`
- **Adding Offline**: When adding a bookmark offline, prompt the user for a short description via a modal/dialog.
- Create the local bookmark object with a flag: `incomplete: true` (or a specific tag).
- **Styling**: Add a visual indicator (e.g., a violet or red border) for incomplete bookmarks.
- **Reloading TODOs**: Add a hook in the online synchronization process to automatically fetch the full HTML content for TODO bookmarks once the network returns.

### 4. Synchronization & Conflict Resolution

#### [NEW] `wwwroot/scripts/syncManager.ts`
- Listens to the `window.addEventListener('online', ...)` event.
- Iterates through the `offlineSyncQueue` and replays actions to the backend.
- **Conflict Handling**: If the backend returns a 409 Conflict (or if we detect the server `version` is newer than our local `base_version`), pause the queue and trigger the Conflict Resolution UI.

### 5. Backend Support (if applicable)

#### [MODIFY] `Controllers/BookmarksController.cs` (or equivalent)
- When syncing an incomplete bookmark, the backend should scrape the URL, retrieve the missing title/metadata, and return the fully populated bookmark to the client to replace the incomplete one.

## Verification Plan

### Automated/Manual Testing
- **Read Offline**: Disconnect network, refresh the page, verify tree notes, stash notes, and bookmarks are visible and readable.
- **Write Offline**: Disconnect network, add a stash note, edit a tree note, and add a bookmark (verify prompt for description and incomplete styling).
- **Sync**: Reconnect network, verify background sync fires, network requests are made, and GitHub is updated.
- **Conflicts**: Simulate a conflict by editing a note on GitHub while the phone is offline, edit the same note offline on the phone, go online, and verify the conflict resolution prompt appears.

