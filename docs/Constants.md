# Constants

## Overview

The `Constants` static class provides application-wide constants, configuration values, and color palettes for the FModel application. It includes application metadata, scaling factors, color definitions, and external links.

## Application Metadata

### APP_PATH
- **Type**: `string` (readonly)
- **Description**: Full path to the application executable
- **Source**: Resolved from command line arguments

### APP_VERSION
- **Type**: `string` (readonly)  
- **Description**: File version of the application
- **Source**: Extracted from executable file version info

### APP_COMMIT_ID
- **Type**: `string` (readonly)
- **Description**: Full Git commit ID of the build
- **Source**: Extracted from product version after '+' delimiter

### APP_SHORT_COMMIT_ID
- **Type**: `string` (readonly)
- **Description**: Truncated Git commit ID (first 7 characters)
- **Usage**: Display purposes where space is limited

## Core Constants

### ZERO_64_CHAR
- **Type**: `string` (const)
- **Value**: 64-character string of zeros
- **Usage**: Default/empty hash representation

### ZERO_GUID
- **Type**: `FGuid` (readonly)
- **Value**: GUID initialized with 0
- **Usage**: Default/empty GUID value

### SCALE_DOWN_RATIO
- **Type**: `float` (const)
- **Value**: 0.01F
- **Usage**: Scaling factor for 3D models and coordinates

### SAMPLES_COUNT
- **Type**: `int` (const)
- **Value**: 4
- **Usage**: Number of samples for rendering operations

## Color Definitions

### UI Colors
- **WHITE**: `#DAE5F2` - Primary light color
- **GRAY**: `#BBBBBB` - Secondary neutral color  
- **RED**: `#E06C75` - Error/warning color
- **GREEN**: `#98C379` - Success/positive color
- **YELLOW**: `#E5C07B` - Caution/highlight color
- **BLUE**: `#528BCC` - Information/accent color

### Color Palette
- **PALETTE_LENGTH**: Property returning the length of COLOR_PALETTE array
- **COLOR_PALETTE**: Array of `Vector3` values representing RGB colors (0-1 range)

#### Palette Colors
1. **Dark Gray**: (0.231, 0.231, 0.231)
2. **Teal**: (0.376, 0.490, 0.545)
3. **Red**: (0.957, 0.263, 0.212)
4. **Green**: (0.196, 0.804, 0.196)
5. **Orange**: (0.957, 0.647, 0.212)
6. **Purple**: (0.612, 0.153, 0.690)
7. **Blue**: (0.129, 0.588, 0.953)
8. **Yellow**: (1.000, 0.920, 0.424)
9. **Brown**: (0.824, 0.412, 0.118)
10. **Light Blue**: (0.612, 0.800, 0.922)

## External Links

### GitHub Repository
- **ISSUE_LINK**: Q&A discussions page
- **GH_REPO**: API endpoint for repository
- **GH_COMMITS_HISTORY**: Commits API endpoint
- **GH_RELEASES**: Releases API endpoint

### External Services
- **DONATE_LINK**: Donation page URL
- **DISCORD_LINK**: Discord server invitation

## Game-Specific Constants

### Manifest Triggers
- **_FN_LIVE_TRIGGER**: `fortnite-live.manifest` - Fortnite live version identifier
- **_VAL_LIVE_TRIGGER**: `valorant-live.manifest` - Valorant live version identifier

### Configuration
- **_NO_PRESET_TRIGGER**: `Hand Made` - Identifier for custom/manual configurations

## Usage

The constants are accessed statically throughout the application:

- Application metadata for about dialogs and version checking
- Color values for UI theming and syntax highlighting  
- URLs for external navigation and API calls
- Game-specific identifiers for version detection
- Color palette for consistent visual representation

## Dependencies

- **System.Numerics**: For Vector3 color representations
- **CUE4Parse.UE4.Objects.Core.Misc**: For FGuid type
- **CUE4Parse.Utils**: For string extension methods
