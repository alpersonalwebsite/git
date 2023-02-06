# Multiple GH accounts

Today is pretty common to have multiple GitHub accounts, one for personal purposes, one for work, one for contributing, etc.

## Multiple identities

If you want to handle multiples identities on the same device you can do the following:
(in this example we are covering 2 cases, one for work and the other one for personal usage)

### Create `~/.gitconfig`

```
[includeIf "gitdir:~/Documents/personal/"]
path = ~/.gitconfig.personal
[includeIf "gitdir:~/Documents/work/"]
path = ~/.gitconfig.work
```

### Create `~/.gitconfig.personal`

```
[user]
name = Your name
email Your personal email
```

### Create `~/.gitconfig.work`

```
[user]
name = Your name
email Your work email
```

### Initialize git

In both folders, `~/Documents/personal/` and `~/Documents/work/` initialize git

```shell
git init
```

To corroborate that everything works as expected, `cd` to each `folder` and execute `git config user.email`
You should see the proper email tied to each configuration.