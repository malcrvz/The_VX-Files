# Digital signatures

Digital signatures use encryption techniques not to hide data but to verify the authenticity and integrity of the data sent over a network. It signs the data with an unique mathematical "name" that can't be changed. Think of it like a DNI for each file or message that also shows the owner is who it says it is. It involves [asymmetric cryptography](symmetric-asymmetric-and-hybrid-cryptography.md) and [hashing](hash-functions.md). \
If a user wants to send a message using only data encryption, sure, nobody will read it, but a man-in-the-middle attack could intercept the message, stop it, create a new one using its own key pair and send it to the destination passing as if its encrypted by the original user, making the encryption useless. This is a goldmine for scammers. But the vulnerability is remediated by digital signature.\


Once again we will use our friends Alice and Bob for easier explanation, we will obviate data encryption for clarity and concision:

1. Bob wants to send a signed message to Alice, so he generates a key pair and writes the message.
2. Bob then [hashes](hash-functions.md) the message, producing an unique, deterministic and fixed-size string that uniquely represents the content of the message. Even 1 letter change would produce a totally different string.
3. Bob then encrypts the hash digest with his private key. This reverses what we saw on encryption, anybody with the public key of Bob can decrypt the message, but only Bob, the only holder of the private key, could have encrypted it. Proving its him.
4. The encrypted hash digest, now called signature, is then attached to the original message, encrypted and sent over to Alice.
5. Now Alice has received the message, decrypts the message using its own private key and the digital signature using Bob's public key. Then she uses the same hashing algorithm on the message. If the hashing of the message and the signature match, it means the message is exactly the same and conserves its full integrity as only the public key of Bob could decrypt the signature. Also proves that the signature its also from Bob, as the matching hashes prove that they both were intended to be together.

Still we would need a further step, to ensure nobody is acting as Bob on the internet and faking his messages with a fake public key we would need a [digital certificate.](digital-certificates.md)

