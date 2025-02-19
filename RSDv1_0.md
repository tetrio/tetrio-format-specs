# RSD v1.0 - Radiance Sound Definition

**Author**: Dr Ocelot

**Common extension**: `.rsd` *(sometimes prefixed with codec, i.e. `.aac.rsd` or `.opus.rsd`)*

**Byte Order**: Little Endian

**Padding/Alignment**: None

## File Layout
| Section            | Description |
|--------------------|-------------|
| **Header**        | Contains the file signature `"TETRIO SOUND 1.0"`. |
| **Sprite Definitions** | Array of Sprite Definition structs.  Must include at least one, and must include an ending with `0` name length. |
| **Audio Header**  | Stores the total size of the audio data in bytes. |
| **Audio Data**    | The raw audio content in a browser-compatible format. |
---
## Section Details

### Header

| Bytes (Hex)                                      | ASCII Representation |
|--------------------------------------------------|----------------------|
| `54 45 54 52 49 4F 20 53 4F 55 4E 44 20 31 2E 30` | `"TETRIO SOUND 1.0"` |

This signature must be present at the beginning of the file.

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
