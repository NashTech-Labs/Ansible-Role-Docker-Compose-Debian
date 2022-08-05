## Step 1 — Creating the Key Pair
**NOTE:**By default recent versions of ssh-keygen will create a 3072-bit RSA key pair, which is secure enough for most use cases (you may optionally pass in the **-b 4096** flag to create a larger 4096-bit key).
```bash
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/vaibhav/.ssh/id_rsa): 
```
Press enter to save the key pair into the .ssh/ subdirectory in your home directory, or specify an alternate path.

If you had previously generated an SSH key pair, you may see the following prompt:
```bash
/home/vaibhav/.ssh/id_rsa already exists.
Overwrite (y/n)?
```
If you choose to overwrite the key on disk, you will not be able to authenticate using the previous key anymore. Be very careful when selecting yes, as this is a destructive process that cannot be reversed.

You should then see the following prompt:
```bash
Enter passphrase (empty for no passphrase):
```
A passphrase adds an additional layer of security to prevent unauthorized users from logging in. 
It is optional, so press enter.
```
Your identification has been saved in /vaibhav/.ssh/id_rsa
Your public key has been saved in /vaibhav/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:/hk7MJ5n5aiqdfTVUZr+2Qt+qCiS7BIm5Iv0dxrc3ks user@host
The key's randomart image is:
+---[RSA 3072]----+
|                .|
|               + |
|              +  |
| .           o . |
|o       S   . o  |
| + o. .oo. ..  .o|
|o = oooooEo+ ...o|
|.. o *o+=.*+o....|
|    =+=ooB=o.... |
+----[SHA256]-----+
```

## Step 2 — Copying the Public Key to Your Ubuntu Server

```bash
$ ssh-copy-id username@remote_host_ip
```
lets say your remote_host_ip = '203.0.113.1'  and host = ubuntu. you will get this message.
```
The authenticity of host '203.0.113.1 (203.0.113.1)' can't be established.
ECDSA key fingerprint is fd:fd:d4:f9:77:fe:73:84:e1:55:00:ad:d6:6d:22:fe.
Are you sure you want to continue connecting (yes/no)? yes
```
type *yes* and Press "ENTER".
Next, the utility will scan your local account for the id_rsa.pub key that we created earlier. When it finds the key, it will prompt you for the password of the remote user’s account:
```
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
username@203.0.113.1's password:
```
Type in the password (your typing will not be displayed, for security purposes) and press ENTER. The utility will connect to the account on the remote host using the password you provided. It will then copy the contents of your ~/.ssh/id_rsa.pub key into a file in the remote account’s home ~/.ssh directory called authorized_keys.
```
Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'username@203.0.113.1'"
and check to make sure that only the key(s) you wanted were added.
```
## Step 3 — Authenticating to Your Ubuntu Server Using SSH Keys

If you have successfully completed one of the procedures above, you should be able to log into the remote host without providing the remote account’s password.

The basic process is the same:
```
$ ssh username@remote_host
The authenticity of host '203.0.113.1 (203.0.113.1)' can't be established.
ECDSA key fingerprint is fd:fd:d4:f9:77:fe:73:84:e1:55:00:ad:d6:6d:22:fe.
Are you sure you want to continue connecting (yes/no)? yes
```
This means that your local computer does not recognize the remote host. Type “yes” and then press ENTER to continue.

If you did not supply a passphrase for your private key, you will be logged in immediately. If you supplied a passphrase for the private key when you created the key, you will be prompted to enter it now (note that your keystrokes will not display in the terminal session for security). After authenticating, a new shell session should open for you with the configured account on the Ubuntu server.