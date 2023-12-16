# Digital signatures & Digital certificates

### Digital signatures

Digital signatures use encryption techniques not to hide data(Confidentiality) but to verify the authenticity and [integrity ](../cia-triad-confidentiality-integrity-and-availability.md)of the data sent over a network. It also has the purpose of creating non-repudiation, once signed and sent, the user can't deny sending the message. \
The process involves [asymmetric cryptography](symmetric-asymmetric-and-hybrid-cryptography.md) and [hashing](hash-functions.md) to sign the data with an unique mathematical "name" or "code" that can't be changed. Think of it like a DNI for each file or message that demonstrates that the owner is who it says it is.\
If a user wants to send a message using only data encryption, sure, nobody will read it, but a man-in-the-middle attack could intercept the message, stop it, create a new one using its own key pair and send it to the destination passing as if its encrypted by the original user. This is a goldmine for scammers. But this vulnerability is remediated by digital signature.\


Once again we will use our friends Alice and Bob for easier explanation, we will obviate data encryption steps for clarity and concision:

1. Bob wants to send a signed message to Alice, so he generates a key pair and writes the message.
2. Bob then [hashes](hash-functions.md) the message, producing an unique, deterministic and fixed-size string that uniquely represents the content of the message. Even 1 letter change would produce a totally different string.
3. Bob then encrypts the hash digest with his private key. This reverses what we saw on encryption, anybody with the public key of Bob can decrypt the message, but only Bob, the only holder of the private key, could have encrypted it. If the public key decrypts the message, it proves its him.
4. The encrypted hash digest, now called signature, is then attached to the original message, encrypted and sent over to Alice.
5. Now Alice has received the message, decrypts the message using its own private key and the digital signature using Bob's public key. Then she uses the same hashing algorithm on the message to compare the results.\
   If Alice can decrypt the signature using Bob's public key it proves authenticity, it's from Bob.\
   If the hash digest of the message and the signature match, it means the message is exactly the same and conserves its full integrity.\
   They matching also creates non-repudiation. Bob can't deny sending that message. Only his private key could have encrypted that matching hash digest.

<figure><img src="../../.gitbook/assets/digitalSignature.png" alt=""><figcaption><p>source: (Sunny Classroom) <a href="https://www.youtube.com/watch?v=TmA2QWSLSPg">https://www.youtube.com/watch?v=TmA2QWSLSPg</a> </p></figcaption></figure>

But this still has a big vulnerability, digital signature alone can't prove authenticity, since the public key is available for anyone, probably in a public server, a man-in-the-middle attack could intercept the message, the bad actor throws the original message away and send its own, redirecting Alice to his own "pretending to be Bob" public key. Alice receives the message and everything looks correct, the hash and the signature match. But it's not coming from the private key of Bob but from the bad actor. \
\
So far, we created confidentiality: \
The message can't be read until decrypted with asymmetric/symmetric or hybrid cryptography. \
We created integrity: \
We proved that the message isn't altered in transit with hashing and digital signature.\
We proved that the message comes in fact from the same private key that matches the public one.\
\
Now we have to prove that the public key comes in fact from the legitimate user, but this key has to be public and can't be encrypted. That's why we need digital certificates and chains of trust.\


***

### Digital certificates

