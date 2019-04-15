# IaaS VM Disaster Recovery 

 
## Task 1 - IaaS VM Disaster Recovery 
In this lab you will create a VM in Azure, replicate it across the country, and perform a test failover to a different Azure region.

1.	Select **+ Create a resource** found on the upper, left corner of the Azure portal.
2.	Select **Windows Server 2016 VM**.
3.	Enter the following information and accept the defaults for the remaining settings:
    * Resource Group: *Create New* **VMDR**
    * Virtual Machine Name: **IaaSVM**
    * Region: **East US**
    * Size: **Standard Ds2_v2**
    * Username: pick a username and *write it down*
    * Password: `Complex.Password`
    * Confirm Password: `Complex.Password`
    * Public inbound ports:  Open RDP
    * Select **Next: Disks >**
4.	Click **Next: Networking**.
5.	Select **Next: Management**.
6.	Click **Create new** for the Diagnostic storage account and enter `yourinitialsshortdate`. Ensure the name resolves (e.g. abc1009), click **OK**, and then click **Next: Advanced >**.
7.	Review the items and then click **Next: Tags**.
8.	Review the items and then click **Next: Review + create**.
9.	Once validation passes click **Create**.

 
## Task 2 - Enable replication
1.	Once your VM is built click on **Go to resource**.
2.	Under **Operations** click **Disaster recovery**.
3.	Under **Basic Settings** and **Target region**, select the target region to which you'll replicate.
4. Click **Advanced Settings** and take note of the resource group, vNET, and availability settings ASR will configure for you.
5. Click **Review + Start replication**, review the settings, and then click **Start replication**.  This starts a job to enable replication for the VM. You may notice that Vaildating takes a few moments to process.  The fabric is ensuring that resources in your target region can be created and there’s no conflicts.

## Task 3 - Track Replication
On the alert button (the bell) click on **Enabling replication for 1 vm(s).**
1.	Notice the steps as they occur in real time.  The longest step in the process in going to be Enable replication.  Select that item and observe the series of steps taking place. IR, or Initial Replication, is the time it takes for the VM to be copied from source location to target location.    
2.	Since it may take 30 minutes to replicate the VM, now may be an appropriate time to take a break.
3.	You can check percentage complete or replication by **Virtual Machines** -> **IaaSVM** -> **Disaster Recovery**.  You may notice status sits at 0% synchronized for some time and then report upwards of 87% complete on next refresh.
4.	When all tasks are complete, you should see a similar status:
 
Until the VM is 100% synchronized and Protected, a test failover is not possible.  It may take several hours for your VM to replicate in a production environment.
 
## Task 4 - Run a Test Failover Disaster Recovery Drill
A test failover executes a failover but does not make the secondary VM active.  A drill validates your replication strategy without data loss or downtime and doesn't affect your production environment.
1.	Click the **Test Failover** icon.
2.	In Test Failover, select Latest (lowest RPO) as the recovery point to use for the failover.  Note the following:
    * **Latest (lowest RPO):** Fails the VM over with the current state of the VM but requires some processing time.
    * **Latest processed (low RTO):** Fails the VM over to the latest recovery point that was processed by the Site Recovery service. The time stamp is shown. With this option, no time is spent processing data, so it provides a low RTO (Recovery Time Objective)
    * **Latest app-consistent:** This option fails over all VMs to the latest app-consistent recovery point. The time stamp is shown.
    * **Custom:** Use this option to fail over to a specific recovery point. This option is useful for performing a test failover.
3.	Select the target Azure virtual network to which Azure VMs in the secondary region will be connected after the failover occurs.  
4.	To start the failover, click **OK**. Track progress by selecting the alert in the Notifications window. 
5.	After the failover finishes (Start the virtual machine task is successful), the replica Azure VM appears in the Azure portal under Virtual Machines. Make sure that the VM is running, sized appropriately, and connected to the appropriate network. Note that the VM does not have a Public IP address.
6.	To delete the VMs that were created during the test failover, select **IaaSVM-test** from **Virtual Machines**, select **Disaster recovery** under  **Operations**, and then choose **Cleanup test failover**. In Notes, record and save any observations associated with the test failover. Click the box for **Testing is complete** and click **Ok**.
If you don’t delete the failover VM, the VM will continue to run and increase your Azure consumption.
 
## Task 5 - Azure to Azure with Azure Site Recovery
We’re now going to fully protect this virtual machine with Azure Site Recovery.  In the failover scenario you just completed, if we went live with the VM in the West Region we'd have a different IP address and would need to front-end the solution with a load balancer in traffic manager.  In this lab we are going to build a failover plan in ASR.

1.	In the Azure portal, click on **Virtual Networks** and note the two existing virtual networks of **VMDR-vnet** and **VMDR-vnet-asr**.
2. Click on **+Add** and enter the following and click **Create**:
    * Name: **VMDR-vnet-failover**
    * Address space: **10.0.0.0/24**
    * Resource Group: **VMDR**
    * Location: **West US**
    * Subnet name: **VMDR-subnet-failover**
    * Subnet Address range: **10.0.0.0/24**

