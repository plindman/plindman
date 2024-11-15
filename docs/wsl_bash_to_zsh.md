# switching WSL bash to zsh

https://blog.joaograssi.com/windows-subsystem-for-linux-with-oh-my-zsh-conemu/
https://ohmyz.sh/
https://www.zsh.org/


## Core istallation

``` sh
sudo apt-get install zsh git
#  choose option 0 to create the config file and prevent this message to show again.
zsh

# oh-my-zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

```

``` sh
# .bashrc
if test -t 1; then
exec zsh
fi
```

``` sh
# .zshrc
# Find and change this ZSH_THEME="robbyrussell" # To this ZSH_THEME="agnoster"
# Well, not going to switch yet
# If you want agnoster, read the blog to discover windows font installation
```

