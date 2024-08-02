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

If your computer says that `zsh` is not available, look up how to install it on your computer. Here are some examples for how to do it o different operating systems:

### Installing ZShell

#### Ubuntu/Debian

```
sudo apt update
sudo apt install zsh
```

#### Fedora
```
sudo dnf install zsh
```

#### Arch Linux
```
sudo pacman -S zsh
```

#### macOS

`zsh` comes pre-installed on macOS. If you need to update it, you can use Homebrew:
```
brew install zsh
```

#### Windows

You can install `zsh` on Windows using the Windows Subsystem for Linux (WSL). First, install a Linux distribution from the Microsoft Store (e.g., Ubuntu), then open the terminal and follow the instructions for Ubuntu/Debian.

After installing `zsh`, make it your default shell:
```
chsh -s $(which zsh)
```

## Step 2: Create and Configure `.zshrc`

Create a file in your home directory called `.zshrc`. This file will define anything you want to run whenever you open a new terminal session. Test it by adding the following line at the top:

```
echo "Hello, world!"
```

Exit the terminal and open a new one. You should see it print "Hello, world!" at the top of the terminal. You can remove this line if you like since you have confirmed it is working. 

### Setting Up Your `PATH` Environment Variable

The `PATH` variable is a set of directories that the shell looks in for programs. By default, it includes system directories like `/usr/bin` and `/bin`. You can add custom directories to your `PATH` so the shell can find your programs more easily.

#### Adding Directories to `PATH`

To add a directory to your `PATH`, use the following:

```
PATH="$PATH:/your/custom/path"
```

You can add multiple directories by separating them with a colon (`:`):

```
PATH="$PATH:/your/custom/path1:/your/custom/path2"
```

Add these lines to your `.zshrc` file to make the changes permanent.

#### Example: Adding Local Binaries to `PATH`

If you have custom binaries in directories like `~/bin` or `~/.local/bin`, you can add them to your `PATH` like this:

```
# Add local bin directories to PATH
PATH="$HOME/bin:$HOME/.local/bin:$PATH"
```

This prepends ~/bin and ~/.local/bin to your existing PATH, ensuring that the shell looks in these directories first.


## Step 3: Customize Your Command Prompt

You can customize your command prompt with ANSI escape sequesnces to add colors and symbols. Add the following lines to your `.zshrc` to set up a colorful prompt:

```
export LC_ALL="en_US.UTF-8"
export term="xterm"
export PROMPT='%(?.%F{2}√.%F{1}X)%F{7}|%F{6}%n%F{248}@%F{6}%m%F{7}:%B%F{5}%3~%b%F{7}%#%f
```

## Step 4: Organize Your Configuration

To keep your `.zshrc` file manageable, you can separate pieces of code into individual files and source them. This approach helps in organizing your configurations and making your setup more modular. 

### Creating the `.sources` Directory

Create a `.sources` directory in your home directory. This directory will contain your separate configuration files. You can create it using the following command:

```
mkdir -p ~/.sources
```

### Example `.zshrc` File

Your `.zshrc` file will source these separate configuration files. Here is an example of how your `.zshrc` file might look:

```
# Prompt
source ~/.sources/setup-prompt.zsh

# Path and bin dirs
source ~/.sources/setup-bins.zsh

# Aliases
source ~/.sources/setup-aliases.zsh
```

### Sourcing the `.zshrc` File

Whenever you make changes to any files within the `.sources` directory or directly to the `.zshrc` file, you need to source the `.zshrc` file to apply the changes. You can do this by running:

```
source ~/.zshrc
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
# Just uncomment them to use and comment out the ones you don't want to use

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

By organizing your configuration files into the .sources directory and sourcing them in your .zshrc file, you keep your setup clean and modular. Remember to source your .zshrc file after making any changes to apply them:

```
source ~/.zshrc
```

Now that you have a ZShell installed and your configuration files set up, your terminal should be looking nice and working efficiently. Thank you for reading and enjoy your personalized terminal setup!
