# Elementary setup

## General
### Power management (powertop)
- Check Powertop tunables tab. If not 'Good', then create **/etc/rc.local**, add the following and make it executable:
```
#!/bin/sh -e
#
# rc.local
#
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.
sudo powertop --auto-tune
exit 0
```
### Make Firefox safer (AppArmor)
- Install apparmor utils:
```
sudo apt install apparmor-utils
```
- Enable AppArmor profile for Firefox:
```
sudo aa-enforce /etc/apparmor.d/usr.bin.firefox
```

### Enable touchpad gestures (fusuma)
- Grant permission to read the touchpad device: `sudo gpasswd -a $USER input` and **reboot**
- Install fusuma and dependencies:
```
sudo apt install libinput-tools ruby xdotool
sudo gem install fusuma
```
- Create ~/.config/fusuma/config.yml and addd the following
``` 
swipe:
  3:
    left:
      command: 'xdotool key super+Right'
    right:
      command: 'xdotool key super+Left'
    up:
      command: 'xdotool key super+w'
    down:
      command: 'xdotool key super+s'
  4:
    left:
      command: 'xdotool key super+Right'
    right:
      command: 'xdotool key super+Left'
    up:
      command: 'xdotool key super+w'
    down:
      command: 'xdotool key super+s'
pinch:
  2:
    in:
      command: 'xdotool key ctrl+plus'
      threshold: 0.1
    out:
      command: 'xdotool key ctrl+minus'
      threshold: 0.1

threshold:
  swipe: 1
  pinch: 1

interval:
  swipe: 1
  pinch: 1
```
- Add `/usr/local/bin/fusuma -d` to the list of Elementary's startup applications
## Development
- [VSCode](https://code.visualstudio.com/docs/setup/linux)
- Node.js with [NVM](https://github.com/nvm-sh/nvm)
-- Install Node.js 12 (LTS): `nvm install --lts=erbium`
- Deferred NVM initialization
-- Edit .bashrc and comment/delete the NVM lines
-- Add the following to the end of .bashrc:
```
# Defer initialization of nvm until nvm, node or a node-dependent command is
# run. Ensure this block is only run once if .bashrc gets sourced multiple times
# by checking whether __init_nvm is a function.
if [ -s "$HOME/.nvm/nvm.sh" ] && [ ! "$(type -t __init_nvm)" = function ]; then
  export NVM_DIR="$HOME/.nvm"
  [ -s "$NVM_DIR/bash_completion" ] && . "$NVM_DIR/bash_completion"
  declare -a __node_commands=('nvm' 'node' 'npm' 'yarn' 'gulp' 'grunt' 'webpack')
  function __init_nvm() {
    for i in "${__node_commands[@]}"; do unalias $i; done
    . "$NVM_DIR"/nvm.sh
    unset __node_commands
    unset -f __init_nvm
  }
  for i in "${__node_commands[@]}"; do alias $i='__init_nvm && '$i; done
fi
```
