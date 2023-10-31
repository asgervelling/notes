An algorithm that uses a [[Block Cipher]] to provide confidentiality and/or authenticity. A block cipher encrypts fixed size blocks of plaintext, one block at a time. Different modes of operation repeat block ciphers in various ways. Some of them allow for patterns to emerge. More secure modes of operation let the encryption key of block $n+1$ be different from the key of block $n$.

Most modes of operation (not ECB) employ an initialization vector, that is combined with the plaintext. This is to ensure that two identical plaintexts do not produce the same ciphertext.

##### Some modes of operation
![[Pasted image 20231031084407.png]]
![[Pasted image 20231031084426.png]]
![[Pasted image 20231031084440.png]]
![[Pasted image 20231031084502.png]]
![[Pasted image 20231031084521.png]]
