## Basics

* Settings > General > Close windows when quitting an app ON
* Settings > Accessibility > Display > Shake mouse pointer to locate OFF
* Scrolling speed and mouse speed to desired
* Settings > Trackpad > Tap to click ON
* Settings > Mouse > Natural scroll OFF

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
