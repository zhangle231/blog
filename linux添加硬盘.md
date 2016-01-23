# linuxÃÌº””≤≈Ã

## ≤Ω÷Ë

'''

[root@leo ~]# fdisk -l

Disk /dev/sdc: 8589 MB, 8589934592 bytes
255 heads, 63 sectors/track, 1044 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00000000

[root@leo ~]# pvcreate /dev/sdc
  Physical volume "/dev/sdc" successfully created

[root@leo ~]# pvdisplay /dev/sdc
  "/dev/sdc" is a new physical volume of "8.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdc
  VG Name
  PV Size               8.00 GiB
  Allocatable           NO
  PE Size               0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               3CmXbu-y4m0-NGhZ-NDEn-gkTb-yWgL-53Z62S

[root@leo ~]# vgdisplay
  --- Volume group ---
  VG Name               vg_leo
  System ID
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  5
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               27.50 GiB
  PE Size               4.00 MiB
  Total PE              7041
  Alloc PE / Size       6530 / 25.51 GiB
  Free  PE / Size       511 / 2.00 GiB
  VG UUID               MR5Ixq-KtNM-EFAG-c471-QLSu-KHCL-h51aNH

  [root@leo ~]# vgextend vg_leo /dev/sdc
  Volume group "vg_leo" successfully extended

  [root@leo ~]# vgdisplay
  --- Volume group ---
  VG Name               vg_leo
  System ID
  Format                lvm2
  Metadata Areas        3
  Metadata Sequence No  6
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                3
  Act PV                3
  VG Size               35.50 GiB
  PE Size               4.00 MiB
  Total PE              9088
  Alloc PE / Size       6530 / 25.51 GiB
  Free  PE / Size       2558 / 9.99 GiB
  VG UUID               MR5Ixq-KtNM-EFAG-c471-QLSu-KHCL-h51aNH

  [root@leo ~]# lvdisplay
  --- Logical volume ---
  LV Path                /dev/vg_leo/lv_root
  LV Name                lv_root
  VG Name                vg_leo
  LV UUID                Szu9Fo-kL4t-jq93-CjTV-CIxO-CGUO-aVbeXk
  LV Write Access        read/write
  LV Creation host, time leo, 2015-12-18 21:36:28 +0800
  LV Status              available
  # open                 1
  LV Size                24.71 GiB
  Current LE             6326
  Segments               2
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0

  
[root@leo ~]# lvextend -L+8G /dev/vg_leo/lv_root
  Extending logical volume lv_root to 32.71 GiB
  Logical volume lv_root successfully resized

  [root@leo ~]# lvdisplay /dev/vg_leo/lv_root
  --- Logical volume ---
  LV Path                /dev/vg_leo/lv_root
  LV Name                lv_root
  VG Name                vg_leo
  LV UUID                Szu9Fo-kL4t-jq93-CjTV-CIxO-CGUO-aVbeXk
  LV Write Access        read/write
  LV Creation host, time leo, 2015-12-18 21:36:28 +0800
  LV Status              available
  # open                 1
  LV Size                32.71 GiB
  Current LE             8374
  Segments               3
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0

  [root@leo ~]# resize2fs /dev/vg_leo/lv_root
resize2fs 1.41.12 (17-May-2010)
Filesystem at /dev/vg_leo/lv_root is mounted on /; on-line resizing required
old desc_blocks = 2, new_desc_blocks = 3
Performing an on-line resize of /dev/vg_leo/lv_root to 8574976 (4k) blocks.
The filesystem on /dev/vg_leo/lv_root is now 8574976 blocks long.

[root@leo ~]# df -h
Filesystem                  Size  Used Avail Use% Mounted on
/dev/mapper/vg_leo-lv_root   33G   25G  6.5G  79% /
tmpfs                       499M     0  499M   0% /dev/shm
/dev/sda1                   485M   33M  427M   8% /boot

'''