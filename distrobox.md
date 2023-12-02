Mini howto:
1) create distrobox assemble file (default is in ~/distrobox.ini) with contents:
```
[arch-kde]
additional_packages="plasma-meta plasma-wayland-session konsole chromium"
image=archlinux:latest
pull=true
init_hooks="sudo rm -rf /run/systemd/system;"
init_hooks="sudo mkdir -p /run/{dbus,systemd};"
init_hooks="sudo ln -fs /run/host/run/systemd/system /run/systemd;"
init_hooks="sudo ln -fs /run/host/run/dbus/system_bus_socket /run/dbus/;"
exported_apps="kwin_wayland_wrapper"
```

2) run
```sh
distrobox assemble create 
```
3) run
```sh
distrobox enter arch-kde
```
4) create nested-session file with contents (mark as executable and add as non steam game):
```sh
#!/bin/sh

# Remove the performance overlay, it meddles with some tasks
unset LD_PRELOAD
 
## Shadow kwin_wayland_wrapper so that we can pass args to kwin wrapper
## whilst being launched by plasma-session
distrobox-enter arch-kde -- mkdir $XDG_RUNTIME_DIR/nested_plasma -p
distrobox-enter arch-kde -- cat <<EOF > $XDG_RUNTIME_DIR/nested_plasma/kwin_wayland_wrapper
#!/bin/sh
/usr/bin/kwin_wayland_wrapper --width 1920 --height 1080 --no-lockscreen \$@
EOF
distrobox-enter arch-kde -- chmod a+x $XDG_RUNTIME_DIR/nested_plasma/kwin_wayland_wrapper
distrobox-enter arch-kde -- export PATH=$XDG_RUNTIME_DIR/nested_plasma:$PATH
 
distrobox-enter arch-kde -- dbus-run-session startplasma-wayland
 
distrobox-enter arch-kde -- rm $XDG_RUNTIME_DIR/nested_plasma/kwin_wayland_wrapper
```

5) make X11 sock available for gamer user via:
```sh
sudo chown -f -R gamer:gamer /tmp/.X11-unix/
```

(has to be rerun at every reboot sadly, but adding it to profile should work here)

6) run the added non-steam from 4)