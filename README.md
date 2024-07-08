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

To keep your `.zshrc` file manageable, you can separate pieces of code into individual files and source them. Create a `.sources` directory in your home directory and place your configuration files there. Your `.zshrc` might look like this:

```
# Prompt
source ~/.sources/setup-prompt.sh

# Path and bin dirs
source ~/.sources/setup-bins.sh

# Aliases for things like nano
source ~/.sources/setup-aliases.sh
```

### Example Configuration Files

Here are the contens of each file in the `.sources` directory:

`setup-prompt.zsh`

```
# File should be sourced

export LC_ALL="en_US.UTF-8"
export term="xterm"
export PROMPT='%(?.%F{2}√.%F{1}X)%F{7}|%F{6}%n%F{248}:%B%F{5}%3~%b%F{7}%#%f '

# Some other options for your PROMPT
# Just Uncomment them to use and comment out the ones you don't want to use

# export PROMPT='%(?.%F{2}√.%F{1}X)%F{7}|%F{6}%n%F{248}@%F{6}%m%F{7}:%B%F{5}%3~%b%F{7}%#%f'
# export PROMPT='%(?.%F{2}√.%F{1}X)%F{7}|%F{6}%n%F{248}:%B%F{5}%3~%b%F{7}%#%f '
# export PROMPT='%(?.%F{2}√.%F{1}X)%F{7}|%B%F{5}%3~%b%F{7}%#%f '
```

`setup-aliases.zsh`

```
# File should be sourced

# Aliases
alias ..='cd ..'
alias nano='/usr/bin/nano -wacuWEMS --tabsize=4'
alias kate='/usr/bin/kate >& /dev/null'
alias grep='/usr/bin/grep --color=auto'
alias ls="ls -G -FC"
#alias ls="ls --color=auto" # use this if ls -F -FC doesn't work
alias c="clear"
alias gcc='gcc -Wall'
```

`setup-bins.zsh`

```
# File should be sourced

mkdir -p "$HOME/Public/bin"
mkdir -p "$HOME/.local/bin"
mkdir -p "$HOME/bin"

chmod u=rwX,go=rX ~/Public

# Paths
existingCustomPath=()
customPath=(
    "$HOME/Public/bin"
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

## Conclusion

Now that you have a ZShell installed and your configuration files set up, your terminal should be looking nice and working efficiently. Thank you for reading and enjoy your personalized terminal setup!
