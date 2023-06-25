Ubuntu 20.04.3 LTS
=================
## 1. 安装

1. 准备
   * 4GB及以上U盘
   * [Rufus](https://rufus.ie/zh/)
   * [Ubuntu ISO](https://ubuntu.com/download/desktop)
1. 制作镜像U盘
   
    |选项|选择|
    |:---:|:---:|
    |设备|U盘名|
    |分区类型|MBR|
    |目标系统类型|BIOS|
    |文件系统|FAT32|
    |簇大小|16k|
1. 启动U盘
2. 选择第一项`Install Ubuntu`
3. 选择语言，安装。
4. 安装类型
   - 清空磁盘
   - 其他选项

        |大小|600MB|*|
        |:---:|:---:|:---:|
        |设备|/dev/sda1|/dev/sda2|
        |分区类型|EFI System|Filesystem|
        |内容|FAT(32bit)|Ext4(v1.0)|
        |挂载|/boot/sfi|文件系统根目录|
5. 创建用户名和密码，重启拔掉U盘。
---
## 2. 环境
### 2.1 设置root用户密码
1. ```sudo passwd root```  
   切换root用户
2. 设置密码，回车，重复输入确认密码
3. ```su root```  
   切换root用户，输入刚才设置的密码
4. ```vi /etc/ssh/sshd_config```  
   修改ssh配置
5. 按`i`编辑文件
6. 找到`#PermitRootLogin`参数和`PasswordAuthentication`参数，将属性修改为`yes`,去掉注释`#`，按`Esc`输入`:wq`保存文件。  
  
### 2.2 设置中文环境
1. ```sudo apt install language-pack-zh-hans -y```  
   安装中文语言包
2. ```locale-gen zh_CN.UTF-8```  
   添加中文
3. ```vim /etc/default/locale```  
     编辑语言环境变量，覆盖粘贴以下内容
   ```
   LANG="zh_CN.UTF-8"
   LANGUAGE="zh_CN:zh:en_US:en"
   LC_NUMERIC="zh_CN.UTF-8"
   LC_TIME="zh_CN.UTF-8"
   LC_MONETARY="zh_CN.UTF-8"
   LC_PAPER="zh_CN.UTF-8"
   LC_IDENTIFICATION="zh_CN.UTF-8"
   LC_NAME="zh_CN.UTF-8"
   LC_ADDRESS="zh_CN.UTF-8"
   LC_TELEPHONE="zh_CN.UTF-8"
   LC_MEASUREMENT="zh_CN.UTF-8"
   LC_ALL=zh_CN.UTF-8
   ```
4. 按`Esc`输入`:wq`保存文件
5. 断开控制台，重新连接

### 2.3 换源
1. ```sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak```  
   备份原文件
2. ```vi /etc/apt/sources.list```  
   编辑配置文件
3. 将下列配置覆盖原文件粘贴
   ```
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
4. ```sudo apt update```  
5. ```sudo apt upgrade```  
   更新 

### 2.4 在图形化页面中使用root用户登录
1. ```vi /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf```
2. 修改内容   
    ```
    [Seat:*]
    user-session=ubuntu
    greeter-show-manual-login= true
    all-guest=false #这个可以 不用配置
    ```
3. ```vi /etc/pam.d/gdm-autologin```注释掉```auth required pam_succeed_if.so user != root quiet_success```
4. ```vi /etc/pam.d/gdm-password```注释掉```auth required pam_succeed_if.so user != root quiet_success```
5. ```vi /root/.profile```
6. ```
    # ~/.profile: executed by Bourne-compatible login shells.
    if [ "$BASH" ]; then
        if [ -f ~/.bashrc ]; then
            . ~/.bashrc
        fi
    fi
    tty -s && mesg n || true
    mesg n || true
   ```
---
## 3.软件

### 3.1 Fail2ban
1. ```sudo apt update```  
   更新软件列表
2. ```sudo apt install fail2ban```  
   安装Fail2ban
3. ```sudo systemctl status fail2ban```  
   检查是否成功运行
4. ```sudo cp /etc/fail2ban/jail.{conf,local}```  
5. 创建个人配置文件，在有需要时修改
   ```
   ignoreip = 127.0.0.1/8 ::1 123.123.123.123 192.168.1.0/24  //IP白名单
   bantime  = 10min  //禁止持续时间
   findtime  = 10m  //失败次数持续时间
   maxretry = 5  //失败尝试次数
   ```
   ```fail2ban-client set sshd unbanip xxx.xxx.xxx.xxx```  
   删除已被限制的IP  
   ```fail2ban-client set sshd banip xxx.xxx.xxx.xxx```  
   禁止已被限制的IP

### 3.2 JDK
1. 安装```sudo apt install openjdk-11-jdk```
2. 验证```java -version```
3. 查看安装路径```sudo update-alternatives --config java```
4. ```vi /etc/environment```添加```JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"```在最后一行
5. 重新配置```source /etc/environment```
6. 验证```echo $JAVA_HOME```

### 3.3 pip
1. 安装pip```sudo apt-get install python3-pip```
2. 更新```sudo pip3 install --upgrade pip```
3. 软连接```sudo ln -s /usr/bin/pip3 /usr/bin/pip```
4. 换源，新建```mkdir ~/.pip/```编辑```vi ./.pip/pip.conf```
5. 写入
    ```
    [global]
    index-url=https://pypi.tuna.tsinghua.edu.cn/simple
    [install]
    trusted-host=pypi.tuna.tsinghua.edu.cn
    ```
6. 查看当前源```pip config list```
7. 更新```sudo pip install --upgrade pip```

### 3.4 v2ray
1. `bash <(wget -qO- -o- https://git.io/v2ray.sh)`  
   使用一键安装脚本安装
2. 提示如下代表安装成功
   ```
   ···········V2Ray script by 233boy·········

   00:00:00)开始安装...
   00:00:00)安装依赖包 > unzip jq qrencode
   00:00:00)下载 V2Ray > https://github.com/XXXXXX...
   00:00:00)下载 V2Ray脚本 > https://github.com/XXXXXX...
   00:00:00)生成配置文件...

   使用协议：VMess-TCP
   -------------- VMess-TCP-XXXXX.json -------------
   协议 (protocol)         = vmess
   地址 (address)          = XXX.XXX.XXX.XXX
   端口 (port)             = XXXXX...
   用户ID (id)             = XXXXXXX-XXXX-XXXX-XXXX-XXXX-XXXXX
   传输协议 (network)      = tcp
   伪装类型 (type)         = none
   ------------- 链接 (URL) -------------
   vmess://XXXXX...
   ------------- END -------------
   ```
3. 将`VMess-TCP-XXXXX.json`块和`链接 (URL)`块复制保存
4. `v2ray`  
   打开管理面板
5. 输入`2`选择更改配置
6. 输入`1`选择更改协议
7. 输入`13`更改需要的协议
8. 加密方式不输入回车
9. 输入自己设置的密码
10. 将输出的`VMess-TCP-XXXXX.json`块和`链接 (URL)`块复制保存
11. 下载链接 [window版](https://github.com/2dust/v2rayN/releases/) [安卓版](https://github.com/2dust/v2rayNG/releases/)


### 3.5 搜狗输入法 
[官方教程](https://pinyin.sogou.com/linux/help.php)
### 3.6 deepin 
[官方教程](https://github.com/zq1997/deepin-wine#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)