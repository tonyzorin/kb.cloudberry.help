# Preparing Azure for Virtual Machine restore

CloudBerry Backup can restore Image Based and Virtual Machine backups to Azure. To do that, your Azure environment must be configured properly. If you do not have configured a Resource Group, a Storage account, an Azure Virtual Network, a Storage Container and a Network Security Group, you should follow the steps below before restoring a Virtual Machine.

### 1. Create a Resource Group <a id="1-create-a-resource-group"></a>

Resource groups enable you to manage all your resources in an application together.

1.1 Go to Azure Portal. Click "Create a resource". Search for Resource Group and select it in the results. Click "Create" button.

1.2 Specify the Resource group name. Select subscription and Resource group location.

{% hint style="info" %}
For faster upload and download connection, you should select the closest location. You can check latency location on [http://azurespeedtest.azurewebsites.net/](http://azurespeedtest.azurewebsites.net/).
{% endhint %}

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-L2ume6fNKg90cMxFbXd%2F-LDvnt0aXIqidLwAOQ-q%2F-LDvo-AF4JGJ2Yk_NSlK%2Fimage.png?alt=media&token=919b528e-2e62-4995-bd77-7e641591175d)

### 2. Create a Storage Account and Container to store restored VM HDD.  <a id="2-create-a-storage-account-and-container-to-store-restored-vm-hdd"></a>

Azure Storage is a service that you can use to store unstructured and partially structured data. IT professionals who deploy Azure virtual machines rely on Azure Storage for storing virtual machine operating system and data disks.

Blobs typically represent unstructured files such as media content, virtual machine disks, backups, or logs. There are three types of blobs.

* _block blob_ is optimized for sequential access, which is ideal for media content.
* _page blob_ offers superior random access capabilities, which is only suited for virtual machine disks.
* _append blob_ applies to data append operations, without the need to modify existing content. This works best with logging and auditing activities.

Find out more information about Azure storage: [https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction)​

**Recommended storage configuration:**

{% hint style="success" %}
VM HDD container:

* **Deployment model:** Resource manager or Classic
* **Account Kind:** StorageV1 or StorageV2
* **Replication:** LRS, GRS, RA-GRS
* **Performance:** Standard or Premium
* **Access tear:** Hot
{% endhint %}

{% hint style="success" %}
Boot diagnostic storage \(if required\):

* **Deployment model:** Resource manager or Classic
* **Account Kind:** StorageV1 or StorageV2
* **Replication:** LRS, GRS, ZRS, RA-GRS
* **Performance:** Standard
* **Access tear:** Hot
{% endhint %}

2.1 To create a new Storage Account open your Resource Group. Click +Add. Search for Storage Account and select it in the results. Click "Create" button. Specify options according to your requirements and recommended storage configuration. Click **Create**.

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-L2ume6fNKg90cMxFbXd%2F-LDvtdRg_pVvHIk8O4HA%2F-LDvu6R007WQ8OF9jR3j%2Fimage.png?alt=media&token=7b6d5015-4d1c-497d-bfe0-48124df10a56)

Next, you need to create a container to store your VM in blobs. A container organizes a set of blobs, similar to a folder in a file system. All blobs reside within a container. A storage account can contain an unlimited number of containers, and a container can store an unlimited number of blobs. Note that the container's name must be in lowercase.

2.2 Open your Resource Group → Your Storage account → Blob Service. Click +Container to add new container. Specify container name and click **OK**.

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-L2ume6fNKg90cMxFbXd%2F-LDgG0JqaCSGWldTkgMS%2F-LDgIKx1Hz18ufV328Ns%2Fimage.png?alt=media&token=f1c1a331-caf9-47d6-aa33-1ff8337377f6)

### 3. Create a Virtual Network with correct Subnet.  <a id="3-create-a-virtual-network-with-correct-subnet"></a>

If you use Static IP addresses in your backed up Virtual Machine, you should use similar or the same subnet in the Azure Virtual Network. In this case, you will able to connect to your restored VM through Internet.

{% hint style="info" %}
Please note that Azure reserves first three IP addresses in a subnet for internal usage.
{% endhint %}

3.1 Go to target Resource Group. Click "Add" button. Search for Virtual Network and select it in the results. Click **Create**.

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-L2ume6fNKg90cMxFbXd%2F-LDgG0JqaCSGWldTkgMS%2F-LDgGsdzpi6uzrVREPKX%2Fimage.png?alt=media&token=8d20235f-3e5b-43d0-9a78-33b3f8de93d3)

3.2 Specify Virtual Network settings and click **Create**.

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-L2ume6fNKg90cMxFbXd%2F-LDgG0JqaCSGWldTkgMS%2F-LDgHCuoQcwMsyhbrIMz%2Fimage.png?alt=media&token=e53fb217-d1f8-4c6f-9f4c-e7ce358f5276)

### 4. Create a Network Security Group <a id="4-create-a-network-security-group"></a>

For security reasons, we strongly recommend you to create a Network Security Group and associate it with a Subnet. You can allow incoming connection to management TCP ports like 22 or 3389 in the _Inbound security rules_ tab.

4.1 Open your Resource Group and click _+Add_ button. Search for Network Security Group and select it in the results. Click **Create**.

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-L2ume6fNKg90cMxFbXd%2F-LDgI_pbMn14vszTAXe6%2F-LDgJ1XvunV7VW6mlycM%2Fimage.png?alt=media&token=cd110bb2-9564-4225-af11-2b3229d4ac60)

4.2 Specify Network Security Group settings and click **Create**.

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-L2ume6fNKg90cMxFbXd%2F-LDgI_pbMn14vszTAXe6%2F-LDgJRVhmMgo__ATZWsb%2Fimage.png?alt=media&token=651efbab-2dd1-481f-82bd-675da254ae61)

4.3 Open created Network Security Group and go to Inbound Security Rules which is part of the Settings group.

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-L2ume6fNKg90cMxFbXd%2F-LDgI_pbMn14vszTAXe6%2F-LDgJYZ_M4bpa9PmPzx-%2Fimage.png?alt=media&token=b796b044-86bb-40f5-b266-654672959422)

4.4 Click **+Add** button to add new security rule. Click **Basic**. Enter 22 in the _Port ranges_ and name as SSH. Click **Add** to add a new security rule.

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-L2ume6fNKg90cMxFbXd%2F-LDgI_pbMn14vszTAXe6%2F-LDgJguBLip8Gw9yG_Ai%2Fimage.png?alt=media&token=9681ed9a-bd34-4ccf-92e0-f19ae89ee608)

Add as many inbound security rules as you need to allow access to services hosted on the VM.

{% hint style="info" %}
There are several common ports which you should allow access to.
{% endhint %}

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-L2ume6fNKg90cMxFbXd%2F-LGkTZvn3UXYGOvu3HGG%2F-LGkUVw5OURBU3frvlpW%2FSelection_341.png?alt=media&token=b074c354-b542-4309-8f0d-5d484206d740)

### 5. Associate Network Security Group with a subnet <a id="5-associate-network-security-group-with-a-subnet"></a>

5.1 When all required rules are added, you need to associate Network Security Group with previously created subnet. Click **Subnets** in the Settings group.

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-L2ume6fNKg90cMxFbXd%2F-LDgJqNHaz7v7dLCa8TE%2F-LDgKmvEttbFqhRnPot8%2Fimage.png?alt=media&token=aa0f451c-cb67-4274-8413-d163bc6e00c6)

5.2 Click "Associate" button. Select the Virtual Network and the Subnet and click OK.

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-L2ume6fNKg90cMxFbXd%2F-LDgJqNHaz7v7dLCa8TE%2F-LDgKwxKd4q08gBKWqqy%2Fimage.png?alt=media&token=774f15df-e29d-42fd-965e-a4e670c9640f)

### 6. Enabling Serial Console  <a id="6-enabling-serial-console"></a>

For testing or troubleshooting purposes, we recommend you enable Serial Console in your Linux or Windows Machine. You will be able to configure and troubleshoot your Azure VM in the command line in Azure Portal. 

Find out more in the links below.

* Linux VM - [Accessing serial console for Linux](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/serial-console#accessing-serial-console-for-linux).
* Windows VM - [Virtual Machine Serial Console](https://docs.microsoft.com/en-us/azure/virtual-machines/troubleshooting/serial-console-windows)

