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

## Installing `oh-my-zsh`

Although this step is optional, I do recommend you install `oh-my-zsh` (if you are on Mac with macOS Catalina or higher) or, if you are using bash, create or update your `~/.bashrc` with the desired configuration.

For the first case, go to https://ohmyz.sh/#install and install `oh-my-zsh` using `curl` or `wget`.

For the second, I like the following configuration (copy and paste the following snippet into your `~/.bashrc`)

```shell
#
# Set the prompt #
#

# Select git info displayed, see /usr/share/git/completion/git-prompt.sh for more
export GIT_PS1_SHOWDIRTYSTATE=1           # '*'=unstaged, '+'=staged
export GIT_PS1_SHOWSTASHSTATE=1           # '$'=stashed
export GIT_PS1_SHOWUNTRACKEDFILES=1       # '%'=untracked
export GIT_PS1_SHOWUPSTREAM="verbose"     # 'u='=no difference, 'u+1'=ahead by 1 commit
export GIT_PS1_STATESEPARATOR=''          # No space between branch and index status
export GIT_PS1_DESCRIBE_STYLE="describe"  # detached HEAD style:
#  contains      relative to newer annotated tag (v1.6.3.2~35)
#  branch        relative to newer tag or branch (master~4)
#  describe      relative to older annotated tag (v1.6.3.1-13-gdd42c2f)
#  default       exactly eatching tag

# Check if we support colours
__colour_enabled() {
    local -i colors=$(tput colors 2>/dev/null)
    [[ $? -eq 0 ]] && [[ $colors -gt 2 ]]
}
unset __colourise_prompt && __colour_enabled && __colourise_prompt=1

__set_bash_prompt()
{
    local exit="$?" # Save the exit status of the last command

    # PS1 is made from $PreGitPS1 + <git-status> + $PostGitPS1
    local PreGitPS1="${debian_chroot:+($debian_chroot)}"
    local PostGitPS1=""

    if [[ $__colourise_prompt ]]; then
        export GIT_PS1_SHOWCOLORHINTS=1

        # Wrap the colour codes between \[ and \], so that
        # bash counts the correct number of characters for line wrapping:
        local Red='\[\e[0;31m\]'; local BRed='\[\e[1;31m\]'
        local Gre='\[\e[0;32m\]'; local BGre='\[\e[1;32m\]'
        local Yel='\[\e[0;33m\]'; local BYel='\[\e[1;33m\]'
        local Blu='\[\e[0;34m\]'; local BBlu='\[\e[1;34m\]'
        local Mag='\[\e[0;35m\]'; local BMag='\[\e[1;35m\]'
        local Cya='\[\e[0;36m\]'; local BCya='\[\e[1;36m\]'
        local Whi='\[\e[0;37m\]'; local BWhi='\[\e[1;37m\]'
        local None='\[\e[0m\]' # Return to default colour

        # No username and bright colour if root
        if [[ ${EUID} == 0 ]]; then
            PreGitPS1+="$BRed\h "
        else
            PreGitPS1+="$Red\u@\h$None:"
        fi

        PreGitPS1+="$Blu\w$None"
    else # No colour
        # Sets prompt like: ravi@boxy:~/prj/sample_app
        unset GIT_PS1_SHOWCOLORHINTS
        PreGitPS1="${debian_chroot:+($debian_chroot)}\u@\h:\w"
    fi

    # Now build the part after git's status

    # Highlight non-standard exit codes
    if [[ $exit != 0 ]]; then
        PostGitPS1="$Red[$exit]"
    fi

    # Change colour of prompt if root
    if [[ ${EUID} == 0 ]]; then
        PostGitPS1+="$BRed"'\$ '"$None"
    else
        PostGitPS1+="$Mag"'\$ '"$None"
    fi

    # Set PS1 from $PreGitPS1 + <git-status> + $PostGitPS1
    __git_ps1 "$PreGitPS1" "$PostGitPS1" '(%s)'

    # echo '$PS1='"$PS1" # debug    
    # defaut Linux Mint 17.2 user prompt:
    # PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[01;34m\] \w\[\033[00m\] $(__git_ps1 "(%s)") \$ '
}

# This tells bash to reinterpret PS1 after every command, which we
# need because __git_ps1 will return different text and colors
PROMPT_COMMAND=__set_bash_prompt
```

Credits: https://stackoverflow.com/users/5353461/tom-hale