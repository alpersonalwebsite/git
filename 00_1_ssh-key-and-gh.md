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
1. For the passphrase any phrase that could not be easily guessed by brute force.

You should have 2 new files:

* id_rsa_your-company-name (private key)
* id_rsa_your-company-name.pub (public key)

Run:

```shell
cat .ssh/id_rsa_your-company-name.pub 
```

Copy to clipboard the content of the file.

## Add the key to GitHub

1. Go to https://github.com/settings/profile
1. Click on `SSH and GPG Keys`
1. Click on `New SSH key`
1. Paste the key's content that you copied earlier and save it as an authentication key.

**Important**
If you are in an `Organization` that has enabled/enforced SAML SSO, go to https://github.com/settings/keys and under your key click on `Configure SSO` and then `Authorize` your Organization.
Then, click on continue in both screens.

## Add the SSH key to the Agent

Run:

```shell
ssh-add ~/.ssh/id_rsa_your-company-name
```

## Create configuration file

Run:

```shell
touch ~/.ssh/config
```

Add to the configuration file:

```
Host github.com-your-company-name
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_your-company-name
```

## Add GitHub SSH keys to `~/.ssh/known_hosts`

We do this to avoid the warning: `The authenticity of host 'github.com (192.30.255.112)' can't be established.`

If `known_hosts` doesn't exist, create it.

Paste the following:

```
github.com ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOMqqnkVzrm0SdG6UOoqKLsabgH5C9okWi0dh2l9GKJl
github.com ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBEmKSENjQEezOmxkZMy7opKgwFB9nkt5YRrYMjNuG5N87uRgg6CLrbo5wAdT/y6v0mKV0U2w0WZ2YB/++Tpockg=
github.com ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAq2A7hRGmdnm9tUDbO9IDSwBK6TbQa+PXYPCPy6rbTrTtw7PHkccKrpp0yVhp5HdEIcKr6pLlVDBfOLX9QUsyCOV0wzfjIJNlGEYsdlLJizHhbn2mUjvSAHQqZETYP81eFzLQNnPHt4EVVUh7VfDESU84KezmD5QlWpXLmvU31/yMf+Se8xhHTvKSCZIFImWwoG6mbUoWf9nzpIoaSjB+weqqUUmpaaasXVal72J+UX2B+2RPW3RcT0eOzQgqlJL3RKrTJvdsjE3JEAvGq3lGHSZXy28G3skua2SmVi/w4yCE6gbODqnTWlg7+wC604ydGXA8VJiS5ap43JXiUFFAaQ==
```

## Interacting with GH

Let's clone a repository to ensure that everything works as expected.

### Clonning with SSH
This requires private key locally and public key on GitHub

```shell
git clone git@github.com:user-or-organization/repo.git
```

### Clonning with HTTPS
This requires a token with the proper permissions

First, let's generate a token (for more information about the usage of tokens instead of passwords, check: https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls)

1. Go to https://github.com/settings/profile
1. Click on Developer settings
1. Click on Personal access tokens and generate a new access token with the permissions you need
1. Copy the token to your clipboard
1. Click on `Configure SSO` and then `Authorize` your Organization. Then, click on continue in both screens.


Then...

If you are clonning one of your personal repositoriers:

```shell
git clone https://github.com/user/repo.git
```

If you need to clone an organization repositorie:

```shell
git clone https://user@github.com/organization/repo.git
```

In either case, paste the token for the password prompt.