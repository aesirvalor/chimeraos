# Controllers Tips'n'Tricks

To hide every device except DualSense:

```
SDL_HINT_GAMECONTROLLER_IGNORE_DEVICES_EXCEPT="0x054C/0x0DF2"
```

Same thing for DualShock:
```
SDL_HINT_GAMECONTROLLER_IGNORE_DEVICES_EXCEPT="0x054C/0x09CC"
```

This ignores specific devices:

```
SDL_HINT_GAMECONTROLLER_IGNORE_DEVICES="0x0B05/0x1ABE,0x045E/0x028E"
```

This works the same way as SDL_HINT_GAMECONTROLLER_IGNORE_DEVICES
```
SDL_HINT_HIDAPI_IGNORE_DEVICES="0x0B05/0x1ABE,0x045E/0x028E"
```

If you do this system shortcuts will stop workig:
```
SDL_HINT_GRAB_KEYBOARD="1"
```

This one will stop enumerating hid devices:
```
SDL_HINT_HIDAPI_ENUMERATE_ONLY_CONTROLLERS
```

This is either 0 or 1:
```
SDL_HINT_JOYSTICK_HIDAPI="1" # 1 is default
```

