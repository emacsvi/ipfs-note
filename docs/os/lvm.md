# 3块硬盘合成一块硬盘挂载到现有系统中
[参考链接](https://www.cnblogs.com/yasmi/articles/4835644.html)
```bash
sudo apt-get install lvm2
sudo fdisk -l
# 先删除 m 是help
sudo fdisk /dev/sdb
d
w
sudo fdisk /dev/sdb
g
n
w
sudo fdisk /dev/sdb
p
t
L
31
w
# 创建物理分区
sudo pvcreate /dev/sda1 /dev/sdb1 /dev/sdc1
sudo pvdisplay

# 创建逻辑分区
sudo vgcreate extspace /dev/sda1 /dev/sdb1 /dev/sdc1

# 创建逻辑分区
sudo lvcreate --name home_ext -l100%FREE extspace
sudo lvdisplay

sudo mkfs.ext4 /dev/extspace/home_ext
# 挂载
sudo mkdir /data
sudo mount /dev/extspace/home_ext /data
sudo vim /etc/fstab
/dev/extspace/home_ext /data ext4 rw,noatime 0 0
```



# 增加swap内存

[ubuntu 18.10增加和设置Swap交换分区](https://juejin.im/post/5c2da2a151882566dc118af5)

```bash
swapon -s
# 创建一个swap文件
sudo dd if=/dev/zero of=/data/swap-yd.img bs=1G count=96
sudo chmod 600 /data/swap-yd.img
# 激活
sudo mkswap /data/swap-yd.img 
# 添加到系统中
sudo swapon /data/swap-yd.img

sudo vim /etc/fstab
// 在最后一行添加如下
/data/swap-yd.img       none     swap   sw       0      0
```

