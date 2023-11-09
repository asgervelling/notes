*ChatGPT note.*

RSA (Rivest-Shamir-Adleman) is an [[Asymmetric Encryption|asymmetric]] cryptographic algorithm widely used for secure communication, [[Digital Signature|digital signatures]], and encryption.

![[Pasted image 20231109160223.png]]

### Key Components

- Public and Private Key Pair: RSA encryption involves a pair of keys—a public key for encryption and a private key for decryption. These keys are mathematically related but computationally challenging to deduce from one another.
- Prime Factorization: The security of RSA relies on the difficulty of factoring the product of two large prime numbers. Breaking the encryption requires factoring a large number into its prime components, which becomes increasingly difficult as the number grows.

### Encryption and Decryption Process

- Encryption: The sender uses the recipient's public key to encrypt the message. The recipient can then decrypt the ciphertext using their private key.
- Decryption: The recipient uses their private key to decrypt the received ciphertext and retrieve the original message.

### Strengths and Applications

- RSA encryption provides robust security for communications over insecure networks.
- Commonly used in secure web browsing (HTTPS), digital signatures, and secure email communication.

### Challenges

- Key Length: With advancing computing power, longer key lengths are required to maintain the same level of security due to improvements in factoring algorithms.
- Resource Intensiveness: RSA operations can be computationally intensive, especially with larger key sizes.

RSA remains a fundamental tool in secure communication despite the need for larger keys as technology evolves.

### Questions
> *A few years ago, it was discovered that some organisations had set up DKIM in such a way that emails were signed with 512-bit RSA keys. Such keys were explicitly allowed in the DKIM standard. Explain why this is a problem.*

With rapidly increasing processing power of computers, RSA keys with a 512-bit length, previously considered to be secure, can be cracked in a short period of time. Today, a minimum of 1024 bit RSA should be used. Organizations like the American National Institute of Standards and Technology (NIST) go further, and recommend a minimum of 2048 bits. [^1]


[^1]: https://hackerone.com/reports/550937#:~:text=Length%20of%20the%20Key,bit%20RSA%20should%20be%20used.