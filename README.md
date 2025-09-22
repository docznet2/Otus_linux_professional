# Otus_linux_professional

########################################################
#              01 Обновление ядра системы
########################################################
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



########################################################
#              02 Работа с mdadm
########################################################


#raid

mdadm --create /dev/md0 -l 5 -n 3 /dev/sd[abc]
echo "DEVICE partitions" > /etc/mdadm/mdadm.conf
mdadm --detail --scan --verbose | awk '/ARRAY/ {print}' >> /etc/mdadm/mdadm.conf


#partitions

parted /dev/md0 mklabel gpt
parted /dev/md0 mkpart primary ext4 2MiB 258MiB
parted /dev/md0 mkpart primary ext4 258MiB 514MiB
parted /dev/md0 mkpart primary ext4 514MiB 770MiB
parted /dev/md0 mkpart primary ext4 770MiB 1026MiB
parted /dev/md0 mkpart primary ext4 1026MiB 1282MiB


#fs

mkfs.ext4 /dev/md0p1
mkfs.ext4 /dev/md0p2
mkfs.ext4 /dev/md0p3
mkfs.ext4 /dev/md0p4
mkfs.ext4 /dev/md0p5


#mounting

part=1;mkdir /fs${part};echo "/dev/md0p${part} /fs${part}        ext4    defaults 1       2" >> /etc/fstab
part=2;mkdir /fs${part};echo "/dev/md0p${part} /fs${part}        ext4    defaults 1       2" >> /etc/fstab
part=3;mkdir /fs${part};echo "/dev/md0p${part} /fs${part}        ext4    defaults 1       2" >> /etc/fstab
part=4;mkdir /fs${part};echo "/dev/md0p${part} /fs${part}        ext4    defaults 1       2" >> /etc/fstab
part=5;mkdir /fs${part};echo "/dev/md0p${part} /fs${part}        ext4    defaults 1       2" >> /etc/fstab
systemctl daemon-reload
mount /fs1
mount /fs2
mount /fs3
mount /fs4
mount /fs5


#add HS drive to array

mdadm /dev/md0 --add /dev/sdd


#degrade array 

root@ubuntu:~# mdadm /dev/md0 --fail /dev/sdd;cat /proc/mdstat;mdadm --detail /dev/md0
mdadm: set device faulty failed for /dev/sdd:  No such device
Personalities : [raid6] [raid5] [raid4]
md0 : active raid5 sda[5] sdc[3] sdb[1]
      2093056 blocks super 1.2 level 5, 512k chunk, algorithm 2 [3/3] [UUU]

unused devices: <none>
/dev/md0:
           Version : 1.2
     Creation Time : Wed Sep 17 19:44:13 2025
        Raid Level : raid5
        Array Size : 2093056 (2044.00 MiB 2143.29 MB)
     Used Dev Size : 1046528 (1022.00 MiB 1071.64 MB)
      Raid Devices : 3
     Total Devices : 3
       Persistence : Superblock is persistent

       Update Time : Fri Sep 19 17:58:13 2025
             State : clean
    Active Devices : 3
   Working Devices : 3
    Failed Devices : 0
     Spare Devices : 0

            Layout : left-symmetric
        Chunk Size : 512K

Consistency Policy : resync

              Name : ubuntu:0  (local to host ubuntu)
              UUID : 30c66efa:211ef961:4690b593:e5b2cb6e
            Events : 66

    Number   Major   Minor   RaidDevice State
       5       8        0        0      active sync   /dev/sda
       1       8       16        1      active sync   /dev/sdb
       3       8       32        2      active sync   /dev/sdc


...........

#rebuild comleted

root@ubuntu:~# mdadm --detail /dev/md0|grep -i -e update -e state;cat /proc/mdstat
       Update Time : Fri Sep 19 17:58:13 2025
             State : clean
    Number   Major   Minor   RaidDevice State
Personalities : [raid6] [raid5] [raid4]
md0 : active raid5 sda[5] sdc[3] sdb[1]
      2093056 blocks super 1.2 level 5, 512k chunk, algorithm 2 [3/3] [UUU]

unused devices: <none>


########################################################
#              02 Работа с LVM
########################################################

#live-cd сессия
#booting from installer CD Ubuntu 22.10
#Switching terminal to shell (Ctrl+Alt+F3)
#Logging in with login ubuntu and empty passwowrd
> sudo -i
> mount /dev/sde3 /mnt
> mkdir /newroot

#прицепляем к ВМ новый диск

> for i in /sys/class/scsi_host/*;do echo "- - -" > ${i}/scan;done
> pvcreate /dev/sdf
> vgcreate sysvg /dev/sdf
> lvcreate -n rootfs -L8G sysvg
> mkfs.ext4 /dev/mapper/sysvg-rootfs
> mount /dev/mapper/sysvg-rootfs /newroot
> rsync --exclude proc --progress -ar /mnt/ /newroot

#и получаю "No space left on device" ибо убунта с графикой была. Что ж, накину места, под PV подключал 20Gb диск.

> lvextend -r -l+100%FREE /dev/mapper/sysvg-rootfs
> rsync --exclude proc --progress -ar /mnt/ /newroot
> mkdir /newroot/proc

> mount --bind /proc /newroot/proc
> mount --bind /dev /newroot/dev
> mount --bind /sys /newroot/sys
> mount --bind /dev/sde2 /newroot/boot/efi
> chroot /newroot
> grub-mkconfig -o /boot/grub/grub.cfg

#Обнаруживаю что initramfs-tools не был установлен

> apt install initramfs-tools
> update-initramfs -u

#Правим fstab

> sed -i -r -e 's/([[:blank:]*]|)([^#].*)([[:blank:]*])(\/)([[:blank:]*])(.*)/\1\/dev\/mapper\/sysvg-rootfs\3\4\5\6' /etc/fstab

#Перезагружаемся... и понимаем, что 1st-stage grub (у меня BIOS инсталляция, как оказалось) ничего не знает о наших манипуляциях.
#Снова грузим инсталлер убунты
#Повторяем монтирования

> mv /newroot/boot /newroot/boot_backup
> mkdir /newroot/boot
> mount --bind /mnt/boot /newroot/boot
> mount /dev/sde2 /newroot/boot/efi
> chroot /newroot
> grub-mkconfig -o /boot/grub/grub.cfg
> update-initramfs -u
> exit
> reboot
...

#Тут я вспомнил, что существует еще grub-install и оставляю /boot на lvm, удаляю раздел sde3

> mv /boot_backup/* /boot/
> rm -rdf /boot_backup/
> parted /dev/sde rm 3

#Создаём еще один для lvm

> parted /dev/sde mkpart primary 541 21000MiB
> pvcreate /dev/sde3
> vgextend sysvg /dev/sde3
> pvmove /dev/sdf
> vgreduce sysvg /dev/sdf
> pvremove /dev/sdf

#Обновляем grub конфиги

> grub-install --target=i386-pc /dev/sde
> grub-mkconfig -o /boot/grub/grub.cfg

#Добавляю 2 новых диска под PV, делаю рескан

> for i in /sys/class/scsi_host/*;do echo "- - -" > ${i}/scan;done
> pvcreate /dev/sdh /dev/sdg
> vgcreate datavg /dev/sdg /dev/sdh

#Мувим home

> lvcreate -n homefs -L5G datavg
> mkfs.ext4 /dev/mapper/datavg-homefs
> mv /home /home_old
> echo "/dev/mapper/datavg-homefs /home               ext4    defaults 0       1" >> /etc/fstab
> mkdir /home
> systemctl daemon-reload && mount /home
> mv /home_old/* /home
> rm -d /home_old

#мувим var

> lvcreate -n varfs -L10G -m1 datavg
> mkfs.ext4 /dev/mapper/datavg-varfs
> init 1
> lsof +D /var
> systemctl stop systemd-journald
> mv /var /var_old
> mkdir /var
> echo "/dev/mapper/datavg-varfs /var             ext4    defaults 0       1" >> /etc/fstab
> systemctl daemon-reload && mount /var
> mv /var_old/* /var/
> rm -rdf /var_old
> init 5
> systemctl start systemd-journald


#Работа со снапшотами

> echo megafile > /home/mike/test
> lvcreate -L100M -s -n homefs_snap /dev/mapper/datavg-homefs
> rm /home/mike/test
> echo megafile2 > /home/mike/test2
> #Разрешаю рута по ssh, выхожу из текущей сесии, блокирующей home, подключаюсь под рутом
> umount /home

> lvconvert --merge /dev/mapper/datavg-homefs_snap

#и почему-то получаю:

  Delaying merge since origin is open.
  Merging of snapshot datavg/homefs_snap will occur on next activation of datavg/homefs.

#фс отмонтирована. Деактивировать LV тоже не удаётся:

root@ubuntu:~# lvchange /dev/datavg/homefs -an
  Logical volume datavg/homefs contains a filesystem in use.

#в mount тоже ничего такого

root@ubuntu:~# mount|grep home
root@ubuntu:~#

#Не знаю что можно еще сделать, перезагружаюсь...

> reboot
> ...
> mount /home

#Проверяем наличие/отсутствие файлов созданных до/после снепшота

root@ubuntu:~# cat /home/mike/test
megafile
root@ubuntu:~# cat /home/mike/test2
cat: /home/mike/test2: No such file or directory
root@ubuntu:~#

