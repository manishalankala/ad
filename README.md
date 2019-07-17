# ad


The goal: 
```
- Jenkins master with two configured slaves machines running on separate containers, that communicating over VPN only. 

- Jenkins is able to execute jobs on the slave machines.
```


Constraints:
```
- Docker images cannot have pre-installed Jenkins
- Communication between the containers should be done via VPN tunnel only (bonus points for IP-SEC VPN)
-  Slave nodes should route all the requests to the internet through the master server via VPN.
- Jenkins Master:
Jenkins master should be installed and configured using Ansible on a Debian 9 (stretch) based image 
- Slave Node:
Slave machines can be setup and configured using Dockerfile or/and Ansible
```

Requirements:
```
- Jenkins web UI interface is accessible from the host machine via web on port 8181.
- Login credentials for Jenkins must be set to user: Adnymics, Password: 1234 
- Jenkins master is able to run “traceroute www.adnymics.com” as a jobs on the slave nodes
- All the constraints are met.
- Instructions on how to spin up the containers and run the Jenkins job.
```

Delivery:
```
- 2 docker image files (one for Jenkins master, the other for the slave node)
- Ansible scripts for Jenkins installation and configuration.
- Documentation file/s
- (bonus) Single executable file that set up everything from scratch and runs the Jenkins job  
```











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
i
net 127.0.0.1  netmask 255.0.0.0

External IP : 35.211.155.102


ssh-keygen -t rsa

copied the content in id_rsa.pub to slave machine /.ssh/authorized_keys


Dockerfile for example


```
# Original credit: https://github.com/jpetazzo/dockvpn

# Smallest base image
FROM alpine:latest

LABEL maintainer="Kyle Manna <kyle@kylemanna.com>"

# Testing: pamtester
RUN echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing/" >> /etc/apk/repositories && \
    apk add --update openvpn iptables bash easy-rsa openvpn-auth-pam google-authenticator pamtester && \
    ln -s /usr/share/easy-rsa/easyrsa /usr/local/bin && \
    rm -rf /tmp/* /var/tmp/* /var/cache/apk/* /var/cache/distfiles/*

# Needed by scripts
ENV OPENVPN /etc/openvpn
ENV EASYRSA /usr/share/easy-rsa
ENV EASYRSA_PKI $OPENVPN/pki
ENV EASYRSA_VARS_FILE $OPENVPN/vars

# Prevents refused client connection because of an expired CRL
ENV EASYRSA_CRL_DAYS 3650

VOLUME ["/etc/openvpn"]

# Internally uses port 1194/udp, remap using `docker run -p 443:1194/tcp`
EXPOSE 1194/udp

CMD ["ovpn_run"]

ADD ./bin /usr/local/bin
RUN chmod a+x /usr/local/bin/*

# Add support for OTP authentication using a PAM module
ADD ./otp/openvpn /etc/pam.d/

```
