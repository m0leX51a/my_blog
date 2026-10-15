# Network 344b0af70f034b5f9ed2a27c4c2f7d70

## Network

#### ARP Protocol

translating IP Address to Mac Address in local network through steps

1. one system broadcast an arp request to switch asking who has ip (192.168.1.1)
2. the system who has the ip respond switch with it’s mac address
3. switch send the mac address to system who send the arp request

#### Subnet mask

![image.png](<../.gitbook/assets/image (8).png>)

#### CIDR

![image.png](<../.gitbook/assets/image 1 (5).png>)

## Mahara tech

### Tcp / ip commands

```csharp
ipconfig // to get host tcp /ip config
ping     // connectivity testing to another device i should connect to 
arp //address resolution protocol // mapping ipv4 with mac 
ftp  server_address //bye to close session   // to conncet file server 
nslookup  /// to get ip from domain name 
route // get routing tabel on  my host 
netstat     /// current conncetions to internet 
```

#### email protocols

* SMTP
* POP2
* IMAP

#### remote desktop

* rdp
* ssh

### Configure DHCP

#### 1- DHCP server configuration

* set static ip address
*   set new scope of ipv4 to enable clients to connect in this scope

    ![image.png](<../.gitbook/assets/image 2 (4).png>)

#### 2- Listen on port 67 UDP on DHCP server

*   to make client order ip from DHCP server

    ```csharp
    ipconfig /renew 

    ##for more info 
    ipconfig /all
    ```

#### 3- DHCP reservations

* to show the clients that have ordered ip before from (Address Leases ) list
* to reserve it (make it reserved for specific hosts ) it can done by reserve it and it will be shown in (Reservation list )
  * then the ipv4 will be connected to mac address of reserved clients

![image.png](<../.gitbook/assets/image 3 (3).png>)

### Configure DNS

![image.png](<../.gitbook/assets/image 4 (2).png>)

* then create zone —> zone means carry entries to make domain name translation and vice versa
* create primary zone for internal network
  *   forward lookup zone

      ![image.png](<../.gitbook/assets/image 5 (1).png>)
  *   reverse lookup zone

      ![image.png](<../.gitbook/assets/image 6 (1).png>)
* then add records or entries so that add domain of parent domain of every client

### Configure remote desktop service

to make remote administration on connected client to make troubleshooting or configuration

* it operate on RDP protocol (TCP) on 3389 Port

#### Steps

![image.png](<../.gitbook/assets/image 7 (1).png>)

*   enable service

    ![image.png](<../.gitbook/assets/image 8 (1).png>)
*   create user account

    ![image.png](<../.gitbook/assets/image 9 (1).png>)
*   configure service

    ![image.png](<../.gitbook/assets/image 10.png>)
*   add user to remote desktop group

    ![image.png](<../.gitbook/assets/image 11.png>)

### Configure FTP service

used to transfer files which’s FTP listen on Port 21

1- through cmd third party program or server (FileZila)

* install filzila server
* create an account
* add data to share to shared folder
*   then from client machine

    ```csharp
    ftp server_domain_name
    	user_name & password 
    	ls 
    	lcd location_to_download_in 
    	get file_name_to_download 
    ```

2-through web interface

```csharp
ftp://server_domain_name
	user_name & password 
```

3- through third party program or server (FileZila)

* install filezila client
* connect to server
* enter username& password
* drag & drop file

### Configure email service

it consists of 3 parts

#### 1-Mail Transfer Agent (MFA)

* Protocol SMTP Port 25 TCP
* To send mails

so to run the server and configure it

* Installation email architecture software
* Create new domain
* Create new User
* Send mails between clients

#### 2-Mail Delivery Agent (MDA)

* (l MAP) Protocol Port 143 TCP
* Post office protocol (POP3) Port 110 TCP
* To send mails

#### 3-Mail User Agent

* To access mails by user interface or third party program
