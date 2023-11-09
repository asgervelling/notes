AES is a [[Block Cipher]].
AES uses a block size of 128 bits (16 bytes).

### Overview
- **AES** is a symmetric encryption algorithm adopted by the U.S. government.
- Replaced the Data Encryption Standard (DES) due to improved security.

### Key Features
- **[[Symmetric Encryption|Symmetric]] Cryptography**: Uses the same key for encryption and decryption.
- **Block Cipher**: Operates on fixed-size blocks of data.
- Supports key sizes of 128, 192, or 256 bits.

#### Encryption Process
1. **Plaintext**: The message is divided into fixed-size blocks.
2. **Key Expansion**: The original key is expanded into a key schedule.
3. **Rounds**: Multiple rounds of substitution, permutation, and XOR operations are applied.
4. **Final Output**: Produces the ciphertext.

#### Decryption Process
1. **Ciphertext**: Blocks are decrypted using the same key.
2. **Rounds in Reverse**: Applies inverse operations to each round to retrieve the original plaintext.

#### Security Features
- AES security is based on its key length and the number of rounds used.
- Resistant to various attacks, especially when employing larger key sizes.

#### Key Applications
- **Data Encryption**: Used for securing data at rest and in transit.
- **Secure Communication**: Commonly used in secure messaging and VPN protocols.
- **File Encryption**: Securing files and archives.

#### Key Parameters
- **Key Size**: Determines the strength of the encryption.
- **Number of Rounds**: 10, 12, or 14 rounds based on the key size.

#### Modes of Operation
- **ECB, CBC, CFB, OFB, CTR**: Different ways to use AES based on the application.

#### Strengths
- Efficiency, high security, and widespread adoption in various applications.


![[Pasted image 20231109221218.png]]
