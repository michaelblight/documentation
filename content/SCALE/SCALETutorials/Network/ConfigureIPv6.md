---
title: "Configuring IPv6"
description: "Provides instructions configuring a network interface and other network settings for IPv6, and configuring an SMB or NFS share for IPv6."
weight: 21
tags:
- network
- interfaces
- smb
- nfs
- ipv6
---

TrueNAS SCALE provides the option to configure network interfaces using either IPv4 or IPv6 addresses.
IPv4 networks cannot see or communicate with an IPv6 website or network unless a gateway or some other implementation is configured to allow it.
See [Implementing IPv6](https://www.truenas.com/docs/references/IPv6/) for more information.

## Configuring IPv6 Addresses
After configuring your network infrastructure for IPv6, assign the IP addresses for your SCALE system.
Use the SCALE UI to configure your network settings.
If setting SCALE up for the first time after a clean install, use the Console Setup menu to enter IPv6 addresses.

### Configuring an Interface Using the Console Setup Menu
If configuring your network settings using the Console Setup menu for the first time after installing SCALE, first configure the interface address.
Type 1, then press <kbd>Enter</kbd>.
Enter **eno1** in **name**, then the IPv6 address in **aliases**.

{{< trueimage src="/images/SCALE/Network/ConfigureIPv6InterfaceWithConsoleSetupMenu.png" alt="SCALE Console Setup Menu Edit Interface" id="SCALE Console Setup Menu Edit Interface" >}}

Save, then select **a** to apply and **p** to make it persist. Type **q** to return to the Console Setup menu.

Next, configure the IPv6 gateway address, and the nameserver addresses. Type **2**, then press <kbd>Enter</kbd>.
Enter the name server addresses provided by your IT department or Internet Service Provider (ISP), and then the gateways.
Save, then select **a** to apply and **p** to make it persist. 

### Adding an IPv6 Interface in the UI
Navigate to the **Network** screen to enter your network settings.

Click on **Add** in the **Interfaces** to open the **Add Interface** screen.

1. Enter **eno1** as the name for the interface if it is the primary interface.
2. Clear the **DHCP** checkbox, then select **Autoconfigure IPv6** if you want to create the IP address using SLAAC.
   This automatically configures the IPv6 address.
   You can only use this option one time to configure an IPv6 address for the system.
3. Enter the IPv6 address assigned to the NIC port if using a fixed IP address assignment.
4. Click **Save**
5. Test the change.
   If adding the primary interface test the network connection by opening a new browser window.
   Enter the IPv6 address inside square brackets in the URL address field, for example, [*ipv6 address*]. 
   After the system comes up, save the changes to the network interface.

To access the UI after configuring an IPv6 address, enter the IPv6 address inside square brackets in the browser URL field.
You cannot access the UI with the assigned host name when the system is configured on an IPv6 network.

### Connecting to the UI IPv6 Address
Unlike IPv4, you must enter the IPv6 address with a square bracket preceding and following the address.
You cannot enter the host name assigned to the SCALE system to access the UI.
For example, enter <code>[<i>ffff:ff:59f8:100::12</i>]</code> into the URL field of the browser window to access the UI.

## Using IPv6 with Sharing Protocols
When configuring an SMB or NFS share, first configure the bind address in the share service.
Next, configure the share user, and add the share and dataset.
Finally, add the share owner to the dataset permissions.

1. Go to **System > Services** click **Advanced Options** then edit the share service.
   
   For SMB, scroll down and select the IPv6 address as the **Bind IP Address** and click **Save**.

   For NFS, also select the IPv6 address in **Bind IP Addresses**.
   Select **Allow non-root mount**, then click **Save**.

2. Go to **Credentials > Local User** to create the share user.

3. Go to either **Shares** or **Datasets** to [create the share and dataset]({{< relref "/SCALE/SCALETutorials/Shares/SMB/_index.md#adding-an-smb-share-and-dataset" >}}). 

4. Modify the ACL permissions.
   Either click on **Edit Filesystem ACL** on the **Shares** screen or go to **Datasets**, select the dataset row, scroll down and click **Edit** on the **Permissions** widget.

   Leave the dataset permissions @owner and @group set to root or change them to the admin user. 
   Next click **Add New** to create a new ACL entry for the share user(s).
   See [Setting Up Permissions]({{< relref "PermissionsSCALE.md" >}}) for more information on adding new entries and modifying dataset permissions.

   See [Adding NFS Shares]({{< relref "AddingNFSShares.md" >}}) or [Windows Shares (SMB)]({{< relref "/SCALE/SCALETutorials/Shares/SMB/_index.md" >}}) for more information on adding shares.

### Mounting and Accessing the Share in Windows
To mount or access the share in Windows, you must enter the share information using a particular syntax or it cannot find nor connect to the share.
 
The syntax requires you to replace each colon (:) in the IPv6 address with a dash (-).
Enter two forward slashes, followed by the IPv6 address with **.ipv6-literal.net** after it, then enter another forward slash, and finally the share name.

For example, <code>\\<i>ffff-ff-59f8-100--12</i>.ipv6-literal.net\<i>v6smbshare</i></code>.

<!-- 
### Setting up Rsync Tasks  Commenting out this section as the rsync task tutorials need attention and there is an open Jira ticket on issues with functionality. Ticket reports issues in CORE but it also applies to SCALE. https://ixsystems.atlassian.net/browse/NAS-129340

### Setting Up Cloud Sync Tasks  This section is commented out until we have Internet access over our IPv6 network so we can test. 

### Using IPv6 with Applications   This section is commented out until we have Internet access over our IPv6 network so we can test. 
-->
