# Linux IP management

## NETPLAN

{% hint style="info" %}
Be very careful with indentation, every space matters!
{% endhint %}

```bash
sudo nano /etc/netplan/X.yaml

#Static IP example
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    enp0s8:  #Interface name
      addresses:
        - 192.168.1.10/24    #IP you want
      routes:
        - to: 0.0.0.0/0      #Everywhere, no filters, proxies, etc
          via: 192.168.1.1   #Gateway
      nameservers:
        addresses: [8.8.8.8, 9.9.9.9]  #DNS and alternative DNS
      dhcp4: no              #Static
      
      
#Dinamic IP example
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    enp3s0:   #Interface name
      dhcp4: true
      
      
#When finished always confirm changes
sudo netplan try           #Tries configuration and if there is any error explains it
sudo netplan apply         #Tries configuration and if functional applies it 

```

***

## IP

```bash
ip a                                  #Lists interfaces with detailed info
ip r                                  #Shows routing table
ip route add IP via GW dev Interface  #Adds a static route
ip route del IP dev Interface         #Deletes a static route

```
