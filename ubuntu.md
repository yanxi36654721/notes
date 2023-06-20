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
   切换root用户
3. 设置密码，回车，重复输入确认密码
4. ```su root```  
   切换root用户，输入刚才设置的密码
5. ```vi /etc/ssh/sshd_config```  
   修改ssh配置
6. 按`i`编辑文件
7. 找到`#PermitRootLogin`参数和`PasswordAuthentication`参数，将属性修改为`yes`,去掉注释`#`，按`Esc`输入`:wq`保存文件。  

**设置中文环境**
1. ```sudo apt install language-pack-zh-hans -y```  
   安装中文语言包
2. ```vim /etc/environment```  
     编辑语言环境变量  
     保留原有内容，在后面粘贴下列配置
   ```
    LANG=zh_CN.UTF-8
    LANGUAGE=en_US:en
    LC_CTYPE="zh_CN.UTF-8"
    LC_NUMERIC="zh_CN.UTF-8"
    LC_TIME="zh_CN.UTF-8"
    LC_COLLATE="zh_CN.UTF-8"
    LC_MONETARY="zh_CN.UTF-8"
    LC_MESSAGES="zh_CN.UTF-8"
    LC_PAPER="zh_CN.UTF-8"
    LC_NAME="zh_CN.UTF-8"
    LC_ADDRESS="zh_CN.UTF-8"
    LC_TELEPHONE="zh_CN.UTF-8"
    LC_MEASUREMENT="zh_CN.UTF-8"
    LC_IDENTIFICATION="zh_CN.UTF-8"
    LC_ALL=zh_CN.UTF-8
   ```
3. 按`Esc`输入`:wq`保存文件
4. 关闭窗口，重新登录shell，使用`root`账户登录
5. ```source /etc/environment```  
   使刚才添加的环境变量生效
6. ```sudo apt install manpages-zh -y```  
   设置控制台中文

**换源**
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

**图形化页面root用户登录**
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

**JDK**
1. 安装```sudo apt install openjdk-11-jdk```
2. 验证```java -version```
3. 查看安装路径```sudo update-alternatives --config java```
4. ```vi /etc/environment```添加```JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"```在最后一行
5. 重新配置```source /etc/environment```
6. 验证```echo $JAVA_HOME```

**pip**
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

**v2ray**
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

### 软件
* 搜狗输入法 [官方教程](https://pinyin.sogou.com/linux/help.php)
* deepin [官方教程](https://github.com/zq1997/deepin-wine#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)