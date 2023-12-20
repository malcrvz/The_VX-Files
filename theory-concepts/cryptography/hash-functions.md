# Hash functions

Commonly called just hash, it's a mathematical function that takes an input and produces a deterministic, fixed-size and non-reversible output as a string of characters. \
The output, called hash, hash digest, hash value, checksum, fingerprint, among other names, will be always the same given the same input and its computationally infeasible to reverse the hash function to obtain the original input. It will also have "Avalanche effect" meaning that even the smallest change in the input will result in a significantly different hash digest. Recent more advanced hash algorithms will have improved "Collision resistance" meaning that is should be unlikely for two different inputs to produce the same hash value.\
\
Here's an example:&#x20;

Hashing the word "spider" with the currently most used hashing algorithm SHA256 will ALWAYS give this string:\
`9bfa0b50a90e669907e78780bcc1e5e972742e0d124b30a67fbeb6371c604891`\
You can check it online [here](https://gchq.github.io/CyberChef/#recipe=SHA2\('256',64,160\)\&input=c3BpZGVy) or using `echo -n "spider" | sha256sum` in your linux distro.\
\
Changing even just one character will produce a totally different string, see "sp1der":\
`1630aee7b5e94f5124a5b9d0b87397804efd0ecd66788bdcdfb9a262bbc3483a`

\
The hash function uses different algorithms to create the digest, each one will give a different result due to them using different mathematical processes to produce the result, but know that all of them will obey the same rules mentioned before.\
Each algorithm has its own story and versions, its a whole world, we will overview the most common ones:

* **SHA-1:** (Secure Hash Algorithm 1) first SHA version, now considered insecure because collision attacks have been demonstrated, the attack involves finding two distinct inputs that hash to the same hash value.
* **SHA256:** designed by the NSA, is part of the SHA-2 family, provides a 256-bit hash value, its widely used around the world.
* **SHA512:** from SHA-2 family, same as SHA256, but the hash value generated is 512-bit long, providing a higher level of security.
* **SHA-3:** latest member of the SHA family, designed to diversify options, improve the security and already have an alternative in case a vulnerability in SHA-2 would be found. Comes in 224, 256, 384 and 512 bit lengths.
* **MD5:** (Message Digest Algorithm 5) was widely used in the past and is still around old systems, but considered deprecated for cryptographic purposes due to vulnerabilities in collision attacks.
* Other popular algorithms worth checking: [Whirlpool](https://en.wikipedia.org/wiki/Whirlpool\_\(hash\_function\)), [Argon2](https://en.wikipedia.org/wiki/Argon2), [BLAKE2](https://en.wikipedia.org/wiki/BLAKE\_\(hash\_function\)#BLAKE2), [bcrypt](https://en.wikipedia.org/wiki/Bcrypt), [Skein](https://en.wikipedia.org/wiki/Skein\_\(hash\_function\)).



When we mention fixed-size, its because the result of the hash will always be the same length of bits, in the SHA families is even indicated in the name, SHA256 will ALWAYS produce a 256 bit length result (no matter you hash a single word or a book) and SHA512 will ALWAYS produce a 512 bit length result, if the input would be insufficient, they will use padding techniques to reach the length. \
To better understand this we will see an example:\
\
The result of hashing the word "chess" with SHA256 will give:\
`ac739dccd121f71261d87461256e9ca70d8e3b6dbbcc9076d5f3b50c73a5d22d`\
Now if you count the characters in the string you will see they are 64, but they are in hexadecimal and each hexadecimal digit equals to 4 binary digits, so 64 \* 4 = 256\
\
Now hashing the same word "chess" with SHA512 will give:\
`447b652b665310bf006ec02447390f5c7940323431b8e5d233084cc0d14e966095789d3af7708988d775e4cadccf311a4f9bba4b3ccfdae5dc3ce1b97a848bc0`\
Which, trust me, are 128 characters and 128 \* 4 = 512\


{% hint style="warning" %}
Hashing != Encrypting\
A hash is meant to be irreversible, useful to validate data, check integrity, store passwords, digital signing, check data synchronization, etc.\
Encrypting is meant to BE reversible, make data gibberish and then turn it back to its original state when needed, using of course a secret key.
{% endhint %}

### Password storing

One of the main uses of hash functions is to store passwords.\
Before you register a password to a website for example, it will be hashed and the value stored in the web's database, not your actual password. This way if a bad actor gets access to the database, all it will see are hashed passwords. Also good for privacy since the website shouldn't have to know your password, you don't know who is in the other side.\
This doesn't mean there aren't ways to get your password, specially if it is very bad, with [password attacks](../../baseline-pentesting/techniques/password-cracking.md). \
There's been cases of websites storing passwords in clear text, creating historical personal information leaks, huge wordlists for the world to use and security scandals like the infamous [rockyou](https://en.wikipedia.org/wiki/RockYou) case. \
Trust no one! Always use unique passwords!

### Salt

When storing passwords, good practitioners will use "salt" too. \
"Salt" is a random auto-generated value in the server side that will be used as an additional input to the hash function.\
Here's a basic example:

1. User creates a new account with the password "ilovechocolate"
2. A random salt, let's say "abc123" is generated.
3. The hash function is applied to the concatenation of the password and the salt, \
   AKA hash ->"ilovechocolateabc123"
4. The resulting hash is stored in the database next to the salt

As each password will also have a unique random extra value added to the hash, even if the hash of "ilovechocolate" is found, the bad actor would still need the salt to crack the password.\
This process has 2 big advantages:

* If two users use the same password it will still be stored with a different hash.
* Will make [password attacks](../../baseline-pentesting/techniques/password-cracking.md) exponentially harder and slower, specially rainbow table attacks.

Salting is no magic solution tho, if the password database is compromised the salt will be compromised too, it will still slow down the process as the bad actor has to add the salt of each user to each try but it will be automated anyways.\
Also if the random auto-generation code is bad it will be easy for bad actors to guess the salt. \
A good solution would be to have another salt, but secret, this is called "Pepper".

### Pepper

Pepper uses a similar principle as salt, its added to the "salt + password" before hashing, but using a random data value that is shared among all the users in the database. \
The pro is that the pepper is stored secretly in a config file or key management system, away from the database, so even if it leaks, the bad actors would still need to compromise the pepper, that will desirably be in a best protected system.\
\
\
Anyway Salt and Pepper have its own defects and as they might be impractical, have to be implemented by a professional and consume extra resources, not everyone uses them. \
So the best practice is for the user to always use long, strong and unique passwords!
