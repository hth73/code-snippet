## Linux Clients

---

[Back to home](../README.md)

---

>my ~/.zshrc file
```bash
# color manpages
alias man="LANG=de_DE.UTF-8 man"
export LESS_TERMCAP_mb=$'\e[1;32m'
export LESS_TERMCAP_md=$'\e[1;32m'
export LESS_TERMCAP_me=$'\e[0m'
export LESS_TERMCAP_se=$'\e[0m'
export LESS_TERMCAP_so=$'\e[01;33m'
export LESS_TERMCAP_ue=$'\e[0m'
export LESS_TERMCAP_us=$'\e[1;4;31m'

# manage history file
setopt EXTENDED_HISTORY       # record timestamp of command in HISTFILE
setopt HIST_IGNORE_SPACE      # ignore commands that start with space    
setopt HIST_VERIFY            # show command with history expansion to user before running it
setopt SHARE_HISTORY          # share command history data
setopt HIST_EXPIRE_DUPS_FIRST # delete duplicates first when HISTFILE size exceeds HISTSIZE
setopt HIST_IGNORE_DUPS       # ignore duplicated commands history list
setopt HIST_IGNORE_ALL_DUPS
setopt HIST_FIND_NO_DUPS
setopt HIST_SAVE_NO_DUPS

export HISTFILE=~/.zsh_history
export HISTTIMEFORMAT="%d %B %H:%M:%S> "
export HISTSIZE=100000
export HISTFILESIZE=100000
export HISTIGNORE="h:history*:x:exit:c:clear*:pwd:up:ver:app:conf:tf:pac:ls:cd"

# load all my ssh key
if [ -z "$SSH_AUTH_SOCK" ] ; then
    eval `ssh-agent`
    grep -slR "PRIVATE" ~/.ssh/ | xargs ssh-add
fi

# direnv hook for zsh
eval "$(direnv hook zsh)"

## to expand the $PATH Variable
export PATH=$HOME/bin:/usr/local/bin:$PATH

# my default working directory
if [ -d ~/repo ] ; then
   cd ~/repo
else
   cd ~
fi

# function create and switch folder
function mkcd () {
   mkdir "$1" && cd "$1"
}

# start commands with sudo rights
if [ $UID -ne 0 ]; then
    alias reboot='sudo reboot'
    alias shutdown='sudo shutdown -h now'
    alias update='sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y'
    alias disk='sudo ncdu'
    alias cmount='mount | column -t'
    alias caconf='sudo dpkg-reconfigure ca-certificates'
fi

## my aliases
alias h='history -i'
alias x='exit'
alias up='uptime'
alias ver='lsb_release -dr && uname -snrm'
alias app='dpkg -l | grep $var1'
alias vi='vim'
alias btop='bpytop'
alias sub='/opt/sublime_text/sublime_text'
alias cfg='sub ~/.zshrc'
alias pull='for i in */.git; do ( echo $i; cd $i/..; git pull; ); done'
alias -s {yml,yaml,txt,json,sh,py,md,hcl,tf}=sub
alias d='dirs -v | head -10'

alias ll='ls -lh'
alias lr='ls -laihR'
alias la='ls -laihF'
alias l='ls -CF'

alias ..='cd ..'
alias ...='cd ../..'
alias ....='cd ../../..'
alias .....='cd ../../../..'

# use aws users for the project
alias ht='aws-vault exec hth --'
alias infra='aws-vault exec infra --'
alias pro='aws-vault exec pro --'
alias dev='aws-vault exec dev --'
alias tst='aws-vault exec tst --'
alias deploy='aws-vault exec --no-session deploy-user --'

## lbs aliases
alias delsrv='ssh-keygen -R server.domain.de'
alias vpn='ssh server.domain.de'
alias repo='$repo'
alias conf='$config'
alias pac='$packer'
alias tf='$terraform'

[ -f ~/.fzf.zsh ] && source ~/.fzf.zsh
```

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

>Useful ssh commands
```bash
## SSH Key übertragen
##
ssh-copy-id -i ~/.ssh/id_ed25519.pub username@hostname.domain.de

## SSH Key Files übertragen
##
USR="hth"
scp ~/.ssh/id_ed25519 ~/.ssh/id_ed25519.pub $USR@server.domain.de:/home/$USR/.ssh

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

>Create passwords
```bash
pwgen -y -c 18 10
# -y = Include at least one special symbol in the password
# -c = Include at least one capital letter in the password.
# -s = Generate completely random, hard-to-memorize passwords.
# Passwortlänge
# Anzahl der Passwörter
```

>Check checksums
```bash
## check checksums
##
sha256sum vagrant_1.9.3.msi
# 3214881d4f27d716edfd59b09a7fb39c03b9bfebc3ec07d80c196ca5e488096c *vagrant_1.9.3.msi

shasum -a256 vagrant_1.9.3.msi
# 3214881d4f27d716edfd59b09a7fb39c03b9bfebc3ec07d80c196ca5e488096c *vagrant_1.9.3.msi

sha512sum vagrant_1.9.3.msi
# 6ba5195b72545eeb616d55b87e3edd0a00e8d44646a8486b98711442ad8de926a961d5accdeefb999e4195e4718ba7e437d8c8a45285deabe2e30cdfa700218e *vagrant_1.9.3.msi

shasum -a512 vagrant_1.9.3.msi
# 6ba5195b72545eeb616d55b87e3edd0a00e8d44646a8486b98711442ad8de926a961d5accdeefb999e4195e4718ba7e437d8c8a45285deabe2e30cdfa700218e *vagrant_1.9.3.msi

md5sum vagrant_1.9.3.msi
# ebc6520f45617c7ebe63f2d3d8440f20 *vagrant_1.9.3.msi
```

>show linux versions
```bash
hostnamectl
lsb_release -a
uname -r
cat /etc/os-release
cat /proc/version
cat /etc/centos-release
rpm -q centos-release
```

>search in files
```bash
grep Suchwort *
grep -r "Suchwort" *
grep -i Suchwort Dateiname (Groß und Kleinschreibung unterdrücken)
grep -w Suchwort Dateiname (Nach ganzen Worten suchen)
grep -wi Suchwort Dateiname
grep -win Suchwort Dateiname (-n Zeilennnummer)
grep -l Suchwort * (Zeige Dateinamen)
grep -v Suchwort * (Alle Dateien ohne dem Suchwort)
grep -ilR /Verzeichnis -e 'Suchwort'
grep -i '^Zeichenkette' Dateiname (Suche nach Wörten die am Anfang mit Zeichenkette anfangen)
grep -n 'Zeichenkette$' Dateiname (Suche nach Wörten die am Ende mit Zeichenkette anfangen)
grep '.Zeichenkette' Dateiname
grep 'M..er' (Mayer Meier Meyer Maier)
grep 'Ap[f]*' Dateiname (Apfel, Apfelsine)
grep 'Ap[a-z]*' Dateiname (Apfel, Apfelsine)
grep '[Aa]p' Dateiname (Apfel,Apfelsine,Papaya)
grep '[Aa][fp][ea]' Dateiname (Papaya)
grep '[Aa][fp][fa]' Dateiname (Apfel,Apfelsine,Papaya)
grep '[^s,l,m]beere' Dateiname (Ergebnis = Erdbeere, Keine Ergebnis = Johannesbeere, Heidelbeere, Brombeere - kein s,l,m vor beere)
grep -l Suchwort *

sudo grep -rnw '/path/to/somewhere/' -e 'pattern'
sudo grep -rnw /etc/ -e "hashicorp"
sudo grep ^ /etc/apt/sources.list /etc/apt/sources.list.d/*

egrep Suchwort Datei
egrep '[lm]+beere' Dateiname
egrep 'Apfel|Birne|Kirsche' Dateiname
egrep '(Erd|Brom)+beere' Dateiname
egrep -n '(Erd|Brom)+beere' Dateiname

fgrep '?' Dateiname
fgrep '*' Dateiname
```

>find files
```bash
find wo? option was?
find /var -name Datei.txt
find /var -name "Beis*"
find . -name "*eis"
find /var -size +10 (512 Byte Blöcke)
find /var -mtime +20 (20 Tage und älter)
find /var -mtime -1 (vom letzten Tag)
find . -mtime +30 (älter als 30 Tage)
find /var -mtime +5 -name "Beis*"
find /var -user hthurnho
find /var -perm 755 (Alle Dateien die Ausführbar sind)
find /var -perm 755 -type f
find /var -perm 755 -type d
find /var -name -mtime -size -user -type -perm (Kombinierbar)

find /var -name "Beis*" -ls

find /var -name "Beis*" -exec ls-l {} \;
find /var -name "Beis*" -print
find /var -name "Beis*" -ok ls-l {} \;
find /var -name "Beispiel.txt" -exec rm {} \;

find /var -name "Beis*" | zip Beispiele -@
find /var -name "Beis*" | xargs zip Beispiele2
find /var -name "Beis*" | xargs tar Beispiele2
find / -name * (Zum Testen von Prozessen)

find . -type s (findet alle socket Dateien)
find . -type d (alle Ordner finden)
find . -type f (alle Datein finden)
find . -type f -name ".*" (alle versteckten Dateien finden)
find . -type d -name ".*" (alle versteckten Ordner finden)
find -newer beispiel.txt (finde Dateien neuer beispiel.txt)
find / -type f -name *.zip -size +4M
```

>manage processes
```bash
ps
kill -l
kill -KILL 1040 (Killt den Prozess)
kill -TERM 1042 (Prozess beendet sich selbst)
ps -e
ps -ef
UID (Besitzer) PID (Prozess ID) PPID (Parent Prozess ID)
ps -ef | grep dtlogin
ps -ef | grep 1042

pgrep -optionen muster
pgrep dtlogin
pgrep -x dtlogin
pgrep -n dtlogin (zuletzt erzeugte Prozess)
pgrep -U hth
pgrep -U hth -l (ProzessID und Prozessname)
pgrep -U hth -l

pkill -h
pkill -u hth

prstat (mit q beenden [Taskmanager])

ptree Prozessnummer (Parentprozess finden mit ps -ef)
```

>create archive
```bash
# tar c(create) t(anzeigen) x(Extrahieren) v(verbose) u(update) z(komprimieren) archiv-name.tar Datei(en) Verzeichnisse oder Wildcard *
tar -c(z)vf archiv.tar(.gz) *
tar -tvf archiv.tar
tar -xvf archiv.tar

tar -uvf archiv.tar eispieldatei.txt
tar -tvf archiv.tar | grep "Bei*"
```