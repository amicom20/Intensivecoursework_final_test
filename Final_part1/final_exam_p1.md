# 1 create a CDH cluster on AWS

## a. linux setup
```
i. add user
		1. sudo useradd training -u 3800
	    2. sudo passwd training
	    3. sudo groupadd skcc
		   sudo usermod -a -G skcc training


	
호스트1		ssh -i /c/Users/SKCC18D00075/Documents/skcc.pem centos@13.209.106.171
호스트2		ssh -i /c/Users/SKCC18D00075/Documents/skcc.pem centos@13.209.135.199
호스트3		ssh -i /c/Users/SKCC18D00075/Documents/skcc.pem centos@13.209.183.51
호스트4		ssh -i /c/Users/SKCC18D00075/Documents/skcc.pem centos@13.209.185.208
호스트5		ssh -i /c/Users/SKCC18D00075/Documents/skcc.pem centos@13.209.196.243

호스트1 172.31.6.53
호스트2 172.31.9.242
호스트3 172.31.2.62
호스트4 172.31.15.245
호스트5 172.31.5.127
```


## 설치 절차
```
1. Enable user / password login for each of the 5 nodes 
a. Create a password for user “centos” 
	sudo passwd centos
	
b. Modify sshd_config to allow password login 
	sudo vi /etc/ssh/sshd_config
		change -> PasswordAuthentication yes
		
c. Restart the sshd.service 
   sudo service sshd restart


2. Setup /etc/hosts with the following information for each of the 5 hosts 
sudo vi /etc/hosts

172.31.6.53 nd1.student4.com student4_nd1
172.31.9.242 nd2.student4.com student4_nd2
172.31.2.62 nd3.student4.com student4_nd3
172.31.15.245 nd4.student4.com student4_nd4
172.31.5.127 nd5.student4.com student4_nd5


3. Change the hostname as necessary to the FQDN that you setup above a. Reboot the host 
sudo hostnamectl set-hostname nd1.student4.com
sudo hostnamectl set-hostname nd2.student4.com
sudo hostnamectl set-hostname nd3.student4.com
sudo hostnamectl set-hostname nd4.student4.com
sudo hostnamectl set-hostname nd5.student4.com



4. On the host that you will install CM: 
	a. Configure the repository for CM 5.15.2 
	
	sudo wget https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/cloudera-manager.repo \
-P /etc/yum.repos.d/
repo 파일의 내용을 아래로 변경
https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/5.15.2/

5. Install JDK on each of the hosts 
  (you may choose to install on just the host where you will install CM 
   and use the Wizard later to install on the rest 
   
   sudo yum install oracle-j2sdk1.7
   
   
4. Install CM 
	
	** Install Cloudera Manager
		sudo yum install -y cloudera-manager-daemons cloudera-manager-server


5. Install and enable Maria DB (or a DB of your choice) 
		i. Don’t forget to secure your DB installation 
		
		sudo yum install -y mariadb-server
		sudo systemctl enable mariadb
		sudo systemctl start mariadb
		sudo /usr/bin/mysql_secure_installation
		(## Root password -> password)
		
6. Install the mysql connector or mariadb connector (모든host에 설치)
	wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.46.tar.gz
	tar zxvf mysql-connector-java-5.1.46.tar.gz

	sudo mkdir -p /usr/share/java/
	cd mysql-connector-java-5.1.46
	sudo cp mysql-connector-java-5.1.46-bin.jar /usr/share/java/mysql-connector-java.jar   


7. Create the necessary users and dbs in your database 
		i. Grant them the necessary rights 
		
		mysql -u root -p

CREATE DATABASE scm DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
CREATE DATABASE amon DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
CREATE DATABASE rman DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
CREATE DATABASE hue DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
CREATE DATABASE metastore DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
CREATE DATABASE sentry DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
CREATE DATABASE nav DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
CREATE DATABASE navms DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
CREATE DATABASE oozie DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;

GRANT ALL ON scm.* TO 'scm'@'%' IDENTIFIED BY 'password';
GRANT ALL ON amon.* TO 'amon'@'%' IDENTIFIED BY 'password';
GRANT ALL ON rman.* TO 'rman'@'%' IDENTIFIED BY 'password';
GRANT ALL ON hue.* TO 'hue'@'%' IDENTIFIED BY 'password';
GRANT ALL ON metastore.* TO 'hive'@'%' IDENTIFIED BY 'password';
GRANT ALL ON sentry.* TO 'sentry'@'%' IDENTIFIED BY 'password';
GRANT ALL ON nav.* TO 'nav'@'%' IDENTIFIED BY 'password';
GRANT ALL ON navms.* TO 'navms'@'%' IDENTIFIED BY 'password';
GRANT ALL ON oozie.* TO 'oozie'@'%' IDENTIFIED BY 'password';

FLUSH PRIVILEGES;
SHOW DATABASES;
EXIT;		


8. Setup the CM database 
	sudo /usr/share/cmf/schema/scm_prepare_database.sh mysql scm scm password
	sudo rm /etc/cloudera-scm-server/db.mgmt.properties
	sudo systemctl start cloudera-scm-server

9. Start the CM server and prepare to install the cluster through the CM GUI installation process
	http://13.209.106.171:7180 접속 및 로그인  admin/admin

host 명 등록
nd1.student4.com
nd2.student4.com
nd3.student4.com
nd4.student4.com
nd5.student4.com
```


## Mysql setting
```
10.Mysql setting (create user, databases and tables)

GRANT ALL ON test.* TO 'training'@'%' IDENTIFIED BY 'training';
```
