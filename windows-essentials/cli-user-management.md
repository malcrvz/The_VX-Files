---
description: Managing users with the command "net" on CMD or Powershell cmdlets
---

# CLI user management

## CMD

```powershell
net user UserX PassX /add            #Creates new user with specified password
net user UserX /active: no           #Deactivates user
net user UserX /delete               #Deletes user
net user UserX /passwordchg: no      #Prohibits user from changing their password
net localgroup GroupX /add           #Creates new local group
net localgroup GroupX UserX /add     #Adds user to specified group
net localgroup GroupX UserX /delete  #Removes user from specified group
net localgroup GroupX                #Lists users in group

```

## Powershell

```powershell
#Adds user and gives non-expiring password
New-LocalUser -Name "UserX" -Password (ConvertTo-SecureString -AsPlainText "PassX" 
Force) -PasswordNeverExpires
Remove-LocalUser -Name "UserX"                          #Removes user
New-LocalGroup -Name "GroupX"                           #Creates new local group
Remove-LocalGroup -Name "GroupX"                        #Removes local group
Add-LocalGroupMember -Group "GroupX" -Member "UserX"    #Adds user to group
Remove-LocalGroupMember -Group "GroupX" -Member "UserX" #Removes user from group
Get-LocalGroupMember -Group "GroupX"                    #Lists users in group


```

