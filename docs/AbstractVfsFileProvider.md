# AbstractVfsFileProvider

Location: `CUE4Parse/CUE4Parse/FileProvider/Vfs/AbstractVfsFileProvider.cs`  
Namespace: `CUE4Parse.FileProvider.Vfs`

Abstract base for mounting and reading Unreal Engine VFS containers (PAK and IoStore). Handles registration, mounting, decryption, file/package access, and lifecycle management.

- Inherits: `AbstractFileProvider`
- Implements: `IVfsFileProvider`

## Highlights

- Registers PAK (`.pak`) and IoStore (`.utoc`/`.ucas`) archives from paths or streams.
- Supports AES decryption by key or per\-game custom decryption delegate.
- Concurrent mounting with events for register/mount/unmount.
- Exposes file and package accessors, plus raw readers for assets.
- Tracks required AES keys and validated keys by GUID.

## Constructor behavior

Automatically assigns a `CustomEncryption` delegate based on `Versions.Game` for known titles. Explicitly set `CustomEncryption` to override.

## Lifecycle

1. Call `RegisterVfs(...)` to enqueue archives.
2. For unencrypted or custom\-encrypted archives, call `MountAsync()`.
3. For AES\-encrypted archives, call `SubmitKey(s)`/`SubmitKeysAsync`, which mounts matching archives.
4. Optionally call `PostMount()` to validate keys via INI configs.
5. Use indexers and helpers to read files and packages.
6. `Unload*` or `Dispose()` to release.

## Properties

- `UnloadedVfs` / `MountedVfs`: Registered vs. mounted archives.
- `Keys`: Known AES keys (`FGuid` → `FAesKey`).
- `RequiredKeys`: GUIDs of archives that still need keys.
- `GlobalData`: `IoGlobalData` from `global.utoc` if present.
- `FilesById`: Package ID to `GameFile` map.
- `CustomEncryption`: Optional `IAesVfsReader.CustomEncryptionDelegate`.

## Events

- `VfsRegistered`
- `VfsMounted`
- `VfsUnmounted`

## Indexers

- `this[string path, string archiveName]`
- `this[string path, IAesVfsReader archive]`

Throw `KeyNotFoundException` if not found.

## Key methods (selected)

- Registration:
    - `RegisterVfs(string path)` / overloads for arrays, `FileInfo[]`, `Stream[]`, random\-access streams.
    - `RegisterVfs(IoChunkToc, IoStoreOnDemandOptions)` for on\-demand IoStore.
- Mounting:
    - `MountAsync()` / `Mount()`: Mounts archives that are not AES\-encrypted or use custom decryption and have a directory index.
    - `SubmitKey(s)` / `SubmitKeysAsync(...)`: Provide AES keys by GUID and mount matching archives.
- Access:
    - `GetArchive(...)` / `TryGetArchive(...)`
    - `TryGetGameFile(...)`, `CreateReader(...)`, `SaveAsset(...)`
    - `LoadPackage(...)`, `TryLoadPackage(...)`, `SavePackage(...)`, `TrySavePackage(...)`
- Maintenance:
    - `PostMount()`: Validates main AES key against INI configs; unmounts archives if invalid.
    - `UnloadAllVfs()` / `UnloadNonStreamedVfs()`
    - `Dispose()`

## Notes

- IoStore `GlobalData` is captured when `global.utoc` or `global_console_win.utoc` is registered.
- `InvalidAesKeyException` is swallowed during mounting; failures are logged.
- Uses concurrent collections; mounting operations are performed in parallel.

## Usage

Register and mount an unencrypted PAK:

~~~csharp
// Register .pak and mount
var provider = new MyVfsFileProvider(versions);
provider.RegisterVfs(@"D:\Game\Content\Paks\pakchunk0-WindowsClient.pak");
await provider.MountAsync();

// Read a file's bytes
var data = provider.SaveAsset("/Game/Path/To/Asset.uasset", "pakchunk0-WindowsClient.pak");
~~~

Register IoStore and mount:

~~~csharp
// Register .utoc with its .ucas (path[] overload pairs them)
provider.RegisterVfs(new[]
{
    @"D:\Game\Content\Paks\global.utoc",
    @"D:\Game\Content\Paks\global.ucas"
});
await provider.MountAsync();

// Use GlobalData if needed
var gd = provider.GlobalData;
~~~

Mount AES\-encrypted archives with a key:

~~~csharp
// Submit AES key for a specific GUID, then affected archives auto-mount
var guid = new FGuid(0x11111111, 0x22222222, 0x33333333, 0x44444444);
var key  = new FAesKey("00112233445566778899AABBCCDDEEFF00112233445566778899AABBCCDDEEFF");
await provider.SubmitKeyAsync(guid, key);
~~~

Load a package:

~~~csharp
// Load and enumerate exports
var pkg = provider.LoadPackage("/Game/Maps/ExampleMap.umap", "global.utoc");
foreach (var (name, bytes) in provider.SavePackage("/Game/Maps/ExampleMap.umap", "global.utoc"))
{
    Console.WriteLine($"{name} -> {bytes.Length} bytes");
}
~~~

Override custom decryption (game\-specific):

~~~csharp
// Assign a custom decryption delegate before Mount/SubmitKey
provider.CustomEncryption = MyDecryptors.MyGameDecrypt;
await provider.MountAsync();
~~~
