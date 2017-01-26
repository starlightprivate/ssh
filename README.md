# ssh
Organize and manage ssh keys per repositories with specific access

# background

As `id_rsa` must be kept confidential and handled by following maximum security best practices, we're using `openssl` with algorithm `aes-256-ecb` to encrypt the data. Generally, if the passphrase for this is longer than 43 (256/6) characters in length, it will be impossible for a normal computer to decrypt within our life time.

# requirements

- At Github, at each repository you want to grant the access, you must upload the public id_rsa (e.g. `starlightgroup-devops.id_rsa.pub`).
- To add, go to the repository (e.g. https://github.com/starlightgroup/ssh) URL.
- Go to on *Settings*
- Go to *Deploy keys*

# to generate a new ssh key

- In your terminal, execute:
    
    ssh-keygen -t rsa -C "devops@starlightgroup.io"

# to decrypt  

    openssl enc -d -aes-256-ecb \
        -in "${id_rsa}.encrypted" \
        -out "${id_rsa}.decrypted"

> example

- In this example, let's decrypt the `starlightgroup-devops/starlightgroup-devops.id_rsa.encrypted`.
- Given that `${id_rsa}` is the name of the *private* rsa file, in your terminal, first assign the filename as this:

    id_rsa="starlightgroup-devops.id_rsa"

- Then, execute the following command to decrypt the id_rsa for use

    openssl enc -d -aes-256-ecb \
        -in "${id_rsa}" \
        -out "${id_rsa}.decrypted"


- Then, enter the password that is only shared by trusted members in your team.

# to encrypt

     openssl enc -aes-256-ecb \
        -in "${id_rsa}.decrypted" \
        -out "${id_rsa}.encrypted"

- In this example, let's encrypted the `starlightgroup-devops/starlightgroup-devops.id_rsa.decrypted`.
- Given that `${id_rsa}` is the name of the *private* rsa file, in your terminal, first assign the filename as this:

    id_rsa="starlightgroup-devops.id_rsa"

- Then, execute the following command to decrypt the id_rsa to encrypt:

    openssl enc -aes-256-ecb \
        -in "${id_rsa}" \
        -out "${id_rsa}.encrypted"

# what to do with decrpyted files

The purpose the the ssh rsa key files is to accesss the repo without having the user token or user logged in. By using these rsa tokens, we can clone the repositories as this:


