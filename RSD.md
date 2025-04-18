# RSD v1.0 - Radiance Sound Definition

**Author**: Dr Ocelot

**Common extension**: `.rsd` *(sometimes prefixed with codec, i.e. `.aac.rsd` or `.opus.rsd`)*

**Byte Order**: Little Endian

**Padding/Alignment**: None

## File Layout
| Section            | Description |
|--------------------|-------------|
| **Header**        | Contains the file signature `"tRSD"` as well as the version info. |
| **Sprite Definitions** | Array of Sprite Definition structs.  Must include at least one, and must include an ending with `0` name length. |
| **Audio Header**  | Stores the total size of the audio data in bytes. |
| **Audio Data**    | The raw audio content in a browser-compatible format. |
---
## Section Details

### Header Signature

| Bytes (Hex)                                      | ASCII Representation |
|--------------------------------------------------|----------------------|
| `74 52 53 44` | `"tRSD"` |

This signature must be present at the beginning of the file.

### Header Version Info

| Type    | Name           | Description |
|---------|----------------|-------------|
| `uint32` | Version Major | The major version. Changes to this number are not backwards compatible to the game. |
| `uint32` | Version Minor | The minor version. Changes to this number are backwards compatible inside the same major version. |

Following the signature is this version info. The version of this spec is 1.0.

### Sprite Definition

Each sprite represents an audio segment and consists of the following fields:

| Type    | Name           | Description |
|---------|----------------|-------------|
| `float32` | Start Offset | Time (in seconds) where the sprite begins. |
| `uint32` or `0` | Name Length   | Length of the sprite name in ASCII characters. `0` indicates that there are no more sprites left.|
| `char[]` | Name          | The name of the sprite in ASCII. It can be any length specified by Name Length, including 0. |

### Audio Header

| Type    | Name          | Description |
|---------|--------------|-------------|
| `Uint32` | Data Size | The total size of the audio data section in bytes. |

### Audio Data

Audio data in a **browser-compatible audio format**,
i.e. an ogg file with the opus codec.

**Do not pad** audio data with silence between sound sprites.
The game will calculate a sprite's length based on the next sprite's offset.
