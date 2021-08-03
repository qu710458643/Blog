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

对于Linux开发者来说，最常用的工具就是shell，其负责与Linux内核进行交互提供了常用的查找、拷贝、重命名等常用的功能，Ubuntu下支持的shell工具可以通过`cat /etc/shells`查看：
```shell
/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/bin/dash
/usr/bin/dash
/bin/zsh
/usr/bin/zsh
```
zsh是一种专为交互使用而设计的shell，对Bourne shell做出了大量改进，同时加入了Bash、ksh及tcsh的某些功能。自2019年起，macOS的默认Shell已从Bash改为Zsh。其介绍参见[维基百科](https://zh.wikipedia.org/wiki/Z_shell),下面介绍其安装与配置。

### zsh安装
1. 安装zsh:`sudo apt install zsh`
2. 将zsh设置为用户的默认shell:`chsh -s /usr/bin/zsh`
3. 如果执行上述命令报错:`chsh: PAM: Authentication failure`,需要使用root权限更改是系统配置：`sudo vim /etc/passwd`
   ![更改用户bash](./pictures/change-user-bash.png)
4. 注销后重新打开shell，即可发现当前用户的默认shell已经修改为zsh
   
### 配置oh-my-zsh
参见[官网](https://ohmyz.sh/)
1. 安装oh-my-zsh需要使用git、curl或者wget: `sudo apt install curl`或者`sudo apt install wget`。
2. 安装oh-my-zsh：
   ```shell
   # via curl
   sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
   # via wget
   sh -c "$(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
   ```
3. 安装插件zsh-autosuggestions
   ```shell
   git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
   ```
4. 安装插件zsh-autosuggestions
   ```shell
   git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
   ```
5. 安装git-open
   ```shell
   git clone https://github.com/paulirish/git-open.git $ZSH_CUSTOM/plugins/git-open
   ```
6. 启用插件，编辑`.zshrc`:`vim ~/.zshrc`，找到`plugins`:
   ```shell
   plugins=(git zsh-autosuggestions zsh-syntax-highlighting git-open)
   ```
7. 更改主题为`agnoster`,编辑`.zshrc`:`vim ~/.zshrc`，找到`ZSH_THEME`:
   ```shell
   ZSH_THEME="agnoster"
   ```
8. 刷新配置，使上述配置生效：`source ~/.zshrc`

