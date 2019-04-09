# Azure Backup


 
## Azure Backup – Files and Folders
In this lab you are going to complete several activities that highlight the capabilities of Azure backup. 
### Build a Virtual Machine
We need some source environments to backup.  
1.	Select **+ Create** a resource found on the upper, left corner of the Azure portal.
2.	Select **Compute**, and then select **Windows Server 2016 Datacenter**.
3.	Enter, or select, the following information and accept the defaults for the remaining settings:
    a) dfasdfasdf
    b) 223234

    a.	Resource Group: BackupVMs
    b .	Virtual Machine Name: Backup
    c.	Region: East US
    d.	Size: Standard Ds2 v2 (DS2_v2)
e.	Username: pick a username
f.	Password: pick a complex password
g.	Confirm Password: pick a complex password
h.	Public inbound ports:  Open RDP
i.	select Next: Disks >
4.	Click Next: Networking.
5.	Click Next: Management.
6.	Click Create new for diagnostic storage account, enter <yourinitials><shortdate>, ensure the name resolves (e.g. abc1009), click OK, and then click Next: Guest config.
7.	Review the items and then click Next: Tags.
8.	Review the items and then click Next: Review + create.
9.	Once validation passes click Create.



 
Connect to your VM
1)	Once deployment is complete, click on Resources Groups within the Azure Portal then BackupVMs.
2)	Click on the Backup Virtual Machine.
3)	Copy the Public IP address and then launch a Remote Desktop session by typing remote into your search bar on your desktop.
4)	Paste your IP address in and hit Connect.
5)	Choose More Choices on the Windows Security screen and then Use a Different Account.
6)	Enter your user name and password. Click Yes when prompted.
7)	Within the RDP session click No on the Networks blade.
8)	Disable IE Enhanced Security Configuration by:
a.	Open Server Manager
b.	Click Local Server 
c.	Click On by IE ESC
d.	Set both modes to Off
e.	Click OK
f.	Minimize Server Manager
Copy sample data
1)	Open and command prompt and paste the following command in:
net use Z: \\wagsazurefiles.file.core.windows.net\data /u:AZURE\wagsazurefiles tCfYh37xGNjIc0czqfTW9+kUHIIhlxRUPh9h4YtD/hh7FiFPn1v32RH7uV0a83E6nAa6kkVU6d+nAAeoBItpJg==
You are mapping a drive to an Azure Files Share
2)	Switch to the root of c: and enter the following:
a.	 md c:\ignite
b.	Cd\ignite
3)	Enter the following command:
a.	Robocopy z:\ c:\ignite brk222* /z
4)	Monitor the file copy process while the 9 files are copied over.
 
Create a recovery services vault
To back up your files and folders, you need to create a Recovery Services vault in the region where you want to store the data. 
1.	Back in the Azure Portal, on the Hub menu, click   
2.	Select Storage, then Backup and Site Recovery (OMS).
3.	The Recovery Services vault blade opens, enter the following:
a.	Name: MyVault
b.	Subscription: Azure Pass
c.	Resource group: BackupVMs
d.	Location: East US
4.	At the bottom of the Recovery Services vault blade click Create.
5.	When the alert appears, pin the vault to the dashboard.
It can take several minutes for the Recovery Services vault to be created. Monitor the status notifications in the upper right-hand area of the portal. Once your vault is created, it appears in the list of Recovery Services vaults. If after several minutes you don't see your vault, click Refresh.
 
Kickoff a Backup
1.	Open the Recovery Services vault then click +Backup.
2.	From the Where is your workload running? drop-down menu, select Azure.
3.	From the What do you want to backup? menu, select Virtual Machine.
4.	Click Backup.
5.	Click Ok on the backup policy tab, and then on the Select virtual machines tab select Backup  and click Ok.  Click Enable backup.
6.	At the portal, select Virtual Machines then Backup.
7.	Under Operations click on Backup and then Backup Now.  
8.	Choose a date to retain the backup and the click OK. Congratulations!  You are now performing a Cloud First Backup!
9.	On the alert click on Triggering backup for MyVM01.
10.	Click on the backup for MyVM01 and monitor status. The first backup takes about 20 minutes to complete.
11.	You can also observe the status by looking at the vault itself.  From the Azure Portal click on Resources Groups then BackupVMs then MyDRVault. Select the backup tab to monitor status of you job.

Delete Data
1.	Within your RDP session to MyVM01, open File Explorer.
2.	Expand This PC, then Windows C:, then ignite.
3.	Delete all the files within the ignite directory.
 
Restore Data
1.	Within the backup vm surf to https://portal.azure.com and logon. 
2.	Select Virtual Machines -> backup ->Backup under Operations.
3.	Click on File Recovery.  Select the [Latest] under Select recovery point and then Download Executable.
4.	Click Run when prompted.  Paste the password to run the script from the portal into PowerShell.
 
If you see that 0 recovery volumes have mounted, there was a problem with the process.  Open up Computer Management from Start/Windows Administrative tools.  Click on Disk Management.  Select the 126 GB partition on Disk 2 and assign a drive letter. Copy the files from the mount volume (typically the F: Drive) to c:\ignite.
 
5.	Notice the actions taken to mount the volume and type Q to exit.
6.	After identifying the files and copying them to a local storage location, remove (or unmount) the additional drives. To unmount the drives, on the File Recovery menu in the Azure portal, click Unmount Disks.
