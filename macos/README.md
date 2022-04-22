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
