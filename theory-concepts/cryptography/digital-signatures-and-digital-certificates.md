# Digital signatures & Digital certificates

### Digital signatures

Digital signatures use encryption techniques not to hide data([Confidentiality](../cia-triad-confidentiality-integrity-and-availability.md)) but to verify the authenticity and integrity of the data sent over a network. It also has the purpose of creating non-repudiation, meaning that once signed and sent, the user can't deny sending the message. \
The process involves [asymmetric cryptography](symmetric-asymmetric-and-hybrid-cryptography.md) and [hashing](hash-functions.md) to sign the data with an unique mathematical "name" or "code" that can't be changed. Think of it like a DNI for each file or message that demonstrates that the owner is who it says it is.\
If a user wants to send a message using only data encryption, sure, nobody will read it, but a man-in-the-middle attack could intercept the message, stop it, create a new one using its own key pair and send it to the destination passing as if its encrypted by the original user. This is a goldmine for scammers. But this vulnerability is remediated by digital signature.\


Once again we will use our friends Alice and Bob for easier explanation, we will obviate data encryption steps for clarity and concision:

1. Bob wants to send a signed message to Alice, so he generates a key pair and writes the message.
2. Bob then [hashes](hash-functions.md) the message, producing an unique, deterministic and fixed-size string that uniquely represents the content of the message. Even 1 letter change would produce a totally different string.
3. Bob then encrypts the hash digest with his private key. This reverses what we saw on [encryption](symmetric-asymmetric-and-hybrid-cryptography.md), anybody with the public key of Bob can decrypt the message, but only Bob, the only holder of the private key, could have encrypted it. If his public key decrypts the message, it proves its him.
4. The encrypted hash digest, now called signature, is then attached to the original message and sent over to Alice.
5. Now Alice has received the message, decrypts the message using its own private key and the digital signature using Bob's public key. Then she uses the same hashing algorithm on the message to compare the results.\
   If Alice can decrypt the signature using Bob's public key it proves authenticity, it's from Bob.\
   If the hash digest of the message and the signature match, it means the message is exactly the same and conserves its full integrity.\
   They matching also creates non-repudiation. Bob can't deny sending that message. Only his private key could have encrypted that matching hash digest.

<figure><img src="../../.gitbook/assets/digitalSignature.png" alt=""><figcaption><p>source: (Sunny Classroom) <a href="https://www.youtube.com/watch?v=TmA2QWSLSPg">https://www.youtube.com/watch?v=TmA2QWSLSPg</a> </p></figcaption></figure>

But this still has a big vulnerability, how do we know its the true public key of Bob? Digital signature alone can't prove authenticity, since the public key is available for anyone, usually in a public server, a man-in-the-middle attack could intercept the message, the bad actor throws the original message away and sends its own, redirecting Alice to his own "pretending to be Bob" public key. Alice receives the message and everything looks correct, the hash and the signature match. But it's not coming from the private key of Bob but from the bad actor, we need a step further. \
\
So far, we created:

* **Confidentiality:** the message can't be read until decrypted with asymmetric/symmetric or hybrid cryptography.&#x20;
* **Integrity:** we proved that the message isn't altered in transit with hashing and digital signature.

\
Now we need the last step, to create **authenticity**, we have to prove that the public key comes in fact from its legitimate user. But there's a problem, the public key has to be public and can't be encrypted, anyone can use it. There will always be a trust gap between the sender and the recipient, we need a third party, somebody to act as a notary. Somebody we can trust that confirms the sender is who it says it it. So now we will learn about digital certificates and chains of trust.\


***

### Digital certificates

They are digital credentials issued by a trusted third party, a file, designed to verify the identity of a user (so it is who it says it is) and that it owns the public key attached. Demonstrating the authenticity we were lacking on the other steps. \
We call this third party trust organization Certificate Authority (CA). They can be commercial entities, governmental, private, etc. CAs act like a notary, an entity with high security politics who is neutral and that can be trusted because if they lie they win nothing and lose a lot. \


<div align="center">

<figure><img src="../../.gitbook/assets/third_party_trust.png" alt="" width="563"><figcaption><p>source: <a href="https://www.networkacademy.io/ccie-enterprise/sdwan/cisco-sd-wan-certificates-explained">https://www.networkacademy.io/ccie-enterprise/sdwan/cisco-sd-wan-certificates-explained</a> </p></figcaption></figure>

</div>

\
Before issuing a digital certificate, CAs will perform a thorough verification of the entity's identity, it may include validating domain ownership, legal business documentation, government-issued identification documents, phone verification...\
\
The certificate file will have an extension like ".cer", ".crt", ".pem" or ".der" and will contain, encoded, all the needed information about the certificate owner, like its name, public key, validity period, issuer of the certificate and most importantly the digital signature of the CA issuer to prove its legitimate. \
\
Alice and Bob to the rescue:

1. Alice contacts with a CA service and after proving his identify with some sort of info, she obtains and downloads the "certificate.cer".
2. Alice sends a message to Bob with the digital certificate attached to it.
3. Bob will decrypt the CA digital signature on the certificate, the public key of the CA can be included or checked online but browsers and OSs come already preinstalled with most major root CA public keys. If the hash matches with the one in the certificate this ensures that the certificate in fact comes from the trusted CA that Alice bought. As only the CA private key could have encrypted it.
4. Once the CA signature is verified, the certificate can be trusted, and if the date of validation is not expired, Bob can trust that the message is in fact coming from Alice, authenticity is proved. \
   The certificate will also already include Alice's public key to use for the rest of the connection. \
   Or in the real world terms, a green padlock appears in your browser's address bar.\


<figure><img src="../../.gitbook/assets/digital_certificate.png" alt=""><figcaption><p>source: <a href="https://id4d.worldbank.org/guide/digital-certificates-and-pki">https://id4d.worldbank.org/guide/digital-certificates-and-pki</a> </p></figcaption></figure>

{% hint style="info" %}
A Certificate Authority that you can try for free is [Let's Encrypt](https://letsencrypt.org/), a non-profit organization that issues certificates using domain ownership validation.
{% endhint %}

{% hint style="info" %}
You could technically do a self-signed certificate, it will still provide an encrypted connection, but its the equivalent of saying "trust me, I am who I say I am". Also your public key will not come preinstalled in browsers, so it will still warn users about the risks of visiting your site.
{% endhint %}



Certification Authorities are based on a Distributed Trust Model, or chain of  trust, where root CAs sign intermediate CAs, distributing the load and delegating authority into more entities, so if a key were to be compromised it would not affect the whole of the certificates.

<figure><img src="../../.gitbook/assets/rootCA.png" alt=""><figcaption><p>source: <a href="https://youtu.be/LPxeYtMDxl0?feature=shared">https://youtu.be/LPxeYtMDxl0?feature=shared</a> </p></figcaption></figure>

At some point of the chain the certificate has to be self-signed, there has to be a starting trusting point, this is handled by the root CA. They will have the most extreme security measures. They will act as a trust anchor, preinstalled in most browsers, with longer certificate lifespans. Root CAs main work is to verify other intermediate CAs that will be more operational and take the workload of the internet.

<figure><img src="../../.gitbook/assets/CA_chainOfTrust.png" alt=""><figcaption><p>source: <a href="https://youtu.be/LPxeYtMDxl0?feature=shared">https://youtu.be/LPxeYtMDxl0?feature=shared</a> </p></figcaption></figure>

#### What if the CA gets compromised?

Digital certificates expire normally in a year or few years, ensuring that not even a extremely long brute force attack could crack the private key. But nothing is 100% secure and they can still be compromised or expired because details on the entity changed. That's why certificates can and will be revoked in time. Every certificate will come with a digitally signed timestamp "stapled" to prove that the certificate is not revoked. This protocol is called OCSP Stapling (Online Certificate Status Protocol).\
The protocol is done in separate servers from the CAs, they are called OCSP Responder servers. This creates scalability, redundancy, geographic distribution and function isolation. \
\
Here are the steps of the OCSP Stapling process:\


1. OCSP Responder servers will have a CRL (Certification Revocation List) a CAs maintained extensive database of revoked certificates. They are the ones in charge of consulting this list, thus reducing a huge load on clients and servers traffic.
2. Every several hours to days, the web server will check if a certificate is revoked.&#x20;
3. The OCSP Responder will consult the CRL for the status and will emit a timestamped response signed by the CA to the web server.
4. When the client wants to connect, the web server will send the certificate with the timestamped OCSP response. Already pre-checked that day before the client asks.
5. The client can trust the certificate and the revocation status since its digitally signed by the CA.\


This protocol apart from securing the certificates, saves a lot of traffic, since the web server will issue a single OCSP request every now and then for a certificate status, and then use it for all its clients. \
Also OCSP servers will not have to deal with the client requests every time a web server is visited.

<figure><img src="../../.gitbook/assets/OSCP_stapling.png" alt=""><figcaption><p>source: <a href="https://youtu.be/WXNKQ_otO_g?feature=shared">https://youtu.be/WXNKQ_otO_g?feature=shared</a> </p></figcaption></figure>

{% hint style="danger" %}
One very important thing to remember is that the whole process of encrypting a connection with confidentiality, integrity and authority, does NOT mean that the site is secure. Any bad actor can and will easily obtain a certificate. The padlock means the connection is private and secured from eavesdroppers, not that the destiny can be trusted.
{% endhint %}
