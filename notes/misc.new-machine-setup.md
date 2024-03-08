---
id: 6kfgcpkttqahew4mzc5qf4p
title: New Machine Setup
desc: ""
updated: 1701474833697
created: 1652123468018
---

# Homebrew

1. Install Homebrew - <https://brew.sh/>

2. Install Dropbox

```sh
brew install --cask dropbox;
```

---

# Log in to Dropbox

---

# Open this doc and follow along

---

# Homebrew Cont.

- Install the rest of the things

```sh
brew install asdf coreutils curl fzf git gh gpg zsh zsh-completions;
brew install --cask brave-browser discord docker iterm2 postgres-unofficial postman rectangle spotify visual-studio-code vlc;
$(brew --prefix)/opt/fzf/install;
```

---

# Run each of the installed casks to initialize

---

# Install Brave extensions

- 1password - <https://chromewebstore.google.com/detail/1password-%E2%80%93-password-mana/aeblfdkhhhdcdjpifhhbdiojplfjncoa>

- uBlock origin - <https://chromewebstore.google.com/detail/ublock-origin/cjpalhdlnbpafiamejdnhcphjbkeiagm>

- BetterTTV - <https://chromewebstore.google.com/detail/betterttv/ajopnjidmegmdimjlfnijceegpefgped>

- RES - <https://chromewebstore.google.com/detail/reddit-enhancement-suite/kbmfpngjjgdllneeigpgjifpgocmfgmb>

- Old Reddit Redirect - <https://chromewebstore.google.com/detail/old-reddit-redirect/dneaehbmnbhcippjikoajpoabadpodje>

---

# Add Bookmarks

- Fastmail - <https://app.fastmail.com/>

- all subreddits - <https://old.reddit.com/r/all/>

- hackernews - <https://news.ycombinator.com/>

- raindrop.io (for bookmarks) - <https://app.raindrop.io/>

---

# Log in to 1Password

---

# Log in to Fastmail

---

# Log in to Reddit

---

# Log in to Twitch

---

# Log in to Discord

---

# Log in to Spotify

---

# Switch to iterm

---

# zsh

1. Install oh-my-zsh

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

2. Symlink local zsh theme, .zshenv, .zsh_profile and .zshrc files to cloud
   versions

```sh
rm ~/.oh-my-zsh/themes/wondermuffin.zsh-theme;
ln -s ~/Dropbox/dotfiles/wondermuffin.zsh-theme ~/.oh-my-zsh/themes/wondermuffin.zsh-theme;

rm ~/.zshenv;
ln -s ~/Dropbox/dotfiles/.zshenv ~/.zshenv;

rm ~/.zsh_profile;
ln -s ~/Dropbox/dotfiles/.zsh_profile ~/.zsh_profile;

rm ~/.zshrc;
ln -s ~/Dropbox/dotfiles/.zshrc ~/.zshrc;
```

3. Source the synced .zshrc file

```sh
source ~/.zshrc;
```

4. Set up the zsh-completions plugin

```sh
git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:-${ZSH:-~/.oh-my-zsh}/custom}/plugins/zsh-completions
```

5. Set up the fast-syntax-highlighting zsh plugin

```sh
git clone https://github.com/z-shell/F-Sy-H.git \
  ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/F-Sy-H

rm ~/.fast-theme.wondermuffin.ini;
ln -s ~/Dropbox/dotfiles/.fast-theme.wondermuffin.ini ~/.fast-theme.wondermuffin.ini;

source ~/.zshrc;

fast-theme ~/.fast-theme.wondermuffin.ini;

source ~/.zshrc;
```

---

# VSCode

- Sign in via Github to sync settings

- To run VSCode with `code` on the command line:

  1. Launch VSCode
  2. Open the Command Palette (F1 / cmd+shift+p)
  3. Type `shell command` to find and run
     `Shell Command: Install 'code' command in PATH`
  4. Restart terminal

- If key repeating isn't working:

```sh
defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool false;
```

---

# git and github

1. Symlink local git configuration files to cloud versions

```sh
rm ~/.gitignore_global;
ln -s ~/Dropbox/dotfiles/.gitignore_global ~/.gitignore_global;

rm ~/.gitconfig;
ln -s ~/Dropbox/dotfiles/.gitconfig ~/.gitconfig;
```

2. Generate a new SSH key - <https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key>

3. Log into gh

```sh
gh auth login;
```

---

# asdf

1. Symlink local `.tool-versions` file to cloud version

```sh
rm ~/.tool-versions;
ln -s ~/Dropbox/dotfiles/.tool-versions ~/.tool-versions;
```

2. Install the `nodejs` plugin

```sh
asdf plugin add nodejs https://github.com/asdf-vm/asdf-nodejs.git
```

4. Install the version of nodejs that appears in `~/.tool-versions`

```sh
asdf install nodejs <VERSION_NUMBER>
```

---

# editorconfig

- Symlink local `.editorconfig` file to cloud version

```sh
rm ~/.editorconfig;
ln -s ~/Dropbox/dotfiles/.editorconfig ~/.editorconfig;
```

---
