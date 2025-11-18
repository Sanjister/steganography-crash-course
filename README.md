Demo accessible at: https://sanjister.github.io/steganography-crash-course/

## Cybersecurity, Confidentiality and Encryption

Confidentiality is one of the vital pillars of cybersecurity, it ensures that data is only available to authorized users.

To prevent confidential data from being read by third parties, we use encryption to scramble data in ways that only authorised parties with their unique secret keys can decode.\
This is how connections between our web browsers and websites are secured (using the HTTPS protocol), emsuring our passwords aren't send as plain text over the internet.

Example:
- Tom wants to send a top secret message to Jerry
- They create a unique key and share it with eachother (can be a word, a number, an image, a date etc...)
- Tom passes his message (e.g. `Hello!`) through an encryption function which uses the key and produces a completely different output e.g. `$a&4P@3_`
- Tom sends this encrypted output to Jerry
- Jerry passes the received data through a decryption function using his key, which translates it back to `Hello!`
- TL;DR: `Hello! 🔑🔒 → ❔ → 🔑🔓 Hello!`

## Steganography

The art or practice of concealing information within a message, image or file. \
Greek *steganos* (covered) + Latin *-graphy*. 

In this project we will be demonstrating **image differencing steganography**.

Our "key" will be an image in which a message will be concealed by slightly altering the base pixel values. These changes should be unnoticeable to the human eye, therefore allowing secret messages to be exchanged discreetly under the form of banal images. All resulting images will also remain the same size as the original.

We will be using **PNGs** that store **RGBA** pixels in 32-bit sequences (8 bits for red, 8 for green, 8 for blue and 8 for alpha) and we will apply the **XOR** operator to encrypt bits.

#### XOR truth table

| X | Y | X XOR Y |
| --- | --- | --- |
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

### Algorithm breakdown

1. **Input Preparation**:
   - Load the base image (the image in which the message will be concealed).
   - Convert the message to be hidden into a binary format \
   e.g. `Hello!` → `01001000 01100101 01101100 01101100 01101111 00100001` (8 bits per character)

2. **Pixel Manipulation**:
   - For each pixel in the base image:
        - Retrieve the pixel's RGB values. \
        e.g. Red: `218`, Green: `41`, Blue: `28`
        - Retreive the **least significant bit (LSB)** of each color value \
        e.g. Red LSB: `0`, Green LSB: `1`, Blue LSB: `0`
        - Modify the LSB by applying XOR with the next message bit values. \
     e.g. If a pixel's LSB is `1` and the next message bit is `0`, then the ecrypted bit (XOR result) is `1`.

3. **Output Generation**:
   - After all bits of the message have been encoded, save the modified image as the output image.

4. **Decoding Process** (for the recipient):
   - Load the base image and encrypted image.
   - Extract the LSB from each pixel's RGB values in parallel (both of the base image and encrypted image). \
   e.g. If the base pixel LSB is 1 and the encrypted pixel LSB is 1, then the decrypted bit is 0 (back to what we had in the previous example).
   - Apply XOR between both LSB to get the original message bit.
   - Reconstruct the binary message from the extracted bits.
   - Convert the binary message back to its original text format.
