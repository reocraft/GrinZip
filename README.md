# Grin Compression

**Author:** Reo Saito

A file compression/decompression tool implemented in Java using Huffman coding. Files are encoded into a `.grin` format and can be decoded back to their original form.

## How It Works

1. **Encoding:** Reads the input file byte-by-byte, builds a frequency map, constructs a Huffman tree from it, and writes the serialized tree + encoded bits to the output file. A magic number (`0x736`) is prepended to identify valid `.grin` files.
2. **Decoding:** Reads the magic number to validate the file, reconstructs the Huffman tree from the serialized header, then decodes the bit stream back to the original bytes.

## Usage

```
java Grin <encode|decode> <infile> <outfile>
```

**Examples:**
```
java Grin encode pg2600.txt pg2600.grin
java Grin decode pg2600.grin pg2600_decoded.txt
```

## Project Structure

```
src/main/java/edu/grinnell/csc207/compression/
├── Grin.java            # Entry point; encode/decode dispatch
├── HuffmanTree.java     # Huffman tree construction, serialization, encode/decode
├── BitInputStream.java  # Bit-level file reader
└── BitOutputStream.java # Bit-level file writer
```

## Requirements

- Java 21+
- Maven (for building/testing)

## Building & Testing

```
mvn compile
mvn test
```

## Notes

- The EOF symbol is encoded as value `256` (9-bit) so it does not conflict with valid byte values (0–255).
- The Huffman tree is serialized as a pre-order bit sequence: `0` + 9-bit value for leaves, `1` for internal nodes.
