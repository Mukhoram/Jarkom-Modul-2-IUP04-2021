# Jarkom-Modul-2-IUP06-2021

### **Number 1**

EniesLobby will be used as DNS Master, Water7 will be used as DNS Slave, and Skypie will be used as Web Server. Loguetown and Alabasta will be the clients. All nodes are connected to the Foosha router <br>

First step is to make the topology, then in Foosha we run a command `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.211.0.0/16`. then we run `echo nameserver 192.168.122.1 > /etc/resolv.conf` in the nodes. <br>

no difficulties <br>


### **Number 2**

Luffy wants to contact Franky who is in EniesLobby with denden mushi. You are asked by Luffy to create the main website by accessing `franky.006.com` with the alias `www.franky.006.com` in the `kaizoku` folder. <br>

In EniesLobby, run `apt-get updat`e and `apt-get install bind9 -y`, then `nano /etc/bind/named.conf.local` file. After that edit `/etc/bind/kaizoku/franky.006.com`. To test it we could try to ping it in `Loguetown` node. <br>

no difficulties <br>

### **Number 3**

After that create a subdomain` super.franky.006.com` with the alias` www.super.franky.006.com` whose DNS is set at EniesLobby and points to Skypie(3). <br>

First `nano /etc/bind/kaizoku/franky.006.com`. We could check it by ping `super.franky.006.com` in `Loguetown`.

### **Number 4**

Also create a reverse domain for the main domain. <br>

We type this in local: <br>
```bash
zone "2.211.192.in-addr.arpa" {
    type master;
    file "/etc/bind/kaizoku/2.211.192.in-addr.arpa";
};
```
Then we edit `/etc/bind/kaizoku/2.210.192.in-addr.arpa`. To check it we use `host -t PTR 192.211.2.2` in `Loguetown`. <br>

no difficulties. <br>

### **Number 5**

In order to still be able to contact Franky if the EniesLobby server is damaged, then make Water7 the DNS Slave for the main domain <br>

In `EniesLobby` we use this in local: <br>
```bash
zone "franky.006.com" {
    type master;
  notify yes;
   also-notify { 192.211.2.3
; }; // Masukan IP Water7 tanpa tanda petik
    allow-transfer { 192.211.2.3
; }; // Masukan IP Water7 tanpa tanda petik
    file "/etc/bind/kaizoku/franky.006.com";
};
```
Then in `Water7`, use `apt-get update`, `apt-get install bind9 -y`. After that we open `nano /etc/bind/named.conf.local`, and use `service bind9 stop`. Go to `Loguetown` and ping it <br>
```bash
zone "franky.006.com" {
    type slave;
    masters { 192.211.2.2; }; // Masukan IP EniesLobby tanpa tanda petik
    file "/var/lib/bind/franky.006.com";
};
```
no difficulties

### **Number 6**

After that there is a subdomain mecha.franky.yyy.com with the alias www.mecha.franky.yyy.com which was delegated from EniesLobby to Water7 with the IP going to Skypie in the sunnygo folder <br>

`nano /etc/bind/kaizoku/franky.006.com` in `EniesLobby` and edit it. After that edit `/etc/bind/named.conf.options`. open `/etc/bind/named.conf.options` in `Water7` <br>

go to local and do: <br>
```bash
zone "mecha.franky.006.com" {
            type master;
            file "/etc/bind/sunnygo/mecha.franky.006.com"; };
```
`/etc/bind/sunnygo/mecha.franky.006.com` open it and edit it <br>

no diffculties <br>

### **Number 7**

furthermore to facilitate communication between Luffy and his comrades, a subdomain was created through Water7 with the name <br>
`general.mecha.franky.iup06.com` with the alias `www.general.mecha.franky.iup06.com` which point to Skypie <br>

water 7 edit `etc/bind/sunnygo/mecha.franky.iup06.com`
![1](https://user-images.githubusercontent.com/74299958/139528596-1caa4b62-969a-45d6-8b16-b5080cb5b850.jpg)








