## Keyboard

Nuke keyboard settings if it thinks your ISO board is ANSI for whatever reason

```
sudo rm /Library/Preferences/com.apple.keyboardtype.plist
```

## Audio

Make Apple Music go away

```
launchctl unload -w /System/Library/LaunchAgents/com.apple.rcd.plist
```
