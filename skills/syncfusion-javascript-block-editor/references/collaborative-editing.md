# Collaborative Editing

Complete reference for real-time collaborative editing, user presence, and version history in the Syncfusion JavaScript BlockEditor.

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Yjs Providers](#yjs-providers)
- [CollaborationSettings API](#collaborationsettings-api)
- [Getting Started](#getting-started)
  - [Step 1: Create a Yjs Document](#step-1-create-a-yjs-document)
  - [Step 2: Create a Yjs Adapter](#step-2-create-a-yjs-adapter)
  - [Step 3: Configure a Provider](#step-3-configure-a-provider)
  - [Step 4: Enable Collaboration](#step-4-enable-collaboration)
- [User Presence and Remote Cursors](#user-presence-and-remote-cursors)
- [Configure the Current User](#configure-the-current-user)
  - [Get Active Users](#get-active-users)
- [Version History](#version-history)
  - [Enable Version History](#enable-version-history)
  - [Access the Version History Instance](#access-the-version-history-instance)
  - [Configure Snapshot Storage](#configure-snapshot-storage)
  - [Version History Methods](#version-history-methods)
  - [Version History Events](#version-history-events)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Code Examples](#code-examples)

## Overview

The Block Editor supports real-time collaborative editing, enabling multiple users to work on the same document simultaneously. Collaboration is powered by **Yjs**, a Conflict-free Replicated Data Type (CRDT) framework that synchronizes document changes across all connected users and automatically resolves conflicts.

With collaboration enabled, users can:

- Edit the same document in real time
- View remote user cursors and selections
- Track active collaborators
- Perform collaboration-aware undo and redo operations
- Create, restore, compare, export, and import document versions

## Prerequisites

Before enabling collaboration, install the `yjs` library and a Yjs provider. See [Yjs Providers](https://docs.yjs.dev/ecosystem/connection-provider) to choose the right provider for your use case.

Inject the `Collaboration` module into the Block Editor before use.

```typescript
import { BlockEditor, Collaboration } from '@syncfusion/ej2-blockeditor';

BlockEditor.Inject(Collaboration);
```

## Yjs Providers

A Yjs provider handles the transport of document updates between connected users. Choose a provider based on your deployment requirements.

| Provider | Type | Use Case |
|----------|------|----------|
| `y-websocket` | Self-hosted | Production deployments with your own WebSocket server |
| `y-webrtc` | Peer-to-peer | Quick local testing and development; no server required |
| `y-indexeddb` | Local storage | Offline persistence within a single browser |
| [Hocuspocus](https://tiptap.dev/docs/hocuspocus/getting-started/overview) | Open-source server | Scalable Node.js server with pluggable storage and Redis support |
| [Liveblocks](https://liveblocks.io/) | Fully managed | Hosted WebSocket infrastructure with REST API and DevTools |
| [PartyKit](https://www.partykit.io/) | Serverless | Serverless provider on Cloudflare; ideal for prototyping |

**Note:** For development and testing, `y-webrtc` or PartyKit allow you to get started without a server. For production, use `y-websocket` or a managed provider such as Liveblocks or Hocuspocus for reliable, persistent synchronization.

## CollaborationSettings API

Use the `collaborationSettings` property of type `CollaborationSettingsModel` to configure collaboration behavior for the Block Editor.

| Property | Type | Description |
|----------|------|-------------|
| `provider` | `any` | Real-time transport used to synchronize document changes |
| `enableAwareness` | `boolean` | Enables user presence, remote cursors, and text selection overlays |
| `adapter` | `CollaborationAdapter` | Provides the Yjs runtime and shared XML fragment |
| `versionHistory` | `VersionHistorySettingsModel` | Configures document version history support |

## Getting Started

The following steps set up real-time collaboration in the Block Editor using Yjs.

### Step 1: Create a Yjs Document

Create a shared Yjs document and XML fragment.

```typescript
import * as Y from 'yjs';

const yDoc = new Y.Doc();
const yFragment = yDoc.getXmlFragment('blockeditor');
```

### Step 2: Create a Yjs Adapter

Create an adapter that provides the Yjs runtime and the shared fragment to the Block Editor.

```typescript
import * as Y from 'yjs';
import { YjsAdapter } from '@syncfusion/ej2-blockeditor';

const adapter: YjsAdapter = {
    yRuntime: Y,
    yXmlFragment: yFragment
};
```

### Step 3: Configure a Provider

Create a provider that connects users to the same shared document.

**Production (y-websocket):**

```typescript
import { WebsocketProvider } from 'y-websocket';

const provider = new WebsocketProvider(
    'wss://your-server-url',
    'document-room-id',
    yDoc
);
```

**Development (y-webrtc):**

```typescript
import { WebrtcProvider } from 'y-webrtc';

const provider = new WebrtcProvider('document-room-id', yDoc);
```

**Note:** For local development, `y-webrtc` or a PartyKit provider work without any server setup. Switch to `y-websocket` or a managed provider for production.

### Step 4: Enable Collaboration

Pass the adapter and provider to the Block Editor through the `collaborationSettings` property.

```typescript
const blockEditor = new BlockEditor({
    collaborationSettings: {
        adapter: adapter,
        provider: provider
    }
});
```

## User Presence and Remote Cursors

The Block Editor can display remote cursors, text selection overlays, and user details on hover. To enable these user presence features, set `enableAwareness` to `true` in `collaborationSettings`.

```typescript
const blockEditor = new BlockEditor({
    collaborationSettings: {
        adapter: adapter,
        provider: provider,
        enableAwareness: true
    }
});
```

## Configure the Current User

Set the current user's display name and cursor highlight color using the `users` and `currentUserId` properties. The `avatarBgColor` value is used for that user's remote cursor and text selection overlay.

| Property | Type | Description |
|----------|------|-------------|
| `id` | `string` | Unique identifier for the user |
| `user` | `string` | Display name shown on remote cursors and presence indicators |
| `avatarBgColor` | `string` | Hex color used for this user's remote cursor and selection highlight |

```typescript
const blockEditor = new BlockEditor({
    users: [{
        id: 'user-1',
        user: 'John Doe',
        avatarBgColor: '#e74c3c'
    }],
    currentUserId: 'user-1'
});
```

### Get Active Users

Retrieve all currently connected users using the `users` property on the block editor instance.

```typescript
const users = blockEditor.users;
```

## Version History

Version History allows you to capture document snapshots and restore earlier versions. This is a built-in capability of the Block Editor and does not require a third-party service.

### Enable Version History

Inject the `VersionHistory` module and configure the `versionHistory` property under `collaborationSettings`.

```typescript
import { BlockEditor, VersionHistory } from '@syncfusion/ej2-blockeditor';

BlockEditor.Inject(VersionHistory);

const myStorage = new CustomVersionStorage(`blockeditor-${uniqueId}`);

const blockEditor = new BlockEditor({
    collaborationSettings: {
        adapter: adapter,
        provider: provider,
        versionHistory: {
            storage: myStorage,
            snapshotInterval: 3000
        }
    }
});
```

### Access the Version History Instance

After the Block Editor initializes, retrieve the version history instance and wait for snapshot data to load before calling any version history methods.

```typescript
const versionHistory = blockEditor.getVersionHistory();
await versionHistory.whenReady();
```

### Configure Snapshot Storage

Version snapshots need to be persisted to enable version history across browser sessions. Implement the `IVersionStorage` interface to provide a custom storage backend for managing snapshots. You can use IndexedDB, a backend database, or any other storage solution suitable for your deployment.

The `IVersionStorage` interface defines the following methods:

| Method | Signature | Description |
|--------|-----------|-------------|
| `saveSnapshot` | `(snapshot: VersionSnapshot): Promise<void>` | Persist a snapshot |
| `loadAllSnapshots` | `(): Promise<VersionSnapshot[]>` | Load all persisted snapshots, ordered by timestamp ascending |
| `loadSnapshot` | `(id: string): Promise<VersionSnapshot \| null>` | Load a single snapshot by id |
| `deleteSnapshot` | `(id: string): Promise<void>` | Permanently remove a snapshot by id |
| `clearAll` | `(): Promise<void>` | Remove all snapshots from storage |

### Version History Methods

#### createSnapshot

Creates a new snapshot of the current document state with an optional label and metadata.

```typescript
const snapshot = await versionHistory.createSnapshot({
    label: 'Before major update',
    modifiedBy: currentUserId
});
```

#### getSnapshots

Retrieves all saved snapshots or a paginated subset. Snapshots are returned in chronological order.

```typescript
// Retrieve all snapshots
const snapshots = versionHistory.getSnapshots();

// Retrieve a paginated subset — getSnapshots(skip, take)
const pagedSnapshots = versionHistory.getSnapshots(20, 40);
```

#### renameSnapshot

Updates the label or metadata of an existing snapshot without modifying its content.

```typescript
await versionHistory.renameSnapshot(snapshotId, 'Release Candidate');
```

#### restoreSnapshot

Reverts the document to a previously saved snapshot state.

```typescript
await versionHistory.restoreSnapshot(snapshotId);
```

**Note:** When a snapshot is restored, the current document state is automatically backed up before the restore operation is applied.

#### compareVersions

Compares two snapshots to identify differences such as added, removed, or modified content. The returned `VersionDiff` object provides a summary of the differences between the two selected versions.

```typescript
const diff = versionHistory.compareVersions(snapshotIdA, snapshotIdB);
```

#### exportSnapshot

Serializes a snapshot into a portable format that can be stored externally or transferred between systems.

```typescript
const exported = await versionHistory.exportSnapshot(snapshotId);
```

#### importSnapshot

Imports a previously exported snapshot back into the version history storage.

```typescript
const imported = await versionHistory.importSnapshot(exported);
```

### Version History Events

Use the following event callbacks in `versionHistory` settings to respond to snapshot life cycle events.

| Event | Description |
|-------|-------------|
| `snapshotCreated` | Triggered when a new snapshot is created |
| `snapshotRestored` | Triggered when a snapshot is restored |

**snapshotCreated:**

```typescript
const blockEditor = new BlockEditor({
    collaborationSettings: {
        versionHistory: {
            storage: myStorage,
            snapshotCreated: ({ snapshot }) => {
                console.log(snapshot.id);
            }
        }
    }
});
```

**snapshotRestored:**

```typescript
const blockEditor = new BlockEditor({
    collaborationSettings: {
        versionHistory: {
            storage: myStorage,
            snapshotRestored: ({ snapshot, backupSnapshot }) => {
                console.log(snapshot.label);
            }
        }
    }
});
```

## Best Practices

1. **Use WebRTC or PartyKit for development** — These providers require no server setup and are ideal for local testing and prototyping before moving to a production provider
2. **Use WebSocket-based providers in production** — `y-websocket`, Hocuspocus, or a managed service like Liveblocks provides reliable, low-latency, persistent synchronization at scale
3. **Use stable room identifiers** — Use a unique document ID as the collaboration room name to prevent unintended document sharing between different documents
4. **Persist snapshots externally** — Store snapshots in a database or cloud storage to preserve version history across sessions
5. **Enable awareness selectively** — Disable `enableAwareness` when user presence information is not required, to reduce network and processing overhead

## Troubleshooting

### Changes Are Not Synchronizing

Verify the following:

- All users are connected to the same collaboration room
- The provider connection is active
- The shared Yjs document is correctly configured

### Remote Cursors Are Not Visible

Verify the following:

- `enableAwareness` is set to `true`
- The configured provider supports the Yjs awareness protocol
- User information is set via the `users` and `currentUserId` properties
- Each user has a unique `id` value

### Remote User Names Are Not Appearing on Cursors

Verify the following:

- The `user` field is populated for all entries in the `users` array

### Version History Is Not Available

Verify the following:

- The `VersionHistory` module is injected into the Block Editor
- A valid `IVersionStorage` implementation is provided
- `whenReady()` has been awaited before accessing snapshots

## Code Examples

### Complete Collaborative Editing Example

```typescript
import { BlockEditor, Collaboration, VersionHistory, YjsAdapter } from '@syncfusion/ej2-blockeditor';
import * as Y from 'yjs';
import { WebsocketProvider } from 'y-websocket';

BlockEditor.Inject(Collaboration, VersionHistory);

// 1. Create the shared Yjs document
const yDoc = new Y.Doc();
const yFragment = yDoc.getXmlFragment('blockeditor');

// 2. Create the Yjs adapter
const adapter: YjsAdapter = {
    yRuntime: Y,
    yXmlFragment: yFragment
};

// 3. Configure the provider
const provider = new WebsocketProvider(
    'wss://your-server-url',
    'document-room-id',
    yDoc
);

// 4. Configure snapshot storage for version history
const myStorage = new CustomVersionStorage('blockeditor-doc-1');

// 5. Initialize the Block Editor with collaboration enabled
const blockEditor: BlockEditor = new BlockEditor({
    users: [{
        id: 'user-1',
        user: 'John Doe',
        avatarBgColor: '#e74c3c'
    }],
    currentUserId: 'user-1',
    collaborationSettings: {
        adapter: adapter,
        provider: provider,
        enableAwareness: true,
        versionHistory: {
            storage: myStorage,
            snapshotInterval: 3000,
            snapshotCreated: ({ snapshot }) => {
                console.log('Snapshot created:', snapshot.id);
            },
            snapshotRestored: ({ snapshot, backupSnapshot }) => {
                console.log('Restored snapshot:', snapshot.label);
            }
        }
    }
});

blockEditor.appendTo('#blockeditor');

// 6. Work with version history once the editor is ready
async function setupVersionHistory(): Promise<void> {
    const versionHistory = blockEditor.getVersionHistory();
    await versionHistory.whenReady();

    const snapshot = await versionHistory.createSnapshot({
        label: 'Initial version',
        modifiedBy: 'user-1'
    });

    const snapshots = versionHistory.getSnapshots();
    console.log('Total snapshots:', snapshots.length);
}

setupVersionHistory();
```
---
