# 大数据
### 准备
1. Ubuntu 20.4
2. JDK11
3. hadoop3.2.3

### 环境安装

**设置root用户密码**
1. `sudo passwd root`
2. 输入密码，重复密码
3. 切换root用户`su root`
4. `vi /etc/ssh/sshd_config`
5. 找到`PermitRootLogin`参数（约34行）和`PasswordAuthentication`参数（约58行）修改值为`yes`,去掉注释`#`。
6. 按`Esc`，输入`:wq`，保存退出
---
# 注意：
# 以下操作需要在root用户下执行，请随时检查输入光标左侧结构

## root用户结构
``` root@xxxxxxxxxx:~#```

## 其他用户结构，如下方情况请输入命令`su root`并输入root用户密码
```xxxx@xxxxxxxxxx:/$ ```

---
**换源**
1. 备份`sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak`
2. 编辑`vi /etc/apt/sources.list`，全部换为以下内容
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
4. 更新源`sudo apt update`
5. 更新软件`sudo apt upgrade`
6. 其中会显示图形化选择页面，使用上下键选择第一个回车。

**宝塔面板**
1. 安装`wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && sudo bash install.sh ed8484bec`
2. 记录安装结束后的信息
```
===========================
Congratulations! Installed successfullu!
============================
外网面板地址：http://XXXXXXXX:8888/XXXXXXXX
内网面板地址：http://XXXXXXXX:8888/XXXXXXXX
username：xxxxxxxx
password：xxxxxxxx
```
3. 使用浏览器访问外网面板地址，输入账号密码登录
4. 点击注册账号，注册完成后回到页面绑定刚才注册的账号密码。
5. 点击右侧终端，账户名输入`root`输入密码，确定

**JDK**
1. 安装`sudo apt install openjdk-11-jdk`
2. 验证`java -version`
3. 查看安装路径`sudo update-alternatives --config java`
4. 编辑`vi /etc/environment`添加`JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"`在最后一行
5. 重新配置`source /etc/environment`
6. 验证`echo $JAVA_HOME`

**PIP**
1. 安装`sudo apt-get install python3-pip`
2. 编辑`vi /root/.pip/pip.conf`删除原有内容，写入以下内容
```
[global]
index-url=https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host=pypi.tuna.tsinghua.edu.cn
```
3. 按下`Esc`，输入`:wq`退出
4. 更新`sudo pip3 install --upgrade pip`

**SSH**
1. 删除ssh`sudo apt-get remove openssh-client openssh-server`
2. 再次安装`sudo apt-get install openssh-client openssh-server`
3. 更新
```
sudo apt-get install ssh
sudo apt-get install pdsh
```
9. 查看是否启用了免密登陆`ssh localhost`
10. 设置免密登陆
```
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
```
### Hadoop安装

**配置**
1. 下载安装包`wget https://mirrors.tuna.tsinghua.edu.cn/apache/hadoop/common/hadoop-3.2.3/hadoop-3.2.3.tar.gz`
2. 进入到下载的目录，一般为/root/`cd /root/`
3. 解压`tar -zxvf /opt/hadoop-3.2.3.tar.gz`
4. 进入hadoop安装目录`cd /opt/hadoop/`
5. 编辑`vi etc/hadoop/hadoop-env.sh`
6. 在文件最前插入一行`export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64`
7. 编辑`vi etc/hadoop/core-site.xml`，找到`<configuration>` `</configuration>`在中间插入
```
<property>
    <name>fs.defaultFS</name>
    <value>hdfs://localhost:9000</value>
</property>
```
8. 编辑`etc/hadoop/hdfs-site.xml`，找到`<configuration>` `</configuration>`在中间插入
```
 <property>
    <name>dfs.replication</name>
    <value>1</value>
</property>
```
9. 编辑`vi etc/hadoop/mapred-site.xml`，找到`<configuration>` `</configuration>`在中间插入
```
<property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
</property>
<property>
    <name>mapreduce.application.classpath</name>
    <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
</property>
```
10. 编辑`vi etc/hadoop/yarn-site.xml`，找到`<configuration>` `</configuration>`在中间插入
```
<property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
</property>
<property>
    <name>yarn.nodemanager.env-whitelist</name>
    <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_HOME,PATH,LANG,TZ,HADOOP_MAPRED_HOME</value>
</property>
```
11. 新建变量
```
cd /
export HADOOP_HOME=/opt/hadoop/
export PATH=$PATH:$HADOOP_HOME/bin
```
12. 生效变量`source /etc/profile`
13. 编辑`vi sbin/start-dfs.sh`和`vi sbin/stop-dfs.sh`加入内容
```
HDFS_DATANODE_USER=root  
HDFS_DATANODE_SECURE_USER=hdfs  
HDFS_NAMENODE_USER=root  
HDFS_SECONDARYNAMENODE_USER=root 
```
14. 格式化HDSF`bin/hdfs namenode -format`
15. 启用hdfs进程`sbin/start-dfs.sh`
16. 启动yarn进程`sbin/start-yarn.sh`
17. 访问`外网地址:8088`

### 注意：服务器关机前请关闭hdfs和yarn
```
sbin/stop-dfs.sh
sbin/stop-yarn.sh
```
