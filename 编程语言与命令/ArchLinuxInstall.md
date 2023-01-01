## 安装
设置终端字体 <br>
`setfont /usr/share/kbd/consolefonts/LatGrkCyr-12x`

**网络** <br>

```
//有线网络
dhcpcd                              // 启动dhcpcd获取网络

//无线网络
iwctl                               // 第一步：进入环境

device list                         // 第二步：列出网卡设备

station wlan0 scan                  // 第三步：扫描网络，wlan0为无线网卡，wlan0 为无线网卡号

station wlan0 get-networks          // 第四步：列出扫描到的网络，wlan0 为无线网卡号

station wlan0 connect 网络名称      // 第五步：连接无线网络，wlan0 为无线网卡号

quit或exit                          // 退出
```
测试网络是否连接 <br>
`ping baidu.com`

更新系统时间 <br>
`timedatectl set-ntp true` <br>
查看时间状态 <br>
`timedatectl status`


查看磁盘 <br>
`fdisk -l`

**设置要分区的硬盘** <br>
`cfdisk /dev/sda`

1. 选择顺序 [GPT]，[New] 输入512M，此分区[Type]选择EFI System

2. Free space创建新分区 1G [Type]Linux swap

3. 剩余空间全部分配给 Linux filesystem

4. [Write]写入 一定要输入yes

5. [Quit]
> EFI文件系统 512M   
    linux swap 大于512N   
    根目录使用剩余空间

对各个分区进行格式化和挂载
```
mkfs.fat -F32 /dev/sda1         //EFI文件系统格式化为Fat32

mkfs.ext4 /dev/sda3             //根目录格式化为ext4

mkswap /dev/sda2                //使用mkswap将交换空间初始化

swapon /dev/sda2                //打开交换空间

mount /dev/sda3 /mnt            //将sda2挂载到更目录下

//创建其他挂载点并挂载到相应的分区

mkdir /mnt/boot                 //创建目录boot

mount /dev/sda1 /mnt/boot       //将sda1挂载到boot
```

修改pacman为国内源<br>`vim /etc/pacman.d/mirrorlist` <br>

如果文件内没有国内源就手动添加到顶部 <br>
```
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
```

安装基本软件包，Linux内核以及常规硬件的固件
```
pacstrap /mnt base base-devel linux linux-firmware vim dhcpcd
```

>可能会出现的问题`ERROR:Failed to install packages to new root` 或 `Error:openssl:signature from "pierre Schmitz <pierre@archlinux.org>" is marginal truat`
>>可以使用`pacman -Sy archlinux-keyring`来解决


`genfstab -U /mnt >> /mnt/etc/fstab` 生成fstab文件


`arch-chroot /mnt` Change root 到新安装的系统


```
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime         //设置时区

hwclock --systohc                                               //同步时间
```

`vim /etc/locale.gen` 移除需要的地区注释，删除en_US.UTF-8 UTF-8,zh_CN UTF-8 UTF-8的'#'符

`locale-gen` 生成locale讯息

`vim /etc/locale.conf` 创建locale.conf并编辑LANG
```
LANG=en_US.UTF8
```

`echo "Blackword" >> /etc/hostname` 创建hostname文件并添加主机名

`vim /etc/hosts` 添加到对应的信息到hosts
```
127.0.0.1 localhost
::1 localhost
127.0.1.1 Blackword.localdomain Blackword
```

`passwd` 设置root秘密

`pacman -S intel-ucode` intel处理器需要安装

`pacman -S os-prober` 硬盘存在其他操作系统的用户安装

`pacman -S grub efibootmgr` 安装grub efi启动管理工具

`grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB`生成GRUB EFI

`grub-mkconfig -o /boot/grub/grub.cfg`

笔记本电脑需要安装的工具<br>
`pacman -S iw wpa_supplicant dialog netctl`

`systemctl enable dhcpcd` 开启dhcpcd服务

`exit` 或者 <kbd>Ctrl</kbd>+<kbd>D</kbd> 退出chroot环境

`umount -R /mnt` 手动卸载被挂载的分区

`reboot`已经安装完成拔掉u盘或光驱重启即可进入系统