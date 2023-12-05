# How to get hibernate working on ROG Ally with ChimeraOS
 
# Step 1. Resize /home/swapfile
```sh
	sudo swapoff /home/swapfile
    sudo dd if=/dev/zero of=/home/swapfile bs=1M count=24576 oflag=append conv=notrunc
    sudo mkswap /home/swapfile
    sudo swapon /home/swapfile
```

# Step 2. Add resume hook to initramfs
```sh
	sudo frzr-unlock
    sudo mv /boot/initramfs-linux.img /boot/initramfs-linux-stock.img
    sudo nano /etc/mkinitcpio.conf # add resume after udev in the HOOKS= line
    sudo mkinitcpio -g /boot/initramfs-linux.img
```

# Step 3. Get resume UUID and offset
Get the UUID of the partition and the swapfile offset
```sh
lsblk -f # note down the UUID
sudo btrfs inspect-internal map-swapfile -r /home/swapfile # note down the offset
```

# Step 4. Update /boot/loader/entries/frzr.conf
add resume=UUID=<UUID from earlier> and resume_offset=<offset from earlier> before nowatchdog in the options line:

```sh
sudo nano /boot/loader/entries/frzr.conf
```
 
# Step 5. Workaround for https://github.com/systemd/systemd/issues/15354

```sh
sudo systemctl edit systemd-logind.service
```

add the following under Anything between here and the comment below will become the contents of the drop-in file

```
[Service]
ProtectHome=read-only
```

# Step 6. (Skip to step 7 if not using handycon) Edit power button behavior
change power_button from SUSPEND to HIBERNATE:

```sh
sudo vim /etc/handygccs/handygccs.conf
```

 
# Step 7. (Skip if using handycon) Edit power button behavior for systemd
change HandlePowerButton to suspend-then-hibernate:

```sh
sudo nano /etc/systemd/logind.conf.d/power_off.conf
```

# Additional Notes
setting: 
HibernateDelaySec=X

in /etc/systemd/sleep.conf

you can configure how much time after sleep it will hibernate
given how fast nvmes are, this is the sweet spot. use sleep-then-hibernate with 30 minutes more or less until it hibernates and continue from where we left with much more power in the battery