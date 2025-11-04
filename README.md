<img alt="ghostmark-logo" src="./assets/images/logo.png" style="margin-left:auto; margin-right:auto; display:block; width:200px;"/>

# Ghostmark

**Ghostmark** is a lightweight, browser-based steganography tool that hides one image inside another using seeded, randomized least significant bit encoding. The entire process runs locally in your browser for complete privacy and repeatable, verifiable results.

## Overview

Ghostmark conceals a black-and-white dithered image within a full-color cover image using a deterministic random sequence derived from a seed. The seed defines which pixels and channels are used during encoding, allowing perfect reconstruction during decoding.

Users can either embed the seed directly into the image for automatic decoding or withhold it, using the seed as a type of password. No data ever leaves the browser; every operation happens client-side only.

## Features

- Seeded, randomized LSB encoding: A Mulberry32 pseudorandom generator ensures reproducible placement of hidden bits.

- Automatic black-and-white dithering: Converts the hidden image to 1-bit monochrome using the Floyd–Steinberg algorithm for balanced contrast and detail.

- Optional seed embedding: Stores the 32-bit seed in the top-left 8×8 pixel block for self-decoding, or omits it entirely for password-protected decoding.

- Offline operation: No network communication, no storage, no tracking.

- Preview-based workflow: Visualizes both the dithered hidden image and the final stego image before saving.

- Deterministic decoding: Guarantees lossless reconstruction when using the same seed.

## Technical Notes

### Encoding process

- The hidden image is resized to match the cover image dimensions.

- The hidden image is converted to grayscale using ITU-R BT.601 luma coefficients (0.299R, 0.587G, 0.114B).

- Floyd–Steinberg dithering distributes quantization errors to neighboring pixels with weights 7/16, 3/16, 5/16, and 1/16.

- Each hidden pixel’s value determines the parity (even or odd) of one randomly chosen color channel in the cover image.

- Randomization of parity and channel choice is controlled by a Mulberry32 PRNG seeded with either a 32-bit integer or a hashed passphrase (FNV-1a 32-bit).

### Seed header

- If enabled, Ghostmark writes the identifier SG1 followed by the 32-bit seed into the least significant bits of the top-left 8×8 block of red channel pixels.

- This allows the decoder to recover the seed automatically.

- If disabled, decoding requires manual seed entry; in this mode, the seed functions as a password.

### Decoding process

- If a header is found, the seed is extracted and used to regenerate the same pseudorandom sequence.

- The parity of each chosen pixel and channel reveals the hidden bit, reconstructed into a black-and-white image.

- When no header is present, the user supplies the seed manually for exact recovery.

## Intended Users

Ghostmark is designed for artists, puzzle creators, and ARG developers who explore concealment and pattern as a creative language. It is equally suited to educators and researchers demonstrating steganography principles or studying data integrity through deterministic randomness.

The tool aims to make digital hiding both transparent and approachable, showing how meaning can persist invisibly within the noise of an image.

## Usage

1. Open the tool in your browser.
2. Upload a cover image and a hidden image.
3. Enter a seed value or generate one automatically.
4. Choose whether to embed the seed header.
5. Click Prepare Hidden to preview the dithered result, then Encode into Cover.
6. Download the resulting stego image.
7. To decode, open the Decode tab, upload the stego image, and click Decode.

## License

MIT License - free for modification and use. Attribution appreciated if used publicly.

## Credit

Created for puzzle solvers, game designers, and curious thinkers. If you use Ghostmark in a puzzle or ARG, I'd love to hear about it.
