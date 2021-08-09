# Linux server

Contains most of the Linux Services

<img src="C:\雨鱼\NaNaOY.github.io\img\Linux图片.png" alt="Linux" style="zoom:160%;" />

---

## mysql

### 1. mysql配置优化
>
> - vim /etc/my.cnf以下只列出my.cnf文件中[mysqld]段落中的内容，其他段落内容对MySQL运行性能影响甚微，因而姑且忽略。 
>
>   代码如下   复制代码
>
> [mysqld] 
>
> port = 3306 
>
> serverid = 1 
>
> socket = /tmp/mysql.sock 
>
> skip-locking 
>
> - 避免MySQL的外部锁定，减少出错几率增强稳定性。 
>
> skip-name-resolve 
>
> - 禁止MySQL对外部连接进行DNS解析，使用这一选项可以消除MySQL进行DNS解析的时间。但需要注意，如果开启该选项，则所有远程主机连接授权都要使用IP地址方式，否则MySQL将无法正常处理连接请求！ 
>
> back_log = 384 
>
> - back_log 参数的值指出在MySQL暂时停止响应新请求之前的短时间内多少个请求可以被存在堆栈中。  如果系统在一个短时间内有很多连接，则需要增大该参数的值，该参数值指定到来的TCP/IP连接的侦听队列的大小。不同的操作系统在这个队列大小上有它自 己的限制。 试图设定back_log高于你的操作系统的限制将是无效的。默认值为50。对于Linux系统推荐设置为小于512的整数。 
>
> key_buffer_size = 256M 
>
> - key_buffer_size指定用于索引的缓冲区大小，增加它可得到更好的索引处理性能。对于内存在4GB左右的服务器该参数可设置为256M或384M。注意：该参数值设置的过大反而会是服务器整体效率降低！ 
>
> max_allowed_packet = 4M 
>
> thread_stack = 256K 
>
> table_cache = 128K 
>
> sort_buffer_size = 6M 
>
> - 查询排序时所能使用的缓冲区大小。注意：该参数对应的分配内存是每连接独占，如果有100个连接，那么实际分配的总共排序缓冲区大小为100 × 6 ＝ 600MB。所以，对于内存在4GB左右的服务器推荐设置为6-8M。 
>
> read_buffer_size = 4M 
>
> - 读查询操作所能使用的缓冲区大小。和sort_buffer_size一样，该参数对应的分配内存也是每连接独享。 
>
> join_buffer_size = 8M 
>
> - 联合查询操作所能使用的缓冲区大小，和sort_buffer_size一样，该参数对应的分配内存也是每连接独享。 
>
> myisam_sort_buffer_size = 64M 
>
> table_cache = 512 
>
> thread_cache_size = 64 
>
> query_cache_size = 64M 
>
> - 指定MySQL查询缓冲区的大小。可以通过在MySQL控制台观察，如果Qcache_lowmem_prunes的值非常大，则表明经常出现缓冲不够的 情况；如果Qcache_hits的值非常大，则表明查询缓冲使用非常频繁，如果该值较小反而会影响效率，那么可以考虑不用查询缓 冲；Qcache_free_blocks，如果该值非常大，则表明缓冲区中碎片很多。 
>
> tmp_table_size = 256M 
>
> max_connections = 768 
>
> - 指定MySQL允许的最大连接进程数。如果在访问论坛时经常出现Too Many Connections的错误提 示，则需要增大该参数值。 
>
> max_connect_errors = 10000000 
>
> wait_timeout = 10 
>
> - 指定一个请求的最大连接时间，对于4GB左右内存的服务器可以设置为5-10。 
>
> thread_concurrency = 8 
>
> - 该参数取值为服务器逻辑CPU数量*2，在本例中，服务器有2颗物理CPU，而每颗物理CPU又支持H.T超线程，所以实际取值为4*2=8 
>
> skip-networking 
>
> - 开启该选项可以彻底关闭MySQL的TCP/IP连接方式，如果WEB服务器是以远程连接的方式访问MySQL数据库服务器则不要开启该选项！否则将无法正常连接！ 
>
> table_cache=1024 
>
> - 物理内存越大,设置就越大.默认为2402,调到512-1024最佳 
>
> innodb_additional_mem_pool_size=4M 
>
> - 默认为2M 
>
> innodb_flush_log_at_trx_commit=1 
>
> - 设置为0就是等到innodb_log_buffer_size列队满后再统一储存,默认为1 
>
> innodb_log_buffer_size=2M 
>
> - 默认为1M 
>
> innodb_thread_concurrency=8 
>
> - 你的服务器CPU有几个就设置为几,建议用默认一般为8 
>
> key_buffer_size=256M 
>
> - 默认为218，调到128最佳 
>
> tmp_table_size=64M 
>
> - 默认为16M，调到64-256最挂 
>
> read_buffer_size=4M 
>
> - 默认为64K 
>
> read_rnd_buffer_size=16M 
>
> - 默认为256K 
>
> sort_buffer_size=32M 
>
> - 默认为256K 
>
> thread_cache_size=120 
>
> - 默认为60 
>
> query_cache_size=32M

## Docker

### 1. Docker架构
>
>Docker 使用客户端-服务器 (C/S) 架构模式，使用远程API来管理和创建Docker容器。
>
>Docker 容器通过 Docker 镜像来创建。
>
>容器与镜像的关系类似于面向对象编程中的对象与类。
>
>
>
>​                                                                          **Docker**	**面向对象**
>​                                                                            容器	        对象
>​                                                                            镜像	        类
>
><img src="C:\雨鱼\NaNaOY.github.io\img\Docker图片.png" alt="Docker" style="zoom:120%;" />
>
>
>
>Docker 镜像(Images)  : Docker 镜像是用于创建 Docker 容器的模板。
>
>Docker 容器(Container): 容器是独立运行的一个或一组应用。
>
>Docker 客户端(Client): Docker 客户端通过命令行或者其他工具使用 Docker API (https://docs.docker.com/reference/api/docker_remote_api) 与 Docker 的守护进程通信。
>
>Docker 主机(Host): 一个物理或者虚拟的机器用于执行 Docker 守护进程和容器。
>
>Docker 仓库(Registry): Docker 仓库用来保存镜像，可以理解为代码控制中的代码仓库.Docker Hub(https://hub.docker.com) 提供了庞大的镜像集合供使用。
>
>Docker Machine: Docker Machine是一个简化Docker安装的命令行工具，通过一个简单的命令行即可在相应的平台上安装Docker，比如VirtualBox、 Digital Ocean、Microsoft Azure。
>
>
>

### 2. CentOS Docker 安装
>
> 
>
> Docker支持以下的CentOS版本：
>
>    CentOS 7 (64-bit)
>    CentOS 6.5 (64-bit) 或更高的版本
>
>  前提条件
>
>  目前，CentOS 仅发行版本中的内核支持 Docker。
>
>  Docker 运行在 CentOS 7 上，要求系统为64位、系统内核版本为 3.10 以上。
>
>  Docker 运行在 CentOS-6.5 或更高的版本的 CentOS 上，要求系统为64位、系统内核版本为 2.6.32-431 或者更高版本。
>  使用 yum 安装（CentOS 7下）
>
>  Docker 要求 CentOS 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的CentOS 版本是否支持 Docker 。
>  通过 uname -r 命令查看你当前的内核版本
>
>  [root@runoob ~]# uname -r 3.10.0-327.el7.x86_64
>
>  [root@runoob ~]# yum -y install docker
>
>  启动 Docker 后台服务
>
>  [root@runoob ~]# service docker start
>
>  测试运行 hello-world
>  [root@runoob ~]#docker run hello-world
>
>  使用脚本安装 Docker
>
>  1、使用 sudo 或 root 权限登录 Centos。
>
>  2、确保 yum 包更新到最新。
>
>  $ sudo yum update
>
>  3、**执行 Docker 安装脚本。**
>
>  $ curl -fsSL https://get.docker.com/ | sh
>
>  执行这个脚本会添加 docker.repo 源并安装 Docker。
>
>  4、启动 Docker 进程。
>
>  $ sudo service docker start
>
>  5、验证 docker 是否安装成功并在容器中执行一个测试的镜像。
>
>  $ sudo docker run hello-world
>
>  镜像加速
>
>  鉴于国内网络问题，后续拉取 Docker 镜像十分缓慢，我们可以需要配置加速器来解决，我使用的是网易的镜像地址：http://hub-mirror.c.163.com。
>
>  新版的 Docker 使用 /etc/docker/daemon.json（Linux） 或者 %programdata%\docker\config\daemon.json（Windows） 来配置 Daemon。
>
>  请在该配置文件中加入（没有该文件的话，请先建一个）：
>
>  {
>   "registry-mirrors": ["http://hub-mirror.c.163.com"]
>  }

## tomcat

### 1. tomcat apr模式开启
>安装apr 1.5.2 
>下载 http://apache.fayea.com//apr/apr-1.5.2.tar.gz
>
>
> - cd /usr/local/src
>
>
> -  wget http://apache.fayea.com//apr/apr-1.5.2.tar.gz
>
>
> - tar -xzvf apr-1.5.2.tar.gz
>
>
> - cd  apr-1.5.2
>
>
> - ./configure --prefix=/usr/local/apr
>
>
> - make 
>
>
> - make install
>
>安装apr-util-1.5.4 
>下载 http://mirrors.hust.edu.cn/apache//apr/apr-util-1.5.4.tar.gz
>
>
> - cd /usr/local/src
>
>
> - wget http://mirrors.hust.edu.cn/apache//apr/apr-util-1.5.4.tar.gz
>
>
> - tar -xzvf apr-util-1.5.4.tar.gz
>
>
> - cd apr-util-1.5.4
>
>
> - ./configure --with-apr=/usr/local/apr
>
>
> - make 
>
>
> - make install
>
>安装 tomcat-native组件
>
>/usr/local/tomcat/bin
>tar -xzvf tomcat-native.tar.gz
>cd tomcat-native-1.2.7-src/native
>./configure --with-apr=/usr/local/apr 
>//出现如下错误
>//Found   OPENSSL_VERSION_NUMBER 0x1000105f (OpenSSL 1.0.1e 11 Feb 2013)
>//Require OPENSSL_VERSION_NUMBER 0x1000200f or greater (1.0.2)
>//configure: error: Your version of OpenSSL is not compatible with this version of tcnative
>
>安装OpenSSL 1.0.2 
>由于centos 7 当前的yum 库只有1.0.1 的OpenSSL，所以我们需要手工安装1.0.2
>
>
> - cd /usr/local/src
>
>
> - wget https://www.openssl.org/source/openssl-1.0.2-latest.tar.gz
>
>
> - tar -xzxf openssl-1.0.2-latest.tar.gz
>
>
> - cd openssl-1.0.2g
>
>
> - ./config --prefix=/usr/local/openssl -fPIC
>
>// 注意这里需要加入 -fPIC参数，否则后面在安装tomcat native 组件会出错
>// 注意：不要按照提示去运行 make depend
>
> - make
>
>
> - make install
>
>
> - mv /usr/bin/openssl ~
>
>
> - ln -s /usr/local/openssl/bin/openssl /usr/bin/openssl
>
>
> - openssl version
>
>// 确认版本信息是1.0.2 
>
>
>重新安装 tomcat-native组件
>
>
> - cd $CATALINA_HOME/bin
>
>
> - tar -xzvf tomcat-native.tar.gz
>
>
> - cd tomcat-native-1.2.7-src/native
>
>
> - ./configure --with-apr=/usr/local/apr --with-ssl=/usr/local/openssl
>
>
> - make 
>
>
> - make install
>
>
>检查是否安装成功
> - cd $CATALINA_HOME
> - cd bin
> - ./catalina.sh run
>//查看输入信息，如果有以下内容，说明安装成功
>//20-Jun-2016 12:28:32.859 INFO [main] org.apache.catalina.core.AprLifecycleListener.lifecycleEvent Loaded APR based Apache Tomcat Native library 1.2.7 using APR version 1.5.2.
>//20-Jun-2016 12:28:32.859 INFO [main] org.apache.catalina.core.AprLifecycleListener.lifecycleEvent APR capabilities: IPv6 [true], sendfile [true], accept filters [false], random [true].
>//20-Jun-2016 12:28:32.862 INFO [main] org.apache.catalina.core.AprLifecycleListener
>
>错误信息:openssl: error while loading shared libraries: libssl.so.1.1
>
>
>在RedHat 6上编译安装openssl后，运行openssl version出现如下错误：
>
>[html] view plain copy
>
>openssl: error while loading shared libraries: libssl.so.1.1: cannot open shared object file: No such file or directory  
>
>
>这是由于openssl库的位置不正确造成的。
>
>解决方法：
>
>在root用户下执行：
>
>[html] view plain copy
>
>    ln -s /usr/local/openssl/lib/libssl.so.1.1 /usr/lib64/libssl.so.1.1  
>    ln -s /usr/local/openssl/lib/libcrypto.so.1.1 /usr/lib64/libcrypto.so.1.1  
>
>apr config报错:
>29605   RM='$RM' 改为 RM='$RM -f'

## rsync

### 1. rsync远程同步文件
>Linux下Rsync+Inotify-tools实现数据实时同步
>
>2014年03月07日 ⁄ Inotify ⁄ 评论数 3 ⁄ 被围观 27,367次+ 
>
>说明：
>操作系统：CentOS 5.X
>源服务器：192.168.21.129
>目标服务器：192.168.21.127，192.168.21.128
>目的：把源服务器上/home/www.osyunwei.com目录实时同步到目标服务器的/home/www.osyunwei.com下
>具体操作：
>第一部分：分别在两台目标服务器192.168.21.127，192.168.21.128上操作
>一、分别在两台在目标服务器安装Rsync服务端
>		1、关闭SELINUX
>	   vi /etc/selinux/config #编辑防火墙配置文件
>
>  SELINUX=enforcing #注释掉
>  SELINUXTYPE=targeted #注释掉
>  SELINUX=disabled #增加
>  :wq! #保存，退出
>  setenforce 0  #立即生效
>  2、开启防火墙tcp 873端口（Rsync默认端口）
>  vi /etc/sysconfig/iptables #编辑防火墙配置文件
>  -A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 873 -j ACCEPT
>  :wq! #保存，退出
>  /etc/init.d/iptables restart #最后重启防火墙使配置生效
>  3、安装Rsync服务端软件
>  yum install rsync xinetd #安装
>  vi /etc/xinetd.d/rsync #编辑配置文件，设置开机启动rsync
>  disable = no #修改为no
>  :wq! #保存退出
>  /etc/init.d/xinetd start #启动（CentOS中是以xinetd来管理Rsync服务的）
>  4、创建rsyncd.conf配置文件
>  vi /etc/rsyncd.conf #创建配置文件，添加以下代码
>  log file = /var/log/rsyncd.log #日志文件位置，启动rsync后自动产生这个文件，无需提前创建
>  pidfile = /var/run/rsyncd.pid  #pid文件的存放位置
>  lock file = /var/run/rsync.lock  #支持max connections参数的锁文件
>  secrets file = /etc/rsync.pass  #用户认证配置文件，里面保存用户名称和密码，后面会创建这个文件
>  motd file = /etc/rsyncd.Motd  #rsync启动时欢迎信息页面文件位置（文件内容自定义）
>  [home_www.osyunwei.com] #自定义名称
>  path = /home/www.osyunwei.com/ #rsync服务端数据目录路径
>  comment = home_www.osyunwei.com #模块名称与[home_www.osyunwei.com]自定义名称相同
>  uid = root #设置rsync运行权限为root
>  gid = root #设置rsync运行权限为root
>  port=873  #默认端口
>  use chroot = no #默认为true，修改为no，增加对目录文件软连接的备份
>  read only = no  #设置rsync服务端文件为读写权限
>  list = no #不显示rsync服务端资源列表
>  max connections = 200 #最大连接数
>  timeout = 600  #设置超时时间
>  auth users = home_www.osyunwei.com_user #执行数据同步的用户名，可以设置多个，用英文状态下逗号隔开
>  hosts allow = 192.168.21.129  #允许进行数据同步的客户端IP地址，可以设置多个，用英文状态下逗号隔开
>  hosts deny = 192.168.21.254 #禁止数据同步的客户端IP地址，可以设置多个，用英文状态下逗号隔开
>  :wq!  #保存,退出
>  5、创建用户认证文件
>  vi /etc/rsync.pass #配置文件，添加以下内容
>  home_www.osyunwei.com_user:123456  #格式，用户名:密码，可以设置多个，每行一个用户名:密码
>  :wq!  #保存，退出
>  6、设置文件权限
>  chmod 600 /etc/rsyncd.conf  #设置文件所有者读取、写入权限
>  chmod 600 /etc/rsync.pass  #设置文件所有者读取、写入权限
>  7、启动rsync
>  /etc/init.d/xinetd start  #启动
>  service xinetd stop   #停止
>  service xinetd restart #重新启动
>  第二部分：在源服务器192.168.21.129上操作
>  一、安装Rsync客户端
>  1、关闭SELINUX
>  vi /etc/selinux/config #编辑防火墙配置文件
>
>  SELINUX=enforcing #注释掉
>  SELINUXTYPE=targeted #注释掉
>  SELINUX=disabled #增加
>  :wq! #保存，退出
>  setenforce 0 #立即生效
>  2、开启防火墙tcp 873端口（Rsync默认端口，做为客户端的Rsync可以不用开启873端口）
>  vi /etc/sysconfig/iptables #编辑防火墙配置文件
>  -A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 873 -j ACCEPT
>  :wq! #保存，退出
>  /etc/init.d/iptables restart #最后重启防火墙使配置生效
>  系统运维  www.osyunwei.com  温馨提醒：qihang01原创内容 版权所有,转载请注明出处及原文链接
>  3、安装Rsync客户端软件
>  whereis rsync   #查看系统是否已安装rsync,出现下面的提示，说明已经安装
>  rsync: /usr/bin/rsync /usr/share/man/man1/rsync.1.gz
>  yum install  xinetd  #只安装xinetd即可，CentOS中是以xinetd来管理rsync服务的
>  yum install rsync xinetd #如果默认没有rsync，运行此命令进行安装rsync和xinetd
>  vi /etc/xinetd.d/rsync #编辑配置文件，设置开机启动rsync
>  disable = no #修改为
>  /etc/init.d/xinetd start #启动（CentOS中是以xinetd来管理rsync服务的）
>  4、创建认证密码文件
>  vi /etc/passwd.txt  #编辑文件，添加以下内容
>  123456 #密码
>  :wq! #保存退出
>  chmod 600 /etc/passwd.txt #设置文件权限，只设置文件所有者具有读取、写入权限即可
>  5、测试源服务器192.168.21.129到两台目标服务器192.168.21.127，192.168.21.128之间的数据同步
>  mkdir /home/www.osyunwei.com/ceshi #在源服务器上创建测试文件夹，然后在源服务器运行下面2行命令
>  rsync -avH --port=873 --progress --delete  /home/www.osyunwei.com/  home_www.osyunwei.com_user@192.168.21.127::home_www.osyunwei.com --password-file=/etc/passwd.txt
>  rsync -avH --port=873 --progress --delete  /home/www.osyunwei.com/  home_www.osyunwei.com_user@192.168.21.128::home_www.osyunwei.com --password-file=/etc/passwd.txt
>  运行完成后，分别在两台目标服务器192.168.21.127，192.168.21.128上查看，在/home/www.osyunwei.com目录下有ceshi文件夹，说明数据同步成功。
>  二、安装Inotify-tools工具，实时触发rsync进行同步
>  1、查看服务器内核是否支持inotify
>  ll /proc/sys/fs/inotify   #列出文件目录，出现下面的内容，说明服务器内核支持inotify
>  -rw-r--r-- 1 root root 0 Mar  7 02:17 max_queued_events
>  -rw-r--r-- 1 root root 0 Mar  7 02:17 max_user_instances
>  -rw-r--r-- 1 root root 0 Mar  7 02:17 max_user_watches
>  备注：Linux下支持inotify的内核最小为2.6.13，可以输入命令：uname -a查看内核
>  CentOS 5.X 内核为2.6.18，默认已经支持inotify
>  2、安装inotify-tools
>  yum install make  gcc gcc-c++  #安装编译工具
>  inotify-tools下载地址：http://github.com/downloads/rvoicilas/inotify-tools/inotify-tools-3.14.tar.gz
>  上传inotify-tools-3.14.tar.gz到/usr/local/src目录下
>  cd /usr/local/src
>  tar zxvf inotify-tools-3.14.tar.gz  #解压
>  cd inotify-tools-3.14 #进入解压目录
>  ./configure --prefix=/usr/local/inotify  #配置
>  make  #编译
>  make install  #安装
>  3、设置系统环境变量，添加软连接
>  echo "PATH=/usr/local/inotify/bin:$PATH" >>/etc/profile.d/inotify.sh
>  source /etc/profile.d/inotify.sh  #使设置立即生效
>  echo "/usr/local/inotify/lib" >/etc/ld.so.conf.d/inotify.conf
>  ln -s /usr/local/inotify/include  /usr/include/inotify
>  4、修改inotify默认参数（inotify默认内核参数值太小）
>  查看系统默认参数值
>  sysctl -a | grep max_queued_events
>  结果是：fs.inotify.max_queued_events = 16384
>  sysctl -a | grep max_user_watches
>  结果是：fs.inotify.max_user_watches = 8192
>  sysctl -a | grep max_user_instances
>  结果是：fs.inotify.max_user_instances = 128
>  修改参数：
>  sysctl -w fs.inotify.max_queued_events="99999999"
>  sysctl -w fs.inotify.max_user_watches="99999999"
>  sysctl -w fs.inotify.max_user_instances="65535"
>  vi /etc/sysctl.conf #添加以下代码
>  fs.inotify.max_queued_events=99999999
>  fs.inotify.max_user_watches=99999999
>  fs.inotify.max_user_instances=65535
>  :wq! #保存退出
>  参数说明：
>  max_queued_events：
>  inotify队列最大长度，如果值太小，会出现"** Event Queue Overflow **"错误，导致监控文件不准确
>  max_user_watches：
>  要同步的文件包含多少目录，可以用：find /home/www.osyunwei.com -type d | wc -l 统计，必须保证max_user_watches值大于统计结果（这里/home/www.osyunwei.com为同步文件目录）
>  max_user_instances：
>  每个用户创建inotify实例最大值
>  系统运维  www.osyunwei.com  温馨提醒：qihang01原创内容 版权所有,转载请注明出处及原文链接
>  5、创建脚本，实时触发rsync进行同步
>  
>  ### vi /usr/local/inotify/rsync.sh   #编辑，添加以下代码
>  #!/bin/sh
>  srcdir=/home/www.osyunwei.com/
>  dstdir=home_www.osyunwei.com
>  excludedir=/usr/local/inotify/exclude.list
>  rsyncuser=home_www.osyunwei.com_user
>  rsyncpassdir=/etc/passwd.txt
>  dstip="192.168.21.127 192.168.21.128"
>  for ip in $dstip
>  do
>  rsync -avH --port=873 --progress --delete  --exclude-from=$excludedir  $srcdir $rsyncuser@$ip::$dstdir --password-file=$rsyncpassdir
>  done
>  /usr/local/inotify/bin/inotifywait -mrq --timefmt '%d/%m/%y %H:%M' --format '%T %w%f%e' -e close_write,modify,delete,create,attrib,move $srcdir |  while read file
>  do
>  for ip in $dstip
>  do
>  rsync -avH --port=873 --progress --delete  --exclude-from=$excludedir  $srcdir $rsyncuser@$ip::$dstdir --password-file=$rsyncpassdir
>  echo "  ${file} was rsynced" >> /tmp/rsync.log 2>&1
>  done
>
>  ### done
>  chmod +x /usr/local/inotify/rsync.sh   #添加脚本执行权限
>  脚本参数说明：
>  srcdir=/home/www.osyunwei.com/  #源服务器同步目录
>  dstdir=home_www.osyunwei.com    #目标服务器rsync同步目录模块名称
>  excludedir=/usr/local/inotify/exclude.list   
>  #不需要同步的目录，如果有多个，每一行写一个目录，使用相对于同步模块的路径；
>  #例如：不需要同步/home/www.osyunwei.com/目录下的a目录和b目录下面的b1目录，exclude.list文件可以这样写
>  a/
>  b/b1/
>  rsyncuser=home_www.osyunwei.com_user  #目标服务器rsync同步用户名
>  rsyncpassdir=/etc/passwd.txt  #目标服务器rsync同步用户的密码在源服务器的存放路径
>  dstip="192.168.21.127 192.168.21.128"  #目标服务器ip，多个ip用空格分开
>  /tmp/rsync.log  #脚本运行日志记录
>  6、设置脚本开机自动执行
>  vi /etc/rc.d/rc.local  #编辑，在最后添加一行
>  sh /usr/local/inotify/rsync.sh & ＃设置开机自动在后台运行脚本
>  :wq!  #保存退出
>  7、测试inotify实时触发rsync同步脚本是否正常运行
>  在源服务器192.168.21.129上创建文件inotify_rsync_ceshi
>  mkdir /home/www.osyunwei.com/inotify_rsync_ceshi
>  重新启动源服务器：192.168.21.129
>  等系统启动之后，查看两台目标服务器192.168.21.127，192.168.21.128的/home/www.osyunwei.com下是否有inotify_rsync_ceshi文件夹
>  然后再在源服务器192.168.21.129创建文件夹inotify_rsync_ceshi_new
>  mkdir /home/www.osyunwei.com/inotify_rsync_ceshi_new
>  继续查看两台目标服务器192.168.21.127，192.168.21.128的/home/www.osyunwei.com下是否有inotify_rsync_ceshi_new文件夹
>  如果以上测试都通过，说明inotify实时触发rsync同步脚本运行正常。
>  至此，Linux下Rsync+Inotify-tools实现数据实时同步完成。
>  ### 扩展阅读
>  inotify参数
>  -m 是保持一直监听
>  -r 是递归查看目录
>  -q 是打印出事件
>  -e create,move,delete,modify,attrib 是指 “监听 创建 移动 删除 写入 权限” 事件
>  
>  ### rsync参数
>  -v, --verbose 详细模式输出
>  -q, --quiet 精简输出模式
>  -c, --checksum 打开校验开关，强制对文件传输进行校验
>  -a, --archive 归档模式，表示以递归方式传输文件，并保持所有文件属性，等于-rlptgoD
>  -r, --recursive 对子目录以递归模式处理
>  -R, --relative 使用相对路径信息
>  -b, --backup 创建备份，也就是对于目的已经存在有同样的文件名时，将老的文件重新命名为~filename。可以使用--suffix选项来指定不同的备份文件前缀。
>  --backup-dir 将备份文件(如~filename)存放在在目录下。
>  -suffix=SUFFIX 定义备份文件前缀
>  -u, --update 仅仅进行更新，也就是跳过所有已经存在于DST，并且文件时间晚于要备份的文件。(不覆盖更新的文件)
>  -l, --links 保留软链结
>  -L, --copy-links 想对待常规文件一样处理软链结
>  --copy-unsafe-links 仅仅拷贝指向SRC路径目录树以外的链结
>  --safe-links 忽略指向SRC路径目录树以外的链结
>  -H, --hard-links 保留硬链结
>  -p, --perms 保持文件权限
>  -o, --owner 保持文件属主信息
>  -g, --group 保持文件属组信息
>  -D, --devices 保持设备文件信息
>  -t, --times 保持文件时间信息
>  -S, --sparse 对稀疏文件进行特殊处理以节省DST的空间
>  -n, --dry-run现实哪些文件将被传输
>  -W, --whole-file 拷贝文件，不进行增量检测
>  -x, --one-file-system 不要跨越文件系统边界
>  -B, --block-size=SIZE 检验算法使用的块尺寸，默认是700字节
>  -e, --rsh=COMMAND 指定使用rsh、ssh方式进行数据同步
>  --rsync-path=PATH 指定远程服务器上的rsync命令所在路径信息
>  -C, --cvs-exclude 使用和CVS一样的方法自动忽略文件，用来排除那些不希望传输的文件
>  --existing 仅仅更新那些已经存在于DST的文件，而不备份那些新创建的文件
>  --delete 删除那些DST中SRC没有的文件
>  --delete-excluded 同样删除接收端那些被该选项指定排除的文件
>  --delete-after 传输结束以后再删除
>  --ignore-errors 及时出现IO错误也进行删除
>  --max-delete=NUM 最多删除NUM个文件
>  --partial 保留那些因故没有完全传输的文件，以是加快随后的再次传输
>  --force 强制删除目录，即使不为空
>  --numeric-ids 不将数字的用户和组ID匹配为用户名和组名
>  --timeout=TIME IP超时时间，单位为秒
>  -I, --ignore-times 不跳过那些有同样的时间和长度的文件
>  --size-only 当决定是否要备份文件时，仅仅察看文件大小而不考虑文件时间
>  --modify-window=NUM 决定文件是否时间相同时使用的时间戳窗口，默认为0
>  -T --temp-dir=DIR 在DIR中创建临时文件
>  --compare-dest=DIR 同样比较DIR中的文件来决定是否需要备份
>  -P 等同于 --partial
>  --progress 显示备份过程
>  -z, --compress 对备份的文件在传输时进行压缩处理
>  --exclude=PATTERN 指定排除不需要传输的文件模式
>  --include=PATTERN 指定不排除而需要传输的文件模式
>  --exclude-from=FILE 排除FILE中指定模式的文件
>  --include-from=FILE 不排除FILE指定模式匹配的文件
>  --version 打印版本信息
>  --address 绑定到特定的地址
>  --config=FILE 指定其他的配置文件，不使用默认的rsyncd.conf文件
>  --port=PORT 指定其他的rsync服务端口
>  --blocking-io 对远程shell使用阻塞IO
>  -stats 给出某些文件的传输状态
>  --progress 在传输时现实传输过程
>  --log-format=formAT 指定日志文件格式
>  --password-file=FILE 从FILE中得到密码
>  --bwlimit=KBPS 限制I/O带宽，KBytes per second
>  -h, --help 显示帮助信息

### 2. rsync配置文件
> - /etc/rsyncd: configuration file for rsync daemon mode
>
> - See rsyncd.conf man page for more options.
>
> - configuration example:
>
>  uid = root
>  gid = root
>  use chroot = no
>  read only = no
>  max connections = 200
>  log file = /var/log/rsyncd.log
>  pid file = /var/run/rsyncd.pid
>  lock file = /var/run/rsync.lock
>  log format = %Y%M%D-%h%m
>  motd file = /etc/rsyncd.Motd
>  secrets file = /etc/rsync.pass
> - exclude = lost+found/
> - transfer logging = yes
>  timeout = 600
> - ignore nonreadable = yes
> - dont compress   = *.gz *.tgz *.zip *.z *.Z *.rpm *.deb *.bz2
>
> - [ftp]
> -      path = /home/ftp
> -       comment = ftp export area
>
>[static]
>path = /home/static/
>comment = static
>auth users = qiyu
>hosts allow = 101.201.108.221 , hosts allow = 192.168.1.187 , hosts allow = 124.65.190.82

## nginx

### 1. nginx概述
>### **Nginx功能概述**
>
>__HTTP基础功能：__
>
>处理静态文件，索引文件以及自动索引；
>反向代理加速(无缓存)，简单的负载均衡和容错；
>FastCGI，简单的负载均衡和容错；
>模块化的结构。过滤器包括gzipping, byte ranges, chunked responses, 以及 SSI-filter 。在SSI过滤器中，到同一个 proxy 或者 FastCGI 的多个子请求并发处理；
>SSL 和 TLS SNI 支持；
>__IMAP/POP3 代理服务功能：__
>
>使用外部 HTTP 认证服务器重定向用户到 IMAP/POP3 后端；
>使用外部 HTTP 认证服务器认证用户后连接重定向到内部的 SMTP 后端；
>认证方法：
>POP3: POP3 USER/PASS, APOP, AUTH LOGIN PLAIN CRAM-MD5;
>IMAP: IMAP LOGIN;
>SMTP: AUTH LOGIN PLAIN CRAM-MD5;
>SSL 支持；
>在 IMAP 和 POP3 模式下的 STARTTLS 和 STLS 支持；
>__支持的操作系统：__
>
>FreeBSD 3.x, 4.x, 5.x, 6.x i386; FreeBSD 5.x, 6.x amd64;
>Linux 2.2, 2.4, 2.6 i386; Linux 2.6 amd64;
>Solaris 8 i386; Solaris 9 i386 and sun4u; Solaris 10 i386;
>MacOS X (10.4) PPC;
>__结构与扩展：__
>
>一个主进程和多个工作进程。工作进程是单线程的，且不需要特殊授权即可运行；
>kqueue (FreeBSD 4.1+), epoll (Linux 2.6+), rt signals (Linux 2.2.19+), /dev/poll (Solaris 7 11/99+), select, 以及 poll 支持；
>kqueue支持的不同功能包括 EV_CLEAR, EV_DISABLE （临时禁止事件）， NOTE_LOWAT, EV_EOF, 有效数据的数目，错误代码；
>sendfile (FreeBSD 3.1+), sendfile (Linux 2.2+), sendfile64 (Linux 2.4.21+), 和 sendfilev (Solaris 8 7/01+) 支持；
>输入过滤 (FreeBSD 4.1+) 以及 TCP_DEFER_ACCEPT (Linux 2.4+) 支持；
>10,000 非活动的 HTTP keep-alive 连接仅需要 2.5M 内存。
>最小化的数据拷贝操作；
>__其他HTTP功能：__
>
>基于IP 和名称的虚拟主机服务；
>Memcached 的 GET 接口；
>支持 keep-alive 和管道连接；
>灵活简单的配置；
>重新配置和在线升级而无须中断客户的工作进程；
>可定制的访问日志，日志写入缓存，以及快捷的日志回卷；
>4xx-5xx 错误代码重定向；
>基于 PCRE 的 rewrite 重写模块；
>基于客户端 IP 地址和 HTTP 基本认证的访问控制；
>PUT, DELETE, 和 MKCOL 方法；
>支持 FLV （Flash 视频）；
>带宽限制；
>__实验特性：__
>
>内嵌的
>perl
>通过
>aio_read() / aio_write()
>的套接字工作的实验模块，仅在 FreeBSD 下。

### 2. 为什么要选择nginx
>Nginx 是一个高性能的 Web 和反向代理服务器, 它具有有很多非常优越的特性:
>
>作为 Web 服务器：相比 Apache，Nginx 使用更少的资源，支持更多的并发连接，体现更高的效率，这点使 Nginx 尤其受到虚拟主机提供商的欢迎。
>能够支持高达 50,000 个并发连接数的响应，感谢 Nginx 为我们选择了 epoll and kqueue 作为开发模型.
>
>作为负载均衡服务器：Nginx 既可以在内部直接支持 Rails 和 PHP，也可以支持作为 HTTP代理服务器 对外进行服务。
>Nginx 用 C 编写, 不论是系统资源开销还是 CPU 使用效率都比 Perlbal 要好的多。
>
>作为邮件代理服务器: Nginx 同时也是一个非常优秀的邮件代理服务器（最早开发这个产品的目的之一也是作为邮件代理服务器），Last.fm 描述了成功并且美妙的使用经验。
>
>Nginx 安装非常的简单，配置文件 非常简洁（还能够支持perl语法），Bugs非常少的服务器: Nginx 启动特别容易，并且几乎可以做到7*24不间断运行，
>即使运行数个月也不需要重新启动。你还能够在 不间断服务的情况下进行软件版本的升级。

### 3. nginx 安装
>nginx可以使用各平台的默认包来安装，本文是介绍使用源码编译安装，包括具体的编译参数信息。
>
>正式开始前，编译环境gcc g++ 开发库之类的需要提前装好，这里默认你已经装好。
>
>ububtu平台编译环境可以使用以下指令
>1.apt-get install build-essential
>2.apt-get install libtool
>centos平台编译环境使用如下指令
>安装make：
>1.yum -y install gcc automake autoconf libtool make
>安装g++:
>1.yum install gcc gcc-c++
>
>下面正式开始
>---------------------------------------------------------------------------
>一般我们都需要先装pcre, zlib，前者为了重写rewrite，后者为了gzip压缩。
>1.选定源码目录
>可以是任何目录，本文选定的是/usr/local/src
>1.cd /usr/local/src
>
>2.安装PCRE库
>ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/ 下载最新的 PCRE 源码包，使用下面命令下载编译和安装 PCRE 包：
>
>1.cd /usr/local/src
>2.wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.37.tar.gz 
>3.tar -zxvf pcre-8.37.tar.gz
>4.cd pcre-8.34
>5../configure
>6.make
>7.make install
>
>3.安装zlib库
>http://zlib.net/zlib-1.2.8.tar.gz 下载最新的 zlib 源码包，使用下面命令下载编译和安装 zlib包：
>
>1.cd /usr/local/src
>2.wget http://zlib.net/zlib-1.2.8.tar.gz
>3.tar -zxvf zlib-1.2.8.tar.gz
>4.cd zlib-1.2.8
>5../configure
>6.make
>7.make install
>
>4.安装ssl（某些vps默认没装ssl)
>
>
>1.cd /usr/local/src
>2.wget https://www.openssl.org/source/openssl-1.0.1t.tar.gz
>3.tar -zxvf openssl-1.0.1t.tar.gz
>
>5.安装nginx
>
>Nginx 一般有两个版本，分别是稳定版和开发版，您可以根据您的目的来选择这两个版本的其中一个，下面是把 Nginx 安装到 /usr/local/nginx 目录下的详细步骤：
>
>
>cd /usr/local/src
>wget http://nginx.org/download/nginx-1.4.2.tar.gz
>tar -zxvf nginx-1.4.2.tar.gz
>cd nginx-1.4.2
>
>./configure --sbin-path=/usr/local/nginx/nginx \
>--conf-path=/usr/local/nginx/nginx.conf \
>--pid-path=/usr/local/nginx/nginx.pid \
>--with-http_ssl_module \
>--with-pcre=/opt/app/openet/oetal1/chenhe/pcre-8.37 \
>--with-zlib=/opt/app/openet/oetal1/chenhe/zlib-1.2.8 \
>--with-openssl=/opt/app/openet/oetal1/chenhe/openssl-1.0.1t
>
>make
>make install
>
>--with-pcre=/usr/src/pcre-8.34 指的是pcre-8.34 的源码路径。
>--with-zlib=/usr/src/zlib-1.2.7 指的是zlib-1.2.7 的源码路径。
>
>安装成功后 /usr/local/nginx 目录下如下
>
>
>fastcgi.conf            koi-win             nginx.conf.default
>fastcgi.conf.default    logs                scgi_params
>fastcgi_params          mime.types          scgi_params.default
>fastcgi_params.default  mime.types.default  uwsgi_params
>html                    nginx               uwsgi_params.default
>koi-utf                 nginx.conf          win-utf
>
>6.启动
>确保系统的 80 端口没被其他程序占用，运行/usr/local/nginx/nginx 命令来启动 Nginx，
>
>netstat -ano|grep 80
>
>如果查不到结果后执行，有结果则忽略此步骤（ubuntu下必须用sudo启动，不然只能在前台运行）
>
>
>sudo /usr/local/nginx/nginx
>
>打开浏览器访问此机器的 IP，如果浏览器出现 Welcome to nginx! 则表示 Nginx 已经安装并运行成功。
>
>
>
>-----------------------------------------------------
>到这里nginx就安装完成了，如果只是处理静态html就不用继续安装了
>
>如果你需要处理php脚本的话，还需要安装php-fpm。
>
>下面安装排错
>
>附：可能遇到的错误和一些帮助信息
>
>1.1编译pcre错误
>
>libtool: compile: unrecognized option `-DHAVE_CONFIG_H'
>libtool: compile: Try `libtool --help' for more information.
>make[1]: *** [pcrecpp.lo] Error 1
>make[1]: Leaving directory `/usr/local/src/pcre-8.34'
>make: *** [all] Error 2
>
>
>解决办法：安装g++,别忘了重新configure
>
>
>apt-get install g++
>apt-get install build-essential
>make clean
>./configure
>make
>
>1.2 make出错
>
>
>make: *** No rule to make target `build', needed by `default'.  Stop.
>./configure: error: SSL modules require the OpenSSL library.
>You can either do not enable the modules, or install the OpenSSL library
>into the system, or build the OpenSSL library statically from the source
>with nginx by using --with-openssl=<path> option.
>
>按照第4步的安装方法或
>ubuntu下
>
>apt-get install openssl
>apt-get install libssl-dev
>
>centos下
>
>yum -y install openssl openssl-devel
>
>2.nginx编译选项
>
>make是用来编译的，它从Makefile中读取指令，然后编译。
>
>make install是用来安装的，它也从Makefile中读取指令，安装到指定的位置。
>
>configure命令是用来检测你的安装平台的目标特征的。它定义了系统的各个方面，包括nginx的被允许使用的连接处理的方法，比如它会检测你是不是有CC或GCC，并不是
>需要CC或GCC，它是个shell脚本，执行结束时，它会创建一个Makefile文件。nginx的configure命令支持以下参数：
>
>--prefix=path    定义一个目录，存放服务器上的文件 ，也就是nginx的安装目录。默认使用 /usr/local/nginx。
>
>--sbin-path=path 设置nginx的可执行文件的路径，默认为  prefix/sbin/nginx.
>
>--conf-path=path  设置在nginx.conf配置文件的路径。nginx允许使用不同的配置文件启动，通过命令行中的-c选项。默认为prefix/conf/nginx.conf.
>
>--pid-path=path  设置nginx.pid文件，将存储的主进程的进程号。安装完成后，可以随时改变的文件名 ， 在nginx.conf配置文件中使用 PID指令。默认情况下，
>文件名 为prefix/logs/nginx.pid.
>
>--error-log-path=path 设置主错误，警告，和诊断文件的名称。安装完成后，可以随时改变的文件名 ，在nginx.conf配置文件中 使用 的error_log指令。
>默认情况下，文件名 为prefix/logs/error.log.
>
>--http-log-path=path  设置主请求的HTTP服务器的日志文件的名称。安装完成后，可以随时改变的文件名 ，
>在nginx.conf配置文件中 使用 的access_log指令。默认情况下，文件名 为prefix/logs/access.log.
>
>--user=name  设置nginx工作进程的用户。安装完成后，可以随时更改的名称在nginx.conf配置文件中 使用的 user指令。
>默认的用户名是nobody。
>
>--group=name  设置nginx工作进程的用户组。安装完成后，可以随时更改的名称在nginx.conf配置文件中 使用的 user指令。默认的为非特权用户。
>--with-select_module --without-select_module 启用或禁用构建一个模块来允许服务器使用select()方法。该模块将自动建立，
>如果平台不支持的kqueue，epoll，rtsig或/dev/poll。
>--with-poll_module --without-poll_module 启用或禁用构建一个模块来允许服务器使用poll()方法。该模块将自动建立，如果平台不支持的kqueue，epoll，rtsig或/dev/poll。
>--without-http_gzip_module — 不编译压缩的HTTP服务器的响应模块。编译并运行此模块需要zlib库。
>--without-http_rewrite_module  不编译重写模块。编译并运行此模块需要PCRE库支持。
>--without-http_proxy_module — 不编译http_proxy模块。
>--with-http_ssl_module — 使用https协议模块。默认情况下，该模块没有被构建。建立并运行此模块的OpenSSL库是必需的。
>--with-pcre=path — 设置PCRE库的源码路径。PCRE库的源码（版本4.4 - 8.30）需要从PCRE网站下载并解压。其余的工作是Nginx的./ configure和make来完成。
>正则表达式使用在location指令和 ngx_http_rewrite_module 模块中。
>
>--with-pcre-jit —编译PCRE包含“just-in-time compilation”（1.1.12中， pcre_jit指令）。
>--with-zlib=path —设置的zlib库的源码路径。要下载从 zlib（版本1.1.3 - 1.2.5）的并解压。其余的工作是Nginx的./ configure和make完成。
>ngx_http_gzip_module模块需要使用zlib 。
>--with-cc-opt=parameters — 设置额外的参数将被添加到CFLAGS变量。
>例如,当你在FreeBSD上使用PCRE库时需要使用:--with-cc-opt="-I /usr/local/include。.如需要需要增加 select()支持的文件数量:--with-cc-opt="-D FD_SETSIZE=2048".
>--with-ld-opt=parameters —设置附加的参数，将用于在链接期间。例如，当在FreeBSD下使用该系统的PCRE库,应指定:--with-ld-opt="-L /usr/local/lib".
>典型实例(下面为了展示需要写在多行，执行时内容需要在同一行)
>
>./configure
>    --sbin-path=/usr/local/nginx/nginx
>    --conf-path=/usr/local/nginx/nginx.conf
>    --pid-path=/usr/local/nginx/nginx.pid
>    --with-http_ssl_module
>    --with-pcre=../pcre-4.4
>    --with-zlib=../zlib-1.1.3

### 4. nginx rewrite
>Nginx Rewrite规则相关指令 
> Nginx Rewrite规则相关指令有if、rewrite、set、return、break等，其中rewrite是最关键的指令。一个简单的Nginx Rewrite规则语法如下：
>
> rewrite ^/b/(.*)\.html /play.php?video=$1 break;
>
> 如果加上if语句，示例如下：
>
> if (!-f $request_filename)
>
> rewrite ^/img/(.*)$ /site/$host/images/$1 last;
>
> Nginx与Apache的Rewrite规则实例对比
>
> 简单的Nginx和Apache 重写规则区别不大，基本上能够完全兼容。例如：
>
> Apache Rewrite 规则：
>
> RewriteRule ^/(mianshi|xianjing)/$ /zl/index.php?name=$1 [L]
>
> RewriteRule ^/ceshi/$ /zl/ceshi.php [L]
>
> RewriteRule ^/(mianshi)_([a-zA-Z]+)/$ /zl/index.php?name=$1_$2 [L]
>
> RewriteRule ^/pingce([0-9]*)/$ /zl/pingce.php?id=$1 [L]
>
> Nginx Rewrite 规则：
>
> rewrite ^/(mianshi|xianjing)/$ /zl/index.php?name=$1 last;
>
> rewrite ^/ceshi/$ /zl/ceshi.php last;
>
> rewrite ^/(mianshi)_([a-zA-Z]+)/$ /zl/index.php?name=$1_$2 last;
>
> rewrite ^/pingce([0-9]*)/$ /zl/pingce.php?id=$1 last;
>
> 由以上示例可以看出，Apache的Rewrite规则改为Nginx的Rewrite规则，其实很简单：Apache的RewriteRule指令换成Nginx的rewrite指令，Apache的[L]标记换成Nginx的last标记，中间的内容不变。
>
> 如果Apache的Rewrite规则改为Nginx的Rewrite规则后，使用nginx -t命令检查发现nginx.conf配置文件有语法错误，那么可以尝试给条件加上引号。例如一下的Nginx Rewrite规则会报语法错误：
>
> rewrite ^/([0-9]{5}).html$ /x.jsp?id=$1 last;
>
> 加上引号就正确了：
> rewrite “^/([0-9]{5}).html$” /x.jsp?id=$1 last;
>
> Apache与Nginx的Rewrite规则在URL跳转时有细微的区别：
>
> Apache Rewrite 规则：
> RewriteRule ^/html/tagindex/([a-zA-Z]+)/.*$ /$1/ [R=301,L]
>
> Nginx Rewrite 规则：
> rewrite ^/html/tagindex/([a-zA-Z]+)/.*$ http://$host/$1/ permanent;
>
> 以上示例中，我们注意到，Nginx Rewrite 规则的置换串中增加了“http://$host”，这是在Nginx中要求的。
>
> 另外，Apache与Nginx的Rewrite规则在变量名称方面也有区别，例如：
>
> Apache Rewrite 规则：
> RewriteRule ^/user/login/$ /user/login.php?login=1&forward=http://%{HTTP_HOST} [L]
>
> Nginx Rewrite 规则：
> rewrite ^/user/login/$ /user/login.php?login=1&forward=http://$host   last;
>
> Apache与Nginx Rewrite 规则的一些功能相同或类似的指令、标记对应关系：
>
> Apache的RewriteCond指令对应Nginx的if指令；
> Apache的RewriteRule指令对应Nginx的rewrite指令；
> Apache的[R]标记对应Nginx的redirect标记；
> Apache的[P]标记对应Nginx的last标记；
> Apache的[R,L]标记对应Nginx的redirect标记；
> Apache的[P,L]标记对应Nginx的last标记；
> Apache的[PT,L]标记对应Nginx的last标记；
>
> 允许指定的域名访问本站，其他域名一律跳转到http://www.aaa.com
>
> Apache Rewrite 规则：
> RewriteCond %{HTTP_HOST}    ^(.*?)\.domain\.com$
> RewriteCond %{HTTP_HOST}    !^qita\.domain\.com$
> RewriteCond %{DOCUMENT_ROOT}/market/%1/index.htm -f
> RewriteRule ^/wu/$ /market/%1/index.htm [L]
>
> Nginx的if指令不支持嵌套，也不支持AND、OR等多条件匹配，相比于Apache的RewriteCond，显得麻烦一些，但是，我们可以通过下一页的Nginx配置写法来实现这个示例：
>
> Nginx Rewrite 规则：
> if ($host ~* ^(.*?)\.domain\.com$) set $var_wupin_city $1;
>   set $var_wupin ‘1′;
>
> if ($host ~* ^qita\.domain\.com$)
>
>   set $var_wupin ‘0′;
>
> if (!-f $document_root/market/$var_wupin_city/index.htm)
>
>   set $var_wupin ‘0′;
>
> if ($var_wupin ~ ‘1′)
>
>   rewrite ^/wu/$ /market/$var_wupin_city/index.htm last;
> }
>
> 
>
> rewrite 的语法
>
> 
>
> 语法: rewrite regex replacement flag
>
> 默认: none
>
> 作用域: server, location, if
>
> This directive changes URI in accordance with the regular expression and the replacement string. Directives are carried out in order of appearance in the configuration file.
>
> 这个指令根据表达式来更改URI，或者修改字符串。指令根据配置文件中的顺序来执行。
>
> Be aware that the rewrite regex only matches the relative path instead of the absolute URL. If you want to match the hostname, you should use an if condition, like so:
>
> last     继续向下匹配
> break    不向下匹配
> redirect   302 临时
> permanent   301 永久
>
> 注意重写表达式只对相对路径有效。如果你想配对主机名，你应该使用if语句。
>
> rewrite只是会改写路径部分的东东，不会改动用户的输入参数，因此这里的if规则里面，你无需关心用户在浏览器里输入的参数，rewrite后会自动添加的，因此，我们只是加上了一个？号和后面我们想要的一个小小的参数 ***https=1就可以了。
>
> nginx的rewrite规则参考：
>
>   ~ 为区分大小写匹配
>   ~* 为不区分大小写匹配
>   !~和!~*分别为区分大小写不匹配及不区分大小写不匹
>
>   -f和!-f用来判断是否存在文件
>   -d和!-d用来判断是否存在目录
>   -e和!-e用来判断是否存在文件或目录
>   -x和!-x用来判断文件是否可执行
>
>   last 相当于Apache里的[L]标记，表示完成rewrite，呵呵这应该是最常用的
>   break 终止匹配, 不再匹配后面的规则
>   redirect 返回302临时重定向 地址栏会显示跳转后的地址
>   permanent 返回301永久重定向 地址栏会显示跳转后的地址
>
> last 
> break
> redirect
> permanent
>
>   $args
>   $content_length
>   $content_type
>   $document_root
>   $document_uri
>   $host
>   $http_user_agent
>   $http_cookie
>   $limit_rate
>   $request_body_file
>   $request_method
>   $remote_addr
>   $remote_port
>   $remote_user
>   $request_filename
>   $request_uri
>   $query_string
>   $scheme
>   $server_protocol
>   $server_addr
>   $server_name
>   $server_port
>   $uri
>
> 结合QeePHP的例子
>
>   if (!-d $request_filename) {
>   rewrite ^/([a-z-A-Z]+)/([a-z-A-Z]+)/?(.*)$ /index.php?namespace=user&amp;controller=$1&amp;action=$2&amp;$3 last;
>   rewrite ^/([a-z-A-Z]+)/?$ /index.php?namespace=user&amp;controller=$1 last;
>   break;
>
> 多目录转成参数
> abc.domian.com/sort/2 => abc.domian.com/index.php?act=sort&name=abc&id=2
>
>   if ($host ~* (.*)\.domain\.com) {
>   set $sub_name $1;
>   rewrite ^/sort\/(\d+)\/?$ /index.php?act=sort&cid=$sub_name&id=$1 last;
>   }
>
> 目录对换
> /123456/xxxx -> /xxxx?id=123456
>
>   rewrite ^/(\d+)/(.+)/ /$2?id=$1 last;
>
> 例如下面设定nginx在用户使用ie的使用重定向到/nginx-ie目录下：
>
>   if ($http_user_agent ~ MSIE) {
>   rewrite ^(.*)$ /nginx-ie/$1 break;
>   }
>
> 目录自动加“/”
>
>   if (-d $request_filename){
>   rewrite ^/(.*)([^/])$ http://$host/$1$2/ permanent;
>   }
>
> 禁止htaccess
>
>   location ~/\.ht {
>   deny all;
>   }
>
> 禁止多个目录
>
>   location ~ ^/(cron|templates)/ {
>   deny all;
>   break;
>   }
>
> 禁止以/data开头的文件
> 可以禁止/data/下多级目录下.log.txt等请求;
>
>   location ~ ^/data {
>   deny all;
>   }
>
> 禁止单个目录
> 不能禁止.log.txt能请求
>
>   location /searchword/cron/ {
>   deny all;
>   }
>
> 禁止单个文件
>
>   location ~ /data/sql/data.sql {
>   deny all;
>   }
>
> 给favicon.ico和robots.txt设置过期时间;
> 这里为favicon.ico为99天,robots.txt为7天并不记录404错误日志
>
>   location ~(favicon.ico) {
>   log_not_found off;
>   expires 99d;
>   break;
>   }
>
>   location ~(robots.txt) {
>   log_not_found off;
>   expires 7d;
>   break;
>   }
>
> 设定某个文件的过期时间;这里为600秒，并不记录访问日志
>
>   location ^~ /html/scripts/loadhead_1.js {
>   access_log   off;
>   root /opt/lampp/htdocs/web;
>   expires 600;
>   break;
>   }
>
> 文件反盗链并设置过期时间
> 这里的return 412 为自定义的http状态码，默认为403，方便找出正确的盗链的请求
> “rewrite ^/ http://leech.divmy.com/leech.gif;”显示一张防盗链图片
> “access_log off;”不记录访问日志，减轻压力
> “expires 3d”所有文件3天的浏览器缓存
>
>   location ~* ^.+\.(jpg|jpeg|gif|png|swf|rar|zip|css|js)$ {
>   valid_referers none blocked *.c1gstudio.com *.c1gstudio.net localhost 208.97.167.194;
>   if ($invalid_referer) {
>   rewrite ^/ http://leech.divmy.com/leech.gif;
>   return 412;
>   break;
>   }
>   access_log   off;
>   root /opt/lampp/htdocs/web;
>   expires 3d;
>   break;
>   }
>
> 只充许固定ip访问网站，并加上密码
>
>   root  /opt/htdocs/www;
>   allow   208.97.167.194;
>   allow   222.33.1.2;
>   allow   231.152.49.4;
>   deny    all;
>   auth_basic “C1G_ADMIN”;
>   auth_basic_user_file htpasswd;
>
> 将多级目录下的文件转成一个文件，增强seo效果
> /job-123-456-789.html 指向/job/123/456/789.html
>
>   rewrite ^/job-([0-9]+)-([0-9]+)-([0-9]+)\.html$ /job/$1/$2/jobshow_$3.html last;
>
> 将根目录下某个文件夹指向2级目录
> 如/shanghaijob/ 指向 /area/shanghai/
> 如果你将last改成permanent，那么浏览器地址栏显是/location/shanghai/
>
>   rewrite ^/([0-9a-z]+)job/(.*)$ /area/$1/$2 last;
>
> 上面例子有个问题是访问/shanghai 时将不会匹配
>
>   rewrite ^/([0-9a-z]+)job$ /area/$1/ last;
>   rewrite ^/([0-9a-z]+)job/(.*)$ /area/$1/$2 last;
>
> 这样/shanghai 也可以访问了，但页面中的相对链接无法使用，
> 如./list_1.html真实地址是/area/shanghia/list_1.html会变成/list_1.html,导至无法访问。
>
> 那我加上自动跳转也是不行咯
> (-d $request_filename)它有个条件是必需为真实目录，而我的rewrite不是的，所以没有效果
>
>   if (-d $request_filename){
>   rewrite ^/(.*)([^/])$ http://$host/$1$2/ permanent;
>   }
>
> 知道原因后就好办了，让我手动跳转吧
>
>   rewrite ^/([0-9a-z]+)job$ /$1job/ permanent;
>   rewrite ^/([0-9a-z]+)job/(.*)$ /area/$1/$2 last;
>
> 文件和目录不存在的时候重定向：
>
>   if (!-e $request_filename) {
>   proxy_pass http://127.0.0.1;
>   }
>
> 域名跳转
>
>   server
>   {
>   listen       80;
>   server_name  jump.88dgw.com;
>   index index.html index.htm index.php;
>   root  /opt/lampp/htdocs/www;
>   rewrite ^/ http://www.88dgw.com/;
>   access_log  off;
>   }
>
> 多域名转向
>
>   server_name  www.7oom.com/  www.divmy.com/;
>   index index.html index.htm index.php;
>   root  /opt/lampp/htdocs;
>   if ($host ~ “c1gstudio\.net”) {
>   rewrite ^(.*) http://www.7oom.com$1/ permanent;
>   }
>
> 三级域名跳转
>
>   if ($http_host ~* “^(.*)\.i\.c1gstudio\.com$”) {
>   rewrite ^(.*) http://top.88dgw.com$1/;
>   break;
>   }
>
> 域名镜向
>
>   server
>   {
>   listen       80;
>   server_name  mirror.c1gstudio.com;
>   index index.html index.htm index.php;
>   root  /opt/lampp/htdocs/www;
>   rewrite ^/(.*) http://www.divmy.com/$1 last;
>   access_log  off;
>   }
>
> 某个子目录作镜向
>
>   location ^~ /zhaopinhui {
>   rewrite ^.+ http://zph.divmy.com/ last;
>   break;
>   }
>
> discuz ucenter home (uchome) rewrite
>
>   rewrite ^/(space|network)-(.+)\.html$ /$1.php?rewrite=$2 last;
>   rewrite ^/(space|network)\.html$ /$1.php last;
>   rewrite ^/([0-9]+)$ /space.php?uid=$1 last;
>
> discuz 7 rewrite
>
>   rewrite ^(.*)/archiver/((fid|tid)-[\w\-]+\.html)$ $1/archiver/index.php?$2 last;
>   rewrite ^(.*)/forum-([0-9]+)-([0-9]+)\.html$ $1/forumdisplay.php?fid=$2&page=$3 last;
>   rewrite ^(.*)/thread-([0-9]+)-([0-9]+)-([0-9]+)\.html$ $1/viewthread.php?tid=$2&extra=page\%3D$4&page=$3 last;
>   rewrite ^(.*)/profile-(username|uid)-(.+)\.html$ $1/viewpro.php?$2=$3 last;
>   rewrite ^(.*)/space-(username|uid)-(.+)\.html$ $1/space.php?$2=$3 last;
>   rewrite ^(.*)/tag-(.+)\.html$ $1/tag.php?name=$2 last;
>
> 给discuz某版块单独配置域名
>
>   server_name  bbs.c1gstudio.com news.c1gstudio.com;
>
>   location = / {
>   if ($http_host ~ news\.divmy.com$) {
>   rewrite ^.+ http://news.divmy.com/forum-831-1.html last;
>   break;
>   }
>   }
>
> discuz ucenter 头像 rewrite 优化
>
>   location ^~ /ucenter {
>   location ~ .*\.php?$
>   {
>   \#fastcgi_pass  unix:/tmp/php-cgi.sock;
>   fastcgi_pass  127.0.0.1:9000;
>   fastcgi_index index.php;
>   include fcgi.conf;
>   }
>
>   location /ucenter/data/avatar {
>   log_not_found off;
>   access_log   off;
>   location ~ /(.*)_big\.jpg$ {
>   error_page 404 /ucenter/images/noavatar_big.gif;
>   }
>   location ~ /(.*)_middle\.jpg$ {
>   error_page 404 /ucenter/images/noavatar_middle.gif;
>   }
>   location ~ /(.*)_small\.jpg$ {
>   error_page 404 /ucenter/images/noavatar_small.gif;
>   }
>   expires 300;
>   break;
>   }
>   }
>
> jspace rewrite
>
>   location ~ .*\.php?$
>   {
>   \#fastcgi_pass  unix:/tmp/php-cgi.sock;
>   fastcgi_pass  127.0.0.1:9000;
>   fastcgi_index index.php;
>   include fcgi.conf;
>   }
>
>   location ~* ^/index.php/
>   {
>   rewrite ^/index.php/(.*) /index.php?$1 break;
>   fastcgi_pass  127.0.0.1:9000;
>   fastcgi_index index.php;
>   include fcgi.conf;
>   }
>
> 另外这里还有一个工具可以直接把apache规则转化为nginx规则
>
> http://www.anilcetin.com/convert-apache-htaccess-to-nginx/
>
> 参考：
>
> http://wiki.nginx.org/NginxChsHttpRewriteModule
>
> http://blog.csdn.net/cnbird2008/archive/2009/08/04/4409620.aspx
> http://www.divmy.com/

### 5. nginx + keepalived双机热备
>keepalived+nginx双机热备+负载均衡
>
>最近因业务扩展，需要将当前的apache 转为nginx(web), 再在web前端放置nginx(负载均衡)。同时结合keepalived 对前端nginx实现HA。
>nginx进程基于于Master+Slave(worker)多进程模型，自身具有非常稳定的子进程管理功能。在Master进程分配模式下，Master进程永远不进行业务处理，只是进行任务分发，从而达到Master进程的存活高可靠性，Slave(worker)进程所有的业务信号都 由主进程发出，Slave(worker)进程所有的超时任务都会被Master中止，属于非阻塞式任务模型。
>Keepalived是Linux下面实现VRRP 备份路由的高可靠性运行件。基于Keepalived设计的服务模式能够真正做到主服务器和备份服务器故障时IP瞬间无缝交接。二者结合，可以构架出比较稳定的软件lb方案。
>
>
>
>
>准备4台电脑来做这个实验:
>
>192.168.232.132        web服务器
>192.168.232.133        web服务器
>192.168.232.134        keepalived nginx
>192.168.232.135        keepalived nginx
>
>虚拟IP (VIP)：192.168.232.16
>134\135两个主机配置虚拟IP
>
>下面以135为例：
>
>vi /etc/sysconfig/network-scripts/ifcfg-eth2:0
>[plain] view plain copy print?
>1.DEVICE=eth2:0  
>2.TYPE=Ethernet  
>3.ONBOOT=yes  
>4.BOOTPROTO=static  
>5.DNS1=192.168.232.2  
>6.IPADDR=192.168.232.16  
>7.NETMASK=255.255.255.0  
>8.GETWAY=192.168.232.2  
>
>
>
>service network restart
>
>使用ifconfig查看效果：
>[plain] view plain copy print?
>   1.eth2      Link encap:Ethernet  HWaddr 00:0C:29:49:90:5B    
>
>2.          inet addr:192.168.232.135  Bcast:192.168.232.255  Mask:255.255.255.0  
>3.          inet6 addr: fe80::20c:29ff:fe49:905b/64 Scope:Link  
>4.          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1  
>5.          RX packets:66322 errors:0 dropped:0 overruns:0 frame:0  
>6.          TX packets:31860 errors:0 dropped:0 overruns:0 carrier:0  
>7.          collisions:0 txqueuelen:1000  
>8.          RX bytes:67624991 (64.4 MiB)  TX bytes:2723877 (2.5 MiB)  
>9.          Interrupt:19 Base address:0x2000  
>10.          
>11.          eth2:0    Link encap:Ethernet  HWaddr 00:0C:29:49:90:5B    
>12.          inet addr:192.168.232.16  Bcast:192.168.232.255  Mask:255.255.255.0  
>13.          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1  
>14.          Interrupt:19 Base address:0x2000  
>15.          
>16.          lo        Link encap:Local Loopback    
>17.          inet addr:127.0.0.1  Mask:255.0.0.0  
>18.          inet6 addr: ::1/128 Scope:Host  
>19.          UP LOOPBACK RUNNING  MTU:16436  Metric:1  
>20.          RX packets:22622 errors:0 dropped:0 overruns:0 frame:0  
>21.          TX packets:22622 errors:0 dropped:0 overruns:0 carrier:0  
>22.          collisions:0 txqueuelen:0  
>23.          RX bytes:1236328 (1.1 MiB)  TX bytes:1236328 (1.1 MiB)  
>
>
>​          
>说明生效了。
>134\135两个主机安装keepalived和nginx
>
>nginx安装：
>
>1、导入外部软件库
>rpm -Uvh http://dl.iuscommunity.org/pub/ius/stable/Redhat/6/i386/epel-release-6-5.noarch.rpm
>rpm -Uvh http://dl.iuscommunity.org/pub/ius/stable/Redhat/6/i386/ius-release-1.0-10.ius.el6.noarch.rpm
>rpm -Uvh http://nginx.org/packages/centos/6/noarch/RPMS/nginx-release-centos-6-0.el6.ngx.noarch.rpm
>以下添加注释
>mirrorlist=http://dmirr.iuscommunity.org/mirrorlist?repo=ius-el6&arch=$basearch
>以下删除注释
>#baseurl=http://dl.iuscommunity.org/pub/ius/stable/Redhat/5/$basearch
>2、yum安装nginx
>yum install nginx
>
>keepalived安装：
>
>安装依赖
>yum -y install gcc gcc+ gcc-c++
>yum install popt-devel openssl openssl-devel libssl-dev libnl-devel popt-devel
>
>安装内核
>yum -y install kernel kernel-devel
>当前kernel代码建立连接 ln -s /usr/src/kerners/2.6....../ /usr/src/linux
>
>安装keepalived
>wget http://www.keepalived.org/software/keepalived-1.2.2.tar.gz
>tar -zxvf keepalived-1.2.2.tar.gz  
>cd keepalived-1.2.2  
>./configure  
>make  
>make install   
>
>拷贝相应的文件
>
>cp /usr/local/sbin/keepalived /usr/sbin/ 
>cp /usr/local/etc/rc.d/init.d/keepalived /etc/init.d/  
>cp /usr/local/etc/sysconfig/keepalived /etc/sysconfig/  
>cp -r /usr/local/etc/keepalived/ /etc/  
>配置keeplived和nginx主机
>
>134/135执行都执行以下操作：
>vi /etc/nginx/conf.d/default.conf
>[plain] view plain copy print?
>1.server {  
>2.    listen       8088;  
>3.    server_name  localhost;  
>4.  
>5.    location / {  
>6.        root   /var/www/html;  
>7.        index  index.html index.htm;  
>8.    }  
>9.  
>10.    error_page   500 502 503 504  /50x.html;  
>11.    location = /50x.html {  
>12.        root   /usr/share/nginx/html;  
>13.    }  
>14.      }  
>
>135执行以下操作：
>vi /var/www/html/index.html
>[plain] view plain copy print?
>1.<html>    
>2.<head>    
>3.<title>Welcome to nginx!</title>    
>4.</head>    
>5.<body bgcolor="white" text="black">    
>6.<center><h1>Welcome to nginx! 192.168.232.135</h1></center>    
>7.</body>    
>8.</html>  
>
>134执行以下操作：
>vi /var/www/html/index.html
>[plain] view plain copy print?
>1.<html>    
>2.<head>    
>3.<title>Welcome to nginx!</title>    
>4.</head>    
>5.<body bgcolor="white" text="black">    
>6.<center><h1>Welcome to nginx! 192.168.232.134</h1></center>    
>7.</body>    
>8.</html>  
>
>134执行以下操作：
>vi /etc/keepalived/keepalived.conf
>[plain] view plain copy print?
>1.! Configuration File for keepalived  
>2.  
>3.  global_defs {  
>4.  notification_email {  
>5.  #acassen@firewall.loc  
>6.  #failover@firewall.loc  
>7.  #sysadmin@firewall.loc  
>8.  }  
>9.  #notification_email_from Alexandre.Cassen@firewall.loc  
>10.  #smtp_server 192.168.200.1  
>11.  #smtp_connect_timeout 30  
>12.  router_id LVS_DEVEL  
>13.  }  
>14.  
>15.  vrrp_script chk_http_port {  
>16.  script "</dev/tcp/127.0.0.1/8088"  
>17.  interval 1  
>18.  weight -2  
>19.  }  
>20.  
>21.  vrrp_instance VI_1 {  
>22.  state MASTER  
>23.  interface eth2  
>24.  virtual_router_id 51  
>25.  priority 100  
>26.  advert_int 1  
>27.  authentication {  
>28.  auth_type PASS  
>29.  auth_pass 1111  
>30.  }  
>31.  virtual_ipaddress {  
>32.  192.168.232.16  
>33.  }  
>34.  track_script {  
>35.  chk_http_port  
>36.  }  
>37.   }     
>
>135执行以下操作：
>vi /etc/keepalived/keepalived.conf
>[plain] view plain copy print?
>1.! Configuration File for keepalived  
>
>2.  
>3.global_defs {  
>4.   notification_email {  
>5.     #acassen@firewall.loc  
>6.     #failover@firewall.loc  
>7.     #sysadmin@firewall.loc  
>8.   }  
>9.   #notification_email_from Alexandre.Cassen@firewall.loc  
>10.   #smtp_server 192.168.200.1  
>11.   #smtp_connect_timeout 30  
>11.  router_id LVS_DEVEL  
>12.  }  
>13.  
>14.  vrrp_script chk_http_port {  
>15.  script "</dev/tcp/127.0.0.1/8088"  
>16.  interval 1  
>17.  weight -2  
>18.  }  
>19.  
>20.  vrrp_instance VI_1 {  
>21.  state BACKUP  
>22.  interface eth2  
>23.  virtual_router_id 51  
>24.  priority 99  
>25.  advert_int 1  
>26.  authentication {  
>27.  auth_type PASS  
>28.  auth_pass 1111  
>29.  }  
>30.  virtual_ipaddress {  
>31.  192.168.232.16  
>32.  }  
>33.  track_script {  
>34.  chk_http_port  
>35.  }  
>36.  }  
>
>Tips：
>
>state   参数值：主的是MASTER、备用的是BACKUP
>priority 参数值： MASTER > BACKUP
>virtual_router_id： 参数值要一样
>测试测试：
>
>两台测试机134\135均启动keepalived和nginx
>service keepalived restart
>service keepalived nginx
>
>验证nginx启动正常：
>访问 master：http://192.168.232.134:8088/
>访问 backup: http://192.168.232.135:8088/
>
>查看keepalived的日志信息：
>
>134\135均打开日志信息方便查看keepalived动态：
>tail -f /var/log/messages
>
>浏览器打开虚拟ip访问：http://192.168.232.16:8080/ ，此时显示IP为192.168.232.134
>服务器层的双机热备（比如服务器宕机、keepalived宕了）测试：
>
>kill 192.168.232.134（master) 的keepalived进程
>killall keepalived
>134的日志信息如下：
>[plain] view plain copy print?
>1.Jun 11 18:03:10 localhost Keepalived_vrrp: VRRP_Instance(VI_1) Sending gratuitous ARPs on eth1 for 192.168.232.16  
>2.Jun 11 18:03:15 localhost Keepalived_vrrp: VRRP_Instance(VI_1) Sending gratuitous ARPs on eth1 for 192.168.232.16  
>3.Jun 11 19:30:44 localhost Keepalived: Terminating on signal  
>4.Jun 11 19:30:44 localhost Keepalived: Stopping Keepalived v1.2.2 (06/10,2014)  
>5.Jun 11 19:30:44 localhost Keepalived_vrrp: Terminating VRRP child process on signal  
>6.Jun 11 19:30:44 localhost Keepalived_healthcheckers: Terminating Healthchecker child process on signal  
>
>
>135的日志信息如下：
>[plain] view plain copy print?
>1.Jun 11 19:30:50 localhost Keepalived_vrrp: VRRP_Instance(VI_1) setting protocol VIPs.  
>2.Jun 11 19:30:50 localhost Keepalived_vrrp: VRRP_Instance(VI_1) Sending gratuitous ARPs on eth2 for 192.168.232.16  
>3.Jun 11 19:30:50 localhost Keepalived_healthcheckers: Netlink reflector reports IP 192.168.232.16 added  
>4.Jun 11 19:30:55 localhost Keepalived_vrrp: VRRP_Instance(VI_1) Sending gratuitous ARPs on eth2 for 192.168.232.16  
>
>
>
>刷新http://192.168.232.16:8080/ ， 此时显示IP为192.168.232.135。
>
>再次启动192.168.232.134的keepalived进程，192.168.232.134会自动接管成为master，192.168.232.135自动转为backup，从测试结果看，备机能成功接管，已经实现了热备。
>应用层（web）的双机热备（比如nginx进程被意外kill、web端口不通）试验：
>
>关闭192.168.232.134（master) 的nginx服务：
>service nginx stop
>
>134的日志信息如下：
>[plain] view plain copy print?
>1.Jun 11 19:38:49 localhost Keepalived_vrrp: VRRP_Script(chk_http_port) failed  
>2.Jun 11 19:38:51 localhost Keepalived_vrrp: VRRP_Instance(VI_1) Received higher prio advert  
>3.Jun 11 19:38:51 localhost Keepalived_vrrp: VRRP_Instance(VI_1) Entering BACKUP STATE  
>4.Jun 11 19:38:51 localhost Keepalived_vrrp: VRRP_Instance(VI_1) removing protocol VIPs.  
>5.Jun 11 19:38:51 localhost Keepalived_healthcheckers: Netlink reflector reports IP 192.168.232.16 removed  
>
>135的日志信息如下：
>[plain] view plain copy print?
>1.Jun 11 19:38:52 localhost Keepalived_vrrp: VRRP_Instance(VI_1) forcing a new MASTER election  
>2.Jun 11 19:38:53 localhost Keepalived_vrrp: VRRP_Instance(VI_1) Transition to MASTER STATE  
>3.Jun 11 19:38:54 localhost Keepalived_vrrp: VRRP_Instance(VI_1) Entering MASTER STATE  
>4.Jun 11 19:38:54 localhost Keepalived_vrrp: VRRP_Instance(VI_1) setting protocol VIPs.  
>5.Jun 11 19:38:54 localhost Keepalived_vrrp: VRRP_Instance(VI_1) Sending gratuitous ARPs on eth2 for 192.168.232.16  
>6.Jun 11 19:38:54 localhost Keepalived_healthcheckers: Netlink reflector reports IP 192.168.232.16 added  
>7.Jun 11 19:38:59 localhost Keepalived_vrrp: VRRP_Instance(VI_1) Sending gratuitous ARPs on eth2 for 192.168.232.16  
>
>
>刷新http://192.168.232.16:8080/ ， 此时显示IP为192.168.232.135。
>
>再次启动192.168.232.134的nginx进程，192.168.232.134会自动接管成为master，192.168.232.135自动转为backup，从测试结果看，备机能成功接管，已经实现了热备。
>为什么主备的参数state都是MASTER，对的你没有看错确实要都设置成一样的，不然并不能实现我们想要的VIP漂浮的效果，我测试很久才发现的.state都设置成MASTER后，会根据
>priority的值大小竞争来决定谁是真正的MASTER，脚本检测也是在失败的时候会把权重减去相应的值，比如原来master（181）的priority=100，如果脚本检测到端口8088无法连接，
>就会priority-2=98，< S-B（150）的priority（99），此时 S-B（150） 将竞争成为master，这样就实现了web应用的热备。
>
>如果以上实验都没有问题了，那么就该nginx负载均衡的配置了，配置修改参见如下：http://blog.csdn.NET/e421083458/article/details/30086413

### 6. nginx动静分离
>随着Nginx高性能Web服务器大量被使用，目前Nginx最新稳定版为1.2.6，张宴兄在实际应用中大量使用Nginx，并分享Nginx高性能Web服务器知识，
>使得Nginx在国内也是飞速的发展。那今天咱们再来温习一下Nginx 动静分离知识，这里仅供参考。
>
>一、实践环境：
>
>系统版本：CentOS6.0 X86_64 
>
>Nginx版本：Nginx-1.2.6 
>
>Tomcat版本：Tomcat-6.0.18 
>二、Nginx安装：
>
>   实际环境中安装Nginx，首先需要安装pcre库，然后再安装Nginx：
>
>#安装pcre支持rewrite库,也可以安装源码，注*安装源码时，指定pcre路径为解压源码的路径，而不是编译后的路径，否则会报错。
>yum install pcre-devel pcre -y
>
>#下载Nginx源码包
>cd /usr/src ;wget -c http://nginx.org/download/nginx-1.2.6.tar.gz
>
>#解压Nginx源码包
>tar -xzf nginx-1.2.6.tar.gz 
>
>#进入解压目录，然后sed修改Nginx版本信息为TDTWS 
>cd nginx-1.2.6 ; sed -i -e 's/1.2.6//g' -e 's/nginx\//TDTWS/g' -e 's/"NGINX"/"TDTWS"/g' src/core/nginx.h 
>
>#预编译Nginx
>./configure --user=www --group=www --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module
>#.configure预编译成功后，执行make命令进行编译
>
>make
>#make执行成功后，执行make install 正式安装
>make install
>#自此Nginx安装完毕！！！ 
>三、配置Nginx：
>
>   这里鉴于我的51CTO博客已经有Tomcat安装和配置了，这里忽略，只配置Nginx。
>
>#进入Nginx应用目录 
>cd /usr/local/nginx/conf 
>#备份原nginx.conf文件 
>mv  nginx.conf  nginx.bak  
>   创建 vi nginx.conf ，并写入如下内容:
>
>user www www; 
>worker_processes 8; 
>worker_cpu_affinity 00000001 00000010 00000100 00001000 00010000 00100000 01000000 10000000; 
>pid /usr/local/nginx/nginx.pid; 
>
>worker_rlimit_nofile 102400; 
>
>events 
>{ 
>use epoll; 
>worker_connections 102400; 
>} 
>http 
>{ 
>  include       mime.types; 
>
>  default_type  application/octet-stream; 
>
>  fastcgi_intercept_errors on; 
>
>  charset  utf-8; 
>
>  server_names_hash_bucket_size 128; 
>  client_header_buffer_size 4k; 
>  large_client_header_buffers 4 32k; 
>  client_max_body_size 300m; 
>
>  sendfile on; 
>  tcp_nopush     on; 
>
>  keepalive_timeout 60; 
>
>  tcp_nodelay on; 
>
>  client_body_buffer_size  512k; 
>  proxy_connect_timeout    5; 
>  proxy_read_timeout       60; 
>  proxy_send_timeout       5; 
>  proxy_buffer_size        16k; 
>  proxy_buffers            4 64k; 
>  proxy_busy_buffers_size 128k; 
>  proxy_temp_file_write_size 128k; 
>
>  gzip on; 
>  gzip_min_length  1k; 
>  gzip_buffers     4 16k; 
>  gzip_http_version 1.1; 
>  gzip_comp_level 2; 
>  gzip_types       text/plain application/x-javascript text/css application/xml; 
>  gzip_vary on; 
>
>
>###2012-12-19 change nginx logs 
>
>log_format  main  '$http_x_forwarded_for - $remote_user [$time_local] "$request" ' 
>              '$status $body_bytes_sent "$http_referer" ' 
>              '"$http_user_agent"  $request_time $remote_addr'; 
>
>#这里为后端服务器wugk应用集群配置，根据后端实际情况修改即可，tdt_wugk为负载均衡名称，可以任意指定
>#但必须跟vhosts.conf虚拟主机的pass段一致，否则不能转发后端的请求。 
>upstream tdt_wugk { 
>    server   10.10.141.30:8080 weight=1 max_fails=2 fail_timeout=30s; 
>    server   10.10.141.30:8081 weight=1 max_fails=2 fail_timeout=30s; 
>    server   10.10.141.31:8080 weight=1 max_fails=2 fail_timeout=30s; 
>    server   10.10.141.31:8081 weight=1 max_fails=2 fail_timeout=30s; 
>    server   10.10.141.32:8080 weight=1 max_fails=2 fail_timeout=30s; 
>    server   10.10.141.32:8081 weight=1 max_fails=2 fail_timeout=30s; 
>} 
>#这里为后端APP应用负载均衡配置，根据后端实际情况修改即可。tdt_app为负载均衡名称，可以任意指定
>upstream tdt_app { 
>    server   10.10.141.40:8080 weight=1 max_fails=2 fail_timeout=30s; 
>    server   10.10.141.40:8081 weight=1 max_fails=2 fail_timeout=30s; 
>    server   10.10.141.41:8080 weight=1 max_fails=2 fail_timeout=30s; 
>    server   10.10.141.41:8081 weight=1 max_fails=2 fail_timeout=30s; 
>    server   10.10.141.42:8080 weight=1 max_fails=2 fail_timeout=30s; 
>    server   10.10.141.42:8081 weight=1 max_fails=2 fail_timeout=30s; 
>} 
>#include引用vhosts.conf，该文件主要用于配置Nginx 虚拟主机  
>include vhosts.conf; 
>} 
>   如上nginx.conf配置完毕,继续配置nginx虚拟主机，继续在当前目录创建vhosts.conf
>
>   vi vhosts.conf 内容如下：
>
>####www.wuguangke.cn 
>server 
>
>  { 
>    listen       80; 
>    server_name  www.wuguangke.cn; 
>    index index.html index.htm; 
>#配置发布目录为/data/www/wugk  
>    root  /data/www/wugk; 
>
>    location / 
>    { 
>         proxy_next_upstream http_502 http_504 error timeout invalid_header; 
>         proxy_set_header Host  $host; 
>         proxy_set_header X-Real-IP $remote_addr; 
>      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
>         proxy_pass http://tdt_wugk; 
>         expires      3d; 
>    } 
>    #动态页面交给http://tdt_wugk，也即我们之前在nginx.conf定义的upstream tdt_wugk 均衡 
>    location ~ .*\.(php|jsp|cgi)?$ 
>    { 
>         proxy_set_header Host  $host; 
>         proxy_set_header X-Real-IP $remote_addr; 
>      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
>         proxy_pass http://tdt_wugk; 
>    } 
>    #配置Nginx动静分离，定义的静态页面直接从Nginx发布目录读取。
>    location ~ .*\.(html|htm|gif|jpg|jpeg|bmp|png|ico|txt|js|css)$ 
>    { 
>    root /data/www/wugk; 
>#expires定义用户浏览器缓存的时间为3天，如果静态页面不常更新，可以设置更长，这样可以节省带宽和缓解服务器的压力
>    expires      3d; 
>    } 
>    #定义Nginx输出日志的路径 
>    access_log  /data/logs/nginx_wugk/access.log main; 
>    error_log   /data/logs/nginx_wugk/error.log  crit; 
>} 
>
>##########chinaapp.sinaapp.com 2012-12-19 
>  server 
>
>  { 
>    listen       80; 
>    server_name  chinaapp.sinaapp.com; 
>    index index.html index.htm; 
>    root  /data/www; 
>
>    location / 
>    { 
>         proxy_next_upstream http_502 http_504 error timeout invalid_header; 
>         proxy_set_header Host  $host; 
>         proxy_set_header X-Real-IP $remote_addr; 
>     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
>         proxy_pass http://tdt_app; 
>         expires      3d; 
>    } 
>     
>    location ~ .*\.(php|jsp|cgi)?$ 
>    { 
>         proxy_set_header Host  $host; 
>         proxy_set_header X-Real-IP $remote_addr; 
>      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
>         proxy_pass http://tdt_app; 
>    } 
>    location ~ .*\.(html|htm|gif|jpg|jpeg|bmp|png|ico|txt|js|css)$ 
>    { 
>    root /data/www/app; 
>    expires      3d; 
>    } 
>     
>    access_log  /data/logs/nginx_app/access.log main; 
>    error_log   /data/logs/nginx_app/error.log  crit; 
>} 
>四、部署测试：
>
>   后端配置好Tomcat服务，并启动，发布的程序需同步到Nginx的/data/www对应的目录，因为配置动静分离后，用户请求你定义的静态页面，默认会去nginx的发布目录请求，而不会到后端请求，所以这时候你要保证后端跟前端的程序保持一致，可以使用Rsync做服务端自动同步。
>
>#检查Nginx配置文件是否配置正确，提示Ok and successful表示正确，如下：  
>[root@WEB-11-151 ~]# /usr/local/nginx/sbin/nginx -t
>nginx: the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok
>nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful
>#启动Nginx服务 
>/usr/local/nginx/sbin/nginx 
>#查看Nginx进程是否启动 
>ps -ef |grep nginx  

### 7. nginx访问日志屏蔽模块
>Nginx日志过滤 使用ngx_log_if不记录特定日志
>
>ngx_log_if是Nginx的一个第三方模块。它在Github上的描述是这样介绍的：ngx_log_if是一个独立的模块,允许您控制不要写的访问日志,类似于Apache的"CustomLog env = XXX"
>
>第一步：
>先到Github下载ngx_log_if地址https://github.com/cfsego/ngx_log_if/
>
>第二步：
>安装第三方模块到Nginx。第三方模块的安装可以参考http://wiki.nginx.org/3rdPartyModules 使用--add-module添加解压后的ngx_log_if如下
>[plain] view plain copy
>1../configure --add-module=/var/local/ngx_log_if-master  
>然后再编译安装Nginx即可。
>
>第三步：
>配置access_log_bypass_if到nginx.conf配置文件中
>[plain] view plain copy
>1.server {  
>2.    location / {  
>3.        access_log_bypass_if ($status = 404); #不记录404状态的所有日志信息  
>4.        access_log_bypass_if ($uri ~* 'images'); #不记录uri中所有images目录下文件的日志信息  
>5.        access_log_bypass_if ($uri = '/index.html'); #不记录uri为/index.html的日志信息  
>6.    access_log_bypass_if ($host ~* 'tonv.cc'); #不记录host为tonv.cc的所有日志信息  
>7.    }  
>8.    }  
>
>重启Nginx即可过滤不显示特定的日志了

### 8. nginx http头(增加修改模块)
>ngx_http_headers_module模块
>一. 前言
>ngx_http_headers_module模块提供了两个重要的指令add_header和expires，来添加 “Expires” 和 “Cache-Control” 头字段，对响应头添加任何域字段。add_header可以用来标示请求访问到哪台服务器上，这个也可以通过nginx模块nginx-http-footer-filter研究使用来实现。expires指令用来对浏览器本地缓存的控制。
>二. add_header指令
>语法: add_header name value;
>默认值: —
>配置段: http, server, location, if in location
>对响应代码为200，201，204，206，301，302，303，304，或307的响应报文头字段添加任意域。如：
>
>add_header From jb51.net
>三. expires指令
>语法: expires [modified] time;
>expires epoch | max | off;
>默认值: expires off;
>配置段: http, server, location, if in location
>在对响应代码为200，201，204，206，301，302，303，304，或307头部中是否开启对“Expires”和“Cache-Control”的增加和修改操作。
>可以指定一个正或负的时间值，Expires头中的时间根据目前时间和指令中指定的时间的和来获得。
>epoch表示自1970年一月一日00:00:01 GMT的绝对时间，max指定Expires的值为2037年12月31日23:59:59，Cache-Control的值为10 years。
>Cache-Control头的内容随预设的时间标识指定：
>·设置为负数的时间值:Cache-Control: no-cache。
>·设置为正数或0的时间值：Cache-Control: max-age = #，这里#的单位为秒，在指令中指定。
>参数off禁止修改应答头中的"Expires"和"Cache-Control"。
>实例一：对图片，flash文件在浏览器本地缓存30天
>
>location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
> {
>      expires 30d;
> }
>
>实例二：对js，css文件在浏览器本地缓存1小时
>
>location ~ .*\.(js|css)$
> {
>      expires 1h;
> }
>
>ngx_headers_more模块
>一. 介绍ngx_headers_more
>ngx_headers_more 用于添加、设置和清除输入和输出的头信息。nginx源码没有包含该模块，需要另行添加。
>该模块是ngx_http_headers_module模块的增强版，提供了更多的实用工具，比如复位或清除内置头信息，如Content-Type, Content-Length, 和Server。
>可以允许你使用-s选项指定HTTP状态码，使用-t选项指定内容类型，通过more_set_headers 和 more_clear_headers 指令来修改输出头信息。如：
>
>more_set_headers -s 404 -t 'text/html' 'X-Foo: Bar';
>
>输入头信息也可以这么修改，如：
>
>location /foo {
>  more_set_input_headers 'Host: foo' 'User-Agent: faked';
>  - now $host, $http_host, $user_agent, and
>  - $http_user_agent all have their new values.
>}
>
>-t选项也可以在more_set_input_headers和more_clear_input_headers指令中使用。
>不像标准头模块，该模块的指示适用于所有的状态码，包括4xx和5xx的。 add_header只适用于200，201，204，206，301，302，303，304，或307。
>
>二. 安装ngx_headers_more
>
>wget 'http://nginx.org/download/nginx-1.5.8.tar.gz'
>tar -xzvf nginx-1.5.8.tar.gz
>cd nginx-1.5.8/
>
> - Here we assume you would install you nginx under /opt/nginx/.
>./configure --prefix=/opt/nginx \
>  --add-module=/path/to/headers-more-nginx-module
>
> 
>
>make
>make install
>
>ngx_headers_more 包下载地址：http://github.com/agentzh/headers-more-nginx-module/tags
>ngx_openresty包含该模块。
>三. 指令说明
>more_set_headers
>语法：more_set_headers [-t <content-type list>]... [-s <status-code list>]... <new-header>...
>默认值：no
>配置段：http, server, location, location if
>阶段：输出报头过滤器
>替换（如有）或增加（如果不是所有）指定的输出头时响应状态代码与-s选项相匹配和响应的内容类型的-t选项指定的类型相匹配的。
>如果没有指定-s或-t，或有一个空表值，无需匹配。因此，对于下面的指定，任何状态码和任何内容类型都讲设置。
>
>more_set_headers  "Server: my_server";
>
>具有相同名称的响应头总是覆盖。如果要添加头，可以使用标准的add_header指令代替。
>单个指令可以设置/添加多个输出头。如：
>
>more_set_headers 'Foo: bar' 'Baz: bah';
>
>在单一指令中，选项可以多次出现，如：
>
> more_set_headers -s 404 -s '500 503' 'Foo: bar';
>
>等同于：
>
> more_set_headers -s '404 500 503' 'Foo: bar';
>
>新的头是下面形式之一：
>
>    Name: Value
>    Name:
>    Name
>
>最后两个有效清除的头名称的值。Nginx的变量允许是头值，如：
>
>set $my_var "dog";
>more_set_headers "Server: $my_var";
>
>注意：more_set_headers允许在location的if块中，但不允许在server的if块中。下面的配置就报语法错误：
>
> - This is NOT allowed!
> server {
>    if ($args ~ 'download') {
>      more_set_headers 'Foo: Bar';
>    }
>    ...
>  }
>more_clear_headers
>
>语法：more_clear_headers [-t <content-type list>]... [-s <status-code list>]... <new-header>...
>默认值：no
>配置段：http, server, location, location if
>阶段：输出报头过滤器
>清除指定的输出头。
>
>more_clear_headers -s 404 -t 'text/plain' Foo Baz;
>
>等同于
>
>more_set_headers -s 404 -t 'text/plain' "Foo: " "Baz: ";
>
>或
>
>more_clear_headers -s 404 -t 'text/plain' Foo Baz;
>
>等同于
>
>more_set_headers -s 404 -t 'text/plain' "Foo: " "Baz: ";
>
>或
>
>more_set_headers -s 404 -t 'text/plain' Foo Baz
>
>也可以使用通配符*，如：
>
>more_clear_headers 'X-Hidden-*';
>
>清除开始由“X-Hidden-”任何输出头。
>more_set_input_headers
>语法：more_set_input_headers [-r] [-t <content-type list>]... <new-header>...
>默认值：no
>配置段：http, server, location, location if
>阶段： rewrite tail
>非常类似more_set_headers，不同的是它工作在输入头（或请求头），它仅支持-t选项。
>注意：使用-t选项的是过滤请求头的Content-Type，而不是响应头的。
>more_clear_input_headers
>语法：more_clear_input_headers [-t <content-type list>]... <new-header>...
>默认值：no
>配置段：http, server, location, location if
>阶段： rewrite tail
>清除指定输入头。如：
>
>more_clear_input_headers -s 404 -t 'text/plain' Foo Baz;
>
>等同于
>
>more_set_input_headers -s 404 -t 'text/plain' "Foo: " "Baz: ";
>
>或
>
>more_clear_input_headers -s 404 -t 'text/plain' Foo Baz;
>
>等同于
>
>more_set_input_headers -s 404 -t 'text/plain' "Foo: " "Baz: ";
>
>或
>
>more_set_input_headers -s 404 -t 'text/plain' Foo Baz
>
>四. ngx_headers_more局限性
>1. 不同于标准头模块，该模块不会对下面头有效： Expires, Cache-Control, 和Last-Modified。
>2. 使用此模块无法删除Connection的响应报头。唯一方法是更改src/ HTTP/ ngx_http_header_filter_module.c文件。
>五. 使用ngx_headers_more
>
> - set the Server output header
>more_set_headers 'Server: my-server';
>
> - set and clear output headers
>location /bar {
>  more_set_headers 'X-MyHeader: blah' 'X-MyHeader2: foo';
>  more_set_headers -t 'text/plain text/css' 'Content-Type: text/foo';
>  more_set_headers -s '400 404 500 503' -s 413 'Foo: Bar';
>  more_clear_headers 'Content-Type';
>
>  - your proxy_pass/memcached_pass/or any other config goes here...
>}
>
> - set output headers
>location /type {
>  more_set_headers 'Content-Type: text/plain';
>  - ...
>}
>
> - set input headers
>location /foo {
>  set $my_host 'my dog';
>  more_set_input_headers 'Host: $my_host';
>  more_set_input_headers -t 'text/plain' 'X-Foo: bah';
>
>  - now $host and $http_host have their new values...
>  - ...
>}
>
> - replace input header X-Foo *only* if it already exists
>more_set_input_headers -r 'X-Foo: howdy';
>
>六. 应用ngx_headers_more
>修改web服务器是什么软件，什么版本，同时隐藏Centent-Type、Accept-Range、Content-Length头信息。
>
>2016113161809381.jpg (861×382)
>
>more_set_headers "Server: jb51.net Web Server";
>more_clear_headers "Content-Type:";
>more_clear_headers "Accept-Ranges: ";
>more_clear_headers "Content-Length: ";
>
>2016113161847323.jpg (827×380)
>
>404状态码添加header
>配置如下：
>
>more_set_headers "Server: jb51.net Web Server";
>more_set_headers -s 404 "Error: Not found";
>more_clear_headers "Content-Type:";
>more_clear_headers "Accept-Ranges: ";
>more_clear_headers "Content-Length: ";
>
>2016113161914791.jpg (836×354)

### 9. nginx平滑升级
>nginx的平滑升级，不间断服务
>
>Nginx更新真的很快，最近nginx的1.0.5稳定版，nginx的0.8.55和nginx的0.7.69旧的稳定版本已经发布。我一项比较喜欢使用新版本的软件，于是把原来的nginx-1.0.2平滑升级至nginx-1.0.5稳定版。并记录这一过程，参照这一过程也适用其他版本的升级，都是照葫芦画瓢的事情。希望对有需要的朋友有点帮助。
>
>1. 开始之前先查看一下当前使用的版本。
>
> - /usr/local/webserver/nginx/sbin/nginx -V
>nginx: nginx version: nginx/1.0.5
>nginx: built by gcc 4.1.2 20080704 (Red Hat 4.1.2-50)
>nginx: TLS SNI support disabled
>nginx: configure arguments: --user=www --group=www --prefix=/usr/local/webserver/nginx --with-http_stub_status_module --with-http_ssl_module --with-http_flv_module --with-cc-opt=-O3 --with-cpu-opt=opteron --with-http_gzip_static_module
>※ 注意红色区域，这是以前编译的参数。马上编辑新版本需要用到。
>
>2.下载新版本：http://nginx.org/en/download.html
>
>然后：解压 > 便以前的准备 > 编译
>
> - tar zxvf nginx-1.0.5.tar.gz
> - cd nginx-1.0.5
> - ./configure 
>--user=www 
>--group=www 
>--prefix=/usr/local/webserver/nginx 
>--with-http_stub_status_module 
>--with-http_ssl_module 
>--with-http_flv_module 
>--with-cc-opt='-O3' 
>--with-cpu-opt=opteron 
>--with-http_gzip_static_module
> - make
>3. 执行完后，这里不用在 make install 了，接下来重名/sbin/nginx为nginx.old
>
> - mv /usr/local/webserver/nginx/sbin/nginx /usr/local/webserver/nginx/sbin/nginx.old
>4. 复制编译后objs目录下的nginx文件到nginx的安装目录sbin/下
>
> - cp objs/nginx /usr/local/webserver/nginx/sbin/
>5. 测试一下新复制过来文件生效情况：
>
> - /usr/local/webserver/nginx/sbin/nginx -t
>nginx: the configuration file /usr/local/webserver/nginx/conf/nginx.conf syntax is ok
>nginx: configuration file /usr/local/webserver/nginx/conf/nginx.conf test is successful
>6. 让nginx把nginx.pid文件修改成nginx.pid.oldbin，随即启动nginx，实现不间断
>
> - kill -USR2 `cat /usr/local/webserver/nginx/nginx.pid`  更新配置文件
> - kill -QUIT `cat /usr/local/webserver/nginx/nginx.pid.oldbin` 优雅的关闭
>7. 升级完成了，最后在看一下升级后的版本
>
> - /usr/local/webserver/nginx/sbin/nginx -v
>nginx: nginx version: nginx/1.0.5

### 10. nginx常用模块
>nginx 常用模块整理
>原创
>
>1. 性能相关配置	
>worker_processes number | auto；
>worker进程的数量；通常应该为当前主机的cpu的物理核心数
>worker_cpu_affinity auto [cpumask] #将work进程绑定在固定cpu上提高缓存命中率 
>例：
>worker_cpu_affinity 0001 0010 0100 1000;
>worker_cpu_affinity 0101 1010;
>
>worker_priority number
>指定worker进程的nice值，设定worker进程优先级： [-20,20]
>
>worker_rlimit_nofile number
>worker进程所能够打开的文件数量上限,默认较小，生产中需要调大如65535
>
>2. 时间驱动events相关的配置
>
>worker_connections number
>每个worker进程所能够打开的最大并发连接数数量，如10240
>总最大并发数： worker_processes * worker_connections
>
>use method
>指明并发连接请求的处理方法,默认自动选择最优方法不用调整
>如：use epoll;
>
>accept_mutex on | off 互斥；
>处理新的连接请求的方法； on指由各个worker轮流处理新请求
>， Off指每个新请求的到达都会通知(唤醒)所有的worker进程，但
>只有一个进程可获得连接，造成“惊群”，影响性能，默认on
>
>3. http核心模块相关配置ngx_http_core_module
>
>3.1web服务模板
>
>server { ... }
>配置一个虚拟主机
>server {
>    listen address[:PORT]|PORT;
>    server_name SERVER_NAME;
>    root /PATH/TO/DOCUMENT_ROOT;
>} 
>注意：
>(1) 基于port；
>listen PORT; 指令监听在不同的端口
>(2) 基于ip的虚拟主机
>listen IP:PORT; IP 地址不同
>(3) 基于hostname
>server_name fqdn; 指令指向不同的主机名
>
>3.2套接字相关配置
>
>listen address[:port] [default_server] [ssl] [http2 | spdy] [backlog=number] [rcvbuf=size] [sndbuf=size]
>
>default_server 设定为默认虚拟主机
>ssl 限制仅能够通过ssl连接提供服务
>backlog=number 超过并发连接数后，新请求进入后援队列的长度
>rcvbuf=size 接收缓冲区大小
>sndbuf=size 发送缓冲区大小
>
>3.3 server_name
>	
>server_name name ...;
>支持*通配任意长度的任意字符
>server_name *.magedu.com www.magedu.*
>支持~起始的字符做正则表达式模式匹配，性能原因慎用
>server_name ~^www\d+\.magedu\.com$   #\d 表示 [0-9]
>匹配优先级机制从高到低：
>(1) 首先是字符串精确匹配 如： www.magedu.com
>(2) 左侧*通配符 如： *.magedu.com
>(3) 右侧*通配符 如： www.magedu.*
>(4) 正则表达式 如： ~^.*\.magedu\.com$
>(5) default_server
>
>3.4 延迟发送选项
>
>tcp_nodelay on | off;
>在keepalived模式下的连接是否启用TCP_NODELAY选项
>当为off时，延迟发送，合并多个请求后再发送
>默认On时，不延迟发送
>可用于： http, server, location
>
>3.5 sendfile
>
>sendfile on | off;
>是否启用sendfile功能，在内核中封装报文直接发送
>默认Off
>
>3.6 隐藏版本信息
>
>server_tokens on | off | build | string
>是否在响应报文的Server首部显示nginx版本
>
>3.7 location匹配
>
>location [ = | ~ | ~* | ^~ ] uri { ... }
>location @name { ... }
>在一个server中location配置段可存在多个，用于实现从uri到文件系统的路径映射； ngnix会根据
>用户请求的URI来检查定义的所有location，并找出一个最佳匹配，而后应用其配置 
>示例：
>server {...
>    server_name www.magedu.com;
>    location /images/ {
>        root /data/imgs/;
>        }
>}
>http://www.magedu.com/images/logo.jpg
>--> /data/imgs/images/logo.jpg 
>=：对URI做精确匹配； 
>^~：对URI的最左边部分做匹配检查，不区分字符大小写
> ~：对URI做正则表达式模式匹配，区分字符大小写
> ~*：对URI做正则表达式模式匹配，不区分字符大小写
> 不带符号：匹配起始于此uri的所有的uri
> 匹配优先级从高到低：
>=, ^~, ～/～*, 不带符号
>
>3.7 路径别名alias path
>
>示例：
>http://www.magedu.com/bbs/index.php
>location /bbs/ {
>    alias /web/forum/;
>} --> /web/forum/index.html
>location /bbs/ {
>    root /web/forum/;
>}     --> /web/forum/bbs/index.html    
> 注意： location中使用root指令和alias指令的意义不同    
>(a) root，相当于追加在root目录后面  
>(b) alias，相当于对location中的url进行替换
>
>3.8 错误页面显示
>
>error_page code ... [=[response]] uri;
>模块： ngx_http_core_module
>定义错误页， 以指定的响应状态码进行响应
>可用位置： http, server, location, if in location
>error_page 404 /404.html
>error_page 404 =200 /404.html  #防止404页面被劫持
>
>3.9 长连接相关配置
>
>keepalive_timeout timeout [header_timeout];
>设定保持连接超时时长， 0表示禁止长连接， 默认为75s
>keepalive_requests number;
>在一次长连接上所允许请求的资源的最大数量，默认为100
>keepalive_disable none | browser ...
>对哪种浏览器禁用长连接
>send_timeout time;
>向客户端发送响应报文的超时时长，此处是指两次写操作之间的间隔时长，而非
>整个响应过程的传输时长
>
>3.10 请求报文缓存
>
>client_body_buffer_size size;
>用于接收每个客户端请求报文的body部分的缓冲区大小；默认为16k；超出此大小时，
>其将被暂存到磁盘上的由client_body_temp_path指令所定义的位置
>client_body_temp_path path [level1 [level2 [level3]]];
>设定用于存储客户端请求报文的body部分的临时存储路径及子目录结构和数量 
>目录名为16进制的数字；
>client_body_temp_path /var/tmp/client_body 1 2 2
>1 1级目录占1位16进制，即2^4=16个目录 0-f
>2 2级目录占2位16进制，即2^8=256个目录 00-ff
>2 3级目录占2位16进制， 即2^8=256个目录 00-ff
>
>3.11 对客户端进行限制相关配置
>
>limit_rate rate;
>限制响应给客户端的传输速率，单位是bytes/second 默认值0表示无限制
>limit_except method ... { ... }，仅用于location
>限制客户端使用除了指定的请求方法之外的其它方法 
>method:GET, HEAD, POST, PUT, DELETE，MKCOL, COPY, MOVE, OPTIONS, PROPFIND,
>PROPPATCH, LOCK, UNLOCK, PATCH
>例：
>limit_except GET {
>    allow 192.168.1.0/24;
>    deny all;
>} 
>除了GET和HEAD 之外其它方法仅允许192.168.1.0/24网段主机使用
>
>4. 访问控制模块ngx_http_access_module
>
>实现基于ip的访问控制功能
>	
>allow address | CIDR | unix: | all;
>deny address | CIDR | unix: | all;
>http, server, location, limit_except
>自上而下检查，一旦匹配，将生效，条件严格的置前
>示例：
>location / {
>    deny 192.168.1.1;
>    allow 192.168.1.0/24;
>    allow 10.1.1.0/16;
>    allow 2001:0db8::/32;
>    deny all;
>}
>
>5. 用户认证模块ngx_http_auth_basic_module
>
>实现基于用户的访问控制，使用basic机制进行用户认证
>	
>auth_basic string | off;
>auth_basic_user_file file;
>location /admin/ {
>    auth_basic "Admin Area";
>    auth_basic_user_file /etc/nginx/.ngxpasswd;
>}
>用户口令：
>1、明文文本：格式name:password:comment
>2、加密文本：由htpasswd命令实现 httpd-tools所提供
>
>6. 状态查看模块ngx_http_stub_status_module
>	
>
>用于输出nginx的基本状态信息
>Active connections:当前状态，活动状态的连接数
>accepts：统计总值，已经接受的客户端请求的总数
>handled：统计总值，已经处理完成的客户端请求的总数
>requests：统计总值，客户端发来的总的请求数
>Reading：当前状态，正在读取客户端请求报文首部的连接的连接数
>Writing：当前状态，正在向客户端发送响应报文过程中的连接数
>Waiting：当前状态，正在等待客户端发出请求的空闲连接数 
>示例：
>location /status {
>    stub_status;
>    allow 172.16.0.0/16;
>    deny all;
>}
>
>7. 日志记录模块ngx_http_log_module
>	
>
>1、 log_format name string ...;
>string可以使用nginx核心模块及其它模块内嵌的变量
>2、 access_log path [format [buffer=size] [gzip[=level]] [flush=time] [if=condition]];
>access_log off;
>访问日志文件路径，格式及相关的缓冲的配置
>buffer=size
>flush=time 
>示例
>log_format compression '$remote_addr-$remote_user [$time_local] '
>'"$request" $status $bytes_sent '
>'"$http_referer" "$http_user_agent" "$gzip_ratio"';
>access_log /spool/logs/nginx-access.log compression buffer=32k; 
>3、 open_log_file_cache max=N [inactive=time] [min_uses=N] [valid=time];
>open_log_file_cache off;
>缓存各日志文件相关的元数据信息
>max：缓存的最大文件描述符数量
>min_uses：在inactive指定的时长内访问大于等于此值方可被当作活动项
>inactive：非活动时长
>valid：验正缓存中各缓存项是否为活动项的时间间隔
>
>8. 压缩相关选项ngx_http_gzip_module
>
>1、gzip on | off;  #启用或禁用gzip压缩
>2、gzip_comp_level level;  #压缩比由低到高： 1 到 9  默认： 1
>3、gzip_disable regex ...;  #匹配到客户端浏览器不执行压缩
>4、gzip_min_length length;  #启用压缩功能的响应报文大小阈值 
>5、gzip_http_version 1.0 | 1.1; #设定启用压缩功能时，协议的最小版本 默认： 1.1
>6、gzip_buffers number size;
>支持实现压缩功能时缓冲区数量及每个缓存区的大小
>默认： 32 4k 或 16 8k
>7、gzip_types mime-type ...;
>指明仅对哪些类型的资源执行压缩操作；即压缩过滤器
>默认包含有text/html，不用显示指定，否则出错
>8、gzip_vary on | off;
>如果启用压缩，是否在响应报文首部插入“Vary: AcceptEncoding
>9、 gzip_proxied off | expired | no-cache | no-store |
>private | no_last_modified | no_etag | auth | any ...;
>nginx对于代理服务器请求的响应报文，在何种条件下启
>用压缩功能
>off：对被代理的请求不启用压缩
>expired,no-cache, no-store， private：对代理服务器
>请求的响应报文首部Cache-Control值任何一个，启用压缩功能
>示例：
>gzip on;
>gzip_comp_level 6;
>gzip_min_length 64;
>gzip_proxied any;
>gzip_types text/xml text/css application/javascript;
>
>9. https模块ngx_http_ssl_module模块：
>	
>
>1、 ssl on | off;
>为指定虚拟机启用HTTPS protocol， 建议用listen指令代替
>2、 ssl_certificate file;
>当前虚拟主机使用PEM格式的证书文件
>3、 ssl_certificate_key file;
>当前虚拟主机上与其证书匹配的私钥文件
>4、 ssl_protocols [SSLv2] [SSLv3] [TLSv1] [TLSv1.1] [TLSv1.2];
>支持ssl协议版本，默认为后三个
>5、 ssl_session_cache off | none | [builtin[:size]]
>[shared:name:size];
>builtin[:size]：使用OpenSSL内建缓存，为每worker进程私有
>[shared:name:size]：在各worker之间使用一个共享的缓存 
>6、 ssl_session_timeout time;
>客户端连接可以复用ssl session cache中缓存的ssl参数的有
>效时长，默认5m
>示例：
>server {
>    listen 443 ssl;
>    server_name www.magedu.com;
>    root /vhosts/ssl/htdocs;
>    ssl on;
>    ssl_certificate /etc/nginx/ssl/nginx.crt;
>    ssl_certificate_key /etc/nginx/ssl/nginx.key;
>    ssl_session_cache shared:sslcache:20m;
>    ssl_session_timeout 10m;
>}
>
>10. 重定向模块ngx_http_rewrite_module：
>
>1、rewrite regex replacement [flag] 
>将用户请求的URI基于regex所描述的模式进行检查，匹配到时将其替换为replacement指定的新的URI
>注意：如果在同一级配置块中存在多个rewrite规则，那么会自下而下逐个检查；被某条件规则替换
>完成后，会重新一轮的替换检查
>隐含有循环机制,但不超过10次；如果超过，提示500响应码， [flag]所表示的标志位用于控制此循环
>机制
>如果replacement是以http://或https://开头，则替换结果会直接以重向返回给客户端 
>[flag]：
>last：重写完成后停止对当前URI在当前location中后续
>的其它重写操作，而后对新的URI启动新一轮重写检查；提前重
>启新一轮循环
>break：重写完成后停止对当前URI在当前location中后
>续的其它重写操作，而后直接跳转至重写规则配置块之后的其它
>配置；结束循环，建议在location中使用
>redirect：临时重定向，重写完成后以临时重定向方式直
>接返回重写后生成的新URI给客户端，由客户端重新发起请求；
>不能以http://或https://开头，使用相对路径，状态码： 302
>permanent:重写完成后以永久重定向方式直接返回重写
>后生成的新URI给客户端，由客户端重新发起请求，状态码：301 
>例：
>rewrite ^/zz/(.*\.html)$  /zhengzhou/$1 break;
>rewrite ^/zz/(.*\.html)$  https://www.dianping/zhengzhou/$1 redirect;
>	
>2、 return
>return code [text];
>return code URL;
>return URL;
>停止处理，并返回给客户端指定的响应码
>	
>3、 rewrite_log on | off;
>是否开启重写日志, 发送至error_log（notice level）
>	
>4、 set $variable value;
>用户自定义变量
>注意：变量定义和调用都要以$开头
>	
>5、 if (condition) { ... }
>引入新的上下文,条件满足时，执行配置块中的配置指令； server, location
>condition：
>比较操作符：
>== 相同
>!= 不同
>~：模式匹配，区分字符大小写
>~*：模式匹配，不区分字符大小写
>!~：模式不匹配，区分字符大小写
>!~*：模式不匹配，不区分字符大小写
>文件及目录存在性判断：
>-e, !-e 存在（包括文件，目录，软链接）
>-f, !-f 文件
>-d, !-d 目录
>-x, !-x 执行 
>浏览器分流示例：
>if ($http_user_agent ~ Chrom) {
>    rewrite ^(.*)$  /chrome/$1 break;                                                      
>    }
>if ($http_user_agent ~ MSIE) {
>    rewrite ^(.*)$  /IE/$1 break;                                                      
>    }
>
>11. 引用模块ngx_http_referer_module
>
>valid_referers none|blocked|server_names|string ...;
>定义referer首部的合法可用值，不能匹配的将是非法值
>none：请求报文首部没有referer首部
>blocked：请求报文有referer首部，但无有效值
>server_names：参数，其可以有值作为主机名或主机名模式
>arbitrary_string：任意字符串，但可使用*作通配符
>regular expression：被指定的正则表达式模式匹配到的字符
>串,要使用~开头，例如： ~.*\.magedu\.com 
>示例：
>valid_referers none block server_names *.magedu.com
>*.mageedu.com magedu.* mageedu.* ~\.magedu\.;
>if ($invalid_referer) {
>return 403;
>}
>
>12. 反向代理模块ngx_http_proxy_module
>
>12.1 proxy_pass URL;
>Context:location, if in location, limit_except
>	
>注意： proxy_pass后面的路径不带uri时，其会将location的uri传递给后端主机
>server {
>    ...
>    server_name HOSTNAME;
>    location /uri/ {
>    proxy_pass http://host[:port]; 最后没有/
>    }
>    ...
>}
>上面示例： http://HOSTNAME/uri --> http://host/uri
>http://host[:port]/ 意味着： http://HOSTNAME/uri --> http://host/ 
>注意：如果location定义其uri时使用了正则表达式的模式，则proxy_pass之后必须不能使用uri; 
>用户请求时传递的uri将直接附加代理到的服务的之后
>server {
>    ...
>    server_name HOSTNAME;
>    location ~|~* /uri/ {
>    proxy_pass http://host; 不能加/
>    }
>    ...
>}
>http://HOSTNAME/uri/ --> http://host/uri/
>
>12.2 proxy_set_header field value;
>设定发往后端主机的请求报文的请求首部的值
>Context: http, server, location
>
>后端记录日志记录真实请求服务器IP
>proxy_set_header X-Real-IP $remote_addr;
>proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
>标准格式如下：
>X-Forwarded-For: client1, proxy1, proxy2 
>如后端是Apache服务器应更改日志格式：
>%h -----> %{X-Real-IP}i
>
>12.3 proxy_cache_path;
>	
>定义可用于proxy功能的缓存； Context:http
>proxy_cache_path path [levels=levels] [use_temp_path=on|off]
>keys_zone=name:size [inactive=time] [max_size=size]
>[manager_files=number] [manager_sleep=time]
>[manager_threshold=time] [loader_files=number] [loader_sleep=time]
>[loader_threshold=time] [purger=on|off] [purger_files=number]
>[purger_sleep=time] [purger_threshold=time];
>
>12.4 调用缓存
>	
>proxy_cache zone | off; 默认off
>指明调用的缓存，或关闭缓存机制； Context:http,
>server, location
>
>12.5
>	
>proxy_cache_key string;
>缓存中用于“键”的内容
>默认值： proxy_cache_key $scheme$proxy_host$request_uri;
>
>12.6
>
>proxy_cache_valid [code ...] time;
>定义对特定响应码的响应内容的缓存时长
>定义在http{...}中
>示例:
>proxy_cache_valid 200 302 10m;
>proxy_cache_valid 404 1m; 
>示例：
>在http配置定义缓存信
>proxy_cache_path /var/cache/nginx/proxy_cache
>levels=1:1:1 keys_zone=proxycache:20m
>inactive=120s max_size=1g;
>调用缓存功能，需要定义在相应的配置段，如server{...}；
>proxy_cache proxycache;
>proxy_cache_key $request_uri;
>proxy_cache_valid 200 302 301 1h;
>proxy_cache_valid any 1m;
>
>12.7
>
>proxy_cache_use_stale;
>proxy_cache_use_stale error | timeout |
>invalid_header | updating | http_500 | http_502 |
>http_503 | http_504 | http_403 | http_404 | off ...
>在被代理的后端服务器出现哪种情况下，可以真接使用过
>期的缓存响应客户端
>
>12.8
>
>proxy_cache_methods GET | HEAD | POST ...;
>对哪些客户端请求方法对应的响应进行缓存， GET和HEAD方法总是被缓存
>
>12.9
>
>proxy_hide_header field;
>By default, nginx does not pass the header fields
>“Date”, “Server”, “X-Pad”, and “X-Accel-...” from the
>response of a proxied server to a client. 用于隐藏后端服
>务器特定的响应首部
>
>12.10
>
>proxy_connect_timeout time;
>定义与后端服务器建立连接的超时时长，如超时会出现502错误，默认为60s，一般不建议超出75s
>
>12.11
>
>proxy_send_timeout time;
>把请求发送给后端服务器的超时时长；默认为60s
>
>12.12
>
>proxy_read_timeout time;
>等待后端服务器发送响应报文的超时时长， 默认为60s
>
>13. 首部信息
>
>add_header name value [always];
>添加自定义首部
>add_header X-Via $server_addr;
>add_header X-Cache $upstream_cache_status;
>add_header X-Accel $server_name;
>add_trailer name value [always];
>添加自定义响应信息的尾部
>
>14. hph 相关模块ngx_http_fastcgi_module
>
>14.1
>
>fastcgi_pass address;
>address为后端的fastcgi server的地址
>可用位置： location, if in location
>
>14.2
>
>fastcgi_index name;
>fastcgi默认的主页资源
>示例： fastcgi_index index.php;
>
>14.3
>
>fastcgi_param parameter value [if_not_empty];
>设置传递给 FastCGI服务器的参数值，可以是文本，变
>量或组合
>
>示例1：
>
>1）在后端服务器先配置fpm server和mariadb-server
>2）在前端nginx服务上做以下配置：
>location ~* \.php$ {
>    fastcgi_pass 后端fpm服务器IP:9000;
>    fastcgi_index index.php;
>    fastcgi_param SCRIPT_FILENAME
>/usr/share/nginx/html$fastcgi_script_name;
>    include     fastcgi.conf;    
>    …    
>}
>
>示例2：
>	
>通过/pm_status和/ping来获取fpm server状态信息（真实服务器端php-fpm配置文件中将这两项
>注释掉）
>location ~* ^/(status|ping)$ {
>    include fastcgi_params;
>    fastcgi_pass 后端fpm服务器IP:9000;
>    fastcgi_param SCRIPT_FILENAME $fastcgi_script_name;
>    include     fastcgi.conf; 
>}
>
>14.4 fastcgi 缓存相关
>	
>fastcgi_cache_path path [levels=levels] [use_temp_path=on|off]
>keys_zone=name:size [inactive=time] [max_size=size]
>[manager_files=number] [manager_sleep=time] [manager_threshold=time]
>[loader_files=number] [loader_sleep=time] [loader_threshold=time]
>[purger=on|off] [purger_files=number] [purger_sleep=time]
>[purger_threshold=time];
>定义fastcgi的缓存；
>path 缓存位置为磁盘上的文件系统
>max_size=size
>    磁盘path路径中用于缓存数据的缓存空间上限
>levels=levels：缓存目录的层级数量，以及每一级的目录数量
>levels=ONE:TWO:THREE
>示例： leves=1:2:2
>keys_zone=name:size
>    k/v映射的内存空间的名称及大小
>inactive=time
>    非活动时长
>
>14.5
>	
>fastcgi_cache zone | off;
>调用指定的缓存空间来缓存数据
>可用位置： http, server, location
>
>14.6
>	
>fastcgi_cache_key string;
>定义用作缓存项的key的字符串
>示例： fastcgi_cache_key $request_rui;
>
>14.7
>
>fastcgi_cache_methods GET | HEAD | POST ...;
>为哪些请求方法使用缓存
>
>14.8
>	
>fastcgi_cache_min_uses number;
>缓存空间中的缓存项在inactive定义的非活动时间内至少要被访问到
>此处所指定的次数方可被认作活动项
>
>14.9
>	
>fastcgi_keep_conn on | off;
>收到后端服务器响应后， fastcgi服务器是否关闭连接，建议启用长连接
>
>14.10
>	
>fastcgi_cache_valid [code ...] time;
>不同的响应码各自的缓存时长
>
>示例：
>	
>http {
>fastcgi_cache_path /var/cache/nginx/fcgi_cache
>levels=1:2:1 keys_zone=fcgicache:20m inactive=120s;
>...
>server {
>    location ~* \.php$ {
>    ...
>    fastcgi_cache fcgicache;
>    fastcgi_cache_key $request_uri;
>    fastcgi_cache_valid 200 302 10m;
>    fastcgi_cache_valid 301 1h;
>    fastcgi_cache_valid any 1m;
>...
>}
>}
>
>15. 代理模块ngx_http_upstream_module模块
>
>用于将多个服务器定义成服务器组，而由proxy_pass,fastcgi_pass等指令进行引用
>
>15.1
>	
>upstream name { ... }
>定义后端服务器组，会引入一个新的上下文
>默认调度算法是wrr
>Context: http
>upstream httpdsrvs {
>server ...
>server...
>...
>}
>
>15.2
>	
>server address [parameters];
>在upstream上下文中server成员，以及相关的参数； Context:upstream
>address的表示格式：
>unix:/PATH/TO/SOME_SOCK_FILE
>IP[:PORT]
>HOSTNAME[:PORT]
>parameters：
>weight=number     权重，默认为1    
>max_conns     连接后端报务器最大并发活动连接数， 1.11.5后支持    
>max_fails=number     失败尝试最大次数；超出此处指定的次数时    
>server将被标记为不可用,默认为1
>fail_timeout=time 后端服务器标记为不可用状态的连接超时时
>长，默认10s
>backup 将服务器标记为“备用”，即所有服务器均不可用时才启用
>down 标记为“不可用”，配合ip_hash使用，实现灰度发布
>
>15.3
>	
>ip_hash 源地址hash调度方法
>
>15.4
>	
>least_conn 最少连接调度算法，当server拥有不同的权重时其为wlc，当所有后端主机连接数相同时，
>则使用wrr，适用于长连接
>
>15.5
>	
>hash key [consistent] 基于指定的key的hash表来实
>现对请求的调度，此处的key可以直接文本、变量或二者组合
>作用：将请求分类，同一类请求将发往同一个upstream
>server，使用consistent参数， 将使用ketama一致性hash算法，
>适用于后端是Cache服务器（如varnish）时使用
>hash $request_uri consistent;
>hash $remote_addr;
>
>15.6
>	
>keepalive 连接数N;
>为每个worker进程保留的空闲的长连接数量,可节约nginx
>端口，并减少连接管理的消耗
>
>15.7
>	
>health_check [parameters];
>健康状态检测机制；只能用于location上下文
>常用参数：
>interval=time检测的频率，默认为5秒
>fails=number：判定服务器不可用的失败检测次数；默认为1次
>passes=number：判定服务器可用的失败检测次数；默认为1次
>uri=uri：做健康状态检测测试的目标uri；默认为/
>match=NAME：健康状态检测的结果评估调用此处指定的match配置块
>注意：仅对nginx plus有效
>
>15.8
>	
>match name { ... }
>对backend server做健康状态检测时，定义其结果判断机制；
>只能用于http上下文
>常用的参数：
>status code[ code ...]: 期望的响应状态码
>header HEADER[operator value]：期望存在响应首
>部，也可对期望的响应首部的值基于比较操作符和值进行比较
>body：期望响应报文的主体部分应该有的内容
>注意：仅对nginx plus有效
>
>16. ngx_stream_core_module模块
>
>模拟反代基于tcp或udp的服务连接，即工作于传输层的反代或调度器
>	
>stream { ... }
>定义stream相关的服务； Context:main
>stream {
>    upstream telnetsrvs {
>        server 192.168.22.2:23;
>        server 192.168.22.3:23;
>        least_conn;
>    }
>server {
>    listen 10.1.0.6:23;
>    proxy_pass telnetsrvs;
>    }
>} 
>listen address:port [ssl] [udp] [proxy_protocol]
>[backlog=number] [bind] [ipv6only=on|off] [reuseport]
>[so_keepalive=on|off|[keepidle]:keepintvl:keepcnt;
>
>17. ngx_stream_proxy_module模块
>
>可实现代理基于TCP， UDP (1.9.13), UNIX-domain
>sockets的数据流
>1 proxy_pass address;
>指定后端服务器地址
>2 proxy_timeout timeout;
>无数据传输时，保持连接状态的超时时长
>默认为10m
>3 proxy_connect_timeout time;
>设置nginx与被代理的服务器尝试建立连接的超时时长
>默认为60s
>
>示例：
>	
>stream {
>    upstream telnetsrvs {
>        server 192.168.10.130:23;
>        server 192.168.10.131:23;
>        hash $remote_addr consistent;
>    }
>    server {
>        listen 172.16.100.10:2323;
>        proxy_pass telnetsrvs;
>        proxy_timeout 60s;
>        proxy_connect_timeout 10s;
>    }
>}

### 11. nginx优化
>编译安装过程优化
>
>1）.减小Nginx编译后的文件大小
>
>在编译Nginx时，默认以debug模式进行，而在debug模式下会插入很多跟踪和ASSERT之类的信息，编译完成后，一个Nginx要有好几兆字节。而在编译前取消Nginx的debug模式，编译完成后Nginx只有几百千字节。因此可以在编译之前，修改相关源码，取消debug模式。具体方法如下：
>
>在Nginx源码文件被解压后，找到源码目录下的auto/cc/gcc文件，在其中找到如下几行：
>
>    # debug  
>    CFLAGS=”$CFLAGS -g” 
>
>注释掉或删掉这两行，即可取消debug模式。
>
>2.为特定的CPU指定CPU类型编译优化
>
>在编译Nginx时，默认的GCC编译参数是“-O”，要优化GCC编译，可以使用以下两个参数：
>
>    --with-cc-opt='-O3' 
>    --with-cpu-opt=CPU  #为特定的 CPU 编译，有效的值包括：
>    pentium, pentiumpro, pentium3, # pentium4, athlon, opteron, amd64, sparc32, sparc64, ppc64 
>
>要确定CPU类型，可以通过如下命令：
>
>    [root@localhost home]#cat /proc/cpuinfo | grep "model name" 
>
>2. 利用TCMalloc优化Nginx的性能
>
>TCMalloc的全称为Thread-Caching Malloc，是谷歌开发的开源工具google-perftools中的一个成员。与标准的glibc库的Malloc相比，TCMalloc库在内存分配效率和速度上要高很多，这在很大程度上提高了服务器在高并发情况下的性能，从而降低了系统的负载。下面简单介绍如何为Nginx添加TCMalloc库支持。
>
>要安装TCMalloc库，需要安装libunwind（32位操作系统不需要安装）和google-perftools两个软件包，libunwind库为基于64位CPU和操作系统的程序提供了基本函数调用链和函数调用寄存器功能。下面介绍利用TCMalloc优化Nginx的具体操作过程。
>
>1).安装libunwind库
>
>可以从http://download.savannah.gnu.org/releases/libunwind下载相应的libunwind版本，这里下载的是libunwind-0.99-alpha.tar.gz。安装过程如下：
>
>    [root@localhost home]#tar zxvf libunwind-0.99-alpha.tar.gz  
>    [root@localhost home]# cd libunwind-0.99-alpha/  
>    [root@localhost libunwind-0.99-alpha]#CFLAGS=-fPIC ./configure  
>    [root@localhost libunwind-0.99-alpha]#make CFLAGS=-fPIC  
>    [root@localhost libunwind-0.99-alpha]#make CFLAGS=-fPIC install 
>
>2).安装google-perftools
>
>可以从http://google-perftools.googlecode.com下载相应的google-perftools版本，这里下载的是google-perftools-1.8.tar.gz。安装过程如下：
>
>    [root@localhost home]#tar zxvf google-perftools-1.8.tar.gz  
>    [root@localhost home]#cd google-perftools-1.8/  
>    [root@localhost google-perftools-1.8]# ./configure  
>    [root@localhost google-perftools-1.8]#make && make install  
>    [root@localhost google-perftools-1.8]#echo "/usr/
>    local/lib" > /etc/ld.so.conf.d/usr_local_lib.conf  
>    [root@localhost google-perftools-1.8]# ldconfig 
>
>至此，google-perftools安装完成。
>
>3).重新编译Nginx
>
>为了使Nginx支持google-perftools，需要在安装过程中添加“–with-google_perftools_module”选项重新编译Nginx。安装代码如下：
>
>    [root@localhostnginx-0.7.65]#./configure \  
>    >--with-google_perftools_module --with-http_stub_status_module  --prefix=/opt/nginx  
>    [root@localhost nginx-0.7.65]#make  
>    [root@localhost nginx-0.7.65]#make install 
>
>到这里Nginx安装完成。
>
>4).为google-perftools添加线程目录
>
>创建一个线程目录，这里将文件放在/tmp/tcmalloc下。操作如下：
>
>    [root@localhost home]#mkdir /tmp/tcmalloc  
>    [root@localhost home]#chmod 0777 /tmp/tcmalloc 
>
>5).修改Nginx主配置文件
>
>修改nginx.conf文件，在pid这行的下面添加如下代码：
>
>    #pid        logs/nginx.pid;  
>    google_perftools_profiles /tmp/tcmalloc; 
>
>接着，重启Nginx即可完成google-perftools的加载。
>
>6).验证运行状态
>
>为了验证google-perftools已经正常加载，可通过如下命令查看：
>
>    [root@ localhost home]# lsof -n | grep tcmalloc  
>    nginx      2395 nobody   9w  REG    8,8       0    1599440 /tmp/tcmalloc.2395  
>    nginx      2396 nobody   11w REG   8,8       0    1599443 /tmp/tcmalloc.2396  
>    nginx      2397 nobody   13w REG  8,8        0    1599441  /tmp/tcmalloc.2397  
>    nginx     2398 nobody    15w REG  8,8     0    1599442 /tmp/tcmalloc.2398 
>
>由于在Nginx配置文件中设置worker_processes的值为4，因此开启了4个Nginx线程，每个线程会有一行记录。每个线程文件后面的数字值就是启动的Nginx的pid值。
>
>至此，利用TCMalloc优化Nginx的操作完成。
>3.Nginx内核参数优化
>
>内核参数的优化，主要是在Linux系统中针对Nginx应用而进行的系统内核参数优化。
>
>下面给出一个优化实例以供参考。
>
>    net.ipv4.tcp_max_tw_buckets = 6000 
>    net.ipv4.ip_local_port_range = 1024 65000  
>    net.ipv4.tcp_tw_recycle = 1 
>    net.ipv4.tcp_tw_reuse = 1 
>    net.ipv4.tcp_syncookies = 1 
>    net.core.somaxconn = 262144 
>    net.core.netdev_max_backlog = 262144 
>    net.ipv4.tcp_max_orphans = 262144 
>    net.ipv4.tcp_max_syn_backlog = 262144 
>    net.ipv4.tcp_synack_retries = 1 
>    net.ipv4.tcp_syn_retries = 1 
>    net.ipv4.tcp_fin_timeout = 1 
>    net.ipv4.tcp_keepalive_time = 30 
>
>将上面的内核参数值加入/etc/sysctl.conf文件中，然后执行如下命令使之生效：
>
>    [root@ localhost home]#/sbin/sysctl -p 
>
>下面对实例中选项的含义进行介绍：
>
>net.ipv4.tcp_max_tw_buckets ：选项用来设定timewait的数量，默认是180 000，这里设为6000。
>
>net.ipv4.ip_local_port_range:选项用来设定允许系统打开的端口范围。在高并发情况否则端口号会不够用。
>
>net.ipv4.tcp_tw_recycle:选项用于设置启用timewait快速回收.
>
>net.ipv4.tcp_tw_reuse:选项用于设置开启重用，允许将TIME-WAIT sockets重新用于新的TCP连接。
>
>net.ipv4.tcp_syncookies:选项用于设置开启SYN Cookies，当出现SYN等待队列溢出时，启用cookies进行处理。
>
>net.core.somaxconn:选项的默认值是128， 这个参数用于调节系统同时发起的tcp连接数，在高并发的请求中，默认的值可能会导致链接超时或者重传，因此，需要结合并发请求数来调节此值。
>
>net.core.netdev_max_backlog:选项表示当每个网络接口接收数据包的速率比内核处理这些包的速率快时，允许发送到队列的数据包的最大数目。
>
>net.ipv4.tcp_max_orphans:选项用于设定系统中最多有多少个TCP套接字不被关联到任何一个用户文件句柄上。如果超过这个数字，孤立连接将立即被复位并打印出警告信息。这个限制只是为了防止简单的DoS攻击。不能过分依靠这个限制甚至人为减小这个值，更多的情况下应该增加这个值。
>
>net.ipv4.tcp_max_syn_backlog:选项用于记录那些尚未收到客户端确认信息的连接请求的最大值。对于有128MB内存的系统而言，此参数的默认值是1024，对小内存的系统则是128。
>
>net.ipv4.tcp_synack_retries参数的值决定了内核放弃连接之前发送SYN+ACK包的数量。
>
>net.ipv4.tcp_syn_retries选项表示在内核放弃建立连接之前发送SYN包的数量。
>
>net.ipv4.tcp_fin_timeout选项决定了套接字保持在FIN-WAIT-2状态的时间。默认值是60秒。正确设置这个值非常重要，有时即使一个负载很小的Web服务器，也会出现大量的死套接字而产生内存溢出的风险。
>
>net.ipv4.tcp_syn_retries选项表示在内核放弃建立连接之前发送SYN包的数量。
>
>如果发送端要求关闭套接字，net.ipv4.tcp_fin_timeout选项决定了套接字保持在FIN-WAIT-2状态的时间。接收端可以出错并永远不关闭连接，甚至意外宕机。
>
>net.ipv4.tcp_fin_timeout的默认值是60秒。需要注意的是，即使一个负载很小的Web服务器，也会出现因为大量的死套接字而产生内存溢出的风险。FIN-WAIT-2的危险性比FIN-WAIT-1要小，因为它最多只能消耗1.5KB的内存，但是其生存期长些。
>
>net.ipv4.tcp_keepalive_time选项表示当keepalive启用的时候，TCP发送keepalive消息的频度。默认值是2（单位是小时）。
>4.　PHP-FPM的优化
>
>如果您高负载网站使用PHP-FPM管理FastCGI，这些技巧也许对您有用:
>
>1）增加FastCGI进程数
>
>把PHP FastCGI子进程数调到100或以上，在4G内存的服务器上200就可以建议通过压力测试获取最佳值。
>
>2）增加 PHP-FPM打开文件描述符的限制
>
>标签rlimit_files用于设置PHP-FPM对打开文件描述符的限制，默认值为1024。这个标签的值必须和Linux内核打开文件数关联起来，例如，要将此值设置为65 535，就必须在Linux命令行执行“ulimit -HSn 65536”。
>
>       然后 增加 PHP-FPM打开文件描述符的限制:
>     # vi /path/to/php-fpm.conf
>    找到“<valuename="rlimit_files">1024</value>”
>把1024更改为 4096或者更高.
>重启 PHP-FPM.
>
>       ulimit -n 要调整为65536甚至更大。如何调这个参数，可以参考网上的一些文章。命令行下执行 ulimit -n 65536即可修改。如果不能修改，需要设置  /etc/security/limits.conf，加入
>
>* hard nofile65536
>
>* soft nofile 65536
>
>3）适当增加max_requests
>
>标签max_requests指明了每个children最多处理多少个请求后便会被关闭，默认的设置是500。
>
><value name="max_requests"> 500 </value>
>
>4.　nginx.conf的参数优化
>
>nginx要开启的进程数 一般等于cpu的总核数 其实一般情况下开4个或8个就可以。
>
>每个nginx进程消耗的内存10兆的模样
>
>worker_cpu_affinity
>仅适用于linux，使用该选项可以绑定worker进程和CPU（2.4内核的机器用不了）
>
>假如是8 cpu 分配如下：
>worker_cpu_affinity 00000001 00000010 00000100 00001000 00010000
>
>00100000 01000000 10000000
>
>nginx可以使用多个worker进程，原因如下：
>
>to use SMP 
>to decrease latency when workers blockend on disk I/O 
>to limit number of connections per process when select()/poll() is
>
>used The worker_processes and worker_connections from the event sections
>
>allows you to calculate maxclients value: k max_clients = worker_processes * worker_connections
>
>worker_rlimit_nofile 102400;
>
>每个nginx进程打开文件描述符最大数目 配置要和系统的单进程打开文件数一致,linux 2.6内核下开启文件打开数为65535，worker_rlimit_nofile就相应应该填写65535 nginx调度时分配请求到进程并不是那么的均衡，假如超过会返回502错误。我这里写的大一点
>
>use epoll
>
>Nginx使用了最新的epoll（Linux 2.6内核）和kqueue（freebsd）网络I/O模型，而Apache则使用的是传统的select模型。
>
>处理大量的连接的读写，Apache所采用的select网络I/O模型非常低效。在高并发服务器中，轮询I/O是最耗时间的操作 目前Linux下能够承受高并发
>
>访问的Squid、Memcached都采用的是epoll网络I/O模型。
>
>worker_connections 65535;
>每个工作进程允许最大的同时连接数 （Maxclient = work_processes *　worker_connections）
>
>keepalive_timeout 75
>
>keepalive超时时间
>
>这里需要注意官方的一句话：
>The parameters can differ from each other. Line Keep-Alive:
>
>timeout=time understands Mozilla and Konqueror. MSIE itself shuts
>
>keep-alive connection approximately after 60 seconds.
>
>client_header_buffer_size 16k
>large_client_header_buffers 4 32k
>客户请求头缓冲大小 
>nginx默认会用client_header_buffer_size这个buffer来读取header值，如果header过大，它会使用large_client_header_buffers来读取
>
>如果设置过小HTTP头/Cookie过大 会报400 错误 nginx 400 bad request
>求行如果超过buffer，就会报HTTP 414错误(URI Too Long) nginx接受最长的HTTP头部大小必须比其中一个buffer大，否则就会报400的HTTP错误(Bad Request)。
>
>open_file_cache max 102400
>
>使用字段:http, server, location 这个指令指定缓存是否启用,如果启用,将记录文件以下信息: ·打开的文件描述符,大小信息和修改时间. ·存在的目录信息. ·在搜索文件过程中的错误信息 -- 没有这个文件,无法正确读取,参考open_file_cache_errors 指令选项:
>·max - 指定缓存的最大数目,如果缓存溢出,最长使用过的文件(LRU)将被移除
>例: open_file_cache max=1000 inactive=20s; open_file_cache_valid 30s; open_file_cache_min_uses 2; open_file_cache_errors on;
>
>open_file_cache_errors
>语法:open_file_cache_errors on | off 默认值:open_file_cache_errors off 使用字段:http, server, location 这个指令指定是否在搜索一个文件是记录cache错误.
>
>open_file_cache_min_uses
>
>语法:open_file_cache_min_uses number 默认值:open_file_cache_min_uses 1 使用字段:http, server, location 这个指令指定了在open_file_cache指令无效的参数中一定的时间范围内可以使用的最小文件数,如 果使用更大的值,文件描述符在cache中总是打开状态.
>open_file_cache_valid
>
>语法:open_file_cache_valid time 默认值:open_file_cache_valid 60 使用字段:http, server, location 这个指令指定了何时需要检查open_file_cache中缓存项目的有效信息.
>开启gzip
>gzip on;
>gzip_min_length 1k;
>gzip_buffers 4 16k;
>gzip_http_version 1.0;
>gzip_comp_level 2;
>gzip_types text/plain application/x-JavaScript text/css
>
>application/xml;
>gzip_vary on;
>
>缓存静态文件：
>
>location ~* ^.+\.(swf|gif|png|jpg|js|css)$ {
>root /usr/local/ku6/ktv/show.ku6.com/;
>expires 1m;
>}
>
>7.   错误排查
>
>1、Nginx 502 Bad Gateway
>
>php-cgi进程数不够用、php执行时间长（mysql慢）、或者是php-cgi进程死掉，都会出现502错误
>
>一般来说Nginx 502 Bad Gateway和php-fpm.conf的设置有关，而Nginx 504 Gateway Time-out则是与nginx.conf的设置有关
>
>1）、查看当前的PHP FastCGI进程数是否够用：
>
>netstat -anpo | grep "php-cgi" | wc -l
>
>　　如果实际使用的“FastCGI进程数”接近预设的“FastCGI进程数”，那么，说明“FastCGI进程数”不够用，需要增大。
>
>2）、部分PHP程序的执行时间超过了Nginx的等待时间，可以适当增加
>
>     nginx.conf配置文件中FastCGI的timeout时间，例如：
>
>http {
>    ......
>    fastcgi_connect_timeout 300;
>    fastcgi_send_timeout 300;
>    fastcgi_read_timeout 300;
>    ......
>}
>2、413 Request Entity Too Large
>     解决：增大client_max_body_size
>
>    client_max_body_size:指令指定允许客户端连接的最大请求实体大小,它出现在请求头部的Content-Length字段. 如果请求大于指定的值,客户端将收到一个"Request Entity Too Large" (413)错误. 记住,浏览器并不知道怎样显示这个错误.
>    
>    php.ini中增大
>post_max_size 和upload_max_filesize
>
>3 Ngnix error.log出现：upstream sent too big header while reading response header from upstream错误
>
>1)如果是nginx反向代理
>   proxy是nginx作为client转发时使用的，如果header过大，超出了默认的1k，就会引发上述的upstream sent too big header （说白了就是nginx把外部请求给后端server，后端server返回的header  太大nginx处理不过来就导致了。
>
>  server {
>        listen       80;
>        server_name  *.xywy.com ;
>
>        large_client_header_buffers 4 16k;
>                            
>        location / {
>                            
>          #添加这3行 
>           proxy_buffer_size 64k;
>           proxy_buffers   32 32k;
>           proxy_busy_buffers_size 128k;
>                            
>           proxy_set_header Host $host;
>           proxy_set_header X-Real-IP       $remote_addr;
>           proxy_set_header X-Forwarded-For  $proxy_add_x_forwarded_for;
>    
>    }
>
>}         
>
>2) 如果是 nginx+PHPcgi 
>
>  错误带有 upstream: "fastcgi://127.0.0.1:9000"。就该 
>
>  多加：
>
>  fastcgi_buffer_size 128k;
>  fastcgi_buffers 4 128k;
>
>server {
>        listen       80;
>        server_name  ddd.com;
>        index index.html index.htm index.php;
>
>        client_header_buffer_size 128k;
>        large_client_header_buffers 4 128k;
>        proxy_buffer_size 64k;
>        proxy_buffers 8 64k;
>        fastcgi_buffer_size 128k;
>        fastcgi_buffers 4 128k;
>
> 
>
>
>        location / {
>                            
>          ......
>                            
>        }
>
>}         
>
>8.   Nginx的php漏洞
>
>漏洞介绍：nginx是一款高性能的web服务器，使用非常广泛，其不仅经常被用作反向代理，也可以非常好的支持PHP的运行。80sec发现其中存在一个较为严重的安全问题，默认情况下可能导致服务器错误的将任何类型的文件以PHP的方式进行解析，这将导致严重的安全问题，使得恶意的攻击者可能攻陷支持php的nginx服务器。
>
>
>漏洞分析：nginx默认以cgi的方式支持php的运行，譬如在配置文件当中可以以
>
>
>location ~ .php$ {
>root html;
>fastcgi_pass 127.0.0.1:9000;
>fastcgi_index index.php;
>fastcgi_param SCRIPT_FILENAME /scripts$fastcgi_script_name;
>include fastcgi_params;
>}
>
>的方式支持对php的解析，location对请求进行选择的时候会使用URI环境变量进行选择，其中传递到后端Fastcgi的关键变量SCRIPT_FILENAME由nginx生成的$fastcgi_script_name决定，而通过分析可以看到$fastcgi_script_name是直接由URI环境变量控制的，这里就是产生问题的点。而为了较好的支持PATH_INFO的提取，在PHP的配置选项里存在cgi.fix_pathinfo选项，其目的是为了从SCRIPT_FILENAME里取出真正的脚本名。
>那么假设存在一个http://www.80sec.com/80sec.jpg，我们以如下的方式去访问
>
>http://www.80sec.com/80sec.jpg/80sec.php
>
>
>将会得到一个URI
>
>/80sec.jpg/80sec.php
>
>经过location指令，该请求将会交给后端的fastcgi处理，nginx为其设置环境变量SCRIPT_FILENAME，内容为
>
>/scripts/80sec.jpg/80sec.php
>
>而在其他的webserver如lighttpd当中，我们发现其中的SCRIPT_FILENAME被正确的设置为
>
>/scripts/80sec.jpg
>
>所以不存在此问题。
>后端的fastcgi在接受到该选项时，会根据fix_pathinfo配置决定是否对SCRIPT_FILENAME进行额外的处理，一般情况下如果不对fix_pathinfo进行设置将影响使用PATH_INFO进行路由选择的应用，所以该选项一般配置开启。Php通过该选项之后将查找其中真正的脚本文件名字，查找的方式也是查看文件是否存在，这个时候将分离出SCRIPT_FILENAME和PATH_INFO分别为
>
>/scripts/80sec.jpg和80sec.php
>
>最后，以/scripts/80sec.jpg作为此次请求需要执行的脚本，攻击者就可以实现让nginx以php来解析任何类型的文件了。
>
>POC： 访问一个nginx来支持php的站点，在一个任何资源的文件如robots.txt后面加上/80sec.php，这个时候你可以看到如下的区别：
>
>访问http://www.80sec.com/robots.txt
>
>HTTP/1.1 200 OK
>Server: nginx/0.6.32
>Date: Thu, 20 May 2010 10:05:30 GMT
>Content-Type: text/plain
>Content-Length: 18
>Last-Modified: Thu, 20 May 2010 06:26:34 GMT
>Connection: keep-alive
>Keep-Alive: timeout=20
>Accept-Ranges: bytes
>
>访问访问http://www.80sec.com/robots.txt/80sec.php
>
>HTTP/1.1 200 OK
>Server: nginx/0.6.32
>Date: Thu, 20 May 2010 10:06:49 GMT
>Content-Type: text/html
>Transfer-Encoding: chunked
>Connection: keep-alive
>Keep-Alive: timeout=20
>X-Powered-By: PHP/5.2.6
>
>其中的Content-Type的变化说明了后端负责解析的变化，该站点就可能存在漏洞。
>
>漏洞厂商：http://www.nginx.org
>
>解决方案：
>
>我们已经尝试联系官方，但是此前你可以通过以下的方式来减少损失
>
>关闭cgi.fix_pathinfo为0
>
>或者
>
>if ( $fastcgi_script_name ~ ..*/.*php ) {
>return 403;
>}

## lsyncd

> rsync 可实现文件的同步/备份，安装配置移步linux 下 rsync 备份/同步文件
> lsyncd 实现了触发式或定时通知事件，可以近实时的同步文件（封装了rsync）,github地址：https://github.com/axkibe/lsyncd
>
> github 的描述(有道翻译)
> Lsyncd监视本地目录树事件监视器接口（inotify或fevent）。它聚合并合并事件几秒钟，然后生成一个（或多个）进程来同步更改。默认情况下使用rsync
> 。因此，Lsyncd是一种轻量级的实时镜像解决方案，它相对容易安装，不需要新的文件系统或块设备，也不妨碍本地文件系统的性能
>
> Rsync + ssh是一种高级操作配置，它使用SSH来操作文件和目录，直接在目标上移动，而不是通过电线重新传输移动目标
>
> 细粒度的定制可以通过配置文件实现。自定义动作配置甚至可以在从shell脚本到用Lua语言编写的代码的级联层中从头编写。
> 这种简单的、强大的、灵活的配置是可以得到的。详情请参阅手册

### 1. 配置文件解析
>----
> - ### User configuration file for lsyncd.
>- ### Simple example for default rsync, but executing moves through on the target.
> - ### For more examples, see /usr/share/doc/lsyncd*/examples/
>- ### sync{default.rsyncssh, source="/var/www/html", host="localhost", targetdir="/tmp/htmlcopy/"}
> 
>settings {
>    -- 日志文件存放位置
>   logfile = "/var/log/lsyncd/lsyncd.log",
>
>    -- 状态文件存放位置 
>    statusFile = "/tmp/lsyncd.stat",
>
>    -- 将lsyncd的状态写入上面的statusFile的间隔，默认10秒 
>    -- statusInterval = 1,　　-- 同步时间间隔
>    
>    -- 是否启用守护模式，默认 true
>    nodaemon = false,
>    
>    -- inotify监控的事件 ,默认是 CloseWrite,还可以是 Modify 或 CloseWrite or Modify  
>    -- inotifyMode = "CloseWrite",
>    
>    -- 最大同步进程  
>    maxProcesses = 8,
>    
>    --累计到多少所监控的事件激活一次同步,即使后面的delay延迟时间还未到  
>    -- maxDelays = 1
>       
>    -- pid文件
>    pidfile = "/kekedata/lsyncd_logs/lsyncd.pid"
>    }
> 
> sync {
>   -- rsync , rsyncssh , direct 三种模式
>   default.rsync,
>
>    -- 同步的源目录，使用绝对路径。
>    source="/kekedata/www/ts.yunying.kekejl.com/oss/pad",
>
>    -- 定义目的地址.对应不同的模式有几种写法,这里使用远程同步的地址，rsync中的地址 
>    target="prod@112.96.28.43::test",
>    
>    -- 默认 true ，允许同步删除。还有 false, startup, running 值  
>    delete = false,
>    
>    -- 哪些文件不同步  
>    -- exclude = { ".*" },   
>    
>    -- 累计事件，等待rsync同步延时时间，默认15秒,最大累计到1000个不可合并的事件(1000个文件变动),  
>    delay = 5,
>    
>    -- 默认 true 当init = false ,只同步进程启动以后发生改动事件的文件，原有的目录即使有差异也不会同步  
>    -- init = true,
>    
>    rsync = {
>        -- rsync 的二进制处理文件
>           binary = "/usr/bin/rsync",
> 
>        -- 归档模式
>        archive = true,
>                            
>        -- 压缩传输
>        compress = true,
>                            
>        -- 增量
>        verbose   = true,
>                            
>        -- 密码文件
>        password_file = "/etc/passwd.txt",
>                            
>        -- 其他 rsync 的配置参数, 限速(--bwlimit KBPS),使用 rsync -v 查看详细参数
>        -- _extra = {"--bwlimit=200", "--port=800"},
>           _extra = {"--port=800"},
>    }
> }

## SQLite 数据库

> SQLite 是一个软件库，实现了自给自足的、无服务器的、零配置的、事务性的 SQL 数据库引擎。SQLite 是在世界上最广泛部署的 SQL 数据库引擎。SQLite 源代码不受版权限制
>
> 为什么要用 SQLite？
>
>     不需要一个单独的服务器进程或操作的系统（无服务器的）。
>     
>     SQLite 不需要配置，这意味着不需要安装或管理。
>     
>     一个完整的 SQLite 数据库是存储在一个单一的跨平台的磁盘文件。
>     
>     SQLite 是非常小的，是轻量级的，完全配置时小于 400KiB，省略可选功能配置时小于250KiB。
>     
>     SQLite 是自给自足的，这意味着不需要任何外部的依赖。
>     
>     SQLite 事务是完全兼容 ACID 的，允许从多个进程或线程安全访问。
>     
>     SQLite 支持 SQL92（SQL2）标准的大多数查询语言的功能。
>     
>     SQLite 使用 ANSI-C 编写的，并提供了简单和易于使用的 API。
>     
>     SQLite 可在 UNIX（Linux, Mac OS-X, Android, iOS）和 Windows（Win32, WinCE, WinRT）中运行。

### 1. 安装方法
>方法一：
>wget http://www.sqlite.org/sqlite-autoconf-3070500.tar.gz
>tar xvzf sqlite-autoconf-3070500.tar.gz
>cd sqlite-autoconf-3070500
>./configure
>make
>sudo make install
>
>方法二：
> sudo yum install sqlite-devel
>
>方法三：
>sudo gem install sqlite3-ruby

## redis 服务

### 1. redis 恢复工具redis-port
>**使用 redis-port 工具将自建 redis 的 rdb文件同步到云数据库**
>
> 更新时间：2018-01-24 15:56:15
> 下载 redis-port
>
> redis-port地址
> 使用示例
>
>   ./redis-port  restore  --input=x/dump.rdb  --target=dst\_host:dst\_port   --auth=dst\_password  [--filterkey="str1|str2|str3"] [--targetdb=DB] [--rewrite] [--bigkeysize=SIZE] [--logfile=REDISPORT.LOG]
>
> 参数说明
>
>   x/dump.rdb : 自建 redis 的 dump 文件路径
>
>   dst_host : 云数据库 redis 域名
>
>   dst_port : 云数据库 redis 端口
>
>   dst_password : 云数据库 redis 密码
>
>   str1|str2|str3 : 过滤具有 str1 或 str2 或 str3 的 key
>
>   DB : 将要同步入云数据库 redis 的 DB
>
>   rewrite : 覆盖已经写入的 key
>
>   bigkeysize=SIZE : 当写入的 value 大于 SIZE 时，走大 key 写入模式
>
> **根据 redis-port 日志查看数据同步状态**
>
> ![ss.jpg](data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wBDAAgGBgcGBQgHBwcJCQgKDBQNDAsLDBkSEw8UHRof Hh0aHBwgJC4nICIsIxwcKDcpLDAxNDQ0Hyc5PTgyPC4zNDL/2wBDAQkJCQwLDBgNDRgyIRwh MjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjL/wAAR CAD4AmwDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL/8QAtRAA AgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0KxwRVS0fAkM2JyggkK FhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNkZWZnaGlqc3R1dnd4eXqDhIWG h4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5usLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl 5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEBAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREA AgECBAQDBAcFBAQAAQJ3AAECAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYk NOEl8RcYGRomJygpKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOE hYaHiImKkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk 5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwDlPHWkS6ivhsWNpbWs08DImnJ5YaPaSS28 cMpHQk5yD1JJqj4PhtdMvNHN5HDJdatcxqFkUEQ2qvy3zcBmZcD0VT2am2fgH+0orW5g1cPa So3zm2YMMOqDapOWVmfAJKj5WJ6c0b3wZcWEqwyalZNL/aK2DorNhCxba7HGApC5xycYyKA6 GxdaNpsCana2VtNBeyaT9oa3e4WVomEyjY3y/eKBWOMEdOhxTZ/CuhXMTLaSSwCya3e9uHnD honhMkjKu0YKkBRySWYA4OKydQ8FT2HiOPSGvFI8j7RNM8ZTyIxnczqckEBcj1BXHJxWnN8O riC1tJH1W2RJrhbaVpkZRG7LuUDglu68YAbjkZIBlct4c1HT/wC0L61lgkvLyS0t1gkVUtYk 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 +XyuVBCsB3JUjg07TfCVzDqcUWsafJJbzAgeRexIEC8s7thgqqu4ngc9+xOojRg1rRIfBs2h RXEtvcx3UUn2mE5EsgZt0qnaGCqCvy8HHI7mrMPiW3n1K/vE1tYTYhbXSorqSUoyjcPOYqpL NglsHA3MSeBtMMvg/wAPtot3f2VxdXkFuonWZLiMNKnneX5fl7SyttKtuPGTjABpn/CN+GIt Wmt5l1ZI7W2E98/2qIi1YE5Qny8M2SigDHzErzgmgaOBPU5IPv60Urbdx2Z254z1xnv70lAg ooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKK KAO+Nz49OpJI9iz3Vwj7s2MOJVyu4SfLhsHafmyQce2cv+1vF8CB2hmUyX+RK9km9roMTw5X cXByMZyB8vA4rptP8UeHdLkewt70/wBnC3K2++1b93MGVvNlAJLlmQZAGBtVemTVa48U6Nf3 7XlzqN2qHWEvUh8tiyxKWBwc7VY7t2BxkkZzzQBga1d+KLy5i0PUbdo5JWRorSKBYw3LBdoU AY3Mx9NxJPOauSXXjjGlI1pO53KLTNmjmVlXapYFSWYL90tkheRxzV/xH4n0qWaXUNOmWTVG tRZp9ngaKGEMzF3jVuVJVguODuZm9M6F/wCOdGurewS2upbUNdQXM4WJtyMiMrhiPvbsqqgZ AVecE0IZx8l/r2t3kcCJvuLSR5xHDAkYVgdzOyqAu75RyeeMc1cvT4v/AOEgsZbi0ddVLGa2 MNpGrSMCSx+VcMcjnOSD1xUN7qWl28F1b2F28ralclru4WIqY4A2QgDcklvmPOPlUeorotP8 a6Jomv2r2qtPp0arDCUQxPaxBtzcHO9nOCx4PBHSkgZ57YG5F9D9kiE1yXARDEsu5jxjawIb 8Qa6tdU8bTxasVhlIR2+2MtoimOTayswwoKvt3BmGG29elczbyRDU1eKeSxiLEq4LM0a891w W469Otel2/jbQLN9Tmjnd5mvLi6iY27AukqgGMA5AbcqqWPG0sBkk0xI4jR73Wr2+hj0+2gu poLbyVR7WJ1EYbOSGBBO4j5jznjvRqOoeJf7Kul1CBhaTXTLPLNZJlpwWJy5XduHzd8gEjgc Va0+8tF1DTJ59Utf9FswALjTd6KwY/u2UZLHBLbsHnjjrU2ua1pep6bqyw6hcBrjUvtdvbzI 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 skN5PG6p5aFJWBVc/dHOQuecdOah+13PkmL7TN5R6pvbacnuM456/WiigCYahdq7yLeTh3xu YSMC2OBk5z06egqq7u+N7s20ALuJOAO3sKKKAL9nrd5YWE9jCYDBM25hJAkhVsFcqWBKnHQj kVVkvbqb/W3MzgYxvcnocjqaKKAGG4mMqymWQyKcq245U5J4Ocjk5+pzWjD4j1OC1uoBNGxu lZZppIleZgw2kbyNwGAOAcUUUAZasybtrsuRtODjIPHPt7VZju7mG2MMVzMkTMGKK5VSw/iI zjPTB60UUAB1K+Nz9pN7cmfbt83zW3bemM5zjHHWoze3LRxxm5lKRZ2KXJC567ecCiigAiur iGGSKKeVI5QA6K5VWA9QDg0kkzzEGWRnKoFXexO1RwAM9sdB0FFFADVmkRo3WRw0WNhDcpg5 yvPHJz9ealfULx1kV7u4KyffUyMQ3J+9k88560UUARwzy28wkgleKQAgMjFSAeOoOelSi+ux bmMXUwQuHKh2wWHRiM4zx169KKKAGw3l1A26G6miYAqCjlflJ5HXpnkioCckkk/40UUAFFFF ABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAU UUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFAH//2Q)
>
> 当出现restore: rdb done时数据同步完成。

### 2. redis内存分析方法
>**Redis 内存分析方法**
>
> 背景
>
> 线上经常遇到用户想知道自己 Redis 实例中数据的内存分布情况。为了不影响线上实例的使用，
> 我们一般会采用 bgsave 生成 dump.rdb 文件，再结合 redis-rdb-tools 和 sqlite 来进行静态分析。
> 总的来说，整个分析的过程简单而实用，是每一个 Redis 的用户都非常值得掌握的一个方法。
>
> 创建备份
>
> 自建 Redis 可在客户端执行 bgsave 生成 rdb 文件。云数据库 Redis 版可以在控制台上可以进行数据备份和下载的操作，下载后的数据为 rdb 格式文件。
> 步骤详见下图：
>
>![redis内存分析](C:\雨鱼\NaNaOY.github.io\img\redis内存分析方法.png)
>
>生成内存快照
>
>redis-rdb-tools 是一个 python 的解析 rdb 文件的工具，主要有以下三个功能：
>
>    生成内存快照
>    转储成 json 格式
>    使用标准的 diff 工具比较两个 dump 文件
>
>在分析内存的使后，我们主要用到它的生成内存快照功能。
>redis-rdb-tools 安装
>
>redis-rdb-tools 有两种安装方式，任选其一即可。
>
>使用 PYPI 安装
>
>    pip install rdbtools
>
>从源码安装
>
>    git clone https://github.com/sripathikrishnan/redis-rdb-tools
>    cd redis-rdb-tools
>    sudo python setup.py install
>
>使用 redis-rdb-tools 生成内存快照
>
>生成内存快照的命令为：
>
>    rdb -c memory dump.rdb > memory.csv
>
>生成 CSV 格式的内存报告。包含的列有：数据库 ID，数据类型，key，内存使用量(byte)，编码。内存使用量包含 key、value 和其他值。
>
>注意：内存使用量是理论上的近似值，在一般情况下，略低于实际值。memory.csv 例子：
>
>    $head memory.csv
>    database,type,key,size_in_bytes,encoding,num_elements,len_largest_element
>    0,string,"orderAt:377671748",96,string,8,8
>    0,string,"orderAt:413052773",96,string,8,8
>    0,sortedset,"Artical:Comments:7386",81740,skiplist,479,41
>    0,sortedset,"pay:id:18029",2443,ziplist,84,16
>    0,string,"orderAt:452389458",96,string,8,8
>
>分析内存快照
>
>SQLite 是一款轻型的数据库。我们可以将前面生成的 csv 导入到数据库中之后，就可以利用 sql 语句很方便的对 Redis 的内存数据进行各种分析了。
>
>导入方法
>
>    sqlite3 memory.db
>    sqlite> create table memory(database int,type varchar(128),key varchar(128),size_in_bytes int,encoding varchar(128),num_elements int,len_largest_element varchar(128));
>    sqlite>.mode csv memory
>    sqlite>.import memory.csv memory
>
>数据导入以后，接下来想怎么分析就怎么分析了，举几个简单的例子：
>
>查询key个数
>
>    sqlite>select count(*) from memory;
>
>查询总的内存占用
>
>    sqlite>select sum(size_in_bytes) from memory;
>
>查询内存占用最高的10个 key
>
>    sqlite>select * from memory order by size_in_bytes desc limit 10;
>
>查询成员个数1000个以上的 list
>
>    sqlite>select * from memory where type='list' and num_elements > 1000 ;
>
>总结
>
>通过使用 redis-rdb-tools + sqlite 的方式，可以方便的对 redis 实例的内存情况进行静态的分析。整个过程也比较简单，获取到 rdb 之后即可。
>
>    rdb -c memory dump.rdb > memory.csv;
>    sqlite3 memory.db
>    sqlite> create table memory(database int,type varchar(128),key varchar(128),size_in_bytes int,encoding varchar(128),num_elements int,len_largest_element varchar(128));
>    sqlite>.mode csv memory
>    sqlite>.import memory.csv memory
>
>实际使用中，发现过一个 List 积攒了10多 GB 的内容，也发现过43 MB 以上的 string 类型的 value， 往往不仅能解答用户的疑惑，而且能够帮助用户排除业务中潜在的风险点，找到业务性能瓶颈。

## ansible

> Ansible是一个简单的自动化运维管理工具，基于Python语言实现，由Paramiko和PyYAML两个关键模块构建，可用于自动化部署应用、配置、编排task(持续交付、无宕机更新等)。主版本大概每2个月发布一次。
> Ansible与Saltstack最大的区别是Ansible无需在被控主机部署任何客户端代理，默认直接通过SSH通道进行远程命令执行或下发配置：相同点是都具备功能强大、灵活的系统管理、状态配置，两者都提供丰富的模板及API，对云计算平台、大数据都有很好的支持。
> 1.Ansible优点:
>
>
>     Agentless，去中心化
>     Stupied Simple ，上手简单，学习曲线平滑
>     SSH by default ，安全，无需安装客户端
>     配置简单、功能强大、扩展性强
>     支持API及自定义模块，可通过Python轻松扩展
>     通过Playbooks来定制强大的配置、状态管理
>     提供一个功能强大、操作性强的Web管理界面和REST API接口——AWX平台
>     幂等性：一种操作重复多次结果相同
>
> 　　2.Ansible的架构图:
>
> ![Ansible架构图](C:\雨鱼\NaNaOY.github.io\img\Ansible架构图.png)
>
> 　Ansible核心组件说明：
>
>     　　Ansible：Ansible的核心程序
>     　　
>     Host Lnventory：记录了每一个由Ansible管理的主机信息，信息包括ssh端口，root帐号密码，ip地址等等。可以通过file来加载，可以通过CMDB加载
>     　　
>     Playbooks：YAML格式文件，多个任务定义在一个文件中，使用时可以统一调用，“剧本”用来定义那些主机需要调用那些模块来完成的功能.
>     　　
>     Core Modules：Ansible执行任何管理任务都不是由Ansible自己完成，而是由核心模块完成；Ansible管理主机之前，先调用core Modules中的模块，然后指明管理Host Lnventory中的主机，就可以完成管理主机。
>     　　
>     Custom Modules：自定义模块，完成Ansible核心模块无法完成的功能，此模块支持任何语言编写。
>     　　
>     Connection Plugins：连接插件，Ansible和Host通信使用
>
> 　　3.ansible执行过程，其中暖色调的代表已经模块化
>
> ![Ansile执行过程](C:\雨鱼\NaNaOY.github.io\img\Ansible执行过程.png)

### 1. ansible 安装和使用
>ansible安装网址
>http://www.linuxidc.com/Linux/2015-02/112774.htm
>
>ansible使用网址
>http://blog.csdn.net/kellyseeme/article/details/50553707
>
>python中文手册
>http://www.ansible.com.cn/docs/developing.html
>
>报错安装以下包
>yum -y install python-dev*
>yum -y install libxml2 libxml2-dev*
>yum -y install libxslt1.1* libxslt*

## codis

> Codis 是一个分布式 Redis 解决方案, 对于上层的应用来说, 连接到 Codis Proxy 和连接原生的 Redis Server 没有明显的区别 (不支持的命令列表),
>  上层应用可以像使用单机的 Redis 一样使用, Codis 底层会处理请求的转发, 不停机的数据迁移等工作, 所有后边的一切事情, 对于前面的客户端来说是透明的, 可以简单的认为后边连接的是一个内存无限大的 Redis 服务。
> Codis 由四部分组成:
> Codis Proxy (codis-proxy)
>     codis-proxy 是客户端连接的 Redis 代理服务, codis-proxy 本身实现了 Redis 协议,
>  表现得和一个原生的 Redis 没什么区别 (就像 Twemproxy), 对于一个业务来说, 可以部署多个 codis-proxy, codis-proxy 本身是无状态的.
> Codis Manager (codis-config)
>     codis-config 是 Codis 的管理工具, 支持包括, 添加/删除 Redis 节点, 添加/删除 Proxy 节点,
>  发起数据迁移等操作. codis-config 本身还自带了一个 http server, 会启动一个 dashboard, 用户可以直接在浏览器上观察 Codis 集群的运行状态.
> Codis Redis (codis-server)
>     codis-server 是 Codis 项目维护的一个 Redis 分支, 基于 2.8.21 开发, 加入了 slot 的支持和
> 原子的数据迁移指令. Codis 上层的 codis-proxy 和 codis-config 只能和这个版本的 Redis 交互才能正常运行.
> ZooKeeper
>
>     Codis 依赖 ZooKeeper 来存放数据路由表和 codis-proxy 节点的元信息,
> codis-config 发起的命令都会通过 ZooKeeper 同步到各个存活的 codis-proxy.
> Codis 支持按照 Namespace 区分不同的产品, 拥有不同的 product name 的产品, 各项配置都不会冲突.

### 1. codis安装搭建
>我的开源中国社区账号

## ELK日志分析服务

> 在日常的运维管理活动，日志非常的重要，当发现error时可以从日志了解报错并及时解决。日志分为系统日志,应用日志,和安全日志，
> 经常的分析日志可以了解服务器的硬件状况，性能以及安全，从而采取预防措施及时纠正任务。
>
> 通常情况下，日志被分散到不同的存储设备上，而企业内部的服务器，少则十几台多则成千上百，如果采取最传统的方式登录每台服务器进行查看，
> 对运维来说难度大劳动强度也大，而且不易管理容易出错不易管理，所以需要一个进行集中化管理日志的解决方案。
>
> 开源实时日志分析平台 ELK 是ELK套件（ELK stack）是指ElasticSearch、Logstash和Kibana三件套。这三个软件可以组成一套日志分析和监控工具。
>
>
> Elasticsearch是个开源分布式搜索引擎，它的特点有：分布式，零配置，自动发现，索引自动分片，索引副本机制，restful风格接口，多数据源，自动搜索负载等。
>
>
> Logstash是一个完全开源的工具，他可以对你的日志进行收集、过滤，并将其存储供以后使用（如，搜索）。
>
>
> Kibana 也是一个开源和免费的工具，它Kibana可以为 Logstash 和 ElasticSearch 提供的日志分析友好的 Web 界面，可以帮助您汇总、分析和搜索重要数据日志。
>
> wKiom1b8nwzBEpTFAADltttSCTk049.png
>
> 如图：Logstash收集AppServer产生的Log，并存放到ElasticSearch集群中，而Kibana则从ES集群中查询数据生成图表，再返回给Browser。
>
> ![Log](C:\雨鱼\NaNaOY.github.io\img\Logstash收集AppServer产生的log.png)

### 1. ELK安装搭建
>网址 : http://blog.51cto.com/liqingbiao/1758880

## kvm搭建

> http://www.linuxidc.com/Linux/2017-01/140007.htm
>
> http://blog.csdn.net/wh211212/article/details/54141412
> 虚拟机克隆
> http://blog.csdn.net/wuliowen/article/details/68484447

### 1. centos7 kvm无法连接问题
>执行
>grubby --update-kernel=ALL --args="console=ttyS0"

### 2. kvm win7系统创建
>virt-install --name win7-woshipin --boot network,cdrom,menu=on --ram 2048 --vcpus=1 --os-variant=win7 
>--accelerate --cdrom=/kekedata/install/cn_windows_7_professional_with_sp1_vl_build_x64_dvd_u_incl_virtio-140506-homemade-by-Jetso.iso 
>--disk path=/kekedata/kvm/images/win7-woshipin.img,size=60,bus=ide --bridge=br0,model=virtio --autostart --vnc --vncport=5904 --vnclisten=0.0.0.0

### 3. kvm克隆
>kvm克隆虚拟机
>http://blog.csdn.net/wanglei_storage/article/details/51106096
>
>
>kvm增加磁盘容量
>http://blog.csdn.net/taiyang1987912/article/details/47973347
>
>kvm分区
>http://www.cnblogs.com/liqing1009/p/6722674.html

## linux查看进程占用网卡流量工具nethogs

> 一、nethogs介绍
> 　　分享一个linux 下检测系统进程占用带宽情况的检查。来自github上的开源工具。
> 　　它不依赖内核中的模块。当我们的服务器网络异常时，可以通过运行nethogs程序来检测是那个程序占用了大量带宽。节省了查找时间。

### 1. 安装搭建
>二、安装方法
>1.在epel 源中可以直接yum 安装
>yum install libpcap nethogs -y
>
>2.源码方式安装
>　　2.1 安装c++ 环境　　
>[root@SaltMaster ~]# yum install gcc-c++ libpcap-devel.x86_64 libpcap.x86_64 ncurses*
>　　2.2 下载编译好的二进制文件
>[root@SaltMaster ~]# git clone https://github.com/raboof/nethogs
>Initialized empty Git repository in /root/nethogs/.git/
>remote: Counting objects: 1193, done.
>remote: Total 1193 (delta 0), reused 0 (delta 0), pack-reused 1193
>Receiving objects: 100% (1193/1193), 1.22 MiB | 29 KiB/s, done.
>Resolving deltas: 100% (789/789), done.
>　　2.3 编译并安装
>[root@SaltMaster ~]# cd nethogs/
>
>[root@SaltMaster nethogs]# make
>[root@SaltMaster nethogs]# make install
>　　完成上面步骤就算安装完成了。如果编译失败的话，大部分是缺少编译环境。
>
>3.使用方法
>	3.1 直接运行nethogs 就可以查看当前占用带宽的进程
>
>![带宽进程](C:\雨鱼\NaNaOY.github.io\img\linux安装进程.png)
>
>​	3.2 我们来测试 找出eth0 上占用大量带宽的程序
>　　nethogs 网卡设备
>　　我们检测 eth0，运行一下命令
>[root@SaltMaster ~]# nethogs eth0
>
>![占用带宽程序](C:\雨鱼\NaNaOY.github.io\img\检测程序.png)
>
> 图中第一行就是测试中的流量记录，表示192.168.40.250这台客户端去访问我的80 端口所占用的带宽。
>在PID 那一列，可以使用 lsof -p  pid 查看进程测试。
>或者用lsof -i :端口号，来查看是哪些进程在占用。如查看80端口  lsof -i :80

## svn

### 1. svn搭建
>CentOS7 源码安装svn1.9.5及httpd配置(ldap验证/ad域验证)
>
>环境：CentOS-7-x86_64-1611 + subversion-1.9.5
>一、源码安装：
>
>1、安装ldap：
>wget ftp://gd.tuwien.ac.at/infosys/network/OpenLDAP/openldap-release/openldap-2.4.44.tgz
>tar -zxv -f openldap-2.4.44.tgz
>cd openldap-2.4.44/
>./configure --prefix=/usr/local/ldap --disable-slapd#--disable-slapd  ===  不需要ldap服务器 只需要lib includemakemake install
>
>2、安装APR：
>wget http://apache.fayea.com//apr/apr-1.5.2.tar.bz2
>tar -jxv -f apr-1.5.2.tar.bz2
>cd apr-1.5.2/
>./configure --prefix=/usr/local/aprmakemake install
>
>3、安装APR-util:
>wget http://apache.fayea.com//apr/apr-util-1.5.4.tar.bz2
>tar -jxv -f apr-util-1.5.4.tar.bz2
>cd apr-util-1.5.4/
>./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr/bin/apr-1-config --with-ldap=ldap --with-ldap-include=/usr/local/ldap/include --with-ldap-lib=/usr/local/ldap/lib#apr-util需要带上ldap功能
>make
>make install
>
>4、安装pcre：
>wget https://ftp.pcre.org/pub/pcre/pcre-8.40.tar.bz2
>tar -jxv -f pcre-8.40.tar.bz2
>cd pcre-8.40/
>./configure --prefix=/usr/local/pcremakemake install
>
>5、安装apache：
>wget http://mirrors.tuna.tsinghua.edu.cn/apache//httpd/httpd-2.4.25.tar.bz2
>tar -jxv -f httpd-2.4.25.tar.bz2 
>cd  httpd-2.4.25/
>./configure --prefix=/usr/local/httpd \
>--with-apr=/usr/local/apr/bin/apr-1-config \--with-apr-util=/usr/local/apr-util/bin/apu-1-config \--with-pcre=/usr/local/pcre/bin/pcre-config \--enable-module=mod_auth_basic \--enable-module=mod_authnz_ldap \--enable-module=mod_dav \--enable-module=mod_dav_fs \--enable-module=mod_dav_lock \--enable-module=mod_authn_file \--enable-module=mod_authz_user \--enable-ldap \--enable-authnz-ldap \--enable-module=mod_ldapmake
>make install
>
>开启防火墙80 端口：
>firewall-cmd --permanent --zone=public --add-port=80/tcpfirewall-cmd --reload
>
>将httpd加入服务：
>cp /usr/local/httpd/bin/apachectl /etc/init.d/httpd
>
>6、安装sqlite：
>wget http://www.sqlite.org/2017/sqlite-autoconf-3160200.tar.gz
>tar -zxv -f sqlite-autoconf-3160200.tar.gz
>cd sqlite-autoconf-3160200/
>./configure --prefix=/usr/local/sqlitemakemake install
>
>7、安装zlib：
>wget http://www.zlib.net/zlib-1.2.11.tar.gz
>tar -zxv -f zlib-1.2.11.tar.gz 
>cd zlib-1.2.11/
>./configure --prefix=/usr/local/zlib
>make
>make install
>
>8、安装serf：
>
>8.1、安装scons ==> serf：
>安装 serf 需要用 scons 安装：
>wget http://prdownloads.sourceforge.net/scons/scons-2.5.1-1.noarch.rpm
>rpm -ivh scons-2.5.1-1.noarch.rpm
>
>8.2、安装 openssl-devel ==> serf：
>如果在serf安装时 如果报错：
>compilation terminated.
>scons: * [buckets/ssl_buckets.o] Error 1
>scons: building terminated because of errors.
>yum install openssl-devel
>
>8.3、安装 serf：
>旧版本svn 是用 neon (--with-neon=/usr/local/neon) 而新版本(1.8开始)是使用serf来支持http访问
>wget https://www.apache.org/dist/serf/serf-1.3.9.tar.bz2
>tar -jxv -f serf-1.3.9.tar.bz2
>cd serf-1.3.9/
>scons PREFIX=/usr/local/serf APR=/usr/local/apr/bin/apr-1-config APU=/usr/local/apr-util/bin/apu-1-config
>scons install
>
>9、安装svn：
>wget http://mirrors.cnnic.cn/apache/subversion/subversion-1.9.5.tar.bz2
>tar -jxv -f subversion-1.9.5.tar.bz2
>cd subversion-1.9.5/
>./configure --prefix=/usr/local/subversion \
>--with-apr=/usr/local/apr/bin/apr-1-config \--with-apr-util=/usr/local/apr-util/bin/apu-1-config \--with-apxs=/usr/local/httpd/bin/apxs \--with-sqlite=/usr/local/sqlite \--with-zlib=/usr/local/zlib \--with-serf=/usr/local/serfmake
>make install
>
>10、如果使用./svn命令出现错误：
>svn: error while loading shared libraries: libserf-1.so.1: cannot open shared object file: No such file or directory
>解决：把serf安装后的 libserf-1.so.1.3.0 复制到svn的安装目录下的lib目录（直接复制libserf-1.so 会出现 链接已断的错误属性 没有用）
>/usr/local/subversion/bin/svn --version
>cp /usr/local/serf/lib/libserf-1.so.1.3.0 /usr/local/subversion/lib/libserf-1.so.1
>
>11、将svn目录下的几个模块复制到apache下，支持ladp功能：
>cp /usr/local/subversion/libexec/mod_authz_svn.so /usr/local/httpd/modules/mod_authz_svn.so
>cp /usr/local/subversion/libexec/mod_dav_svn.so /usr/local/httpd/modules/mod_dav_svn.so
>
>
>二：httpd.conf配置：
>
>1、匿名登录：
>#开启模块
>LoadModule dav_module modules/mod_dav.so
>LoadModule dav_svn_module modules/mod_dav_svn.so
>#url访问路径，可以不等于 实际的根目录 (指定的路径不要和www的冲突 不然可能会是301报错)
><Location /svn>  
>    DAV svn
>    #仓库的根目录(增加/删除仓库的时候 不需要重启apache服务)
>    SVNParentPath /svn  
></Location>
>
>2、账号密码验证（明文）：
>Basic -- htpasswd 基本认证
>Digest -- htdigest 基本认证的改进
>创建密码文件：
>-c 创建文件(第一次需要，后面几个用户就不需要了)
>-m md5
>/usr/local/httpd/bin/htpasswd -c -m /etc/svn-auth.htpasswd username1/usr/local/httpd/bin/htpasswd -m /etc/svn-auth.htpasswd username2
>/usr/local/httpd/bin/htdigest -c /etc/svn-auth.htdigest "AuthName" username1/usr/local/httpd/bin/htdigest /etc/svn-auth.htdigest "AuthName" username2
>
>#开启模块#下面三个顺序是必须的
>LoadModule dav_module modules/mod_dav.so
>LoadModule dav_svn_module modules/mod_dav_svn.so
>LoadModule authz_svn_module modules/mod_authz_svn.so
>
>LoadModule auth_basic_module modules/mod_auth_basic.so
>LoadModule auth_digest_module modules/mod_auth_digest.so
>LoadModule authn_file_module modules/mod_authn_file.so
>LoadModule authz_user_module moduels/mod_authz_user.so
>
><Location /svn>
>    DAV svn
>    SVNParentPath /svn
>
>    #这一行必须要！
>    Require valid-user
>    AuthType Basic
>    AuthName "SVN-basic" 
>    
>    #AuthType Digest
>    #AuthName "SVN-digest" 
>    
>    AuthBasicProvider file
>    
>    #账号密码文件
>    AuthUserFile /svn/svn-auth.htpasswd  
>    #权限文件
>    AuthzSVNAccessFile /svn/svntest.conf
></Location>
>
>3、ldap登陆：
>#开启模块#下面三个顺序是必须的
>LoadModule dav_module modules/mod_dav.so
>LoadModule dav_svn_module modules/mod_dav_svn.so
>LoadModule authz_svn_module modules/mod_authz_svn.so
>
>LoadModule ldap_module modules/mod_ldap.so
>LoadModule authnz_ldap_module modules/mod_authnz_ldap.so
>
>LoadModule auth_basic_module modules/mod_auth_basic.so
>LoadModule authn_file_module modules/mod_authn_file.so
>LoadModule authz_user_module moduels/mod_authz_user.so
>
><Location /svn>
>    DAV svn
>    SVNParentPath /svnt
>
>    Require valid-user
>    AuthType Basic
>    AuthName "SVN-ldap" 
>    AuthBasicProvider ldap#   AuthzLDAPAuthoritative 属性在2.4版本中已经取消： http://httpd.apache.org/docs/2.4/upgrading.html#run-time#   AuthzLDAPAuthoritative off
>    AuthLDAPURL "ldap://192.168.1.1/ou=People,dc=abc,dc=net"
>    AuthLDAPBindDN "cn=admin,dc=abc,dc=net"
>    AuthLDAPBindPassword "abcd"#    AuthzSVNAccessFile /svnt/svntest.conf
></Location>
>
>4、AD域登陆：
>#开启模块#下面三个顺序是必须的
>LoadModule dav_module modules/mod_dav.so
>LoadModule dav_svn_module modules/mod_dav_svn.so
>LoadModule authz_svn_module modules/mod_authz_svn.so
>
>LoadModule ldap_module modules/mod_ldap.so
>LoadModule authnz_ldap_module modules/mod_authnz_ldap.so
>
>LoadModule auth_basic_module modules/mod_auth_basic.so
>LoadModule authn_file_module modules/mod_authn_file.so
>LoadModule authz_user_module moduels/mod_authz_user.so
>
><Location /svn>
>    DAV svn
>    SVNParentPath /svnt
>
>    Require valid-user
>    AuthType Basic
>    AuthName "SVN-ad" 
>    AuthBasicProvider ldap
>    
>    AuthLDAPURL "ldap://192.168.1.2/dc=abc,dc=com?samaccountName?sub?(objectClass=*)"
>    
>    AuthLDAPBindDN "administrator"
>    AuthLDAPBindPassword "abcd"
>###    AuthzSVNAccessFile /svnt/az.conf
></Location>
>
>三：az.conf 权限控制：
>#设置组[groups]group1=username1
>[/]#组
>@group1=rw#用户username2=rw
>#这里就是svn下的目录名[repos_name:/]  
>@group1=rwusername2=rw

### 2. svn ssl访问nginx转发问题
>502问题
>
>set $fixed_destination $http_destination; 
>       if ( $http_destination ~* ^https(.*)$ ) { 
>            set $fixed_destination http$1; 
>       } 
>proxy_set_header Destination $fixed_destination; 
>       }

## lvs+keepalived

> 使用keepalived实现LVS的DR模式热备
>
> 一、说明：
> 在《LVS之一：三种工作模式的优缺点比较（NAT/TUN/DR）》文章讲述了LVS的三种转发模式对比，我们先就用LVS的DR模式来实现Web应用的负载均衡。为了防止LVS服务器自身的单点故障导致整个Web应用无法提供服务，因此还得利用Keepalived实现lvs的高可用性，keepalived主要使用VRRP协议来保存链路的高可用性。而VRRP(Virtual Router Redundancy Protocol)协议本身是用于实现路由器冗余的协议。
> LVS-DR模式的原理如下（借用别人的图）：
>
> 即客户端访问VIP（域名解析到VIP），LVS接收情况后根据负载均衡调度算法，转发请求到真实服务器，真实服务器接收到客户端请求后，将处理结果直接返回给客户端。
> 二、网络拓扑及IP规划：
>
> 说明：
> 1、  LVS服务器和WEB服务器必须在同一网段；
> 2、  多台Web服务器间使用NFS服务器等共享存储存放用户上传附件，以保障数据的一致性（如上传图片）。下文讨论
> IP地址规划：
> VIP              192.168.18.60
> LVS-MASTER     192.168.18.51
> LVS-BACKUP     192.168.18.52
> NFS   Server       192.168.18.20
> MySQL               192.168.18.18
> WEB1        192.168.18.61
> WEB2        192.168.18.62
> 三、安装LVS+Keepalived：
> 1、使用yum在线安装：
> [root@lvs1 ~]# yum install ipvsadm  keepalived –y
> 2、检查安装结果：
> [root@lvs1 ~]# rpm -qa |grep ipvsadm
> ipvsadm-1.25-10.el6.x86_64
> [root@lvs1 ~]# rpm -qa |grep keepalived
> keepalived-1.2.7-3.el6.x86_64
> 3、服务配置：
> [root@lvs1 ~]# chkconfig keepalived on   ##设置为开机启动
> [root@lvs1 ~]# service keepalived start   ##立即启动keepalived服务
> 四、配置LVS服务器：
> 1、  备份配置文件：
> [root@lvs1 ~]# cp /etc/keepalived/keepalived.conf /etc/keepalived/keepalived.bak
> 2、  修改配置文件，具体如下：
> 原始的配置文件包含了三种模式的配置，而这里我们只需要用到DR模式，因此最终保留配置如下：
> [root@lvs1 ~]#  vim /etc/keepalived/keepalived.conf
>
>  -   global_defs {                       ##全局配置部分
>
>  -   notification_email {             ##下面几行均为全局通知配置，可以实现出现问题后报警，但功能有限，因此注释掉，并采用Nagios监视lvs运行情况
>  -   admin@toxingwang.com
>  -   }
>  -   notification_email_from master@toxingwang.com
>  -   smtp_server smtp.exmail.qq.com
>  -   smtp_connect_timeout 30
> router_id LVS_MASTER             ##设置lvs的id，在一个网络内应该是唯一的
> }
> vrrp_instance VI_1 {            ##设置vrrp组，唯一且同一LVS服务器组要相同
> state MASTER             ##备份LVS服务器设置为BACKUP
> interface eth0             # #设置对外服务的接口
> virtual_router_id 51        ##设置虚拟路由标识
> priority 100                                    #设置优先级，数值越大，优先级越高，backup设置为99，这样就能实现当master宕机后自动将backup变为master，而当原master恢复正常时，则现在的master再次变为backup。
> advert_int 1            ##设置同步时间间隔
> authentication {         ##设置验证类型和密码，master和buckup一定要设置一样
> auth_type PASS
> auth_pass 1111
> }
> virtual_ipaddress {          ##设置VIP，可以多个，每个占一行
> 192.168.18.60
> }
> }
> virtual_server 192.168.18.60 80 {
> delay_loop 6            ##健康检查时间间隔，单位s
> lb_algo wrr             ##负载均衡调度算法设置为加权轮叫
>       Dh  目标地址散列，把同一个IP地址的请求,发送给同一个server
> lb_kind DR                              ##负载均衡转发规则
> nat_mask 255.255.255.0   ##网络掩码，DR模式要保障真是服务器和lvs在同一网段
> persistence_timeout 50    ##会话保持时间，单位s
> protocol TCP                           ##协议
> real_server 192.168.18.61 80 {      ##真实服务器配置，80表示端口
> weight 3                              ##权重
> TCP_CHECK {                       ##服务器检测方式设置
> connect_timeout 5    ##连接超时时间
> nb_get_retry 3
> delay_before_retry 3
> connect_port 80
> }
> }
> real_server 192.168.18.62 80 {
> weight 3
> TCP_CHECK {
> connect_timeout 10
> nb_get_retry 3
> delay_before_retry 3
> connect_port 80
> }
> }
> }
> 作为高可用的备份lvs服务器配置只需在上述配置中的master修改backup即可，其他保持相同。
> 3、  分别在两台lvs服务器上重启服务：
> [root@lvs1 ~]# service keepalived start
> 注：由于keepalived配置文件有语法错误也能启动，因此看到启动了lvs服务，不代表配置文件没有错误，如果遇到lvs不能正常转发，及时跟踪日志进行处理。
> 日志跟踪方法：
> 1、开两个ssh窗口连接到lvs服务器，第一个窗口运行如下命令：
> [root@lvs1 ~]# tail -F /var/log/message
> 2、第二个窗口重新启动keepalived服务，同时观察窗口1中日志的变化，然后根据日志提示解决即可。
> 五、WEB服务器IP绑定配置：
> 在两台WEB服务器安装并配置（非本文重点，不做web安装配置讲解），建立测试网页，先使用实际IP进行访问测试，能正常访问后，进行如下操作：
> 在前面的文章中介绍DR模式时提到：负载均衡器也只是分发请求，应答包通过单独的路由方法返回给客户端。但实际上客户端访问时，IP都是指向的负载均衡器的ip（也就是LVS的VIP），如何能让真是服务器处理IP头为VIP的请求，这就需要做下面的操作，将VIP绑定到真实服务器的lo网口（回环），为了防止IP广播产生IP冲突，还需关闭IP广播，具体如下：
> 1、编辑开机启动脚本：
>     [root@web1 ~]# vim /etc/init.d/realserver
> #!/bin/bash
> #chkconfig: 2345 79 20
> #description:realserver
> SNS_VIP=192.168.18.60
> . /etc/rc.d/init.d/functions
> case "$1" in
> start)
> ifconfig lo:0 $SNS_VIP netmask 255.255.255.255 broadcast $SNS_VIP
> /sbin/route add -host $SNS_VIP dev lo:0
> echo "1" >/proc/sys/net/ipv4/conf/lo/arp_ignore
> echo "2" >/proc/sys/net/ipv4/conf/lo/arp_announce
> echo "1" >/proc/sys/net/ipv4/conf/all/arp_ignore
> echo "2" >/proc/sys/net/ipv4/conf/all/arp_announce
> sysctl -p >/dev/null 2>&1
> echo "RealServer Start OK"
> ;;
> stop)
> ifconfig lo:0 down
> route del $SNS_VIP >/dev/null 2>&1
> echo "0" >/proc/sys/net/ipv4/conf/lo/arp_ignore
> echo "0" >/proc/sys/net/ipv4/conf/lo/arp_announce
> echo "0" >/proc/sys/net/ipv4/conf/all/arp_ignore
> echo "0" >/proc/sys/net/ipv4/conf/all/arp_announce
> echo "RealServer Stoped"
> ;;
> *)
> echo "Usage: $0 {start|stop}"
> exit 1
> esac
> exit 0
> 脚本说明：
> arp_ignore: 定义接收到ARP请求时的响应级别；
> 0：只要本地配置的有相应地址，就给予响应；默认；
> 1：仅在请求的目标地址配置在到达的接口上的时候，才给予响应；
> arp_announce：定义将自己地址向外通告时的通告级别；
> 0：将本地任何接口上的任何地址向外通告；默认；
> 1：试图仅向目标网络通告与其网络匹配的地址；
> 2：仅向与本地接口上地址匹配的网络进行通告；
> 2、  设置脚本开机启动并立即启动：
> [root@web1 ~]# chkconfig realserver on
> [root@web1 ~]# service realserver start
> RealServer Start OK
> 3、  检测IP配置：
> [root@web1 ~]# ifconfig
> eth0      Link encap:Ethernet  HWaddr 00:50:56:B3:54:5A
> inet addr:192.168.18.61  Bcast:192.168.18.255  Mask:255.255.255.0
> inet6 addr: fe80::250:56ff:feb3:545a/64 Scope:Link
> UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
> RX packets:98940 errors:4 dropped:4 overruns:0 frame:0
> TX packets:82037 errors:0 dropped:0 overruns:0 carrier:0
> collisions:0 txqueuelen:1000
> RX bytes:23077218 (22.0 MiB)  TX bytes:16043349 (15.3 MiB)
> Interrupt:18 Base address:0x2000
>
> lo        Link encap:Local Loopback
> inet addr:127.0.0.1  Mask:255.0.0.0
> inet6 addr: ::1/128 Scope:Host
> UP LOOPBACK RUNNING  MTU:16436  Metric:1
> RX packets:300 errors:0 dropped:0 overruns:0 frame:0
> TX packets:300 errors:0 dropped:0 overruns:0 carrier:0
> collisions:0 txqueuelen:0
> RX bytes:528420 (516.0 KiB)  TX bytes:528420 (516.0 KiB)
>
> lo:0      Link encap:Local Loopback
> inet addr:192.168.18.60  Mask:255.255.255.255
> UP LOOPBACK RUNNING  MTU:16436  Metric:1
> 4、  测试LVS：
> 在lvs master服务器上运行如下命令：
> [root@lvs1 ~]# watch -n 1 ipvsadm -ln
> 在多台客户端通过浏览器访问VIP或域名（域名需解析到VIP），看能否获取到正确的页面，且同时观察上述窗口的变化。
>
>
> 从上图（为了方便观察，使用了压力测试）可以看出，访问被正常的分担到了两台后端web服务器 。

### 1. keepalived配置说明
>! Configuration File for keepalived
>
>global_defs {                 ##全局配置部分
> -  notification_email {      ##下面几行均为全局通知配置,可以实现出现问题后报警,但功能有限,因此注释掉，
>                              ##并采用Nagios监控lvs运行情况
> -    acassen@firewall.loc
> -    failover@firewall.loc
> -    sysadmin@firewall.loc
> -  }
> -  notification_email_from Alexandre.Cassen@firewall.loc
> -  smtp_server 192.168.200.1
> -  smtp_connect_timeout 30
>    router_id LVS_MASTER
>    }
>
>vrrp_instance VI_1 {         ##设置vrrp组,唯一且同一LVS服务器组要相同
>    state MASTER             ##备份LVS服务器设置为BACKUP
>    interface eth1           ##设置对外服务的接口
>    virtual_router_id 51     ##设置虚拟路由标识
>    priority 100             ##设置优先级,数值越大,优先级越高,backup设置为99,这样就能实现当master宕机后
>                             ##自动将backup变为master,而当原master恢复正常时,则现在的master再次变为backup.
>    advert_int 1             ##设置同步时间间隔
>    authentication {         ##设置验证类型和密码,master和backup一定要设置一样
>        auth_type PASS
>        auth_pass 1111
>    }
>    virtual_ipaddress {      ##设置VIP,可以多个,每个占一行
>        192.168.1.183
>    }
>}
>
>virtual_server 192.168.1.183 8180 {
>    delay_loop 6               ##健康检查时间间隔,单位s
>    lb_algo rr                ##负载均衡调度算法设置为加权轮叫(询)
>    lb_kind DR                ##负载均衡转发规则
>    nat_mask 255.255.255.0     ##网络掩码,DR模式要保障真实服务器和lvs在同一网段
>    persistence_timeout 50     ##会话保持时间,单位s
>    protocol TCP               ##协议
>
>    real_server 192.168.1.181 8180 {  ##真实服务器配置,80表示端口
>        weight 3                    ##权重
>        TCP_CHECK {                 ##服务器检测方式设置
>            connect_timeout 10       ##连接超时时间   
>            nb_get_retry 3          ##失败重试次数
>            delay_before_retry 3    ##失败重试的间隔时间
>            connect_port 8180         ##连接的后端web端口
>        
>    }
>}
>
>#virtual_server 10.10.10.2 1358 {
> -    delay_loop 6
> -    lb_algo rr 
> -    lb_kind NAT
> -    persistence_timeout 50
> -    protocol TCP
>
> -    sorry_server 192.168.200.200 1358
>
>    real_server 192.168.1.182 8180 {
>        weight 3
>        TCP_CHECK {
>            connect_timeout 10
>            nb_get_retry 3
>            delay_before_retry 3
>            connect_port 8180
>        }
>    }
>}

## rocketmq

### 1. rocketmq内核优化
>vm.overcommit_memory      是否允许内存的过量分配，允许进程分配比它实际使用的更多的内存。
>0：当用户申请内存的时候，内核会去检查是否有这么大的内存空间，当超过地址空间会被拒绝
>1：内核始终认为，有足够大的内存空间，直到它用完了位置
>2：内核禁止任何形式的过量分配内存
>Memory allocation limit = swapspace + physmem * (overcommit_ratio / 100)
>
>vm.drop_caches      写入数值可以使内核释放page_cache，dentries和inodes缓存所占的内存。
>1：只释放page_cache
>2：只释放dentries和inodes缓存
>3：释放page_cache、dentries和inodes缓存
>
>vm.zone_reclaim_mode   参数只有在启用CONFIG_NUMA选项时才有效,zone_reclaim_mode用来控制在内存域OOM时，如何来回收内存。
>0：禁止内存域回收，从其他zone分配内存
>1：启用内存域回收
>2：通过回写脏页回收内存
>4：通过swap回收内存
>
>
>vm.max_map_count     定义了一个进程能拥有的最多的内存区域
>
>vm.dirty_background_ratio   当脏页所占的百分比（相对于所有可用内存，即空闲内存页+可回收内存页）达到dirty_background_ratio时，write调用会唤醒内核的flusher线程开始回写脏页数据，直到脏页比例低于此值，与dirty_ratio不同，write调用此时并不会阻塞。
>
>
>vm.dirty_ratio     脏页所占的百分比（相对于所有可用内存，即空闲内存页+可回收内存页）达到dirty_ratio时，write调用会唤醒内核的flusher线程开始回写脏页数据，直到脏页比例低于此值，注意write调用此时会阻塞
>
>
>vm.dirty_expire_centisecs      脏数据的过期时间，超过该时间后内核的flusher线程被唤醒时会将脏数据回写到磁盘上，单位是百分之一秒。
>
>vm.page-cluster   	参数控制一次写入或读出swap分区的页面数量。它是一个对数值，如果设置为0，表示1页；如果设置为1，表示2页；如果设置为2，则表示4页。
>
>
>vm.swappiness          该值越高则linux越倾向于将部分长期没有用到的页swap，即便有足够空余物理内存(1~100)

### 2. rocketmq启动文档
>启动主节点
>nohup sh bin/mqnamesrv & > rmq.log
>
>nohup sh mqbroker -c $ROCKETMQ_HOME/conf/2m-noslave/broker-a.properties > /dev/null 2>&1 &
>
>nohup sh mqbroker -c $ROCKETMQ_HOME/conf/2m-noslave/broker-b.properties > /dev/null 2>&1 &

### 3. rocketmq搭建
>为什么选择RocketMQ
>我们来看看官方回答：
>
>“我们研究发现，对于ActiveMQ而言，随着越来越多的使用queues和topics，其IO成为了瓶颈。某些情况下，消费者缓慢（消费能力不足）还会拖慢生产者（造成消息阻塞）。虽然我们做了最大努力进行优化：节流、断路器或者回退，但是并不能进行优雅的扩展。因此我们开始专注于使用时下非常流行的kafka，但是仍然不能满足我们的要求，如低延迟和高可靠性，详情见这里。在这样的背景下，我们决定开发一个新的消息中间件来处理一系列广泛的使用场景，包括从传统的发布/订阅场景到高容量的实时交易系统中不允许消息丢失的场景。”
>
>各位看官也可以撮这里去看看RocketMQ与ActiveMQ以及Kafka的比较。
>
>核心概念
>生产者（Producer）：消息发送方，将业务系统中产生的消息发送到brokers（brokers可以理解为消息代理，生产者和消费者之间是通过brokers进行消息的通信），rocketmq提供了以下消息发送方式：同步、异步、单向。
>
>生产者组（Producer Group）：相同角色的生产者被归为同一组，比如通常情况下一个服务会部署多个实例，这多个实例就是一个组，生产者分组的作用只体现在消息回查的时候，即如果一个生产者组中的一个生产者实例发送一个事务消息到broker后挂掉了，那么broker会回查此实例所在组的其他实例，从而进行消息的提交或回滚操作。
>
>消费者（Consumer）：消息消费方，从brokers拉取消息。站在用户的角度，有以下两种消费者。
>
>主动消费者（PullConsumer）：从brokers拉取消息并消费。
>
>被动消费者（PushConsumer）：内部也是通过pull方式获取消息，只是进行了扩展和封装，并给用户预留了一个回调接口去实现，当消息到底的时候会执行用户自定义的回调接口。
>
>消费者组（Consumer Group）：和生产者组类似。其作用体现在实现消费者的负载均衡和容错，有了消费者组变得异常容易。需要注意的是：同一个消费者组的每个消费者实例订阅的主题必须相同。
>
>主题（Topic）：主题就是消息传递的类型。一个生产者实例可以发送消息到多个主题，多个生产者实例也可以发送消息到同一个主题。同样的，对于消费者端来说，一个消费者组可以订阅多个主题的消息，一个主题的消息也可以被多个消费者组订阅。
>
>消息（Message）：消息就像是你传递信息的信封。每个消息必须指定一个主题，就好比每个信封上都必须写明收件人。
>
>消息队列（Message Queues）：在主题内部，逻辑划分了多个子主题，每个子主题被称为消息队列。这个概念在实现最大并发数、故障切换等功能上有巨大的作用。
>
>标签（Tag）：标签，可以被认为是子主题。通常用于区分同一个主题下的不同作用或者说不同业务的消息。同时也是避免主题定义过多引起性能问题，通常情况下一个生产者组只向一个主题发送消息，其中不同业务的消息通过标签或者说子主题来区分。
>
>消息代理（Broker）：消息代理是RockerMQ中很重要的角色。它接收生产者发送的消息，进行消息存储，为消费者拉取消息服务。它还存储消息消耗相关的元数据，包括消费群体，消费进度偏移和主题/队列信息。
>
>命名服务（Name Server）：命名服务作为路由信息提供程序。生产者/消费者进行主题查找、消息代理查找、读取/写入消息都需要通过命名服务获取路由信息。
>
>消息顺序（Message Order）：当我们使用DefaultMQPushConsumer时，我们可以选择使用“orderly”还是“concurrently”。
>orderly：消费消息的有序化意味着消息被生产者按照每个消息队列发送的顺序消费。如果您正在处理全局顺序为强制的场景，请确保您使用的主题只有一个消息队列。注意：如果指定了消费顺序，则消息消费的最大并发性是消费组订阅的消息队列数。
>concurrently：当同时消费时，消息消费的最大并发仅限于为每个消费客户端指定的线程池。注意：此模式不再保证消息顺序。
>安装与调试
>官方要求的环境：
>64bit OS, Linux/Unix/Mac is recommended;
>64bit JDK 1.7+;
>Maven 3.2.x
>Git
>我的环境：（我喜欢使用较新的版本得意）
>CentOS Linux release 7.3.1611;
>64bit JDK 1.8.0_91;
>apache-maven-3.5.0;
>Git 1.8.3.1
>安装jdk
>麻烦各位看官自行搜索，资料多的吓人。。。微笑
>安装maven
>先去官网下载maven
>
>然后上传到安装目录，解压：
>[plain] view plain copy
>sudo tar zxvf apache-maven-3.5.0-bin.tar.gz  
>解压完成设置环境变量：
>[plain] view plain copy
>sudo vi /etc/profile  
>
>
>然后使环境变量生效：
>[plain] view plain copy
>source /etc/profile  
>最后验证是否安装成功：
>[plain] view plain copy
>mvn -v  
>安装Git（so easy大笑）
>先检查看看是否已经安装过了：
>[plain] view plain copy
>git --version  
>如果没有就开始安装：
>[plain] view plain copy
>sudo yum install git  
>安装完毕再看看：
>[plain] view plain copy
>git --version  
>下面进行RocketMq安装
>编译：
>[plain] view plain copy
>> git clone https://github.com/apache/incubator-rocketmq.git  
>> [plain] view plain copy
>> cd incubator-rocketmq  
>> [plain] view plain copy
>> mvn clean package install -Prelease-all assembly:assembly -U  
>> [plain] view plain copy
>> cd target/apache-rocketmq-all  
>> 在执行mvn编译的时候，你可能会遇到如下的问题：
>
>这是由于没有权限创建目录造成的。所以，要么你切换到root用户，要么使用sudo：
>[plain] view plain copy
>sudo mvn clean package install -Prelease-all assembly:assembly -U  
>提示：sudo: mvn: command not found。好吧，也是醉了。我们还需要在你当前用户的Home目录下的一个隐藏文件（.bashrc）中添加点东西：
>[plain] view plain copy
>> cd ~  
>> [plain] view plain copy
>> sudo vi .bashrc  
>
>添加完成后，执行：source .bashrc  使修改生效。然后再重新执行看看：
>[plain] view plain copy
>sudo mvn clean package install -Prelease-all assembly:assembly -U  
>时间稍微有点长，我的环境用了16分钟，请看官耐心等待，完成后如下图：
>
>启动RocketMQ
>修改默认配置
>由于RocketMQ默认配置要求很高，比如内存至少就要4个G，开发调试环境根本吃不消，所以我们开始启动前需要先修改这些参数。否则的话，我们很有会遇到内存分配或者不够的问题。
>修改target/apache-rocketmq-all/bin/runserver.sh
>[plain] view plain copy
>JAVA_OPT="${JAVA_OPT} -server -Xms256m -Xmx256m -Xmn128m -XX:PermSize=128m -XX:MaxPermSize=320m"  
>修改target/apache-rocketmq-all/bin/runbroker.sh
>[plain] view plain copy
>JAVA_OPT="${JAVA_OPT} -server -Xms256m -Xmx256m -Xmn128m  
>修改target/apache-rocketmq-all/bin/tools.sh
>[plain] view plain copy
>JAVA_OPT="${JAVA_OPT} -server -Xms256m -Xmx256m -Xmn128m -XX:PermSize=128m -XX:MaxPermSize=128m"  
>启动NameServer
>进入target/apache-rocketmq-all目录下
>[plain] view plain copy
>> nohup sh bin/mqnamesrv &  
>> [plain] view plain copy
>> tail -f ~/logs/rocketmqlogs/namesrv.log  
>> [plain] view plain copy
>> The Name Server boot success...  
>> 启动Broker
>> [plain] view plain copy
>> nohup sh bin/mqbroker -n localhost:9876 &  
>> [plain] view plain copy
>> tail -f ~/logs/rocketmqlogs/broker.log   
>> [plain] view plain copy
>> The broker[%s, 172.17.0.1:10911] boot success...  
>> 开放端口
>> [plain] view plain copy
>> sudo vi /etc/sysconfig/iptables  
>
>然后重启生效：
>[plain] view plain copy
>sudo systemctl restart iptables  
>添加ROCKETMQ_HOME环境变量
>[plain] view plain copy
>sudo vi /etc/profile  
>
>
>
>[plain] view plain copy
>source /etc/profile  
>
>
>java客户端
>pom.xml
>[html] view plain copy
><rocketmq.version>4.0.0-incubating</rocketmq.version>  
>
><dependency>  
>    <groupId>org.apache.rocketmq</groupId>  
>    <artifactId>rocketmq-client</artifactId>  
>    <version>${rocketmq.version}</version>  
></dependency>  
><dependency>  
>    <groupId>org.apache.rocketmq</groupId>  
>    <artifactId>rocketmq-common</artifactId>  
>    <version>${rocketmq.version}</version>  
></dependency>  
>生产者
>[java] view plain copy
>import org.apache.rocketmq.client.exception.MQClientException;  
>import org.apache.rocketmq.client.producer.DefaultMQProducer;  
>import org.apache.rocketmq.client.producer.SendResult;  
>import org.apache.rocketmq.common.message.Message;  
>
>import java.util.concurrent.TimeUnit;  
>
>public class Producer {  
>    public static void main(String[] args) throws MQClientException,  
>            InterruptedException {  
>        /** 
>         * 一个应用创建一个Producer，由应用来维护此对象，可以设置为全局对象或者单例<br> 
>         * 注意：ProducerGroupName需要由应用来保证唯一<br> 
>         * ProducerGroup这个概念发送普通的消息时，作用不大，但是发送分布式事务消息时，比较关键， 
>         * 因为服务器会回查这个Group下的任意一个Producer 
>         */  
>        DefaultMQProducer producer = new DefaultMQProducer("ProducerGroupName");  
>        producer.setNamesrvAddr("192.168.56.101:9876");  
>        producer.setInstanceName("Producer");  
>        producer.setVipChannelEnabled(false);  
>
>        /** 
>         * Producer对象在使用之前必须要调用start初始化，初始化一次即可<br> 
>         * 注意：切记不可以在每次发送消息时，都调用start方法 
>         */  
>        producer.start();  
>      
>        /** 
>         * 下面这段代码表明一个Producer对象可以发送多个topic，多个tag的消息。 
>         * 注意：send方法是同步调用，只要不抛异常就标识成功。但是发送成功也可会有多种状态，<br> 
>         * 例如消息写入Master成功，但是Slave不成功，这种情况消息属于成功，但是对于个别应用如果对消息可靠性要求极高，<br> 
>         * 需要对这种情况做处理。另外，消息可能会存在发送失败的情况，失败重试由应用来处理。 
>         */  
>        for (int i = 0; i < 1; i++) {  
>            try {  
>                {  
>                    Message msg = new Message("TopicTest1",// topic  
>                            "TagA",// tag  
>                            "OrderID001",// key  
>                            ("Hello MetaQ").getBytes());// body  
>                    SendResult sendResult = producer.send(msg);  
>                    System.out.println(sendResult);  
>                }  
>      
>                {  
>                    Message msg = new Message("TopicTest2",// topic  
>                            "TagB",// tag  
>                            "OrderID0034",// key  
>                            ("Hello MetaQ").getBytes());// body  
>                    SendResult sendResult = producer.send(msg);  
>                    System.out.println(sendResult);  
>                }  
>      
>                {  
>                    Message msg = new Message("TopicTest3",// topic  
>                            "TagC",// tag  
>                            "OrderID061",// key  
>                            ("Hello MetaQ").getBytes());// body  
>                    SendResult sendResult = producer.send(msg);  
>                    System.out.println(sendResult);  
>                }  
>            } catch (Exception e) {  
>                e.printStackTrace();  
>            }  
>            TimeUnit.MILLISECONDS.sleep(1000);  
>        }  
>      
>        /** 
>         * 应用退出时，要调用shutdown来清理资源，关闭网络连接，从MetaQ服务器上注销自己 
>         * 注意：我们建议应用在JBOSS、Tomcat等容器的退出钩子里调用shutdown方法 
>         */  
>        producer.shutdown();  
>    }  
>}  
>消费者
>
>[java] view plain copy
>import org.apache.rocketmq.client.consumer.DefaultMQPushConsumer;  
>import org.apache.rocketmq.client.consumer.listener.ConsumeConcurrentlyContext;  
>import org.apache.rocketmq.client.consumer.listener.ConsumeConcurrentlyStatus;  
>import org.apache.rocketmq.client.consumer.listener.MessageListenerConcurrently;  
>import org.apache.rocketmq.client.exception.MQClientException;  
>import org.apache.rocketmq.common.message.MessageExt;  
>
>import java.util.List;  
>
>public class PushConsumer {  
>
>    /** 
>     * 当前例子是PushConsumer用法，使用方式给用户感觉是消息从RocketMQ服务器推到了应用客户端。<br> 
>     * 但是实际PushConsumer内部是使用长轮询Pull方式从MetaQ服务器拉消息，然后再回调用户Listener方法<br> 
>     */  
>    public static void main(String[] args) throws InterruptedException,  
>            MQClientException {  
>        /** 
>         * 一个应用创建一个Consumer，由应用来维护此对象，可以设置为全局对象或者单例<br> 
>         * 注意：ConsumerGroupName需要由应用来保证唯一 
>         */  
>        DefaultMQPushConsumer consumer = new DefaultMQPushConsumer(  
>                "ConsumerGroupName");  
>        consumer.setNamesrvAddr("192.168.56.101:9876");  
>        consumer.setInstanceName("Consumber");  
>      
>        /** 
>         * 订阅指定topic下tags分别等于TagA或TagC或TagD 
>         */  
>        consumer.subscribe("TopicTest1", "TagA || TagC || TagD");  
>        /** 
>         * 订阅指定topic下所有消息<br> 
>         * 注意：一个consumer对象可以订阅多个topic 
>         */  
>        consumer.subscribe("TopicTest2", "*");  
>      
>        consumer.registerMessageListener(new MessageListenerConcurrently() {  
>      
>            /** 
>             * 默认msgs里只有一条消息，可以通过设置consumeMessageBatchMaxSize参数来批量接收消息 
>             */  
>            @Override  
>            public ConsumeConcurrentlyStatus consumeMessage(  
>                    List<MessageExt> msgs, ConsumeConcurrentlyContext context) {  
>                System.out.println(Thread.currentThread().getName()  
>                        + " Receive New Messages: " + msgs.size());  
>      
>                MessageExt msg = msgs.get(0);  
>                if (msg.getTopic().equals("TopicTest1")) {  
>                    // 执行TopicTest1的消费逻辑  
>                    if (msg.getTags() != null && msg.getTags().equals("TagA")) {  
>                        // 执行TagA的消费  
>                        System.out.println(new String(msg.getBody()));  
>                    } else if (msg.getTags() != null  
>                            && msg.getTags().equals("TagC")) {  
>                        // 执行TagC的消费  
>                    } else if (msg.getTags() != null  
>                            && msg.getTags().equals("TagD")) {  
>                        // 执行TagD的消费  
>                    }  
>                } else if (msg.getTopic().equals("TopicTest2")) {  
>                    System.out.println(new String(msg.getBody()));  
>                }  
>      
>                return ConsumeConcurrentlyStatus.CONSUME_SUCCESS;  
>            }  
>        });  
>      
>        /** 
>         * Consumer对象在使用之前必须要调用start初始化，初始化一次即可<br> 
>         */  
>        consumer.start();  
>      
>        System.out.println("Consumer Started.");  
>    }  
>}  