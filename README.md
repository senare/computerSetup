# computerSetup

Boot from USB

- Setup keyboard layout

        loadkeys se-lat6

- Setup fonts

        setfont sun12x22

- Verify the boot mode (UEFI)

        efivar -l    
        ls /sys/firmware/efi/efivars
        
        
        
- Setup WIFI
    
        ip link set wlp59s0 up
        wpa_supplicant -B -i wlp59s0 -c <(wpa_passphrase MJNET secret)
        
- Setup IP     
        
        dhcpdc        

- Verify connection to internet

        ping archlinux.org

- Update the system clock

        timedatectl set-ntp true    

- Partition the disks

        fdisk -l
        gdisk /dev/nvme0n1
        
        gpt 
        BOOT/EFI    100   MiB   ef00 
        ROOT        100   GiB   8300
        HOME        365.7 GB    8300
- Format

        mkfs.fat -F32   /dev/nvme0n1p1
        mkfs.ext4       /dev/nvme0n1p2
        mkfs.ext4       /dev/nvme0n1p3

- Mount 

        mount /dev/nvme0n1p2 /mnt
        mkdir /mnt/boot
        mount /dev/nvme0n1p1 /mnt/boot
        mkdir /mnt/home
        mount /dev/nvme0n1p3 /mnt/home
        
- Fix mirrors

        vim /etc/pacman.d/mirrorlist

- Install

        pacstrap /mnt base base-devel
        
- Fstab

        genfstab -U /mnt >> /mnt/etc/fstab

- Chroot

        arch-chroot /mnt

- Time zone
        
        ln -sf /usr/share/zoneinfo/Europe/Stockholm /etc/localtime
        hwclock --systohc
        
- Localization  (sv_SE.UTF-8  UTF-8)
 
        vi /etc/locale.gen
        locale-gen
        
        LANG=sv_SE.UTF-8 >> /etc/locale.conf
        KEYMAP=se-lat6 >> /etc/vconsole.conf
        
- Network
    
        bigbertha >> /etc/hostname
        
        /etc/hosts
        127.0.0.1	localhost
        ::1		    localhost
        127.0.1.1	bigbertha.mjnet	bigbertha
        
- Passwd
        
        passwd
        
