# FIoStoreTocCompressedBlockEntry

## Overview

The `FIoStoreTocCompressedBlockEntry` struct represents a compressed block entry in Unreal Engine's IO Store Table of Contents (TOC). It contains information about a compressed data block including its offset, compressed size, uncompressed size, and compression method.

## Constants

- **SIZE**: 12 bytes - The fixed size of a compressed block entry (5 + 3 + 3 + 1)
- **OffsetBits**: 40 - Number of bits used for the offset field
- **OffsetMask**: `(1ul << 40) - 1ul` - Bit mask for extracting the offset
- **SizeBits**: 24 - Number of bits used for size fields
- **SizeMask**: `(1 << 24) - 1` - Bit mask for extracting size values
- **SizeShift**: 8 - Bit shift amount for size extraction

## Properties

- **Offset** (`long`): The byte offset of the compressed block in the container file
- **CompressedSize** (`uint`): Size of the data when compressed
- **UncompressedSize** (`uint`): Size of the data when uncompressed
- **CompressionMethodIndex** (`byte`): Index into the compression methods array

## Constructor

Creates a new instance by reading from an FArchive stream using unsafe pointer operations for efficient bit manipulation.

### Implementation Details
- Uses unsafe code with stack-allocated byte array for performance
- Extracts offset using a 40-bit mask from the first 8 bytes
- Extracts compressed size by shifting and masking the second 4 bytes
- Extracts uncompressed size by masking the third 4 bytes
- Extracts compression method index from the upper 8 bits of the third 4 bytes

## Bit Layout

The 12-byte structure is packed as follows:
- Bytes 0-7: Offset (40 bits) + unused bits
- Bytes 8-11: Compressed size (24 bits) shifted left 8 bits
- Bytes 8-11: Uncompressed size (24 bits) in lower bits
- Bytes 8-11: Compression method index (8 bits) in upper bits of bytes 8-11

## Methods

### ToString()
Returns a string representation showing the offset and size transformation in the format "Offset {offset}: From {compressedSize} To {uncompressedSize}".

## Usage

This struct is used when parsing IO Store TOC files to understand the compression layout of data blocks within the container. Each entry describes how to locate and decompress a specific block of data.

## Performance Notes

- Uses unsafe code for direct memory access and bit manipulation
- Stack allocation avoids heap allocation for the temporary byte array
- Efficient bit operations for extracting packed data fields
