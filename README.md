# ssh
Organize and manage ssh keys per repositories with specific access

# background

As `id_rsa` must be kept confidential and handled by following maximum security best practices, we're using `openssl` with algorithm `aes-256-ecb` to encrypt the data. Generally, if the passphrase for this is longer than 43 (256/6) characters in length, it will be impossible for a normal computer to decrypt within our life time.

# requirements

- At Github, at each repository you want to grant the access, you must upload the public id_rsa (e.g. `starlightgroup-devops.id_rsa.pub`).
- To add, go to the repository (e.g. https://github.com/starlightgroup/ssh) URL.
- Go to on *Settings*
- Go to *Deploy keys*

# to generate a new ssh key, then suffix with `decrypted`

> In your terminal:

```
ssh-keygen -t rsa -C "devops@starlightgroup.io"
mv starlightgroup-devops.id_rsa starlightgroup-devops.id_rsa.decrypted
```

> example

```
$ ssh-keygen -t rsa -C "devops@starlightgroup.io"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/melvyn/.ssh/id_rsa): ./starlightgroup-devops.id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in ./starlightgroup-devops.id_rsa.
Your public key has been saved in ./starlightgroup-devops.id_rsa.pub.
The key fingerprint is:
SHA256:4VlMHQDB0a11vvaZfbSW4dFQSd30jqEaA1bcirj69C0 devops@starlightgroup.io
The key's randomart image is:
+---[RSA 2048]----+
|      .+o+oo.o+ o|
|        oo.ooo +o|
|       .. =.o = o|
|       ..B . + *.|
|        o o + .o=|
|         S .  ..O|
|        o      Oo|
|       o .E.  . =|
|      ... EEE  ..|
+----[SHA256]-----+
$ mv starlightgroup-devops.id_rsa starlightgroup-devops.id_rsa.decrypted
```



# to decrypt  

> format

```
openssl enc -d -aes-256-ecb \
    -in "${id_rsa}.encrypted" \
    -out "${id_rsa}.decrypted"
```

> example

- In this example, let's decrypt the `starlightgroup-devops/starlightgroup-devops.id_rsa.encrypted`.
- Given that `${id_rsa}` is the name of the *private* rsa file, in your terminal, first assign the filename as this:

    id_rsa="starlightgroup-devops.id_rsa"

- Then, execute the following command to decrypt the id_rsa for use

```
openssl enc -d -aes-256-ecb \
    -in "${id_rsa}" \
    -out "${id_rsa}.decrypted"
```


- Then, enter the password that is only shared by trusted members in your team.

# to encrypt

> format

```
openssl enc -aes-256-ecb \
    -in "${id_rsa}.decrypted" \
    -out "${id_rsa}.encrypted"
```

> example

- In this example, let's encrypted the `starlightgroup-devops/starlightgroup-devops.id_rsa.decrypted`.
- Given that `${id_rsa}` is the name of the *private* rsa file, in your terminal, first assign the filename as this:

```
id_rsa="starlightgroup-devops.id_rsa"
```

- Then, execute the following command to decrypt the id_rsa to encrypt:

```
    openssl enc -aes-256-ecb \
        -in "${id_rsa}" \
        -out "${id_rsa}.encrypted"
```

# what to do with decrpyted files

> example

The purpose the the ssh rsa key files is to accesss the repo without having the user token or user logged in. By using these rsa tokens, we can clone the repositories as this:

```
sudo ssh-agent bash -c 'ssh-add ~/.ssh/starlightgroup-devops.id_rsa.decrypted; git clone git@github.com:starlightgroup/devops'
```


