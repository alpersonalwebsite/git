# SSH key and GitHub

## Create a SSH key

In your terminal go to `~/.ssh/`
If you don't have this folder, just create it.

Then, run:

```shell
ssh-keygen -t rsa -b 4096 -C "your-email"
```

Be sure you are replacing `your-email` with your real `email address`.

In the prompt...

1. For the name of the file enter: `id_rsa_your-company-name`
2. For the passphrase any phrase that could not be easily guessed by brute force.

You should have 2 new files:

* id_rsa_your-company-name (private key)
* id_rsa_your-company-name.pub (public key)

Run:

```shell
cat .ssh/id_rsa_your-company-name.pub 
```

Copy to clipboard the content of the file.