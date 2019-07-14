# ad




Server 1

Name : jm

ifconfig

inet 10.128.0.7  netmask 255.255.255.255  broadcast 10.128.0.7
inet 127.0.0.1  netmask 255.0.0.0
External IP : 35.209.43.70



sudo yum install ansible






Server 2

Name : jmslave1

ifconfig

inet 10.142.0.14  netmask 255.255.255.255  broadcast 10.142.0.14
inet 127.0.0.1  netmask 255.0.0.0
External IP : 35.211.155.102


ssh-keygen -t rsa

copied the content in id_rsa.pub to slave machine /.ssh/authorized_keys
