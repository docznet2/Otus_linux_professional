# Otus_linux_professional
                Обновление ядра системы
> bash -x /tmp/kernupd.sh 2>&1 | tee /out.log

==============================================================
/out.log:


+ cat /etc/lsb-release /etc/os-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=22.10
DISTRIB_CODENAME=kinetic
DISTRIB_DESCRIPTION="Ubuntu 22.10"
PRETTY_NAME="Ubuntu 22.10"
NAME="Ubuntu"
VERSION_ID="22.10"
VERSION="22.10 (Kinetic Kudu)"
VERSION_CODENAME=kinetic
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=kinetic
LOGO=ubuntu-logo
+ echo ''

+ uname -r
5.19.0-21-generic
+ echo ''

+ wget -P /tmp/ https://kernel.ubuntu.com/mainline/v6.17-rc4/amd64/linux-headers-6.17.0-061700rc4-generic_6.17.0-061700rc4.202508312336_amd64.deb
+ wget -P /tmp/ https://kernel.ubuntu.com/mainline/v6.17-rc4/amd64/linux-headers-6.17.0-061700rc4_6.17.0-061700rc4.202508312336_all.deb
+ wget -P /tmp/ https://kernel.ubuntu.com/mainline/v6.17-rc4/amd64/linux-image-unsigned-6.17.0-061700rc4-generic_6.17.0-061700rc4.202508312336_amd64.deb
+ wget -P /tmp/ https://kernel.ubuntu.com/mainline/v6.17-rc4/amd64/linux-modules-6.17.0-061700rc4-generic_6.17.0-061700rc4.202508312336_amd64.deb
+ echo ''

+ dpkg -i /tmp/linux-modules-6.17.0-061700rc4-generic_6.17.0-061700rc4.202508312336_amd64.deb
Selecting previously unselected package linux-modules-6.17.0-061700rc4-generic.
(Reading database ... 140346 files and directories currently installed.)
Preparing to unpack .../linux-modules-6.17.0-061700rc4-generic_6.17.0-061700rc4.202508312336_amd64.deb ...
Unpacking linux-modules-6.17.0-061700rc4-generic (6.17.0-061700rc4.202508312336) ...
Setting up linux-modules-6.17.0-061700rc4-generic (6.17.0-061700rc4.202508312336) ...
+ dpkg -i /tmp/linux-image-unsigned-6.17.0-061700rc4-generic_6.17.0-061700rc4.202508312336_amd64.deb
Selecting previously unselected package linux-image-unsigned-6.17.0-061700rc4-generic.
(Reading database ... 148373 files and directories currently installed.)
Preparing to unpack .../linux-image-unsigned-6.17.0-061700rc4-generic_6.17.0-061700rc4.202508312336_amd64.deb ...
Unpacking linux-image-unsigned-6.17.0-061700rc4-generic (6.17.0-061700rc4.202508312336) ...
Setting up linux-image-unsigned-6.17.0-061700rc4-generic (6.17.0-061700rc4.202508312336) ...
I: /boot/vmlinuz.old is now a symlink to vmlinuz-5.19.0-21-generic
I: /boot/vmlinuz is now a symlink to vmlinuz-6.17.0-061700rc4-generic
I: /boot/initrd.img is now a symlink to initrd.img-6.17.0-061700rc4-generic
Processing triggers for linux-image-unsigned-6.17.0-061700rc4-generic (6.17.0-061700rc4.202508312336) ...
/etc/kernel/postinst.d/dracut:
dracut: Generating /boot/initrd.img-6.17.0-061700rc4-generic
dracut: Disabling early microcode, because kernel does not support it. CONFIG_MICROCODE_[AMD|INTEL]!=y
/etc/kernel/postinst.d/zz-runlilo:
Warning: Not updating LILO; /etc/lilo.conf not found!
/etc/kernel/postinst.d/zz-update-grub:
Sourcing file `/etc/default/grub'
Sourcing file `/etc/default/grub.d/init-select.cfg'
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-6.17.0-061700rc4-generic
Found initrd image: /boot/initrd.img-6.17.0-061700rc4-generic
Found linux image: /boot/vmlinuz-5.19.0-21-generic
Found initrd image: /boot/initrd.img-5.19.0-21-generic
Found memtest86+ image: /boot/memtest86+.elf
Found memtest86+ image: /boot/memtest86+.bin
Warning: os-prober will not be executed to detect other bootable partitions.
Systems on them will not be added to the GRUB boot configuration.
Check GRUB_DISABLE_OS_PROBER documentation entry.
done
+ dpkg -i /tmp/linux-headers-6.17.0-061700rc4_6.17.0-061700rc4.202508312336_all.deb
Selecting previously unselected package linux-headers-6.17.0-061700rc4.
(Reading database ... 148377 files and directories currently installed.)
Preparing to unpack .../linux-headers-6.17.0-061700rc4_6.17.0-061700rc4.202508312336_all.deb ...
Unpacking linux-headers-6.17.0-061700rc4 (6.17.0-061700rc4.202508312336) ...
Setting up linux-headers-6.17.0-061700rc4 (6.17.0-061700rc4.202508312336) ...
+ echo ''

+ grub-set-default 0
==============================================================

> reboot

...

> root@ubuntu:~# cat /boot/efi/EFI/ubuntu/grub.cfg
search.fs_uuid 8f154a69-ab4f-4c46-b99b-d06119bcfd0f root hd0,gpt3
set prefix=($root)'/boot/grub'
configfile $prefix/grub.cfg

> root@ubuntu:~# grep -i -e 'menuentry ' -e '/boot' /boot/grub/grub.cfg|head -n3
menuentry 'Ubuntu' --class ubuntu --class gnu-linux --class gnu --class os $menuentry_id_option 'gnulinux-simple-8f154a69-ab4f-4c46-b99b-d06119bcfd0f' {
        linux   /boot/vmlinuz-6.17.0-061700rc4-generic root=UUID=8f154a69-ab4f-4c46-b99b-d06119bcfd0f ro find_preseed=/preseed.cfg auto noprompt priority=critical locale=en_US quiet splash $vt_handoff
        initrd  /boot/initrd.img-6.17.0-061700rc4-generic

> root@ubuntu:~# uname -r
6.17.0-061700rc4-generic

==============================================================
#end
==============================================================

#Пакет linux-headers-6.17.0-061700rc4-generic_6.17.0-061700rc4.202508312336_amd64.deb не встал на Ubuntu 22.10:

    dpkg: dependency problems prevent configuration of linux-headers-6.17.0-061700rc4-generic:
    linux-headers-6.17.0-061700rc4-generic depends on libc6 (>= 2.38); however:
    Version of libc6:amd64 on system is 2.36-0ubuntu4.
    linux-headers-6.17.0-061700rc4-generic depends on libelf1t64 (>= 0.144); however:
    Package libelf1t64 is not installed.
    linux-headers-6.17.0-061700rc4-generic depends on libssl3t64 (>= 3.0.0); however:
    Package libssl3t64 is not installed.

Но тем не менее, как видно в листинге выше, ОС на новом ядре запустилась.
Можно, плиз, небольшое пояснение относително того что не поставилось (роль этого пакета) и где это гипотетически может выстрелить, если задумать всерьез такое эксплуатировать?
