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
3. ```vi /etc/ssh/sshd_config```
4. 找到`#Authentication`，将`PermitRootLogin`参数和`PasswordAuthentication`参数修改为`yes`,去掉注释`#`，保存文件。  

**换源**
1. 备份```sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak```
2. ```vi /etc/apt/sources.list```
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
4. 更新源```sudo apt update```
5. 更新软件```sudo apt upgrade```

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

### 软件
* 搜狗输入法 [官方教程](https://pinyin.sogou.com/linux/help.php)
* deepin [官方教程](https://github.com/zq1997/deepin-wine#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)