first install openssh to connect in windows cmd.
make sure you have selected briged network instead nat in virtual box

```sh
pacman -Sy
```
```sh
pacman -S openssh
```
> passwd [remember the password to connect on windows cmd]
> to connect:``` ssh root@ip```
> if you get this while trying to connect ssh:
[
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ED25519 key sent by the remote host is
SHA256:lVSVwYb2k6jOtlOSke0d/AbW02EElF7UhJlYzJEM8No.
Please contact your system administrator.
Add correct host key in C:\\Users\\u/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in C:\\Users\\u/.ssh/known_hosts:3
Host key for 192.168.7.9 has changed and you have requested strict checking.
Host key verification failed.
]
try this:
> on your windows cmd
```ssh-keygen -R localhost(ip)```
```ssh-keyscan -H localhost(ip) >> C:\Users\u\.ssh\known_hosts```



# Check for UEFI mode
```ls /sys/firmware/efi/efivars```

# If the directory is present, it means you're booted in UEFI mode.

# Partitioning the disk (use 'gdisk' or 'cfdisk' for GPT partitioning)
```cfdisk /dev/sda```

# Create EFI partition (size 512M, type EF00)
# Create root partition (remaining space, type 8300)

# Format partitions
```mkfs.fat -F32 /dev/sda1```
```mkfs.ext4 /dev/sda2```
# Mount the partitions
```mount /dev/sda2 /mnt```
```mkdir /mnt/boot```
```mount /dev/sda1 /mnt/boot```

# Install base system and XFCE desktop environment
```pacstrap /mnt base linux linux-firmware xfce4 xfce4-goodies```

# Generate fstab
```genfstab -U /mnt >> /mnt/etc/fstab```

# Change root into the new system
```arch-chroot /mnt```

# Set timezone
```ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime```
```hwclock --systohc```

# Set the system locale
```echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen```
```locale-gen```
```echo "LANG=en_US.UTF-8" > /etc/locale.conf```

# Set hostname
```echo "seniority" > /etc/hostname```

# Configure hosts file
```echo "127.0.0.1 localhost" >> /etc/hosts```
```echo "::1 localhost" >> /etc/hosts```
```echo "127.0.1.1 seniority.localdomain seniority" >> /etc/hosts```

> add user (if dont add user you wont be able to login after installition finished
```useradd -m seniority```
```passwd seniority```
> for ex:1 and 1 again
```usermod -aG wheel username```


# Install and configure bootloader (GRUB)
```pacman -S grub efibootmgr```
```grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB```
```grub-mkconfig -o /boot/grub/grub.cfg```


# Requierd Program and tools
```pacman -S firefox gnome-terminal vim nano vlc ffmpeg base-devel git gimp htop neofetch gparted zip unzip networkmanager wireless_tools wpa_supplicant dialog``` 

# Exit chroot
```exit```

# Unmount partitions
```umount -R /mnt```

# Reboot
```reboot```
