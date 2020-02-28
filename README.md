# Install & Configure Zabbix Agent for Netway Managed Server Servcie 
Document &amp; Script to install &amp; configure Zabbix Agent for Netway Manage Server Service

## Pre-Install Zabbix Agent 
  - Allow IP: 203.78.103.9 เครื่อง server ที่ต้องการติดตั้ง
  - Open Port Firwall 
    - TCP IN 10050,10051,10052 
    - TCP OUT 10050,10051,10052 

## Install Zabbix Agent CentOS 
### Install Repository ตาม version os ที่ใช้งาน
##### CentOS 7
```
rpm -Uvh https://repo.zabbix.com/zabbix/4.2/rhel/7/x86_64/zabbix-release-4.2-2.el7.noarch.rpm 
```
##### CentOS 6 
```
rpm -Uvh https://repo.zabbix.com/zabbix/4.2/rhel/7/x86_64/zabbix-release-4.2-2.el7.noarch.rpm    
```
##### CentOS 5 
```
rpm -Uvh https://repo.zabbix.com/zabbix/4.2/rhel/5/x86_64/zabbix-release-4.2-2.el5.noarch.rpm 
```

### Install zabbix agent 
```
# yum install zabbix-agent -y 
```
Edit file /etc/zabbix/zabbix_agent.conf 
```
Server=203.78.103.9 
ServerActive=203.78.103.9 
Hostname=[Servername] 
EnableRemoteCommands=1 
```
Download file get_server_for_zabbix.pl จะได้ไฟล์ get_server_for_zabbix.pl 
```
# wget http://203.78.107.84/get_server.tar.gz; tar -xvzf get_server.tar.gz 
```
run perl and restart zabbix-agent เพื่อใช้งาน 
```
# perl get_server_for_zabbix.pl 
# systemctl enable zabbix-agent.service 
# systemctl restart zabbix-agent 
```
 
## Install Zabbix agent on Windows server 

ทำการ RDP เข้าไปที่ server ที่ต้องการติดตั้ง 

ทำการ Dowload file install: 
```
https://www.zabbix.com/downloads/4.2.8/zabbix_agent-4.2.8-windows-amd64-openssl.msi 
```
ทำการ Extract file ที่ได้จากการ dowload และทำการติดตั้งตามรายละเอียดด้านล่าง 

Host name: ชื่อเครื่องที่ต้องการติดตั้ง 
```
Zabbix Server IP/DNS: 203.78.103.9 
Agent listen port: 10050 
```
 
ทำการ Download file get metadata โดยดาวน์โหลดได้ที่  
```
wget http://203.78.107.84/get_server.zip 
```
ทำการ extract file `get_server.zip` จะได้ไฟล์ `get_server_for_zabbix.pl `

ทำการ move file ที่ได้ไปที่ `C:\Program Files\Zabbix Agent`

แก้ไขไฟล์ `zabbix_agentd.conf` โดยทำการแก้ไขในส่วนของ  
```
Server=203.78.103.9 
ServerActive=203.78.103.9 
Hostname=[Servername] 
EnableRemoteCommands=1 
```
ทำการติดตั้ง `Strawberryperl` เพื่อให้สามารถใช้งานคำสั่ง `perl` ได้ 

Download and setup perl from __[http://strawberryperl.com/](http://strawberryperl.com/)__

Install `CPAN module` with command line ผ่านโปรแกรม `strawberryperl`
```
> cpan Win32 Net::Domain User Win32::Service 
```

เปิดโปรแกรม Powershell จาก run คำสั่ง 
```
> cd 'C:\Program Files\Zabbix Agent\' 
> perl .\get_server_for_zabbix.pl 
```
 
Allow port `10050`,`10051`,`10052` บน `Firewall` ทั้ง `TCP IN` และ `TCP OUT` 

ทำการ Restart zabbix_agentd โดยดำเนินการได้ดังนี้ 
ไปที่เมนู `Run` จากนั้นใส่ข้อมูล `services.msc`
```
Restart zabbix_agentd 
```
 

## Install Zabbix Agent Debian/Ubuntu 

Allow IP: 203.78.103.9 เครื่อง server ที่ต้องการติดตั้ง 

Open Port Firewall 
```
iptables -A INPUT -p tcp --dport 10050 -j ACCEPT 
iptables -A INPUT -p tcp --dport 10051 -j ACCEPT 
iptables -A INPUT -p tcp --dport 10052 -j ACCEPT 
```
Dowload & Install Repository, zabbix-agent ตาม version os ที่ใช้งาน 

#### Debian  
Note! For Debian 9, substitute 'buster' with 'stretch' in the commands. For Debian 8, substitute 'buster' with 'jessie' in the commands. 
```
# wget https://repo.zabbix.com/zabbix/4.2/debian/pool/main/z/zabbix-release/zabbix-release_4.2-2+buster_all.deb 
# dpkg -I zabbix-release_4.2-2+buster_all.deb 
# apt-get update 
# apt-get install zabbix-agent 
```

#### Ubuntu 18.04 
For Ubuntu 16.04, substitute 'bionic' with 'xenial' in the commands. 

For Ubuntu 14.04, substitute 'bionic' with 'trusty' in the commands. 
```
wget https://repo.zabbix.com/zabbix/4.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_4.2-2+bionic_all.deb 
# dpkg -i zabbix-release_4.2-2+bionic_all.deb 
# apt-get update 
# apt-get install zabbix-agent 
```

Edit file /etc/zabbix/zabbix_agent.conf 
```
Server=203.78.103.9 
ServerActive=203.78.103.9 
Hostname=[Servername] 
EnableRemoteCommands=1 
```
Download file `get_server_for_zabbix.pl` จะได้ไฟล์ `get_server_for_zabbix.pl` 
```
wget http://203.78.107.84/get_server.tar.gz; tar -xvzf get_server.tar.gz 
```
run perl and restart zabbix-agent 
```
# perl get_server_for_zabbix.pl 
# systemctl enable zabbix-agent.service 
# systemctl restart zabbix-agent 
```
