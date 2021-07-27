# Ubuntu配置

## 挂在第二块硬盘

使用GParted对硬盘进行分区以后第二块硬盘就会在文件管理器中显示出来。Gparted可以通过以下指令安装。
```shell
sudo apt install gparted
```
## 开机启动慢
1. 查看系统启动慢的原因，通过指令`systemd-analyze blame`，分析启动耗时。
   
2. 编辑grub选项`sudo  vi /etc/defaults/grub`，在开机时打印开机日志，找到grub中的`GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"`，将其改为`GRUB_CMDLINE_LINUX_DEFAULT=""`
3. 更新grub:`update-grub2`
4. 重启电脑，可以看到当前系统需要找到第二块硬盘，但是当前的UUID是错误的。需要将第二块硬盘的UUID修正以下。
5. 通过`sudo blkid /dev/sdb`,即可获取到当前的UUID，修正以后，恢复grub设置，重新启动。
   ![get-dev-sdb-uuid](./pictures/blkid-dev-sdb.png)

## 安装zsh
