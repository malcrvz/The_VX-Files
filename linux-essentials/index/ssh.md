# SSH

SSH (Secure SHell), is a protocol used in Unix-based OSs to control systems remotely using an [encrypted](../../cybersecurity-theory/index/cryptography/symmetric-asymmetric-and-hybrid-cryptography.md) secure connection. It lets you run commands on another system as if you were in front of it.\
\
It was first developed in 1995 by [Tatu Ylönen](https://es.wikipedia.org/wiki/Tatu\_Yl%C3%B6nen) after he discovered there was a password sniffer being used at his university, because in those times people still used non-encrypted remote access protocols like Telnet or RLogin. Back in the 90's it may have not been that important still, because of the early days of the internet and remote access not being as used as today, sysadmins would typically work in the same building as the servers are in, but in this modern age you can imagine how dangerous would be to send the admin login credentials of a server containing a full clients' database as plain text from the home of a remote worker. Encrypted remote access is a must. \
The first version of the protocol "SSH-1" had vulnerabilities, so Tatu and his team developed SSH-2 which became the de facto standard for secure remote access. The developing has continued over the years and is still ongoing.\
In 1999 OpenSSH was created, it's the open source implementation of the SSH protocol, implementation meaning a software you can use to execute that protocol, remember a protocol is just a set of rules. It is now widely used around the world and maintained by the [OpenBSD project](https://www.openbsd.org/). \
It probably comes in your Linux distro and its what you have been already using it when you `ssh` in the terminal.\
Even though OpenSSH is installed in almost all distros, don't panic, the server daemon is usually disabled and requires root. You can connect to remote servers but nobody could connect to you unless configured so.\


***

{% code title="Syntax" %}
```bash
##SSH Client
ssh user@IP                        #Basic connection, will ask if we trust the fingerprint and save it on .ssh/known_hosts then asks for the password of the user you are connecting to
ssh user@IP -p n                   #Specify port to connect, by default ssh goes on port 22
ssh-keygen                         #Generate a asymmetric key pair, by default RSA and stored in ~/.ssh/ as id_rsa and id_rsa.pub !!Careful if writting the same name it will overwrite existing key
ssh-keygen -t <type>               #Specify asymmetric algorithm to use: dsa | ecdsa | ecdsa-sk | ed25519 | ed25519-sk | rsa !By default the standard rsa
ssh-keygen -C "comment"            #Adds a comment at the end of the public key, if not added by default will be your "username@hostname" 
ssh-copy-id -i ~/.ssh/pubkey.pub user@IP #Directly copies the specified public key into the authorized_keys file of the speficied server
ssh -i ~/.ssh/privateKey user@IP   #Connects using the specified private key, expects its public matching key to be at the server's authorized_keys file
ssh-add ~/-ssh/privateKey          #Uses the ssh-agent to store the private key credentials in cache memory until closing the session, so you only have to input the password 1 time


##SSHD(Daemon) Server
#For debian-based it's "ssh", other distros could use "sshd"
systemctl start ssh              #Starts sshd service
systemctl status ssh             #Check sshd service status
systemctl restart ssh            #Restart the sshd service
systemctl stop ssh               #Stops the sshd service !Will NOT close already established connections
systemctl enable ssh             #Starts sshd service automatically at system start
systemctl disable ssh            #Disables sshd service from starting automatically at system start          

#In /etc/sshd_config file - Uncomment to activate - Restart to apply changes
Port n                            #Establish port to listen for ssh connections
PermitRootLogin yes|no            #Establish if you are able to login as root via ssh  ?For security, if the user you will connect already has sudo or you are not the sysadmin its recommended to "no"
PermitRootLogin prohibit-password #Root will be able to login, but only using public key authentication, not password
PasswordAuthentication yes|no     #Establish if users will be able to login via password, if "no" they will need public key authentication ?"no" and using public key are considered best practice
#"Permission denied (publickey)" error indicates that the server has password authentication disabled

```
{% endcode %}

***

{% hint style="info" %}
**Authorized\_keys file:** once connected to a server, you can copy your public key into the file `authorized_keys` inside the `.ssh/` directory to connect without password. If you need multiple, each public key has to be in its own line. If you have a matching private key stored in your `.ssh/` directory, the ssh server will automatically log you in. \
If you established a password on the private key it will ask you for it before being able to login, this gives another level of security and gains us time if the private key is stolen.\
\
**Known hosts:** once you connect to a server it will ask if you trust the fingerprint(hashed value that uniquely identifies the server), if you accept, the server's fingerprint and public key will be stored at your `~/.ssh/known_hosts` file as:\
`|n|hashed-username|hashed-IP-or-hostname|algorithm-used|public-key`\
next time it won't ask again, as it implies the server is to be trusted.\
If you connect regularly to a server and suddenly it asks you again if you trust the fingerprint (and the known\_hosts files is OK) suspect a man-in-the-middle attack, if the alert prompts it indicates that the server is not in the known list so it may be a rogue server.\
\
**Log files:** register each connection and when/where it came from, for Debian-based its usually at `/var/log/auth.log`  and Red Hat-based at `/var/log/secure`\
\
**Key pair:** the RSA keys generated with `ssh-keygen` are exclusive for ssh connections, meaning they are not compatible or interchangeable with other key pairs as for example the ones used on TLS or PGP.\
\
**Permissions:** if the `~/.ssh` directory has too much flexible permissions like `o+rwx` you will not be able to connect via key, as ssh considers that folder and therefore its keys insecure. It will prompt you for the password even if specifying the `-i` argument. \
The `~/.ssh` directory should only be accessible by you and only you.\

{% endhint %}

{% hint style="success" %}
To save connections as alias, you can create a config file "`touch ~/.ssh/config"` and use the following structure to add as many as you want:\
\
`Host serverName`\
&#x20; `Hostname IP`\
&#x20; `Port 22`\
&#x20; `User Username`\
\
Now you can use "ssh serverName" and it will redirect to that connection.
{% endhint %}

