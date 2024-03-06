# jack2
This image contains jack2 and everything required to easily use it.

Just start QJackCtl and press Start. Pipewire will also re-route to the chosen interface.

If Start is not working set these two scripts: one before starting jack and the other after having it running:

```sh
systemctl --user stop pipewire.socket
systemctl --user start pipewire.socket
```

In KDE remember to enable virtual devices to be able to play music from pipewire while jack2 is running