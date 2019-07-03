# iTerm2

## You must have iTerm2 + oh-my-zsh + zsh-autosuggestion

### Install iTerm2
Download .zip, unzip it and mv iTerm2.app to Applications. [iTerm2](https://www.iterm2.com/downloads.html)

### Install zsh
```
brew install zsh
```
### Install oh-my-zsh
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

### Install plugin zsh-autosuggestion
```
cd ~/.oh-my-zsh/custom/plugins/
git clone https://github.com/zsh-users/zsh-autosuggestions
vim ~/.zshrc
```
find the plugins part, modify it from

```
plugins=(git)
```
to
```
plugins=(
      git
      zsh-autosuggestions
)
```
Save it and compile.
```
vim ~/.zshrc
```

Enjoy!