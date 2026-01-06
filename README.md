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
#              02-03 Работа с LVM
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
> mv /home_old/* /  
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
>  Delaying merge since origin is open.
>  Merging of snapshot datavg/homefs_snap will occur on next activation of datavg/homefs.

#фс отмонтирована. Деактивировать LV тоже не удаётся:  
> root@ubuntu:~# lvchange /dev/datavg/homefs -an  
>  Logical volume datavg/homefs contains a filesystem in use.  

#в mount тоже ничего такого  
> root@ubuntu:~# mount|grep home  
> root@ubuntu:~#  

#Не знаю что можно еще сделать, перезагружаюсь...  

> reboot  
> ...  
> mount /home  

#Проверяем наличие/отсутствие файлов созданных до/после снепшота  
> root@ubuntu:~# cat /home/mike/test  
> megafile  
> root@ubuntu:~# cat /home/mike/test2  
> cat: /home/mike/test2: No such file or directory  
> root@ubuntu:~#  


########################################################  
#              05 ZFS
########################################################  
#Определить алгоритм с наилучшим сжатием  

> mkdir -p /zpool/pool{1..4}  
> root@ubuntu:~# wget -P /zpool/*/pglog.log https://gutenberg.org/cache/epub/2600/pg2600.converter.log   

----------------------------------------
#recordsize=128k (default), sector - 4k  

> recordsize=128k  
> zpool create -o ashift=12 -O compression=zstd -O recordsize=${recordsize} -R /zpool pool1 /dev/sda  
> zpool create -o ashift=12 -O compression=lz4 -O recordsize=${recordsize} -R /zpool pool2 /dev/sdb  
> zpool create -o ashift=12 -O compression=zle -O recordsize=${recordsize} -R /zpool pool3 /dev/sdc  
> zpool create -o ashift=12 -O compression=gzip-9 -O recordsize=${recordsize} -R /zpool pool4 /dev/sdd  

> find /zpool/ -maxdepth 1 -type d -exec cp -r /tmp/pg2600.converter.log {}/ \;  

> root@ubuntu:~# zfs get all|grep compress|grep -v ref  
> pool1  compressratio         3.35x                      -  
> pool1  compression           zstd                       local  
> pool2  compressratio         2.17x                      -  
> pool2  compression           lz4                        local  
> pool3  compressratio         1.00x                      -  
> pool3  compression           zle                        local  
> pool4  compressratio         3.48x                      -  
> pool4  compression           gzip-9                     local  

> zpool destroy pool1  
> zpool destroy pool2  
> zpool destroy pool3  
> zpool destroy pool4  

----------------------------------------
#recordsize=1M, sector - 4k  

> recordsize=1M  
> zpool create -o ashift=12 -O compression=zstd -O recordsize=${recordsize} -R /zpool pool1 /dev/sda  
> zpool create -o ashift=12 -O compression=lz4 -O recordsize=${recordsize} -R /zpool pool2 /dev/sdb  
> zpool create -o ashift=12 -O compression=zle -O recordsize=${recordsize} -R /zpool pool3 /dev/sdc  
> zpool create -o ashift=12 -O compression=gzip-9 -O recordsize=${recordsize} -R /zpool pool4 /dev/sdd  

> root@ubuntu:~# find /zpool/ -maxdepth 1 -type d -exec cp -r /tmp/pg2600.converter.log {}/ \;  

> root@ubuntu:~# zfs get all|grep compress|grep -v ref  
> pool1  compressratio         4.01x                      -  
> pool1  compression           zstd                       local  
> pool2  compressratio         2.29x                      -  
> pool2  compression           lz4                        local  
> pool3  compressratio         1.00x                      -  
> pool3  compression           zle                        local  
> pool4  compressratio         3.79x                      -  
> pool4  compression           gzip-9                     local  

> zpool destroy pool1  
> zpool destroy pool2  
> zpool destroy pool3  
> zpool destroy pool4  


----------------------------------------
#recordsize=128k (default), sector - 512b  

> recordsize=128k  
> ashift=9  
> zpool create -o ashift=${ashift} -O compression=zstd -O recordsize=${recordsize} -R /zpool pool1 /dev/sda  
> zpool create -o ashift=${ashift} -O compression=lz4 -O recordsize=${recordsize} -R /zpool pool2 /dev/sdb  
> zpool create -o ashift=${ashift} -O compression=zle -O recordsize=${recordsize} -R /zpool pool3 /dev/sdc  
> zpool create -o ashift=${ashift} -O compression=gzip-9 -O recordsize=${recordsize} -R /zpool pool4 /dev/sdd  

> find /zpool/ -maxdepth 1 -type d -exec cp -r /tmp/pg2600.converter.log {}/ \;  

> zfs get all|grep compress|grep -v ref  
> pool1  compressratio         3.20x                      -  
> pool1  compression           zstd                       local  
> pool2  compressratio         2.00x                      -  
> pool2  compression           lz4                        local  
> pool3  compressratio         1.00x                      -  
> pool3  compression           zle                        local  
> pool4  compressratio         3.28x                      -  
> pool4  compression           gzip-9                     local  

#Делаем вывод, что фаворит зависит от множества факторов :)  Голосую за zstd



#Импортировать ФС, проверить статус
> wget -O archive.tar.gz --no-check-certificate 'https://drive.usercontent.google.com/download?id=1MvrcEp-WgAQe57aDEzxSRalPAwbNN1Bb&export=download' 
> tar -xzvf archive.tar.gz
> zpool import -d zpoolexport/ otus  
> zfs umount otus/hometask2  
> zfs umount otus  
> zfs set mountpoint=/zpool/otus otus  
> zfs mount otus  
> zfs mount otus/hometask2  

> root@ubuntu:/zpool/pool3# df -Th|grep otus  
> otus           zfs    350M  128K  350M   1% /zpool/otus  
> otus/hometask2 zfs    352M  2,0M  350M   1% /zpool/otus/hometask2  

> root@ubuntu:/# zpool status otus
>   pool: otus
>   state: ONLINE
> status: Some supported and requested features are not enabled on the pool.
>         The pool can still be used, but some features are unavailable.
> action: Enable all features using 'zpool upgrade'. Once this is done,
>         the pool may no longer be accessible by software that does not support
>         the features. See zpool-features(7) for details.
> config:
> 
>         NAME                    STATE     READ WRITE CKSUM
>         otus                    ONLINE       0     0     0
>           mirror-0              ONLINE       0     0     0
>             /zpoolexport/filea  ONLINE       0     0     0
>             /zpoolexport/fileb  ONLINE       0     0     0


#Работа со снапшотом
> wget -O otus_task2.file --no-check-certificate 'https://drive.usercontent.google.com/download?id=1wgxjih8YZ-cqLqaZVa0lA3h3Y029c3oI&export=download'  

> root@ubuntu:/# file /otus_task2.file  
> /otus_task2.file: ZFS snapshot (little-endian machine), version 17, type: ZFS, destination GUID: FFFFFFB5 FFFFFFB7 FFFFFFB8 78 35 30 FFFFFF92 FFFFFFFC, name: 'otus/test@today'  

> zfs receive otus/hometask2_test@snap1 < /otus_task2.file  
> root@ubuntu:/# find /zpool/otus/hometask2_test/ -type f -name '*secret*'  
> /zpool/otus/hometask2_test/task1/file_mess/8.2 El secreto de sus ojos (2009).mp4  
> /zpool/otus/hometask2_test/task1/file_mess/secret_message  
>
> root@ubuntu:/# cat /zpool/otus/hometask2_test/task1/file_mess/secret_message  
> https://otus.ru/lessons/linux-hl/  



########################################################  
#              06 NFS, FUSE
########################################################  

#Server:  
> root@ubuntu:\~# apt install nfs-kernel-server nfs-common  
> root@ubuntu:\~# mkdir -p /nfsroot/nfs1/upload  

> root@ubuntu:\~# chown -R mike:mike /nfsroot/  
> root@ubuntu:\~# chown -R nobody:nogroup /nfsroot/nfs1/upload  
> root@ubuntu:\~# chmod -R 0755 /nfsroot/  
> root@ubuntu:\~# chmod -R 0777 /nfsroot/nfs1/upload  
> 
> root@ubuntu:\~# echo '/nfsroot/nfs1 192.168.206.137(rw,root_squash,sync,nohide)' >> /etc/exports  
> root@ubuntu:\~# systemctl reload nfs-server.service  



#Client:  
> root@debian:\~# apt install nfs-common  
> root@debian:\~# mkdir /nfs1  
> root@debian:\~# echo '192.168.206.130:/nfsroot/nfs1       /nfs1    nfs     rw,hard,intr,lock,noexec,nosuid,nfsvers=3,proto=tcp,retrans=10,x-systemd.mount-timeout=15s,auto,_netdev     0       0' >> /etc/fstab  
> root@debian:\~# systemctl daemon-reload  
> root@debian:\~# mount /nfs1  
> root@debian:\~# df -Th|grep nfs  
> 192.168.206.130:/nfsroot/nfs1 nfs        39G   15G   22G  41% /nfs1  

> root@debian:\~# echo hello > /nfs1/test  
> -bash: /nfs1/test: Permission denied  
> root@debian:\~# echo hello > /nfs1/upload/test  
> root@debian:\~#  
> root@debian:\~# reboot  

#Server  
> root@ubuntu:/nfsroot/nfs1# showmount -a  
> All mount points on ubuntu:  
> 192.168.206.137:/nfsroot/nfs1  



########################################################  
#      08 Загрузка системы, работа с загрузчиком
########################################################  
#Включить отображение меню Grub.  
(grep -qixE '^(#|[[:blank:]]*)(GRUB_TIMEOUT_STYLE)([[:blank:]]*|)(=)([[:blank:]]*|)(.*)' /etc/default/grub && sed -r -i -e 's/^(#|[[:blank:]]*)(GRUB_TIMEOUT_STYLE)([[:blank:]]*|)(=)([[:blank:]]*|)(.*)/\1\2\3\4\5menu/' /etc/default/grub) || echo GRUB_TIMEOUT_STYLE=menu >> /etc/default/grub  
(grep -qixE '^(#|[[:blank:]]*)(GRUB_TIMEOUT)([[:blank:]]*|)(=)([[:blank:]]*|)(.*)' /etc/default/grub && sed -r -i -e 's/^(#|[[:blank:]]*)(GRUB_TIMEOUT)([[:blank:]]*|)(=)([[:blank:]]*|)(.*)/\1\2\3\4\510/' /etc/default/grub) || echo GRUB_TIMEOUT=10 >> /etc/default/grub  

#Готовимся к переименованию LV  
new_root_lvname=roofs_renamed  
new_root_vgname=sysvg  
dm_device=$(readlink -f $(df /|tail -n1|awk '{print $1}'))  
mapper_olddev=$(df /|tail -n1|awk '{print $1}')  

#Узнаем vg+lv на котором размещен / (root)  
while IFS=$'/' read -a line;do  
      if [[ -n $(lvs -a -S "lv_name=${line[3]} && vg_name=${line[2]}") ]];then  
            old_vgname="${line[2]}"  
            old_lvname="${line[3]}"  
            break   
      fi 
done <<< $(find -L /dev -mindepth 2 -maxdepth 2 -samefile $(df /|tail -n1|awk '{print $1}') 2>/dev/null|grep -v -e '/dev/mapper')  

#Выполняем 
vgrename ${old_vgname} ${new_root_vgname}  
lvrename /dev/${new_root_vgname}/${old_lvname} ${new_root_lvname}  
if [[ "$?" == "0" ]];then  
      new_mapper_device=$(find -L /dev/mapper -samefile  $dm_device)  
      sed -i -e "s~${old_vgname}~${new_root_vgname}~" /etc/fstab  
      sed -i -r -e "s~([[:blank:]*]|)([^#].*)([[:blank:]*])(/)([[:blank:]*])(.*)~\1\\${new_mapper_device}\3\4\5\6~" /etc/fstab  
      sed -i -e "s~${mapper_olddev}~${new_mapper_device}~" /boot/grub/grub.cfg  

      update-initramfs -u  
fi  

#Проверяем корректность загрузки  
reboot  



########################################################  
#      09 Инициализация системы. Systemd  
########################################################  
  
####################################################    
 #1. Написать service, который будет раз в 30 секунд мониторить лог на предмет наличия ключевого слова (файл лога и ключевое слово должны задаваться в /etc/default).  
#Скрипт для развёртывания:  
  
> #Конфиг-файл для службы  
> cat << EOF > /etc/default/watchlog  
> wrd1=fail  
> log=/var/log/auth.log  
> EOF  
>   
> #Исполняемая часть службы  
> cat << EOF > /opt/watchlog.sh  
> #!/bin/bash  
> wrd1=\$1  
> log=\$2  
>   
> #grep -qi fail \${log} && echo logger "\$(date): Achtung! Keyword has been found in log."  
> #Скучно, будем сразу писать найденную строку по ключевому слову!  
>   
> grep -i \${wrd1} \${log}|while read -r line;do  
>         logger "\$(date): Achtung! Keyword has been found in log. Found in string: \$line"  
> done  
> EOF  
>   
>   
> chmod +x /opt/watchlog.sh  
>   
> #Создадим службу и таймер  
> cat << EOF > /etc/systemd/system/watchlog.service  
> [Unit]  
> Description=Log checker  
>   
> [Service]  
> Type=oneshot  
> EnvironmentFile=/etc/default/watchlog  
> ExecStart=/opt/watchlog.sh \$wrd1 \$log  
> EOF  
>   
> cat << EOF > /etc/systemd/system/watchlog.timer  
> [Unit]  
> Description=Run watchlog script every 5 second  
>   
> [Timer]  
> #Run every 5 second  
> OnUnitActiveSec=5  
> Unit=watchlog.service  
>   
> [Install]  
> WantedBy=multi-user.target  
> EOF  
>   
>   
> systemctl daemon-reload  
> systemctl enable watchlog.timer  
> systemctl start watchlog.timer  
> exit  
>   


#Приводим в действие скрипт установки и настройки лог-чекера:  
root@ubuntu:~# date  
> Вс 12 окт 2025 19:47:42 MSK  
> root@ubuntu:~# /tst.sh;sleep 20;tail -n 15 /var/log/syslog  
> Oct 12 19:47:10 ubuntu systemd[1]: Finished Log checker.  
> Oct 12 19:47:25 ubuntu systemd[1]: Starting Log checker...  
> Oct 12 19:47:25 ubuntu root: Вс 12 окт 2025 19:47:25 MSK: Achtung! Keyword has been found in log. Found in string: Oct 12 18:51:40 ubuntu sshd[7296]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.206.1  user=root  
> Oct 12 19:47:25 ubuntu root: Вс 12 окт 2025 19:47:25 MSK: Achtung! Keyword has been found in log. Found in string: Oct 12 18:51:43 ubuntu sshd[7296]: Failed password for root from 192.168.206.1 port 56289 ssh2  
> Oct 12 19:47:25 ubuntu root: Вс 12 окт 2025 19:47:25 MSK: Achtung! Keyword has been found in log. Found in string: Oct 12 19:07:18 ubuntu dbus-daemon[3349]: [system] Failed to activate service 'org.bluez': timed out (service_start_timeout=25000ms)  
> Oct 12 19:47:25 ubuntu systemd[1]: watchlog.service: Deactivated successfully.  
> Oct 12 19:47:25 ubuntu systemd[1]: Finished Log checker.  
> Oct 12 19:47:44 ubuntu systemd[1]: Starting Log checker...  
> Oct 12 19:47:44 ubuntu systemd[1]: Reloading.  
> Oct 12 19:47:44 ubuntu root: Вс 12 окт 2025 19:47:44 MSK: Achtung! Keyword has been found in log. Found in string: Oct 12 18:51:40 ubuntu sshd[7296]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.206.1  user=root  
> Oct 12 19:47:44 ubuntu root: Вс 12 окт 2025 19:47:44 MSK: Achtung! Keyword has been found in log. Found in string: Oct 12 18:51:43 ubuntu sshd[7296]: Failed password for root from 192.168.206.1 port 56289 ssh2  
> Oct 12 19:47:44 ubuntu root: Вс 12 окт 2025 19:47:44 MSK: Achtung! Keyword has been found in log. Found in string: Oct 12 19:07:18 ubuntu dbus-daemon[3349]: [system] Failed to activate service 'org.bluez': timed out (service_start_timeout=25000ms)  
> Oct 12 19:47:44 ubuntu systemd[1]: watchlog.service: Deactivated successfully.  
> Oct 12 19:47:44 ubuntu systemd[1]: Finished Log checker.  
> Oct 12 19:47:44 ubuntu systemd[1]: Reloading.  
root@ubuntu:~#  
  
####################################################    
 #2. Установить spawn-fcgi и создать unit-файл (spawn-fcgi.sevice) с помощью переделки init-скрипта    
#Служба    
  root@ubuntu:~# systemctl cat spawn-fcgi.service    
> #/etc/systemd/system/spawn-fcgi.service    
> [Unit]    
> Description=Run php-cgi as app server    
>   
>   
> [Service]    
> Type=simple    
> PIDFile=/var/run/php_cgi.pid    
> EnvironmentFile=/etc/default/phpfastcgi    
> ExecStart=/usr/bin/spawn-fcgi -a ${server_ip} -p ${server_port} -u ${server_user} -g ${server_group} -P ${pidfile} -C ${server_childs} -f ${php_cgi}    
>   
> [Install]    
> WantedBy=multi-user.target    
  root@ubuntu:~#    
>   
#Конфиг службы    
root@ubuntu:~# cat /etc/default/phpfastcgi    
> spawnfcgi="/usr/bin/spawn-fcgi"    
> php_cgi="/usr/bin/php-cgi"    
> prog=$(basename $php_cgi)    
> server_ip=127.0.0.1    
> server_port=9000    
> server_user=www-data    
> server_group=www-data    
> server_childs=5    
> pidfile="/var/run/php_cgi.pid"    
> socket="/var/run/php-fcgi.sock"    
>   
> root@ubuntu:~# cat /etc/default/phpfastcgi    
> spawnfcgi="/usr/bin/spawn-fcgi"    
> php_cgi="/usr/bin/php-cgi"    
> prog=$(basename $php_cgi)    
> server_ip=127.0.0.1    
> server_port=9000    
> server_user=www-data    
> server_group=www-data    
> server_childs=5    
> pidfile="/var/run/php_cgi.pid"    
> socket="/var/run/php-fcgi.sock"    

#Запускаем службу    
root@ubuntu:~# systemctl daemon-reload;systemctl restart spawn-fcgi.service    
#Проверяем статус    
root@ubuntu:~# systemctl status spawn-fcgi.service    
> ● spawn-fcgi.service - Run php-cgi as app server    
     > Loaded: loaded (/etc/systemd/system/spawn-fcgi.service; disabled; preset: enabled)    
     > Active: active (running) since Sun 2025-10-12 20:29:25 MSK; 5s ago    
   > Main PID: 21403 (php-cgi)    
      > Tasks: 6 (limit: 2181)    
     > Memory: 7.4M    
        > CPU: 6ms    
     > CGroup: /system.slice/spawn-fcgi.service    
             > ├─21403 /usr/bin/php-cgi    
             > ├─21404 /usr/bin/php-cgi    
             > ├─21405 /usr/bin/php-cgi      
             > ├─21406 /usr/bin/php-cgi  
             > ├─21407 /usr/bin/php-cgi    
             > └─21408 /usr/bin/php-cgi    
>   
> окт 12 20:29:25 ubuntu systemd[1]: Started Run php-cgi as app server.    
> окт 12 20:29:25 ubuntu spawn-fcgi[21402]: spawn-fcgi: child spawned successfully: PID: 21403    
>   
> #Убеждаемся что порт прослушивается    
  root@ubuntu:~# netstat -tulpn|grep 9000    
> tcp        0      0 127.0.0.1:9000          0.0.0.0:*               LISTEN      21403/php-cgi    
root@ubuntu:~#    
  
####################################################  
 #3. Доработать unit-файл Nginx (nginx.service) для запуска нескольких инстансов сервера с разными конфигурационными файлами одновременно.    
  
root@ubuntu:/etc/nginx# systemctl status nginx@inst*    
> ● nginx@inst2.service - A high performance web server and a reverse proxy server    
>      Loaded: loaded (/etc/systemd/system/nginx@.service; disabled; preset: enabled)    
>      Active: active (running) since Sun 2025-10-12 21:01:29 MSK; 47s ago    
>        Docs: man:nginx(8)    
>    Main PID: 3010 (nginx)    
      Tasks: 3 (limit: 2181)    
>      Memory: 3.1M    
        CPU: 9ms    
     CGroup: /system.slice/system-nginx.slice/nginx@inst2.service    
>              ├─3010 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on; -c /etc/nginx/nginx-inst2.conf"    
             ├─3011 "nginx: worker process"    
             └─3012 "nginx: worker process"    
  
> окт 12 21:01:29 ubuntu systemd[1]: Starting A high performance web server and a reverse proxy server...    
> окт 12 21:01:29 ubuntu systemd[1]: Started A high performance web server and a reverse proxy server.    
  
> ● nginx@inst1.service - A high performance web server and a reverse proxy server    
>      Loaded: loaded (/etc/systemd/system/nginx@.service; disabled; preset: enabled)    
>      Active: active (running) since Sun 2025-10-12 21:01:27 MSK; 48s ago    
>        Docs: man:nginx(8)    
>    Main PID: 3003 (nginx)    
      Tasks: 3 (limit: 2181)    
>      Memory: 3.1M    
        CPU: 9ms    
     CGroup: /system.slice/system-nginx.slice/nginx@inst1.service    
>              ├─3003 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on; -c /etc/nginx/nginx-inst1.conf"    
             ├─3004 "nginx: worker process"    
             └─3005 "nginx: worker process"    
  
> окт 12 21:01:27 ubuntu systemd[1]: Starting A high performance web server and a reverse proxy server...    
> окт 12 21:01:27 ubuntu systemd[1]: Started A high performance web server and a reverse proxy server.    
  
root@ubuntu:/etc/nginx# netstat -tulpn|grep -i 808    
> tcp        0      0 0.0.0.0:8080            0.0.0.0:*               LISTEN      3003/nginx: master    
> tcp        0      0 0.0.0.0:8081            0.0.0.0:*               LISTEN      3010/nginx: master    


########################################################  
#      10 Bash 
########################################################  
#Пишем скрипт

#!/usr/bin/env bash
scriptfile=$(basename "${0}")
dup_run_count=$(ps -C ${scriptfile} --no-headers --format pid)
dup_run_count=$(echo ${dup_run_count}|wc -w)
if [[ "${dup_run_count}" -gt "1" ]];then exit;fi

#Variables
service=nginx
service_start="$(date +'%s' -d "$(systemctl show --property ExecMainStartTimestamp ${service}.service|awk '{print $2,$3,$4}')")"
service_start_human="$(date -d @${service_start})"
log="/var/log/nginx/access.log"
err_log="/var/log/nginx/error.log"
tmp_log=/tmp/nginx_from_last_start
tmp_err_log=/tmp/nginx_from_last_start_err

AttClientAddresses=/tmp/ClientAddresses.log
AttTopPages=/tmp/TopPages.log
AttRecurnCodes=/tmp/RecurnCodes.log
AttNginxErrors=/tmp/NginxErrors.log
Recipient=vasya@neverdomain


#Generate tmp err-log file
while read -r line; do
    record_ts=$(date +'%s' -d "$(awk '{print $1,$2}'|sed 's~/~-~g')")
	if [[ "${record_ts}" -ge "${service_start}" ]];then
		echo ${line}
	fi
done < ${err_log} > ${tmp_err_log}

#Generate tmp log file
while read -r line; do
    record_ts=$(date +'%s' -d "$(echo ${line}|awk -F'[\[\]]' '{print $2}'|sed -r -e 's~/~-~g' -e 's~:~ ~')")
	if [[ "${record_ts}" -ge "${service_start}" ]];then
		echo ${line}
	fi
done < ${log} > ${tmp_log}

#Top client addresses
echo -en "\nTop client addresses:\n"
for addr in $(awk '{print $1}' ${tmp_log}|sort -u);do 
	echo $(grep "^${addr} " ${tmp_log}|wc -l) ${addr}
done|sort -n -k1 -r|tee ${AttClientAddresses}

#Top pages
echo -en "\nTop pages:\n"
for page in $(grep -oE 'GET.*' ${tmp_log}|awk '{print $2}'|sort -u);do 
	echo $(grep "GET ${page} " ${tmp_log}|wc -l) ${page}
done|sort -n -k1 -r|tee ${AttTopPages}

#Top return-codes
echo -en "\nTop return-codes:\n"
for code in $(awk '{print $9}' ${tmp_log}|sort -u);do 
	echo $(grep "\"[[:blank:]]*${code}[[:blank:]]*[0-9]*[[:blank:]]*\"" ${tmp_log}|wc -l) ${code}
done|sort -n -k1 -r|tee ${AttRecurnCodes}

#Errors
echo -en "\nErrors:\n"
cat ${tmp_err_log}|tee ${AttNginxErrors}

#Send
sendemail -f noreply@neverdomain -t ${Recipient} -u "Some Nginx tops" -m "Stats since ${service_start_human}" -a ${AttClientAddresses} -a ${AttTopPages} -a ${AttRecurnCodes} -a ${AttNginxErrors}

exit

#Вывод:
> root@ubuntu:~# ./script.sh
> 
> Top client addresses:
> 10 192.168.206.137
> 7 192.168.206.1
> 2 127.0.0.1
> 
> Top pages:
> 15 /
> 2 /hehe.html
> 1 /kek.html
> 1 /hehe
> 
> Top return-codes:
> 13 200
> 5 304
> 1 404
> 
> Errors:
> 2025/10/18 21:51:03 [notice] 3234#3234: using inherited sockets from "6;7;"
> root@ubuntu:~#

########################################################  
#      12 Управление процессами  
########################################################  

#!/bin/bash
for pfolder in $(ls -ld /proc/*/|grep -o '/proc/[0-9]*/$');do
        pid=$(basename ${pfolder} 2>/dev/null)
        user=$(stat -c '%U' ${pfolder} 2>/dev/null)
        cmdline=$(cat ${pfolder}/cmdline 2>/dev/null|tr -d '\0';echo -en '\n'|sed 's/--/ --/g')
        state=$(cat ${pfolder}/stat 2>/dev/null|awk '{print $3}')
        command=$(cat ${pfolder}/comm 2>/dev/null)
        exec=$(readlink -f /${pfolder}/exe 2>/dev/null)
        terminal=$((test -e "${pfolder}/fd/0" && readlink -f ${pfolder}/fd/0|sed 's~/dev/~~') || echo null)
        echo "${pid}~~~${user}~~~${terminal}~~~${state}~~~${command}~~~${cmdline}~~~"
done|sort -h -k1|column -t -s'~~~' -N 'pid,user,terminal,state,command,cmdline'




########################################################  
#      18 Vagrant 
########################################################  
Vagrant.configure("2") do |config|
	config.vm.define "homework" do |h|
		h.vm.hostname = "homework.localdomain"
			h.vm.box = "ubuntu/jammy64"
			#h.vm.box = "generic/debian11"
			h.vm.box_check_update = false
			h.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"


			h.vm.provider "virtualbox" do |vb|
			        vb.memory = "2048"
			        vb.cpus = "2"
					
					
							#Disks
					#Проверяем наличие нужного дискового контроллера
					#vm_id = vb.instance_variable_get(:@machine).id rescue nil
					#check_cmd = "VBoxManage showvminfo #{vm_id} --machinereadable | grep 'storagecontrollername.*SCSI Controller'"
					#controller_exists = system(check_cmd)
					#Добавляем контроллер если отсутствует
					#unless controller_exists
					#	vb.customize ["storagectl", :id, "--name", "SCSI", "--add", "scsi" ]
					#end
					
					#vb.customize ["storagectl", :id, "--name", "SCSI", "--add", "scsi" ]
					#Создаем файлы виртуальных дисков
					(2..4).each do |i|
						unless File.exist?("../disk#{i}.vdi")
							vb.customize ['createhd', '--filename', "../disk#{i}.vdi",  '--variant', 'Standard', '--size', '250']
						end
						#Подключаем диски к машине
						vb.customize ['storageattach', :id, '--storagectl', 'SCSI', '--port', "#{i}", '--device', 0, '--type', 'hdd', '--medium', "../disk#{i}.vdi"]
					end

			end
	

		    config.vm.provision "shell", inline: <<-SHELL
				disknum=0
				for disk in $(lsblk -o PATH,PTUUID|awk '{if ($2 == "") print $1}');do
					disknum=$((disknum+1))
					sudo mkfs.ext4 $disk
					uuid=""
					uuid=$(sudo blkid $disk --output value|head -n1)
					if ! [[ "${uuid}" == "" ]];then
						mkdir /disk${disknum}
						grep "UUID=${uuid} /disk${disknum}           ext4    defaults        0       1" /etc/fstab || echo "UUID=${uuid} /disk${disknum}           ext4    defaults        0       1" >> /etc/fstab
					fi
				done
				systemctl daemon-reload
				mount -a
				
				apt-get update
				apt-get install -y nginx
			SHELL


	  end
end


