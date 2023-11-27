# Windows CLI IP management

## CMD

```powershell
ipconfig /all                          #Gives detailed info about network configuration
netsh interface show interface         #Lists all interfaces and status

#Change to a static IP address
netsh interface ip set address name="InterfaceX" static IP NETMASK GATEWAY


```

## Powershell

```powershell
#Change IP
New-NetIPAddress -InterfaceAlias "InterfaceX" -IPAddress "IP" -PrefixLenght NN 
$New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress "192.168.1.15" -PrefixLenght 24
#Add gateway
New-NetRoute -InterfaceAlias "InterfaceX" -DestinationPrefix "0.0.0.0/0" -NextHop "GatewayX"
#Removes IP from interface
Remove-NetIPAddress -InterfaceAlias "InterfaceX" -IPAddress "IPtoRemove"

```
