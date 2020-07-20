# Install & Configure Zabbix Agent for Netway Managed Server Servcie 
    Document &amp; Script to install &amp; configure Zabbix Agent for Netway Manage Server Service

## Install Zabbix Agent CentOS 
1. Allow IP: 203.78.103.9 เครื่อง server ที่ต้องการติดตั้ง
2. Open Port Firwall 
    - TCP IN 10050,10051,10052 
    - TCP OUT 10050,10051,10052 

3. Install Repository ตาม version os ที่ใช้งาน
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

4. Install zabbix agent 
    ```
    # yum install zabbix-agent -y 
    ```
5. Edit file /etc/zabbix/zabbix_agentd.conf 
    ```
    Server=203.78.103.9 
    ServerActive=203.78.103.9 
    Hostname=[Servername] 
    EnableRemoteCommands=1 
    ```
6. Download file get_server_for_zabbix.pl จะได้ไฟล์ get_server_for_zabbix.pl 
    ```
    # wget http://203.78.107.84/get_server.tar.gz; tar -xvzf get_server.tar.gz 
    ```
7. run perl and restart zabbix-agent เพื่อใช้งาน 
    ```
    # perl get_server_for_zabbix.pl 
    # systemctl enable zabbix-agent.service 
    # systemctl restart zabbix-agent 
    ```
 
## Install Zabbix agent on Windows server 

1. ทำการ RDP เข้าไปที่ server ที่ต้องการติดตั้ง 

2. ทำการ Dowload file install: 
    ```
    https://www.zabbix.com/downloads/4.2.8/zabbix_agent-4.2.8-windows-amd64-openssl.msi 
    ```
3. ทำการ Extract file ที่ได้จากการ dowload และทำการติดตั้งตามรายละเอียดด้านล่าง 
    ```
    Host name: ชื่อเครื่องที่ต้องการติดตั้ง 
    Zabbix Server IP/DNS: 203.78.103.9 
    Agent listen port: 10050 
    ```
    ![zabbix-setup](/images/zabbix_w1.png)
4. ทำการ Download file get metadata โดยดาวน์โหลดได้ที่  
    ```
    wget http://203.78.107.84/get_server.zip 
    ```
5. ทำการ extract file `get_server.zip` จะได้ไฟล์ `get_server_for_zabbix.pl `

6. ทำการ move file ที่ได้ไปที่ `C:\Program Files\Zabbix Agent`

7. แก้ไขไฟล์ `zabbix_agentd.conf` โดยทำการแก้ไขในส่วนของ  
    ```
    Server=203.78.103.9 
    ServerActive=203.78.103.9 
    Hostname=[Servername] 
    EnableRemoteCommands=1 
    ```
8. ทำการติดตั้ง `Strawberryperl` เพื่อให้สามารถใช้งานคำสั่ง `perl` ได้ 

    - Download and setup perl from __[http://strawberryperl.com/](http://strawberryperl.com/)__

    - Install `CPAN module` with command line ผ่านโปรแกรม `strawberryperl`
        ```
        > cpan Win32 Net::Domain User Win32::Service 
        ```

9. เปิดโปรแกรม Powershell จาก run คำสั่ง 

    Run: `cd 'C:\Program Files\Zabbix Agent\'`

    ![image-zabbix-setup](/images/zabbix_w2.png)

    Run: `perl .\get_server_for_zabbix.pl`

    ![image-zabbix-setup](/images/zabbix_w3.png)


10. Allow port `10050`,`10051`,`10052` บน `Firewall` ทั้ง `TCP IN` และ `TCP OUT` 
    ![image-zabbix-setup](/images/zabbix_w4.png)

11. ทำการ Restart zabbix_agentd โดยดำเนินการได้ดังนี้ 
ไปที่เมนู `Run` จากนั้นใส่ข้อมูล `services.msc`
    ![image-zabbix-setup](/images/zabbix_w5.png)

    ```
    Restart zabbix_agentd 
    ```
    ![image-zabbix-setup](/images/zabbix_w6.png)
    

## Install Zabbix Agent Debian/Ubuntu 

1. Allow IP: 203.78.103.9 เครื่อง server ที่ต้องการติดตั้ง 

2. Open Port Firewall 
    ```
    iptables -A INPUT -p tcp --dport 10050 -j ACCEPT 
    iptables -A INPUT -p tcp --dport 10051 -j ACCEPT 
    iptables -A INPUT -p tcp --dport 10052 -j ACCEPT 
    ```
3. Dowload & Install Repository, zabbix-agent ตาม version os ที่ใช้งาน 

    __Debian__

    __Note!__ For Debian 9, substitute 'buster' with 'stretch' in the commands. For Debian 8, substitute 'buster' with 'jessie' in the commands. 

    ```
    # wget https://repo.zabbix.com/zabbix/4.2/debian/pool/main/z/zabbix-release/zabbix-release_4.2-2+buster_all.deb 
    # dpkg -I zabbix-release_4.2-2+buster_all.deb 
    # apt-get update 
    # apt-get install zabbix-agent 
    ```

    __Ubuntu 18.04__

    For __Ubuntu 16.04__, substitute 'bionic' with 'xenial' in the commands. 

    For __Ubuntu 14.04__, substitute 'bionic' with 'trusty' in the commands. 
    ```
    # wget https://repo.zabbix.com/zabbix/4.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_4.2-2+bionic_all.deb 
    # dpkg -i zabbix-release_4.2-2+bionic_all.deb 
    # apt-get update 
    # apt-get install zabbix-agent 
    ```

4. Edit file /etc/zabbix/zabbix_agent.conf 
    ```
    Server=203.78.103.9 
    ServerActive=203.78.103.9 
    Hostname=[Servername] 
    EnableRemoteCommands=1 
    ```
5. Download file `get_server_for_zabbix.pl` จะได้ไฟล์ `get_server_for_zabbix.pl` 
    ```
    wget http://203.78.107.84/get_server.tar.gz; tar -xvzf get_server.tar.gz 
    ```
6. run perl and restart zabbix-agent 
    ```
    # perl get_server_for_zabbix.pl 
    # systemctl enable zabbix-agent.service 
    # systemctl restart zabbix-agent 
    ```
