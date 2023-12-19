# ssh & key pair

SSH (Secure SHell), is a protocol used in Unix-based OSs to control systems remotely using an [encrypted](../theory-concepts/cryptography/symmetric-asymmetric-and-hybrid-cryptography.md) secure connection.\
It was first developed in 1995 by [Tatu Ylönen](https://es.wikipedia.org/wiki/Tatu\_Yl%C3%B6nen) after he discovered there was a password sniffer being used at his university, as in that times people still used non-encrypted remote access protocols like Telnet or RLogin. Back in those days it may have not been that important still, because of the early days of the internet and remote access not being as used as today, sysadmins would typically work in the same building as the servers are in, but in this modern age you can imagine how dangerous would be to send the admin login credentials of a server containing a full clients' database as plain text from the home of a remote worker in another country. Encrypted remote access is a must. \
The first version SSH-1 had vulnerabilities, so Tatu and his team developed SSH-2 which became the de facto standard for secure remote access. The developing has continued over the years and is still ongoing.\
In 1999 OpenSSH was created, it's the open source implementation of the SSH protocol, implementation meaning a software you can use to execute that protocol, remember a protocol is just a set of rules. It is now widely used around the world and maintained by the [OpenBSD project](https://www.openbsd.org/). \
It probably comes in your Linux distro and its what you have been using when you `ssh` in the terminal.\


\
\
comandos\
como usarlo con public key -i\
como generar keys, donde se guardan, authorized keys, etc\
redirigir a asymmetric cryptography

