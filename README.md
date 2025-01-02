# SCCM - MECM - CMPivot - WQL
SCCM queries, CMPivot, etc.  For packages, see Powershell-Scripts

## Queries

### Driver Error Root Cause Analysis

Select all machines having a specific driver error code active

(Code 38 USB Driver errors) - Prior McAfee DLP / Citrix Version bug

(Code 28 Chipset serial driver error) - Update chipset and serial drivers (Bluetooth headphones don't work)
```
select SMS_R_System.Name, SMS_R_System.LastLogonUserName, SMS_G_System_PNP_DEVICE_DRIVER.ConfigManagerErrorCode, SMS_G_System_PNP_DEVICE_DRIVER.DeviceID, SMS_G_System_PNP_DEVICE_DRIVER.Name, SMS_G_System_PNP_DEVICE_DRIVER.PNPDeviceID, SMS_R_System.SMBIOSGUID from  SMS_R_System inner join SMS_G_System_PNP_DEVICE_DRIVER on SMS_G_System_PNP_DEVICE_DRIVER.ResourceID = SMS_R_System.ResourceId where SMS_G_System_PNP_DEVICE_DRIVER.ConfigManagerErrorCode = 38 order by SMS_G_System_PNP_DEVICE_DRIVER.Name DESC
```

### Software
Select all machines having Software installed using Add/Remove Programs DisplayName (Sigmaplot 13.0)
```
select SMS_R_SYSTEM.ResourceID,SMS_R_SYSTEM.ResourceType,SMS_R_SYSTEM.Name,SMS_R_SYSTEM.SMSUniqueIdentifier,SMS_R_SYSTEM.ResourceDomainORWorkgroup,SMS_R_SYSTEM.Client from SMS_R_System inner join SMS_G_System_ADD_REMOVE_PROGRAMS on SMS_G_System_ADD_REMOVE_PROGRAMS.ResourceId = SMS_R_System.ResourceId where SMS_G_System_ADD_REMOVE_PROGRAMS.DisplayName like "Sigmaplot 13%"
```
 

Other examples:
Adobe Flash Player%

Select all machine having Software installed using Add/Remove Programs DisplayName, using OR (QuantStudio Name variations)
```
select distinct SMS_R_System.NetbiosName, SMS_G_System_COMPUTER_SYSTEM.UserName, SMS_G_System_ADD_REMOVE_PROGRAMS.DisplayName, SMS_G_System_ADD_REMOVE_PROGRAMS.ProdID, SMS_G_System_ADD_REMOVE_PROGRAMS.Version, SMS_R_System.operatingSystem from  SMS_G_System_ADD_REMOVE_PROGRAMS inner join SMS_R_System on SMS_R_System.ResourceId = SMS_G_System_ADD_REMOVE_PROGRAMS.ResourceID inner join SMS_G_System_COMPUTER_SYSTEM on SMS_G_System_COMPUTER_SYSTEM.ResourceID = SMS_R_System.ResourceId where SMS_G_System_ADD_REMOVE_PROGRAMS.DisplayName like "%QuantStudio%" or SMS_G_System_ADD_REMOVE_PROGRAMS.DisplayName like "%Quant Studio%"
```
 

Other Examples: DNAStar%, %DNAStar%

Select all machines having Software installed using Installed Software ProductName
```
select SMS_R_System.Name, SMS_G_System_INSTALLED_SOFTWARE.ProductName, SMS_G_System_INSTALLED_SOFTWARE.ProductVersion from  SMS_R_System inner join SMS_G_System_INSTALLED_SOFTWARE on SMS_G_System_INSTALLED_SOFTWARE.ResourceID = SMS_R_System.ResourceId where SMS_G_System_INSTALLED_SOFTWARE.ProductName like "%google chrome%"
```
 

Other Examples

| Possible FlexLM or Sentinel HASP Network Licensing| | | |
| ----- | ----- | ----- | ----- |
| %DNAStar% | %Sigmaplot%| %Origin% | %Keystone% |
| %autodesk% | %bionumerics% | %acslx% | %matlab% |
| %autocad% | %spss% | %comsol% | %arcgis% | 
| %solidworks% | %labview% | %abaqus% | %schrodinger% |
| %graphpad% | HASPKeyDriver

| SAS | | | |
| ----- | ----- | ----- | ----- |
| %SAS% | %JMP% | %DataFlux% | %Enterprise Miner% |
| %Visual Analytics% | %Text Miner% | %EBI% | %Enterprise Reporter% |

| Adobe Products: | | | |
| ----- | ----- | ----- | ----- |
| %after effect% | %animate% | %audition% | %bridge% |
| %captivate% | %cold fusion% | %contribute% | %creative cloud% |
| %dreamweaver% | %edge animate% | %flash professional% | %Illustrator% | 
| %InCopy% | %InDesign% | %lightroom% | %muse% |
| %photoshop% | %prelude% | %premier pro% | %presenter% |
| %robohelp% | %framemaker%| %Lifecycle%Design% | |

| Misc Licensing | | | |
| ----- | ----- | ----- | ----- |
| %gartner% | %ideation% | %java% | %macvector% |
| %malwarebytes% | %genexplain% | %spss% | %biobase%

| Misc Licensing 2| | | |
| ----- | ----- | ----- | ----- |
| %bomgar% | %Sudaan%| %filemaker% | %gartner% |
| %qlikview% | %symantec% | %xmatters% | %s-plus% |

| For Science! Omit -NF| | | |
| ----- | ----- | ----- | ----- |
| %ACD-NMR% | %AcslX% | %AppliedBiosystems% | %BerkeleyMadonna% | 
| %Brucker% | %CannonMFA% | CellProfiler | CFX BioRad - NF
| CFX Maestro - NF | ChemBioOffice | ChenomxNMR | CLCGenomics |
| Dragonfly | DTOpenLayers | Heska | epoc |
| ImageScope | FlashGel | Gen5 | Graphpad |
| HALO | iBright | ImageJ | ImageQuant |
| ImageStudio | Insight | JMP | MacroPath |
| MassLynx | Mastersizer | MetaXpress | Micrografx | 
| MinION | NanoAnalyze | NanoDrop | NiceLabel |
| OSPSuite | Phoenix | QIIME | RVis | 
| SAS | SBEM | SlideWriter | Unicorn |
| SoftMax | StyleWriter | Xcalibur | Zetasizer -NF |
| HALO | G2M | Magnolia | QuantStudio |
| RStudio | MiSeQ | Actilife | Agilent | 
| Analyze IQ | CardioECR | Cellometer | cellSens | 
| CFX Manager -NF | Chemistry4D | Comet | GastroPlus | 
| GloMax | Image-Pro | ImageXpress | iQ5 | 
| LinMot | Lumenera | Mathworks | MED-PC | 
| MicroWriter | novalink | OL 756 | PathView |
| SpotFire | Statistica | Systat | titmus
| tractioninstant | transomics | vironova | Vortex
| chimera | cytoscape | digitizeit | emachineshop |
| Accelrys | MathType | PeakScanner | Rotor-Gene | 
| StatXact | Teraterm | transomics | USEPA |
| VariantReporter | VAS |

Project & Visio Licensing - Determine the count difference between Pro and Standard Versions
```
select SMS_R_System.NetbiosName, SMS_G_System_COMPUTER_SYSTEM.UserName, SMS_G_System_INSTALLED_SOFTWARE.ProductName, SMS_R_System.SystemOUName, SMS_R_System.operatingSystem from  SMS_R_System inner join SMS_G_System_INSTALLED_SOFTWARE on SMS_G_System_INSTALLED_SOFTWARE.ResourceID = SMS_R_System.ResourceId inner join SMS_G_System_COMPUTER_SYSTEM on SMS_G_System_COMPUTER_SYSTEM.ResourceID = SMS_R_System.ResourceId where SMS_G_System_INSTALLED_SOFTWARE.ProductName like "%Visio Standard%" or SMS_G_System_INSTALLED_SOFTWARE.ProductName like "%Visio Pro%" or SMS_G_System_INSTALLED_SOFTWARE.ProductName like "%Project Pro%" or SMS_G_System_INSTALLED_SOFTWARE.ProductName like "%Project Standard%"
```
 

Select all machines having Software installed below a specific version number (Adobe < 10.1):
```
select SMS_R_System.Name, SMS_R_System.OperatingSystemNameandVersion, SMS_G_System_ADD_REMOVE_PROGRAMS.DisplayName, SMS_G_System_ADD_REMOVE_PROGRAMS.Version from  SMS_R_System inner join SMS_G_System_ADD_REMOVE_PROGRAMS on SMS_G_System_ADD_REMOVE_PROGRAMS.ResourceID = SMS_R_System.ResourceId where SMS_G_System_ADD_REMOVE_PROGRAMS.DisplayName like "Adobe Acrobat%" and SMS_G_System_ADD_REMOVE_PROGRAMS.Version < "10.1"
```
 

Find Software GUID string (Origin)
```
select SMS_R_System.NetbiosName, SMS_G_System_COMPUTER_SYSTEM.UserName, SMS_G_System_ADD_REMOVE_PROGRAMS.DisplayName, SMS_G_System_ADD_REMOVE_PROGRAMS.ProdID, SMS_G_System_ADD_REMOVE_PROGRAMS.Version from  SMS_G_System_ADD_REMOVE_PROGRAMS inner join SMS_R_System on SMS_R_System.ResourceId = SMS_G_System_ADD_REMOVE_PROGRAMS.ResourceID inner join SMS_G_System_COMPUTER_SYSTEM on SMS_G_System_COMPUTER_SYSTEM.ResourceId = SMS_R_System.ResourceId where SMS_G_System_ADD_REMOVE_PROGRAMS.DisplayName like "%origin%"
```
 

Select all machines having Software installed by ARP Display Name (OneDrive)
```
select *  from  SMS_R_System inner join SMS_G_System_INSTALLED_SOFTWARE on SMS_G_System_INSTALLED_SOFTWARE.ResourceID = SMS_R_System.ResourceId where SMS_G_System_INSTALLED_SOFTWARE.ARPDisplayName like "Microsoft OneDrive"
```
 

Select machines having a specific EXE (or other specific file) in directory contents (Spybot Search & Destroy)
```
select SMS_R_System.Name, SMS_G_System_SoftwareFile.FileName, SMS_G_System_SoftwareFile.FilePath from  SMS_R_System inner join SMS_G_System_SoftwareFile on SMS_G_System_SoftwareFile.ResourceID = SMS_R_System.ResourceId where SMS_G_System_SoftwareFile.FileName = "sdshred.exe"
```
 

Select machines having a specific EXE and version number with File path (IE 11)
```
select *  from  SMS_R_System inner join SMS_G_System_SoftwareFile on SMS_G_System_SoftwareFile.ResourceID = SMS_R_System.ResourceId inner join SMS_G_System_OPERATING_SYSTEM on SMS_G_System_OPERATING_SYSTEM.ResourceID = SMS_R_System.ResourceId where SMS_G_System_SoftwareFile.FilePath like "%\\Program Files\\Internet Explorer\\" and SMS_G_System_SoftwareFile.FileName like "iexplore.exe" and SMS_G_System_SoftwareFile.FileVersion like "11.%"
```
 

Select all machines specific Windows version level
```
select distinct SMS_R_System.Name, SMS_G_System_OPERATING_SYSTEM.BuildNumber, SMS_G_System_OPERATING_SYSTEM.BuildType, SMS_G_System_OPERATING_SYSTEM.Caption, SMS_G_System_OPERATING_SYSTEM.Description, SMS_G_System_OPERATING_SYSTEM.Name, SMS_G_System_OPERATING_SYSTEM.Version, SMS_G_System_OPERATING_SYSTEM.ProductType, SMS_G_System_OPERATING_SYSTEM.PlusProductID, SMS_G_System_OPERATING_SYSTEM.PlusVersionNumber, SMS_G_System_UAComputerStatus.UpgradeAnalyticsStatus from  SMS_R_System inner join SMS_G_System_OPERATING_SYSTEM on SMS_G_System_OPERATING_SYSTEM.ResourceID = SMS_R_System.ResourceId inner join SMS_G_System_UAComputerStatus on SMS_G_System_UAComputerStatus.ResourceID = SMS_R_System.ResourceId order by SMS_R_System.Name
```
 
### Client Health

Select all Servers having a common hostname prefix, and check if SCCM client health is good
```
select SMS_R_System.ResourceId, SMS_R_System.ResourceType, SMS_R_System.Name, SMS_R_System.SMSUniqueIdentifier, SMS_R_System.ResourceDomainORWorkgroup, SMS_R_System.Client, SMS_R_System.OperatingSystemNameandVersion from  SMS_R_System where SMS_R_System.NetbiosName like "HostNamePrefix%" order by SMS_R_System.OperatingSystemNameandVersion
```
 

Select all Workstations not having SCCM client installed
```
select SMS_R_System.NetbiosName, SMS_R_System.Client from  SMS_R_System where SMS_R_System.Client not like "1" OR (SMS_R_System.Client IS NULL) order by SMS_R_System.NetbiosName
```
 

Select all Servers not having SCCM Client installed
```
select SMS_R_System.NetbiosName, SMS_R_System.Client from  SMS_R_System where SMS_R_System.Client < "1" OR (SMS_R_System.Client IS NULL) order by SMS_R_System.NetbiosName
```
GUID Compare to check client health
```
select SMS_R_System.NetbiosName, SMS_R_System.SMSUniqueIdentifier, SMS_R_System.PreviousSMSUUID from  SMS_R_System
```
SCCM Client less than specific version
```
select distinct SMS_R_System.Name, SMS_R_System.ClientVersion, SMS_R_System.Client, SMS_R_System.SMSAssignedSites from  SMS_R_System where SMS_R_System.ClientVersion < "5.00.8000.1000" or SMS_R_System.Client = "0" order by SMS_R_System.Name
```

### Auditing

Is the Windows workstation in the correct Operating system OU?  Win10 should be in Windows 10 folder(compare diff between the two queries)
```
select distinct SMS_R_System.NetbiosName, SMS_R_System.operatingSystem, SMS_R_System.SystemOUName, SMS_R_System.LastLogonUserName from  SMS_R_System where SMS_R_System.SystemOUName like "%Windows7%"
```
```
select distinct SMS_R_System.NetbiosName, SMS_R_System.operatingSystem, SMS_R_System.SystemOUName, SMS_R_System.LastLogonUserName from  SMS_R_System where SMS_R_System.SystemOUName like "%Windows10%"
```
 
Duplicate SCCM host records, one has client, one does not:
```
select SMS_R_SYSTEM.ResourceID,SMS_R_SYSTEM.ResourceType,SMS_R_SYSTEM.Name,SMS_R_SYSTEM.SMSUniqueIdentifier,SMS_R_SYSTEM.ResourceDomainORWorkgroup,SMS_R_SYSTEM.Client from SMS_R_System where   SMS_R_System.Name in (select name  from  SMS_R_System where SMS_R_System.Client is not null) and   SMS_R_System.Client is null
```
Duplicate records, same name, different GUID
```
select r.ResourceId, r.ResourceType, r.Name, r.SMSUniqueIdentifier, r.ResourceDomainORWorkgroup, r.Client from  SMS_R_System as r full join SMS_R_System as s1 on s1.ResourceId = r.ResourceId full join SMS_R_System as s2 on s2.Name = s1.Name where s1.Name = s2.Name and s1.Name not like "%unknown%" and s1.ResourceId != s2.ResourceId
```
Search for specific / duplicate MAC addresses
```
select SMS_R_System.Name, SMS_R_System.SMSUniqueIdentifier from  SMS_R_System where SMS_R_System.MACAddresses = "00:00:00:00:00:00" or SMS_R_System.MACAddresses = "00:00:00:00:00:00"  order by SMS_R_System.Name
```
Last Logged on Username
```
select Name, SMSAssignedSites, IPAddresses, IPSubnets, ADSiteName, OperatingSystemNameandVersion, ResourceDomainORWorkgroup, LastLogonUserDomain, LastLogonUserName, SMSUniqueIdentifier, ResourceId, ResourceType, NetbiosName, ClientType from sms_r_system where Client = 1 and LastLogonUserName = ##PRM:sms_r_system.LastLogonUserName##
```

### Local Administrators

Local Machine Administrators list
```
select SMS_R_System.NetbiosName, SMS_G_System_LocalAdmins.AccountName, SMS_G_System_LocalAdmins.GroupName, SMS_G_System_LocalAdmins.TimeStamp, SMS_R_System.OperatingSystemNameandVersion from  SMS_R_System inner join SMS_G_System_LocalAdmins on SMS_G_System_LocalAdmins.ResourceID = SMS_R_System.ResourceId where SMS_G_System_LocalAdmins.GroupName like "%Admin%" and
SMS_G_System_LocalAdmins.AccountName not like "%Contoso-Admins%"
and SMS_G_System_LocalAdmins.AccountName not like "%Contoso-T2%"
```

### Imaging

Machine Imaging completion date (useful for troubleshooting)
```
select distinct SMS_R_System.NetbiosName, SMS_G_System_SYSTEM_CONSOLE_USER.SystemConsoleUser, SMS_G_System_OPERATING_SYSTEM.InstallDate from  SMS_R_System inner join SMS_G_System_OPERATING_SYSTEM on SMS_G_System_OPERATING_SYSTEM.ResourceID = SMS_R_System.ResourceId inner join SMS_G_System_SYSTEM_CONSOLE_USER on SMS_G_System_SYSTEM_CONSOLE_USER.ResourceID = SMS_R_System.ResourceId order by SMS_G_System_OPERATING_SYSTEM.InstallDate
``` 

### Network adapter troubleshooting

Show all connected wireless network adapters MAC address and IP addresses
```
select distinct SMS_G_System_NETWORK_ADAPTER.ProductName, SMS_G_System_NETWORK_ADAPTER.MACAddress, SMS_G_System_NETWORK_ADAPTER_CONFIGURATION.DNSHostName from  SMS_G_System_NETWORK_ADAPTER inner join SMS_G_System_NETWORK_ADAPTER_CONFIGURATION on SMS_G_System_NETWORK_ADAPTER_CONFIGURATION.MACAddress = SMS_G_System_NETWORK_ADAPTER.MACAddress where SMS_G_System_NETWORK_ADAPTER.ProductName like "%Wireless%" or SMS_G_System_NETWORK_ADAPTER.ProductName like "%Wifi%" or SMS_G_System_NETWORK_ADAPTER.ProductName like "%Wi-Fi%"
```

### Test Rings
Select Machines having an Odd last number for MidStage Prod deployments (50% target).  Example uses Odd, swap to even numbers to get final 50%
```
select SMS_R_SYSTEM.ResourceID,SMS_R_SYSTEM.ResourceType, SMS_R_SYSTEM.Name,SMS_R_SYSTEM.SMSUniqueIdentifier, SMS_R_SYSTEM.ResourceDomainORWorkgroup, SMS_R_SYSTEM.Client from SMS_R_System where SMS_R_System.Name like "%[13579]"
```
 

Select an Active Directory group of users (useful when doing Test Rings)  Some useful choices are selecting a department group, or application group.
```
select SMS_R_SYSTEM.ResourceID,SMS_R_SYSTEM.ResourceType,SMS_R_SYSTEM.Name,SMS_R_SYSTEM.SMSUniqueIdentifier,SMS_R_SYSTEM.ResourceDomainORWorkgroup,SMS_R_SYSTEM.Client, SMS_R_User.UniqueUserName FROM SMS_R_System JOIN SMS_UserMachineRelationship ON SMS_R_System.Name=SMS_UserMachineRelationship.MachineResourceName JOIN SMS_R_User ON SMS_UserMachineRelationship.UniqueUserName=SMS_R_User.UniqueUserName Where SMS_R_User.UniqueUserName in (select UniqueUserName from SMS_R_User where UserGroupName = "DOMAIN\\Contoso App Users")
``` 

## Proactive Services

Find users with low free disk space on C (potential performance issues)
```
select distinct SMS_R_System.NetbiosName, SMS_R_System.LastLogonUserName, SMS_G_System_LOGICAL_DISK.Caption, SMS_G_System_LOGICAL_DISK.DeviceID, SMS_G_System_LOGICAL_DISK.Size, SMS_G_System_LOGICAL_DISK.FreeSpace, SMS_R_System.CreationDate from  SMS_R_System inner join SMS_G_System_LOGICAL_DISK on SMS_G_System_LOGICAL_DISK.ResourceID = SMS_R_System.ResourceId where SMS_G_System_LOGICAL_DISK.DeviceID = "C:" order by SMS_G_System_LOGICAL_DISK.FreeSpace
```
Wireless network adapters that have been disabled by CMErrorCode 22 (Find Disabled Wifi NIC)  They will likely want the adapter re-enabled, and may need an administrator (potential reduction in service tickets)
```
select SMS_R_System.NetbiosName, SMS_G_System_NETWORK_ADAPTER.Description, SMS_G_System_NETWORK_ADAPTER.ConfigManagerErrorCode, SMS_R_System.OperatingSystemNameandVersion, SMS_R_System.Active, SMS_R_System.SystemOUName, SMS_G_System_COMPUTER_SYSTEM.UserName, SMS_G_System_COMPUTER_SYSTEM.Model, SMS_G_System_PC_BIOS.SMBIOSBIOSVersion from  SMS_R_System inner join SMS_G_System_NETWORK_ADAPTER on SMS_G_System_NETWORK_ADAPTER.ResourceID = SMS_R_System.ResourceId inner join SMS_G_System_COMPUTER_SYSTEM on SMS_G_System_COMPUTER_SYSTEM.ResourceID = SMS_R_System.ResourceId inner join SMS_G_System_PC_BIOS on SMS_G_System_PC_BIOS.ResourceID = SMS_R_System.ResourceId where SMS_G_System_NETWORK_ADAPTER.ConfigManagerErrorCode = 22 and SMS_R_System.OperatingSystemNameandVersion not like "%5.1%" and SMS_G_System_NETWORK_ADAPTER.Description not like "%VPN%" and SMS_G_System_NETWORK_ADAPTER.Description not like "%Virtual%" and SMS_G_System_NETWORK_ADAPTER.Description not like "%Bluetooth%" and SMS_R_System.NetbiosName like "%LaptopPrefixDesignation-Optional%"
``` 

### Uninstall Command:

MSI: msiexec /x {GUID} /quiet /norestart

EXE: setup.exe /uninstall

MACRO: "setup.exe" -x -s -f1"%~dp0Uninstall.iss"

### Install Command

Install Sourcefiles from External location for external source control.  No file distribution mechanism needed.  SCCM can automate the deployment when developers make a change to the program (gitops in SCCM).  Notice that Execution Policy should not be bypassed in this scenerio, and signed powershell content should be enabled.  This command is testing only for demonstration:
```
powershell.exe -windowstyle hidden -ExecutionPolicy Bypass AppSCCMInstall.ps1 "\\fileserver.contoso.com\Install\App"
```
 

## Scripts

Local administrators: ```net localgroup administrators```

Add RemoteDesktop AD Users groups: ```net localgroup "remote desktop users" "Contoso Remote Control" /add```

Last Reboot Time: ```Get-CimInstance -ClassName win32_operatingsystem | select csname, lastbootuptime```


## CMPivot

Local Admins lookup:

Administrators | where Name !contains 'CustomAdmin' and Name !contains 'Domain Admins' and Name !contains 'Contoso-T2-ADMINS' and Name !contains 'Contoso-Workstation-SVCACCT' and Name !contains 'Contoso System Admins' and Name !contains 'Contoso Workstation Admins'

 

## Collections


Basic Example of Machine collection targeting Endnote users:
```
select SMS_R_SYSTEM.ResourceID,SMS_R_SYSTEM.ResourceType,SMS_R_SYSTEM.Name,SMS_R_SYSTEM.SMSUniqueIdentifier,SMS_R_SYSTEM.ResourceDomainORWorkgroup,SMS_R_SYSTEM.Client from SMS_R_System inner join SMS_G_System_ADD_REMOVE_PROGRAMS on SMS_G_System_ADD_REMOVE_PROGRAMS.ResourceID = SMS_R_System.ResourceId where SMS_G_System_ADD_REMOVE_PROGRAMS.DisplayName like "Endnote%"
```
 
AD based user groups (useful for building test ring collections)
```
select SMS_R_SYSTEM.ResourceID,SMS_R_SYSTEM.ResourceType,SMS_R_SYSTEM.Name,SMS_R_SYSTEM.SMSUniqueIdentifier,SMS_R_SYSTEM.ResourceDomainORWorkgroup,SMS_R_SYSTEM.Client from SMS_R_System  JOIN SMS_UserMachineRelationship ON SMS_R_System.Name=SMS_UserMachineRelationship.MachineResourceName  JOIN SMS_R_User ON SMS_UserMachineRelationship.UniqueUserName=SMS_R_User.UniqueUserName  Where SMS_R_User.UniqueUserName in (select UniqueUserName from SMS_R_User where UserGroupName = "Contoso\\Contoso Customer Support")
```