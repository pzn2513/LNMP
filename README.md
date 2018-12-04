# LNMP
CentOS + Nginx + PHP7 + MySQL8 + phpMyAdmin 一键安装
整个安装流程有较酷炫的效果，适合装逼

# 安装指南：
使用root连接服务器
```bash
yum install -y git
git clone https://github.com/pzn2513/LNMP
chmod 700 LNMP/lnmp-install
cd LNMP
./lnmp-install
mysql -uroot -p
```
复制粘贴以上命令，等待安装……


安装完成！
获取MySQL临时密码，登陆后需修改密码
```bash
set global validate_password.policy=LOW;
grep 'temporary password' /var/log/mysqld.log
alter user 'root'@'localhost' identified by 'Your.Password';

#新建一个admin用户，密码类型native，用来给phpmyadmin连接数据库（当然，也可以改root密码类型）
insert mysql.user VALUES ('localhost', 'admin', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', 'Y', '', '', '', '', '0', '0', '0', '0', 'mysql_native_password', '*84AAC12F54AB666ECFC2A83C676908C8BBC381B1', 'N', '2018-04-21 11:25:16', null, 'N', 'Y', 'Y', null, null, null);
FLUSH PRIVILEGES;
alter user 'admin'@'localhost' identified by 'Your.Password';

其他操作
show variables like 'character%';
select host,user,plugin,authentication_string from mysql.user;
```
MySQL8的配置变化比较大，如果无法成功配置可以寻求帮助，见[Bugs & Issues](#bugs--issues)

输入服务器ip访问phpmyadmin： ip/phpMySQLAdmin-secret
如果访问出现403，检查selinux状态，如开启关闭即可
```bash
sestatus
setenforce 0
#上为临时关闭，永久关闭需要配置 SELINUX=disabled
vi /etc/selinux/config
```

Default Location
================
| Nginx Location             | Path                                             |
|----------------------------|--------------------------------------------------|
| Web root location          | /usr/share/nginx/html                            |
| Main Configuration File    | /etc/nginx/nginx.conf                            |

| phpMyAdmin Location        | Path                                             |
|----------------------------|--------------------------------------------------|
| Installation location      | /usr/share/nginx/html/phpMySQLAdmin-secret       |

| PHP Location               | Path                                             |
|----------------------------|--------------------------------------------------|
| Configuration File         | /etc/php.ini                                     |
| Configuration File         | /etc/php-fpm.d/www.conf                                     |

| MySQL Location             | Path                                             |
|----------------------------|--------------------------------------------------|
| my.cnf Configuration File  | /etc/my.cnf                                      |


Process Management
==================
| Process     | Command                                                         |
|-------------|-----------------------------------------------------------------|
| Nginx       | service nginx ()                                                |
| php-fpm     | service php-fpm ()                                              |
| MySQL       | systemctl (start\|stop\|status\|restart) mysqld.service         |


Bugs & Issues
=============
Please feel free to report any bugs or issues to us, email to: pznforwork@outlook.com or [issues](https://github.com/pzn2513/LAMP-autoinstall/issues) on Github.


License
=======
Copyright (C) 2016 - 2018 PZN

Licensed under the [GPLv3](https://github.com/pzn2513/LICENSE/blob/master/README.md) License.
