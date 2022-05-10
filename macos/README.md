## Basics

#### Settings to reconfigure

* Settings > General > Close windows when quitting an app ON
* Settings > Accessibility > Display > Shake mouse pointer to locate OFF
* Scrolling speed and mouse speed to desired
* Settings > Trackpad > Tap to click ON
* Settings > Mouse > Natural scroll OFF

#### Lock screen Command+L

* Settings > Keyboard > Shortcuts > App Shortcuts > Add
   * Application: All Applications
   * Menu Title: Lock Screen # case sensitive lol
   * Shortcut: set to desired

#### Don't clutter the desktop with screenshots

```
defaults write com.apple.screencapture location /Users/Sam.Nicholls/Pictures/Screenshots
```

## Screen

#### Blurry font on non-4k monitor

* Install [BetterDummy](https://github.com/waydabber/BetterDummy)
* Set real screen to be a mirror of the dummy

## Keyboard

#### Layout

* Set to British PC to have a semblance of familiarity
* Keyboard > Shortcuts > Input sources > Unset all options
* Keyboard > [x] Use F1, F2... as standard keys
* Keyboard > Text > unset Correct spelling, capitalise words, add full stop
* Keybaord > Text > set smart quotes to `"` and `'`

#### Rebind Mission Control

* Keyboard > Shortcuts > Mission Control (rebind command and arrows to ctrl+alt and arrows)

#### Take screenshots normally

* Keyboard > Shortcuts > Screenshots (rebind to F13 and shift+F13)

#### Nuke keyboard settings if it thinks your ISO board is ANSI for whatever reason

```
sudo rm /Library/Preferences/com.apple.keyboardtype.plist
```

#### Remove delay in activating Caps Lock

* Activate Slow Keys with lowest acceptance window from Accessibility settings
* Discovered this has the nasty side affect of preventing keys easily being repeated

## Audio

#### Make Apple Music go away

```
launchctl unload -w /System/Library/LaunchAgents/com.apple.rcd.plist
```

## Terminal

#### Make Home and End work

* Terminal > Preferences > Profile > Keyboard
* Add:
    *  Key: Home (End)
    *  Modifier: None
    *  Action: Send Text \O33OH (\O33OF)
    *  Note: Press ESC to send \O33


Alternatively, add the following to `.zshrc`

```
bindkey '\e[H'    beginning-of-line
bindkey '\e[F'    end-of-line
```

#### Get rid of command-d split pane

* Settings > Keyboard > Shortcuts > App Shortcuts > Add
   * Application: Terminal
   * Menu Title: Split Pane # case sensitive lol
   * Shortcut: set to something garbage like shift+ctrl+command+alt+2


## Rosetta

### Get an x86_64 terminal

* Install iTerm
* Duplicate iTerm (right click > Duplicate)
* Right click > Get info > [x] Open with Rosetta

## Packages

### Install brew

* Install the xcode command line tools by attempting to do absolutely anything code related on the command line
* Install brew by following their dodgy instructions to wget a script into your shell
    * Do not leave it to brew to install the xcode components you need as it will just do nothing for hours

### x86 brew

* Installing brew the default way does not allow for x86 applications to be installed because why not
* [Follow these instructions](https://medium.com/mkdir-awesome/how-to-install-x86-64-homebrew-packages-on-apple-m1-macbook-54ba295230f)

```
cd ~/Downloads
mkdir homebrew
curl -L https://github.com/Homebrew/brew/tarball/master | tar xz --strip 1 -C homebrew
sudo mv homebrew /usr/local/homebrew
```

```
export PATH=$HOME/bin:/usr/local/bin:$PATH
# for intel x86_64 brew
alias axbrew='arch -x86_64 /usr/local/homebrew/bin/brew'
```

### LibreSSL and OpenSSL nonsense

```
export CPPFLAGS="-I$(axbrew --prefix openssl)/include"; export LDFLAGS="-L$(axbrew --prefix openssl)/lib";
```

* See [here for more](https://github.com/pyenv/pyenv/wiki/Common-build-problems#error-the-python-ssl-extension-was-not-compiled-missing-the-openssl-lib)
