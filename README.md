# Slave Node on Jenkins

###### This is a repository containing mainly notes on how to set up a Slave Node on Jenkins

## SSH

### SSH Protocol
The SSH protocol is used to login in securely and remotely from one system into another. This makes it ideal for remotely monitoring network infrastructure.

You need some special software to be able to use the SSH protocol:

- SSH daemon: the remote systems need this
- SSH client: the system used to remotely access the servers need This

These two pieces of software combine to make a proper communication channel using the SSH protocol.

### SSH keys
SSH keys are an authentication method used to gain access to this encrypted connection between systems.

A popular choice for a set of keys would be an RSA 2048-bit encryption, equivalent to a 617 digit long password.

Linux and Mac can create a key pair using a terminal window and Windows OS can use external software (shock) like PuTTY.

The keys come in _pairs_, a public and private key.

Who has access to these keys defines what type of SSH key pair they are:
- Private and Public key remain with the user: _User Keys_
- Private and Public key remain on a remote system: _Host Keys_

#### User Keys
The private key remains on the system used to remotely control and is used to decrypt information that is exchanged in the SSH protocol. Private keys should never be shared with anyone.

A public key is used to encrypt information, can be shared, and is used by both the user and the remote server. On the user's side, the local machine, the public key is stored in either the SSH key management software, or a file in that local computer.

### Gaining access to your remote system using SSH

First off, you need to put your public SSH key in the correct place on the remote server you need to access.

Then, use your ssh username to gain access by executing the following command in the terminal window:

    ssh username@my_ip_address

using this information, the protocol then decides which public key to use to authenticate you. The remote server will then encrypt a random challenge message and send it back to the client. This message is then decrypted by the private key on your system and once decrypted, is compared with an existing session ID and sent to the server. If the message matches what the server sent out, the client is authenticated, and access is granted.

This process proves to the server that your have the correct private key to the public key on the server, hence you are authenticated.

## Host Keys, Known Hosts and Authorized Keys

##### Host Keys

Host Keys are key pairs, typically using RSA, DSA or ECDSA algorithms. Public host keys are distributed to the SSH clients and private keys are stored on the SSH servers.

Each host (i.e. computer) should have a unique host key. Sharing of key pairs is not advised, and may leave systems open to _man in the middle_ attacks, but sharing may sometimes be acceptable and practical.

In OpenSSH, host keys will usually be stored in the /etc/ssh directory, in files starting with ssh_host_<rsa/dsa/ecdsa/ed25519>\_key.

Host keys are usually generated when OpenSSH is first installed or when the computer is first booted. The ssh-keygen program can be used to create new pairs of keys.

##### Known Hosts
SSH clients store every host key for every host they have ever connected to. These stored host keys are called _known host keys_ and the collection is often called _known hosts_.

They are stored in /etc/ssh/known_hosts in the OpenSSH machine and in .ssh/known_hosts in each users home directory.

### Simply Copy Paste

Using the command:

    scp -i <location of ssh key> <location of file> <user, e.g. ubuntu>@<publicDNS>:<location to move file to in shh>

We can move files from our local directory, to inside our SSH machine instance.

To move files, we can use this syntax:

    scp -i <location of ssh key> -r <location of file> <user, e.g. ubuntu>@<publicDNS>:<location to move file to in shh>
