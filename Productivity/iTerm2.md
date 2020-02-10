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


### key bindings 

Added profile.
```
Move left between words: cmd ⌘← | send escape sequence | b
Move right between words: cmd ⌘→ | send escapes sequence | f
Start of the line: Shift ⇧ ← | send hex code | 0x01 or send escape sequence | [H
End of the line : Shift ⇧→ | send hex code | 0x05 or send escape sequence | [F
Delete previous word cmd ⌘←Delete | send hex code | 0x17
Delete entire line Shift ⇧←Delete | send hex code | 0x15
```
