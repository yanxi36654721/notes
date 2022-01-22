# Ubuntu 20.04.3 LTS

## 安装
1. 准备
   * 4GB及以上U盘
   * [Rufus](https://rufus.ie/zh/)
   * [Ubuntu ISO](https://ubuntu.com/download/desktop)
2. 制作镜像U盘
   
    |选项|选择|
    |:---:|:---:|
    |设备|U盘名|
    |分区类型|MBR|
    |目标系统类型|BIOS|
    |文件系统|FAT32|
    |簇大小|16k|
3. 启动U盘
4. 选择第一项`Install Ubuntu`
5. 选择语言，安装。
6. 安装类型
   - 清空磁盘
   - 其他选项

        |大小|600MB|*|
        |:---:|:---:|:---:|
        |设备|/dev/sda1|/dev/sda2|
        |分区类型|EFI System|Filesystem|
        |内容|FAT(32bit)|Ext4(v1.0)|
        |挂载|/boot/sfi|文件系统根目录|
7. 创建用户名和密码，重启拔掉U盘。
## 环境
### 设置
**设置root用户密码**
1. ```sudo passwd root```
2. 输入密码，回车，重复
3. ```sudo gedit /etc/ssh/sshd_config```
4. 找到`#Authentication`，将`PermitRootLogin`参数和`PasswordAuthentication`参数修改为`yes`,去掉注释`#`，保存文件。  

**换源**
1. 备份```sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak```
2. ```sudo gedit /etc/apt/sources.list```
3. ```
    deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted
    deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted
    deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal universe
    deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates universe
    deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal multiverse
    deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates multiverse
    deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
    deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted
    deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security universe
    deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security multiverse
    ```
4. sudo apt update
5. sudo apt upgrade
### 软件
* 搜狗输入法 [官方教程](https://pinyin.sogou.com/linux/help.php)
* 
## 使用
gedit