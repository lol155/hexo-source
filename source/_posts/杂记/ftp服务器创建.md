---
title: FTP服务器简单创建
categories: 学习
tags: [ftp,linux]
toc: true
date: 2017-11-04 22:15:10
scaffolds:
---
# 1. FTP服务器简单创建
## 1.1. １.环境
CentOS release 6.5 (Final)-amd64 
  
- 查看linux版本:  
>[root@localhost vsftpd]# **cat /etc/issue**  
CentOS release 6.5 (Final)  
Kernel \r on an \m  
[root@localhost vsftpd]# **lsb_release -a**  
LSB Version:	:base-4.0-amd64:base-4.0-noarch:core-4.0-amd64:core-4.0-noarch:graphics-4.0-amd64:graphics-4.0-noarch:printing-4.0-amd64:printing-4.0-noarch  
Distributor ID:	CentOS  
Description:	CentOS release 6.5 (Final)  
Release:	6.5  
Codename:	Final  


## 1.2. 安装vsftpd
- 安装vsftpd
	
>yum install vsftpd

- 安装成功后查看
 
>[root@localhost vsftpd]# **rpm -qa |grep vsftpd**  
>vsftpd-2.2.2-21.el6.x86_64

- 开启ftp服务

>[root@localhost vsftpd]# **service vsftpd start**  
>为 vsftpd 启动 vsftpd：                                    [确定]

- 停止ftp服务

>[root@localhost vsftpd]# **service vsftpd stop**  
>关闭 vsftpd：                                              [确定]

## 1.3. 创建用户

　　专门创建一个ftp链接的用户，限制其只能通过ftp方式登录，只能访问指定给他的目录。

### 1.3.1. 创建用户
	
- 创建分组,用于存放ftp用户(可以省略)    

>groupadd ftpgroups     

- 创建ftp用户，并加入ftpgroups组，/home/ftp是自己建的目录，不存在就自己创建一个(目录自己定义)  

>useradd -d /home/ftp/ftptest -g ftpgroups ftptest	
  
　　-d 后面是指定用户目录的 -g 分配所属组 (如果没创建上面的组 可以省略-g 及后面的组名 ftpgroups)

- 给用户授权文件夹的写权限(我做的需要上传文件)

>chmod -R 755 /home/ftp


### 1.3.2. 设置密码

>passwd ftptest  

　　输入两次密码

### 1.3.3. 设置不允许用于登录

>usermod -s /sbin/nologin ftptest

　　没设置这个的时候,虽然没有ssh的登录权限,但是用xshell链接的时候会进入~bash对其他文件进行操作. 

## 1.4. 配置vsftpd

　　安装vsftpd之后,在/etc/vsftpd 下有三个配置文件:
>-rw------- 1 root root  125 2月   8 14:46 **ftpusers**   
-rw------- 1 root root  384 2月   8 15:35 **user_list**    
-rw------- 1 root root 4765 2月   8 16:35 **vsftpd.conf**  
-rwxr--r-- 1 root root  338 5月  11 2016 vsftpd_conf_migrate.sh  

### 1.4.1. 配置文件

- ftpusers 可以简单理解为黑名单,其中的用户不能访问ftp,总是生效的
- user_list 与 vsftpd.conf中的配置userlist_enable=YES 和 userlist_deny=NO功能使用,可以设置成黑名单,白名单(只有此文件里的用户才能使用ftp),具体设置看下面
- vsftpd.conf 主配置文件

修改一下配置文件让其只能访问自身目录

>vi /etc/vsftpd/vsftpd.conf


如下设置

>chroot_local_user=YES  
>chroot_list_enable=YES  
>chroot_list_file=/etc/vsftpd/chroot_list

chroot_list 文件自己创建，如果需要让某些用户可以访问其他目录，就把用户加到这个文件中

解释一下userlist_enable=YES,userlist_deny=NO   
userlist_enable 为YES时user_list文件有效,NO 则不生效  
userlist_deny 为YES时user_list文件为黑名单,NO为白名单  

通过配置指定user_list

>userlist_file=/etc/vsftpd/user_list

# 2. 我的配置

ftpusers 不变  
chroot_list 创建了内容为空  
user\_list 修改为


		# vsftpd userlist
		# If userlist_deny=NO, only allow users in this file
		# If userlist_deny=YES (default), never allow users in this file, and
		# do not even prompt for a password.
		# Note that the default vsftpd pam config also checks /etc/vsftpd/ftpusers
		# for users that are denied.
		#root
		#bin
		#daemon
		#adm
		#lp
		#sync
		#shutdown
		#halt
		#mail
		#news
		#uucp
		#operator
		#games
		#nobody
		ftptest


vsftpd.conf修改为

	# Example config file /etc/vsftpd/vsftpd.conf
	#
	# The default compiled in settings are fairly paranoid. This sample file
	# loosens things up a bit, to make the ftp daemon more usable.
	# Please see vsftpd.conf.5 for all compiled in defaults.
	#
	# READ THIS: This example file is NOT an exhaustive list of vsftpd options.
	# Please read the vsftpd.conf.5 manual page to get a full idea of vsftpd's
	# capabilities.
	#
	# Allow anonymous FTP? (Beware - allowed by default if you comment this out).
	anonymous_enable=NO
	#
	# Uncomment this to allow local users to log in.
	local_enable=YES
	#
	# Uncomment this to enable any form of FTP write command.
	write_enable=YES
	#
	# Default umask for local users is 077. You may wish to change this to 022,
	# if your users expect that (022 is used by most other ftpd's)
	local_umask=022
	#
	# Uncomment this to allow the anonymous FTP user to upload files. This only
	# has an effect if the above global write enable is activated. Also, you will
	# obviously need to create a directory writable by the FTP user.
	#anon_upload_enable=YES
	#
	# Uncomment this if you want the anonymous FTP user to be able to create
	# new directories.
	#anon_mkdir_write_enable=YES
	#
	# Activate directory messages - messages given to remote users when they
	# go into a certain directory.
	dirmessage_enable=YES
	#
	# The target log file can be vsftpd_log_file or xferlog_file.
	# This depends on setting xferlog_std_format parameter
	xferlog_enable=YES
	#
	# Make sure PORT transfer connections originate from port 20 (ftp-data).
	connect_from_port_20=YES
	#
	# If you want, you can arrange for uploaded anonymous files to be owned by
	# a different user. Note! Using "root" for uploaded files is not
	# recommended!
	#chown_uploads=YES
	#chown_username=whoever
	#
	# The name of log file when xferlog_enable=YES and xferlog_std_format=YES
	# WARNING - changing this filename affects /etc/logrotate.d/vsftpd.log
	#xferlog_file=/var/log/xferlog
	#
	# Switches between logging into vsftpd_log_file and xferlog_file files.
	# NO writes to vsftpd_log_file, YES to xferlog_file
	xferlog_std_format=YES
	#
	# You may change the default value for timing out an idle session.
	#idle_session_timeout=600
	#
	# You may change the default value for timing out a data connection.
	#data_connection_timeout=120
	#
	# It is recommended that you define on your system a unique user which the
	# ftp server can use as a totally isolated and unprivileged user.
	#nopriv_user=ftpsecure
	#
	# Enable this and the server will recognise asynchronous ABOR requests. Not
	# recommended for security (the code is non-trivial). Not enabling it,
	# however, may confuse older FTP clients.
	#async_abor_enable=YES
	#
	# By default the server will pretend to allow ASCII mode but in fact ignore
	# the request. Turn on the below options to have the server actually do ASCII
	# mangling on files when in ASCII mode.
	# Beware that on some FTP servers, ASCII support allows a denial of service
	# attack (DoS) via the command "SIZE /big/file" in ASCII mode. vsftpd
	# predicted this attack and has always been safe, reporting the size of the
	# raw file.
	# ASCII mangling is a horrible feature of the protocol.
	#ascii_upload_enable=YES
	#ascii_download_enable=YES
	#
	# You may fully customise the login banner string:
	#ftpd_banner=Welcome to blah FTP service.
	#
	# You may specify a file of disallowed anonymous e-mail addresses. Apparently
	# useful for combatting certain DoS attacks.
	#deny_email_enable=YES
	# (default follows)
	#banned_email_file=/etc/vsftpd/banned_emails
	#
	# You may specify an explicit list of local users to chroot() to their home
	# directory. If chroot_local_user is YES, then this list becomes a list of
	# users to NOT chroot().
	#chroot_local_user=YES
	#chroot_list_enable=YES
	# (default follows)
	#chroot_list_file=/etc/vsftpd/chroot_list
	#
	# You may activate the "-R" option to the builtin ls. This is disabled by
	# default to avoid remote users being able to cause excessive I/O on large
	# sites. However, some broken FTP clients such as "ncftp" and "mirror" assume
	# the presence of the "-R" option, so there is a strong case for enabling it.
	#ls_recurse_enable=YES
	#
	# When "listen" directive is enabled, vsftpd runs in standalone mode and
	# listens on IPv4 sockets. This directive cannot be used in conjunction
	# with the listen_ipv6 directive.
	listen=YES
	#
	# This directive enables listening on IPv6 sockets. To listen on IPv4 and IPv6
	# sockets, you must run two copies of vsftpd with two configuration files.
	# Make sure, that one of the listen options is commented !!
	#listen_ipv6=YES
	
	pam_service_name=vsftpd
	userlist_enable=YES
	tcp_wrappers=YES
	userlist_deny=NO
	userlist_file=/etc/vsftpd/user_list
	chroot_local_user=YES
	chroot_list_enable=YES
	chroot_list_file=/etc/vsftpd/chroot_list
	
# 3. JAVA上传下载

## 3.1. java需要的jar

commons-io-2.1.jar  
commons-net-1.4.1.jar

## 3.2. java代码demo

	package ftp;
	import java.io.File;
	import java.io.FileInputStream;
	import java.io.FileOutputStream;
	import java.io.IOException;
	
	import org.apache.commons.io.IOUtils;
	import org.apache.commons.net.ftp.FTPClient;


	public class FtpTest1 {
		public static void main(String[] args) { 
	        testUpload(); 
	        testDownload(); 
	    } 
	
	    /** 
	     * FTP上传单个文件测试 
	     */ 
	    public static void testUpload() { 
	        FTPClient ftpClient = new FTPClient(); 
	        FileInputStream fis = null; 
	
	        try { 
	        	ftpClient.connect("192.168.0.1"); 
	        	System.out.print(ftpClient.getReplyString());
	            ftpClient.login("username", "password"); 
	            System.out.println(ftpClient.getReplyCode() + ftpClient.getReplyString());
	            File srcFile = new File("C:\\Users\\jk\\Desktop\\img.jpg"); 
	            System.out.println(srcFile.exists());
	            fis = new FileInputStream(srcFile); 
	            //设置上传目录 
	            ftpClient.changeWorkingDirectory("/admin"); 
	            System.out.println(ftpClient.getReplyCode() + ftpClient.getReplyString());
	            ftpClient.setBufferSize(1024); 
	            System.out.println(ftpClient.getReplyCode() + ftpClient.getReplyString());
	            ftpClient.setControlEncoding("GBK"); 
	            System.out.println(ftpClient.getReplyCode() + ftpClient.getReplyString());
	            //设置文件类型（二进制） 
	            ftpClient.setFileType(FTPClient.BINARY_FILE_TYPE); 
	            System.out.println(ftpClient.getReplyCode() + ftpClient.getReplyString());
	            ftpClient.storeFile("5.gif", fis); 
	            System.out.println(ftpClient.getReplyCode() + ftpClient.getReplyString());
	        } catch (IOException e) { 
	            e.printStackTrace(); 
	            throw new RuntimeException("FTP客户端出错！", e); 
	        } finally { 
	            IOUtils.closeQuietly(fis); 
	            try { 
	                ftpClient.disconnect(); 
	            } catch (IOException e) { 
	                e.printStackTrace(); 
	                throw new RuntimeException("关闭FTP连接发生异常！", e); 
	            } 
	        } 
	    } 
	
	    /** 
	     * FTP下载单个文件测试 
	     */ 
	    public static void testDownload() { 
	        FTPClient ftpClient = new FTPClient(); 
	        FileOutputStream fos = null; 
	
	        try { 
	        	ftpClient.connect("192.168.0.1"); 
	        	System.out.print(ftpClient.getReplyString());
	            ftpClient.login("username", "password"); 
	            System.out.print(ftpClient.getReplyString());
	            
	            String remoteFileName = "/3.gif"; 
	            fos = new FileOutputStream("C:/Users/jk/Desktop/3.jpg"); 
	
	            ftpClient.setBufferSize(1024); 
	            System.out.print(ftpClient.getReplyString());
	            //设置文件类型（二进制） 
	            ftpClient.setFileType(FTPClient.BINARY_FILE_TYPE); 
	            System.out.print(ftpClient.getReplyString());
	            ftpClient.retrieveFile(remoteFileName, fos); 
	            System.out.print(ftpClient.getReplyString());
	        } catch (IOException e) { 
	            e.printStackTrace(); 
	            throw new RuntimeException("FTP客户端出错！", e); 
	        } finally { 
	            IOUtils.closeQuietly(fos); 
	            try { 
	                ftpClient.disconnect(); 
	                System.out.print(ftpClient.getReplyString());
	            } catch (IOException e) { 
	                e.printStackTrace(); 
	                throw new RuntimeException("关闭FTP连接发生异常！", e); 
	            } 
	        } 
	    } 
	}



## 3.3. 其他（我没有用到,服务器之前就用着）

如果还是登陆不了ftp，那很有可能是selinux的问题，这个东西把他关掉就行

vi /etc/selinux/config

SELINUX=enforcing 设置成SELINUX=disabled

 

重启一下服务器

reboot

重启完了别忘了把vsftpd服务打开，默认是自启的。

 

如果连接不上，很可能是防火墙阻止了，尝试关闭防火墙

systemctl stop firewalld.service #停止firewall  
systemctl disable firewalld.service #禁止firewall开机启动  
firewall-cmd --state #查看默认防火墙状态（关闭后显示notrunning，开启后显示running）  
 

如果出现远程文件夹无法显示的情况，请使用主动模式连接，在你的ftp工具上设置。

## 3.4. linux下ftp配置文件详解

	# 匿名用户配置   
	anonymous_enable=YES         # 是否允许匿名ftp,如否则选择NO   
	anon_upload_enable=YES       # 匿名用户是否能上传   
	anon_mkdir_write_enable=YES  # 匿名用户是否能创建目录   
	anon_other_write_enable=YES  # 修改文件名和删除文件   
	  
	# 本地用户配置   
	local_enable=YES # 是否允许本地用户登录   
	local_umask=022  # umask 默认755   
	write_enable=YES   
	
	chroot_local_user=YES  # 本地用户禁锢在宿主目录中   
	chroot_list_enable=YES # 是否将系统用户限止在自己的home目录下   
	chroot_list_file=/etc/vsftpd.chroot_list # 列出的是不chroot的用户的列表   
	  
	chown_upload=YES  # 是否改变上传文件的属主   
	chown_username=username # 如果是需要输入一个系统用户名   
	  
	userlist_enable=YES   
	userlist_deny=NO   
	  
	deny_email_enable=YES # 是否允许禁止匿名用户使用某些邮件地址   
	banned_email_file=/etc/vsftpd.banned_emails # 禁止邮件地址的文件路径   
	  
	ftpd_banner=Welcome to chenlf FTP service. # 定制欢迎信息   
	dirmessage_enable=YES # 是否显示目录说明文件, 需要收工创建.message文件   
	message_file= # 设置访问一个目录时获得的目录信息文件的文件名,默认是.message   
	  
	xferlog_enable=YES # 是否记录ftp传输过程   
	xferlog_file=/var/log/vsftpd.log # ftp传输日志的路径和名字   
	xferlog_std_format=YES # 是否使用标准的ftp xferlog模式   
	  
	ascii_upload_enable=YES   # 是否使用ascii码方式上传文件   
	ascii_download_enable=YES # 是否使用ascii码方式下载文件   
	  
	connect_from_port_20=YES # 是否确信端口传输来自20(ftp-data)   
	  
	nopriv_user=ftpsecure # 运行vsftpd需要的非特权系统用户默认是nobody   
	  
	async_abor_enable=YES # 是否允许运行特殊的ftp命令async ABOR.   
	  
	# FTP服务器的资源限制   
	  
	idle_session_timeout=600 # 设置session超时时间   
	data_connection_timeout=120 # 设置数据传输超时时间   
	  
	max_clients=50 # 用户最大连接数 默认是0不限止   
	max_per_ip=5   # 每个IP地址最大连接数   
	  
	anon_max_rate=102400  # 匿名的下载速度 KB   
	local_max_rate=102400 # 普通用户的下载速度 KB   
	  
	其他配置文件   
	
	/etc/xinetd.d/vsftpd   
	  
	service ftp   
	{   
	socket_type = stream   
	wait = no   
	user = root   
	server = /usr/local/sbin/vsftpd   
	# server_args =   
	# log_on_success += DURATION USERID   
	# log_on_failure += USERID   
	nice = 10   
	disable = no   
	}   
	  
	/etc/pam.d/vsftpd   
	PAM 认证   
	  
	/etc/vsftpd.chroot_list   
	此文件包含对服务器上所有FTP内容有权限的用户名。对其他用户来说，他们在服务器上的主目录对他们显示为根目录。   
	  
	/etc/shells   
	在允许本地用户登录之前，系统默认检查是否有有效的用户 shell。以防 PAM 认证不可用的情况。   
	/etc/ftpusers   
	此文件包含*禁止*FTP登录的用户名，通常有 "root"， "uucp"， "news" 之类，因为这些用户权限太高，登录 FTP 误操作危险性大。   
	  
	防火墙设置   
	  
	如果是用默认的SuSEFirewall2，在 YaST-系统-/etc/sysconfig 编辑器，network-SuSEfirewall2   
	  
	把 ftp 添加到 FW_SERVICES_EXT_TCP，比如你还要打开 ssh 那么   
	  
	FW_SERVICES_EXT_TCP "ftp ssh"   
	  
	如果你需要被动模式 FTP 和 nat，在 YaST-系统-/etc/sysconfig 编辑器，network-SuSEfirewall2   
	  
	FW_LOAD_MODULES "ip_conntrack_ftp ip_nat_ftp"   
	  
	  
	另一种方式直接修改防火墙配置文件：   
	# cd /etc/sysconfig/   
	# vi SuSEfirewall2   
	FW_SERVICES_EXT_TCP "ftp 21 telnet 23"   
	