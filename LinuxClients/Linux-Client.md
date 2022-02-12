## Linux Clients

---

[Back to home](../README.md)

---

>Install nextcloud client
```bash
sudo add-apt-repository ppa:nextcloud-devs/client
sudo apt update
sudo apt install nextcloud-client
```

>Install sublime-text
```bash
sudo apt update
sudo apt install dirmngr gnupg apt-transport-https ca-certificates software-properties-common

curl -fsSL https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
sudo add-apt-repository "deb https://download.sublimetext.com/ apt/stable/"
sudo apt install sublime-text

## install package Control
## https://packagecontrol.io/installation
## ctrl + shift + p
## Install Package Control

## useful plugins in sublime-text
##
# Git, AutoFileName, All Autocomplete, SideBarEnhancements, Compare Side-By-Side, Theme - Afterglow, Powershell, CSS3, HTML5, Terraform, MarkdownPreview

## preferences - settings
##
{
  "color_inactive_tabs": true,
  "color_scheme": "Packages/Theme - Afterglow/Afterglow.tmTheme",
  "font_size": 10,
  "ignored_packages":
  [
    "CSS",
    "Markdown",
    "Vintage",
  ],
  "status_bar_brighter": true,
  "tab_size": 2,
  "tabs_small": true,
  "theme": "Afterglow-blue.sublime-theme",
  "translate_tabs_to_spaces": true,
  "default_encoding": "UTF-8",
  "show_encoding": true,
}
```

>Install powershell
```bash
## Powershell installieren
##
sudo apt-get update
sudo apt-get install wget apt-transport-https software-properties-common -y

cd /tmp
wget -q https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb

sudo apt-get update
sudo apt-get install powershell -y

# Start PowerShell
pwsh

## AWSPowershell Module installlieren
##
Install-Module -Name AWSPowerShell
Import-Module -Name AWSPowerShell
```

>Install fuzzy-search (fzf)
```bash
cd ~
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```

>useful ssh commands
```bash
## SSH Key übertragen
##
ssh-copy-id -i ~/.ssh/id_ed25519.pub username@hostname.domain.de

## SSH Key Files übertragen
##
USR="hth"
scp ~/.ssh/id_rsa ~/.ssh/id_rsa.pub $USR@server.domain.de:/home/$USR/.ssh

## Remote Ordner anlegen über ssh
##
ssh $USR@server.domain.de 'test -d ~/.aws || mkdir ~/.aws'

## Remote files bearbeiten über ssh
##
vim scp://$USR@server.domain.de//home/$USR/.aws/config
```

>Uninstall packages completely
```bash
sudo dpkg --get-selections | grep <packagename> | awk '{ print $1}'| xargs sudo apt-get -y --purge autoremove
```