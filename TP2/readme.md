# Part I

# I
winget install --exact --id Microsoft.AzureCLI --version 2.67.0

az account show

az login

Pour creer la vm : 
 az vm create -g tp -n tp2 --image Ubuntu2204 --admin-username azureuser --ssh-key-values C:\Users\bylal\.ssh\id_ed25519.pub

 ssh azureusere@ip_public

 azureuser@tp2:~$ systemctl status walinuxagent

```
 walinuxagent.service - Azure Linux Agent
     Loaded: loaded (/lib/systemd/system/walinuxagent.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2025-04-02 07:14:01 UTC; 5min ago
   Main PID: 757 (python3)
      Tasks: 6 (limit: 4023)
     Memory: 40.5M
        CPU: 2.352s
     CGroup: /system.slice/walinuxagent.service
             ├─ 757 /usr/bin/python3 -u /usr/sbin/waagent -daemon
             └─1369 python3 -u bin/WALinuxAgent-2.12.0.2-py3.9.egg -run-exthandlers

Apr 02 07:14:10 tp2 python3[1369]:     pkts      bytes target     prot opt in     out     source               destinat>
Apr 02 07:14:10 tp2 python3[1369]: Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
Apr 02 07:14:10 tp2 python3[1369]:     pkts      bytes target     prot opt in     out     source               destinat>
Apr 02 07:14:10 tp2 python3[1369]: Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)
Apr 02 07:14:10 tp2 python3[1369]:     pkts      bytes target     prot opt in     out     source               destinat>
Apr 02 07:14:10 tp2 python3[1369]:        0        0 ACCEPT     tcp  --  *      *       0.0.0.0/0            168.63.129>
Apr 02 07:14:10 tp2 python3[1369]:       97    13103 ACCEPT     tcp  --  *      *       0.0.0.0/0            168.63.129>
Apr 02 07:14:10 tp2 python3[1369]:        0        0 DROP       tcp  --  *      *       0.0.0.0/0            168.63.129>
Apr 02 07:14:10 tp2 python3[1369]: 2025-04-02T07:14:10.355802Z INFO ExtHandler ExtHandler Looking for existing remote a>
Apr 02 07:14:10 tp2 python3[1369]: 2025-04-02T07:14:10.376441Z INFO ExtHandler ExtHandler 
```

azureuser@tp2:~$ systemctl status cloud-init

```
● cloud-init.service - Cloud-init: Network Stage
     Loaded: loaded (/lib/systemd/system/cloud-init.service; enabled; vendor preset: enabled)
     Active: active (exited) since Wed 2025-04-02 07:14:01 UTC; 6min ago
   Main PID: 508 (code=exited, status=0/SUCCESS)
        CPU: 1.855s

Apr 02 07:14:01 tp2 cloud-init[513]: | . + .   +o      |
Apr 02 07:14:01 tp2 cloud-init[513]: |  . . .o*o.      |
Apr 02 07:14:01 tp2 cloud-init[513]: |  .o  .+o= E     |
Apr 02 07:14:01 tp2 cloud-init[513]: | o..*o oS .      |
Apr 02 07:14:01 tp2 cloud-init[513]: |o..Bo=Bo..       |
Apr 02 07:14:01 tp2 cloud-init[513]: |..o =o+* .       |
Apr 02 07:14:01 tp2 cloud-init[513]: |   +  +o+        |
Apr 02 07:14:01 tp2 cloud-init[513]: |    o+oo.        |
Apr 02 07:14:01 tp2 cloud-init[513]: +----[SHA256]-----+
Apr 02 07:14:01 tp2 systemd[1]: Finished Cloud-init: Network Stage.
```

# II

depuis la deuxime machine : 
```bash
azureuser2@tp22:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 7c:ed:8d:0a:c7:a4 brd ff:ff:ff:ff:ff:ff
    inet 10.0.0.6/24 metric 100 brd 10.0.0.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::7eed:8dff:fe0a:c7a4/64 scope link
       valid_lft forever preferred_lft forever
```
```bash
azureuser2@tp22:~$ ping 10.0.0.5
PING 10.0.0.5 (10.0.0.5) 56(84) bytes of data.
64 bytes from 10.0.0.5: icmp_seq=1 ttl=64 time=1.64 ms
64 bytes from 10.0.0.5: icmp_seq=2 ttl=64 time=0.906 ms
^C
--- 10.0.0.5 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 0.906/1.274/1.642/0.368 ms
```

et inversement : 
```bash
azureuser@tp2:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 60:45:bd:9b:74:17 brd ff:ff:ff:ff:ff:ff
    inet 10.0.0.5/24 metric 100 brd 10.0.0.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::6245:bdff:fe9b:7417/64 scope link
       valid_lft forever preferred_lft forever
```
```bash
$ ping 10.0.0.6
PING 10.0.0.6 (10.0.0.6) 56(84) bytes of data.
64 bytes from 10.0.0.6: icmp_seq=1 ttl=64 time=0.936 ms
64 bytes from 10.0.0.6: icmp_seq=2 ttl=64 time=0.929 ms
^C
--- 10.0.0.6 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 0.929/0.932/0.936/0.003 ms
```

# Part 2

# I

```bash
 az vm create -g tp -n tp2_3 --image Ubuntu2204 --admin-username azureuser3 --ssh-key-values C:\Users\bylal\.ssh\id_ed25519.pub --custom-data C:\Users\bylal\leo\cloud-init.txt
```


 ssh lebgultime@98.71.211.29


# II
```
 #cloud-config
users:
  - default
  - name: lebgultime
    sudo: true
    shell: /bin/bash
    ssh_authorized_keys:
      - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMLiTC7HPP/1/xMAM9anTtYASnOWwtLO9LLd0ibqsI/P bylal@LAPTOP-J211BJCB
    passwd : 
      - $6$xaK9VaZ7P/aprS3f$6KUAFbmivk93bT94XYqUd2H4H.1w1eEEFfATqT./BvHkBAC4CZiiy7W.o9qqDjeHczdlnW.isYZ1oy4PPJbey0

runcmd:
  - apt-get install ca-certificates curl
  - install -m 0755 -d /etc/apt/keyrings
  - curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
  - chmod a+r /etc/apt/keyrings/docker.asc
  - echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  - apt-get update -y
  - apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
  - systemctl enable --now docker
  - usermod -aG docker lebgultime
  - docker run -d alpine:latest
  ```

  # Part 3

```
az vm list -o table
Name            ResourceGroup          Location    Zones
--------------  ---------------------  ----------  -------
tp2_5           TP                     westeurope
tp2magueule-vm  TP2MAGUEULE-RESOURCES  westeurope
```

azureuser5@node1:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 60:45:bd:9e:45:7d brd ff:ff:ff:ff:ff:ff
    inet 10.0.0.5/24 metric 100 brd 10.0.0.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::6245:bdff:fe9e:457d/64 scope link
       valid_lft forever preferred_lft forever
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
    link/ether 12:f6:c3:0d:70:51 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::10f6:c3ff:fe0d:7051/64 scope link
       valid_lft forever preferred_lft forever

```bash
azureuser5@node1:~$ ping 10.0.0.4
PING 10.0.0.4 (10.0.0.4) 56(84) bytes of data.
64 bytes from 10.0.0.4: icmp_seq=1 ttl=64 time=1.08 ms
64 bytes from 10.0.0.4: icmp_seq=2 ttl=64 time=0.883 ms
^C
--- 10.0.0.4 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 0.883/0.980/1.078/0.097 ms
```

```
 ssh -J azureuser5@20.160.198.181 azureuser6@10.0.0.4
 ```

```bash
 azureuser6@node2:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 60:45:bd:91:65:3b brd ff:ff:ff:ff:ff:ff
    inet 10.0.0.4/24 metric 100 brd 10.0.0.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::6245:bdff:fe91:653b/64 scope link
       valid_lft forever preferred_lft forever
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
    link/ether 6a:30:32:e7:e6:6e brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::6830:32ff:fee7:e66e/64 scope link
       valid_lft forever preferred_lft forever
```