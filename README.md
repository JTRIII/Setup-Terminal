# Terminal Setup 

## Step 1: Ensure Your Shell is ZShell

First, check which shell you're currently using:

```
echo $SHELL
```

If it's not ZShell (`zsh`), change it with this command:

```
chsh -s /bin/zsh
```

If your computer says that `zsh` is not available, look up how to install it on your computer. It might be as simple as:

```
sudo apt install zsh
```

## Step 2: Create and Configure `.zshrc`

Create a file in your home directory called `.zshrc`. This file will want define anything you want to run whenever you open a new terminal session. Test it by adding the following line at the top:

```
echo "Hello, world!"
```

Exit the terminal and open a new one. You should see it print "Hello, world!" at the top of the terminal. You can remove this line if you like since you have confirmed it is working. 

### Setting Up Your `PATH` Environment Variable

The `PATH` variable is a set of directories that the shell looks in for programs. You can add directories to your `PATH` like this:

```
PATH="$PATH:/some/new/path"
```

## Step 3: Customize Your Command Prompt

You can customize your command prompt with ANSI escape sequesnces to add colors and symbols. Add the following lines to your `.zshrc` to set up a colorful prompt:

```
export LC_ALL="en_US.UTF-8"
export term="xterm"
export PROMPT='%(?.%F{2}√.%F{1}X)%F{7}|%F{6}%n%F{248}@%F{6}%m%F{7}:%B%F{5}%3~%b%F{7}%#%f
```

## Step 4: Organize Your Configuration

As you can imagine your `.zshrc` file can get rather large.  So you may want to separate pieces of code into individual files and then call them from withing the `.zshrc` file.  To do this you have to make sure you run them with the command `source`.  This command executes a shell script but keeps any changes to environment variables.  Without `source` the shell script is run in a sandbox and the shell is returned to the initial state before the program was executed.  For us, in this case, the entire point is to have the changes persist in our shell, so we must use `source`.

Then you can `source` the files you want in the `.zshrc` file. So you would probably just want these lines in your `zshrc`:

````
# Prompt
source ~/.sources/setup-prompt.sh

# Path and bin dirs
source ~/.sources/setup-bins.sh

# Aliases for things like nano
source ~/.sources/setup-aliases.sh
```

What I have in each of these files I have talked about:

#######################################################
.zshrc 

```
# Prompt
source ~/.sources/setup-prompt.zsh

# Path and bin dirs
source ~/.sources/setup-bins.zsh

# Aliases for things like nano
source ~/.sources/setup-aliases.zsh
```

In the .sources/ directory I have the files:
setup-prompt.zsh
setup-bins.zsh
setup-aliases.zsh

#######################################################
setup-prompt.zsh:

```
# File should be sourced


# Aliases
alias ..='cd ..'
alias nano='/usr/bin/nano -wacuWEMS --tabsize=4'
alias kate='/usr/bin/kate >& /dev/null'
alias grep='/usr/bin/grep --color=auto'
alias ls="ls -G -FC"
atom() {
    if [ -z "$DISPLAY" ]; then
        printf "Atom can only be opened from the command line when you are logged in using a graphical user interface.  Consider using `nano`.\n"
    else
        # shellcheck disable=SC1035
        /usr/local/bin/atom "$@" &!
        # shellcheck enable=SC1035
    fi
}
```

In this file I put the alias ls to change the colors of files and directories. 
If ```alias ls="ls -G -FC"``` does not do anything you can also write: ```alias ls="ls --color=auto"

#######################################################
The next file setup-bins.zsh:

```
# File should be sourced


mkdir -p "$HOME/Public/bin"
mkdir -p "$HOME/.local/bin"
mkdir -p "$HOME/bin"

chmod u=rwX,go=rX ~/Public

# Paths
existingCustomPath=()
customPath=(
    "$james_home/Public/bin"
    "$HOME/.local/bin"
    "$HOME/bin"
)
for p in $customPath; do
    if [ -d "$p" ]; then
        existingCustomPath=("$p" "${existingCustomPath[@]}")
    fi
done
path=("${existingCustomPath[@]}" "${path[@]}")
typeset -Ua path
export PATH
```

Now, I know Luke May added this file, but I honestly don't think I need it. If you do not want to add it then don't. I don't think it really does anything besides create a couple of directories. 

#######################################################
Final file setup-prompt.zsh:

```
# File should be sourced


export LC_ALL="en_US.UTF-8"
export term="xterm"
export PROMPT='%(?.%F{2}√.%F{1}X)%F{7}|%F{6}%n%F{248}:%B%F{5}%3~%b%F{7}%#%f '

# export PROMPT='%(?.%F{2}√.%F{1}X)%F{7}|%F{6}%n%F{248}@%F{6}%m%F{7}:%B%F{5}%3~%b%F{7}%#%f'


# export PROMPT='%(?.%F{2}√.%F{1}X)%F{7}|%F{6}%n%F{248}:%B%F{5}%3~%b%F{7}%#%f '


# export PROMPT='%(?.%F{2}√.%F{1}X)%F{7}|%B%F{5}%3~%b%F{7}%#%f '
```

This file is what helps make the command line look nice with a checkmark if things run correctly or an X if they don't. 
Each of the commented out export lines were lines that I changed until I got the look I wanted. 


#######################################################
Now that you have the zsh shell and these files you should be good to go with a nice looking terminal. 
