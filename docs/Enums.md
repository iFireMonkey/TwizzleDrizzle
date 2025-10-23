# FModel Enums

## Overview

This file contains enumeration definitions used throughout the FModel application for type-safe configuration, status tracking, and operation modes.

## Enumerations

### EBuildKind
Represents the build configuration type.
- **Debug**: Debug build configuration
- **Release**: Release build configuration  
- **Unknown**: Unknown or unspecified build type

### EErrorKind
Defines error handling strategies.
- **Ignore**: Ignore the error and continue
- **Restart**: Restart the application
- **ResetSettings**: Reset application settings to defaults

### SettingsOut
Specifies types of settings reload operations.
- **ReloadLocres**: Reload localization resources
- **ReloadMappings**: Reload mapping files

### EStatusKind
Tracks application or operation status with descriptive comments.
- **Ready**: Application is ready for use
- **Loading**: Currently performing operations
- **Stopping**: Attempting to stop current operations
- **Stopped**: Operations have been stopped
- **Failed**: Operation crashed or failed
- **Completed**: Operation completed successfully

### EAesReload
Controls AES key reloading behavior with user-friendly descriptions.
- **Always**: `"Always"` - Reload keys on every operation
- **Never**: `"Never"` - Never automatically reload keys
- **OncePerDay**: `"Once Per Day"` - Reload keys once daily

### EDiscordRpc
Controls Discord Rich Presence integration.
- **Always**: `"Always"` - Always show Discord presence
- **Never**: `"Never"` - Never show Discord presence

### ELoadingMode
Defines asset loading strategies with detailed descriptions.
- **Multiple**: `"Multiple"` - Load multiple selected assets
- **All**: `"All"` - Load all available assets
- **AllButNew**: `"All (New)"` - Load all assets except new ones
- **AllButModified**: `"All (Modified)"` - Load all assets except modified ones
- **AllButPatched**: `"All (Except Patched Assets)"` - Load all assets except patched ones

### ECompressedAudio
Controls compressed audio playback behavior.
- **PlayDecompressed**: `"Play the decompressed data"` - Play audio after decompression
- **PlayCompressed**: `"Play the compressed data (might not always be a valid audio data)"` - Attempt to play compressed data directly

### EIconStyle
Defines icon rendering styles for assets.
- **Default**: `"Default"` - Standard icon appearance
- **NoBackground**: `"No Background"` - Icon without background
- **NoText**: `"No Text"` - Icon without text labels
- **Flat**: `"Flat"` - Flat design style
- **Cataba**: `"Cataba"` - Custom Cataba style

### EEndpointType
Specifies types of API endpoints.
- **Aes**: AES encryption key endpoints
- **Mapping**: Asset mapping endpoints

### EBulkType
Flag enumeration for bulk operation types using bitwise operations.
- **None**: `0` - No operations selected
- **Auto**: `1 << 0` - Automatic detection
- **Properties**: `1 << 1` - Property files
- **Textures**: `1 << 2` - Texture assets
- **Meshes**: `1 << 3` - Mesh geometry
- **Skeletons**: `1 << 4` - Skeletal structures
- **Animations**: `1 << 5` - Animation data

## Attributes Used

### Description Attribute
Many enum values use the `[Description]` attribute to provide user-friendly display names that differ from the enum member names. This is commonly used in UI dropdowns and configuration displays.

### Flags Attribute
The `EBulkType` enum uses the `[Flags]` attribute to enable bitwise combination of values, allowing multiple operation types to be selected simultaneously.

## Usage Examples

Status tracking:
```csharp
var status = EStatusKind.Loading;
if (status == EStatusKind.Completed) { /* handle completion */ }
```

Bulk operations with flags:
```csharp
var operations = EBulkType.Textures | EBulkType.Meshes;
if (operations.HasFlag(EBulkType.Textures)) { /* process textures */ }
```

Configuration with descriptions:
```csharp
var mode = ELoadingMode.AllButNew;
var description = mode.GetDescription(); // Returns "All (New)"
```

### Dependencies
- **System**: For basic enum functionality and Flags attribute
- **System.ComponentModel**: For Description attribute support