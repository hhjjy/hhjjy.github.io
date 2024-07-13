1. 分區調整大小
![image](https://github.com/user-attachments/assets/2f13ebf4-c308-43fd-8af5-90afb061247d)
![image](https://github.com/user-attachments/assets/3524e32a-00d3-418a-a583-7cfc70491ec2)
![image](https://github.com/user-attachments/assets/3962b8bf-24ae-492b-bf91-fd79b7bed6c7)
![image](https://github.com/user-attachments/assets/0ca2d176-a779-4e29-87a4-726c3cfa3656)

![image](https://github.com/user-attachments/assets/4b5ed318-d404-45a4-945a-4a27ecf4a398)
![image](https://github.com/user-attachments/assets/cfd14e3d-9f7b-4ad2-a31d-bc5385e6ff1a)


2. 調整物理捲大小 
chunchun@chunchun:~$ lsblk
NAME                      MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
loop0                       7:0    0 130.1M  1 loop /snap/docker/2915
loop1                       7:1    0  74.2M  1 loop /snap/core22/1380
loop2                       7:2    0  38.8M  1 loop /snap/snapd/21759
sr0                        11:0    1  1024M  0 rom  
vda                       253:0    0    90G  0 disk 
├─vda1                    253:1    0     1M  0 part 
├─vda2                    253:2    0     2G  0 part /boot
└─vda3                    253:3    0    88G  0 part 
  └─ubuntu--vg-ubuntu--lv 252:0    0    30G  0 lvm  /
chunchun@chunchun:~$ sudo pvresize /dev/vda3
  Physical volume "/dev/vda3" changed
  1 physical volume(s) resized or updated / 0 physical volume(s) not resized
chunchun@chunchun:~$ sudo pvdisplay
  --- Physical volume ---
  PV Name               /dev/vda3
  VG Name               ubuntu-vg
  PV Size               <88.00 GiB / not usable 1.00 MiB
  Allocatable           yes 
  PE Size               4.00 MiB
  Total PE              22527
  Free PE               14848
  Allocated PE          7679
  PV UUID               nwMiNl-PckC-L1X6-suuc-kerS-4zeR-OlaWij

3. 調整邏輯捲大小

chunchun@chunchun:~$ sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
  Size of logical volume ubuntu-vg/ubuntu-lv changed from <30.00 GiB (7679 extents) to <88.00 GiB (22527 extents).
  Logical volume ubuntu-vg/ubuntu-lv successfully resized.
chunchun@chunchun:~$ sudo lvdisplay
  --- Logical volume ---
  LV Path                /dev/ubuntu-vg/ubuntu-lv
  LV Name                ubuntu-lv
  VG Name                ubuntu-vg
  LV UUID                wjF41R-VvhP-aZmO-BEBc-pAiu-1MzU-k5pmpt
  LV Write Access        read/write
  LV Creation host, time ubuntu-server, 2024-07-08 12:40:43 +0000
  LV Status              available
  # open                 1
  LV Size                <88.00 GiB
  Current LE             22527
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           252:0

4. 擴展檔案系統大小
![image](https://github.com/user-attachments/assets/d2868ee1-b209-4d2a-80a1-c967f16f70c9)




完整步驟範例
以下是完整的操作步驟：

擴展分區：

bash
複製程式碼
sudo fdisk /dev/vda

Command (m for help): p
Command (m for help): d
Partition number (1,2, default 3): 3
Command (m for help): n
Select (default p): p
Partition number (1-4, default 3): 3
First sector (保持默認起始位置): 4198400
Last sector (使用所有可用空間): 按 Enter
Command (m for help): t
Partition number (3): 3
Hex code (type L to list all codes): 8e
Command (m for help): w
重新掃描分區表：

bash
複製程式碼
sudo partprobe
擴展物理卷 (PV)：

bash
複製程式碼
sudo pvresize /dev/vda3
擴展邏輯卷 (LV)：

bash
複製程式碼
sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
擴展檔案系統：

對於 ext4：

bash
複製程式碼
sudo resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
對於 xfs：

bash
複製程式碼
sudo xfs_growfs /dev/mapper/ubuntu--vg-ubuntu--lv
檢查結果：

bash
複製程式碼
df -h
通過這些步驟，你應該能成功擴展虛擬機的磁碟空間至 80GB 並在虛擬機內使其生效。如果還有問題或需要進一步的幫助，請告知。