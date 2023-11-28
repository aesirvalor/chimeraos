# How to get hibernate working on ROG Ally with ChimeraOS
 
# Step 1. Resize /home/swapfile
	sudo swapoff /home/swapfile
    sudo dd if=/dev/zero of=/home/swapfile bs=1M count=24576 oflag=append conv=notrunc
    sudo mkswap /home/swapfile
    sudo swapon /home/swapfile
 
# Step 2. Add resume hook to initramfs
	sudo frzr-unlock
    sudo mv /boot/initramfs-linux.img /boot/initramfs-linux-stock.img
    sudo vim /etc/mkinitcpio.conf
    	add resume after udev in the HOOKS= line
    sudo mkinitcpio -g /boot/initramfs-linux.img
 
# Step 3. Get resume UUID and offset
	lsblk -f
    	Note down the UUID
    sudo btrfs inspect-internal map-swapfile -r /home/swapfile
    	Note down the offset
 
# Step 4. Update /boot/loader/entries/frzr.conf
	sudo vim /boot/loader/entries/frzr.conf
    	add resume=UUID=<UUID from earlier> and resume_offset=<offset from earlier> before nowatchdog in the options line
 
# Step 5. Workaround for https://github.com/systemd/systemd/issues/15354
	sudo systemctl edit systemd-logind.service
    	add the following under ### Anything between here and the comment below will become the contents of the drop-in file
        	[Service]
			ProtectHome=read-only
 
# Step 6. (Skip to step 7 if not using handycon) Edit power button behavior
	sudo vim /etc/handygccs/handygccs.conf
    	change power_button from SUSPEND to HIBERNATE
 
# Step 7. (Skip if using handycon) Edit power button behavior for systemd
	sudo vim /etc/systemd/logind.conf.d/power_off.conf
		change HandlePowerButton to suspend-then-hibernate