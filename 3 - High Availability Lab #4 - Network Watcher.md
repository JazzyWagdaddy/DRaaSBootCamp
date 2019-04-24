#Network Watcher

To test network communication with Network Watcher, first enable a network watcher in at least one Azure region, and then use Network Watcher's IP flow verify capability.

## Task 1 - Enable network watcher
1)	In the portal, select **All services**. In the Filter box, enter **Network Watcher**. When **Network Watcher** appears in the results, select it.
2)	Enable network watcher in the region where you deployed your VMs. Select **Regions**, to expand it, and then select ... to the right of East US.
3)	Select **Enable Network Watcher**.

## Task 2 - Packet Capture and examination
1.	Under **Network diagnostics tools**, select **Packet Capture**, then **+Add**.
2.	Enter the following and click **Ok**:
    * Resource Group: **LBGR**
    * Target VM: **VM1**
    * Packet Capture Name: **Packets**
    * Capture configuration: **Storage Account**
    * Storage Accounts:  Select the storage account previously provisioned
3.	Find the public IP address for the load balancer **LoadBalancer**.
4.	Shutdown VM2 and then copy the public IP address and paste it into the address bar of your browser. The default page of IIS web server is displayed in the browser, noting LBVM1 or LBVM2 as you refresh your browser.
5.	Stop the packet capture by returning to **Network Watcher** > **Packet Capture** > **Right-Click** > **Stop**.
6.	If you click on the .cap file it will display the blob properties.  Download the file to your documents folder on your local computer.
7.	Install **Network Monitor** from the following:
    * https://www.microsoft.com/en-US/download/details.aspx?id=4865 
8.	Launch **Network Monitor** and open the capture file from your Documents folder.  Examine the contents of your packet capture session.

