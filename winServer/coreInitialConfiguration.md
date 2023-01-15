
# Initial Configuration

- Change admin password, if needed
``` cmd
net user Username *
```
- Change US layout
``` powershell
Set-WinSystemLocale en-US
Set-WinUserLanguageList en-US -Force
```
- Configure static IP address
- Add computer to domain and change computer name
- Enable ICMP

- Enable File and Printer sharing  in firewall Inbound Rules  
*File and Printer Sharing (NB-Session-In)*  
*File and Printer Sharing (SMB-In)*

- ICMP in firewall Inbound Rules  
*File and Printer Sharing (Echo Request –ICMPv4-In)*

- Switch Embedded Teaming
``` powershell
New-VMSwitch -Name SETswitch -NetAdapterName "NIC1","NIC2","NIC3" -EnableEmbeddedTeaming $true
```
- Install iDRAC Service Module for Windows
- Install UPS software


- Disable firewall for specific profile
``` powershell
Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False
```

- Set time zone
``` powershell
Get-TimeZone
Get-TimeZone -ListAvailable
Set-TimeZone -Name "Eastern Standard Time (US & Canada)"
```