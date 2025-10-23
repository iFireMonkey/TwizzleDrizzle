# FIoStoreTocHeader

## Overview

The `FIoStoreTocHeader` class represents the header structure for Unreal Engine's IO Store Table of Contents (TOC) files. It contains metadata about the IO store container including compression, encryption, and chunk information.

## Constants

- **SIZE**: 144 bytes - The fixed size of the TOC header
- **TOC_MAGIC**: `{0x2D, 0x3D, 0x3D, 0x2D, 0x2D, 0x3D, 0x3D, 0x2D, 0x2D, 0x3D, 0x3D, 0x2D, 0x2D, 0x3D, 0x3D, 0x2D}` - Magic bytes representing "-==--==--==--==-"

## Properties

### Core Information
- **TocMagic** (`byte[]`): Magic bytes for file validation (16 bytes)
- **Version** (`EIoStoreTocVersion`): Version of the TOC format
- **TocHeaderSize** (`uint`): Size of the header in bytes
- **ContainerId** (`FIoContainerId`): Unique identifier for the container
- **ContainerFlags** (`EIoContainerFlags`): Flags indicating container properties (compressed, encrypted, etc.)

### Entry Counts
- **TocEntryCount** (`uint`): Number of entries in the TOC
- **TocCompressedBlockEntryCount** (`uint`): Number of compressed block entries
- **TocCompressedBlockEntrySize** (`uint`): Size of compressed block entries (for sanity checking)
- **TocChunkPerfectHashSeedsCount** (`uint`): Number of perfect hash seeds for chunks
- **TocChunksWithoutPerfectHashCount** (`uint`): Number of chunks without perfect hash

### Compression Information
- **CompressionMethodNameCount** (`uint`): Number of compression methods
- **CompressionMethodNameLength** (`uint`): Length of compression method names
- **CompressionBlockSize** (`uint`): Size of compression blocks

### Storage Information
- **DirectoryIndexSize** (`uint`): Size of the directory index
- **PartitionCount** (`uint`): Number of partitions in the container
- **PartitionSize** (`ulong`): Size of each partition

### Security
- **EncryptionKeyGuid** (`FGuid`): GUID for encryption key identification

## Constructor

Creates a new instance by reading from an FArchive stream. Validates the magic bytes and reads all header fields in the correct order.

### Validation
- Throws `ParserException` if the magic bytes don't match the expected TOC_MAGIC sequence
- Ensures proper alignment after reading all fields (4-byte alignment)

## Related Enums

### EIoStoreTocVersion
Defines the version of the TOC format:
- **Invalid** (0): Invalid version
- **Initial**: First version
- **DirectoryIndex**: Added directory indexing
- **PartitionSize**: Added partition size support
- **PerfectHash**: Added perfect hash support
- **PerfectHashWithOverflow**: Perfect hash with overflow handling
- **OnDemandMetaData**: Added on-demand metadata
- **RemovedOnDemandMetaData**: Removed on-demand metadata
- **ReplaceIoChunkHashWithIoHash**: Replaced chunk hash with IO hash
- **Latest**: Current latest version

### EIoContainerFlags
Bit flags indicating container properties:
- **None** (0): No special flags
- **Compressed** (1 << 0): Container uses compression
- **Encrypted** (1 << 1): Container is encrypted
- **Signed** (1 << 2): Container is digitally signed
- **Indexed** (1 << 3): Container has indexing
- **OnDemand** (1 << 4): Container supports on-demand loading

## Usage

This class is typically used when parsing Unreal Engine IO store files (.utoc) to understand the structure and properties of the associated container data. The header provides essential information for reading and interpreting the rest of the TOC file.
