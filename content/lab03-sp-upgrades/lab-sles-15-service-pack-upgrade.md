---
title: "Task 01. SLES 15 Service Pack Upgrade"
weight: 40
---

<!--
Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
SPDX-License-Identifier: MIT-0
-->

When SUSE releases Service Packs, it is expected that customers move to these new Service Packs in order to remain supported under that general lifecycle of SLES.

In this lab, the aim is to update a server to the next available Service Pack.

### Activity 1: Run the upgrade

#### Step 1: Prepare the instance for the Service Pack upgrade.

In the same way the HPC module needed to be removed for the DMS process to function, the same applies for a Service Pack Upgrade. All active modules need an upgrade path, and since the HPC module is not supported with the base subscription of SLES 15, if it is left enabled, the Service Pack upgrade will fail. Thus, we will need to remove it again:

::alert[If a customer who wishes to upgrade to SLES 15 needs to use the HPC module, they will need to purchase the SUSE Linux Enterprise Server for HPC subscription. This is beyond the scope of this workshop, however, so we will assume that we are good to disable/remove the module.]

:::code{showCopyAction=true showLineNumbers=true} 
sudo zypper rm sle-module-hpc-release-POOL sle-module-hpc-release
:::

If the HPC module has already been removed, then it should tell you that there is nothing to do and you can proceed to the next step. Otherwise, the output will look similar to the following:

:::code{}
ec2-user@ip-172-31-84-252:~> sudo zypper rm sle-module-hpc-release-POOL sle-module-hpc-release
Reading installed packages...
Resolving package dependencies...

The following 2 packages are going to be REMOVED:
  sle-module-hpc-release sle-module-hpc-release-POOL

The following product is going to be REMOVED:
  "HPC Module"

2 packages to remove.
After the operation, 2.0 KiB will be freed.
Continue? [y/n/v/...? shows all options] (y): y
(1/2) Removing sle-module-hpc-release-12-1.5.x86_64 ..............................................................[done]
(2/2) Removing sle-module-hpc-release-POOL-12-1.5.x86_64 .........................................................[done]
:::

With the HPC module removed, proceed to the next step.

#### Step 2: Run the Upgrade

Run the migration: 

:::code{showCopyAction=true showLineNumbers=true} 
sudo zypper migration
:::

:::code{}
ec2-user@ip-172-31-18-31:~> sudo zypper migration

Executing 'zypper patch-check --updatestack-only'

Refreshing service 'Basesystem_Module_15_SP1_x86_64'.
Refreshing service 'Desktop_Applications_Module_15_SP1_x86_64'.
Refreshing service 'Development_Tools_Module_15_SP1_x86_64'.
Refreshing service 'Legacy_Module_15_SP1_x86_64'.
Refreshing service 'Public_Cloud_Module_15_SP1_x86_64'.
Refreshing service 'Python_2_Module_15_SP1_x86_64'.
Refreshing service 'SUSE_Linux_Enterprise_Server_15_SP1_x86_64'.
Refreshing service 'Server_Applications_Module_15_SP1_x86_64'.
Refreshing service 'Web_and_Scripting_Module_15_SP1_x86_64'.
Building repository 'SLE-Module-Basesystem15-SP1-Pool' cache .........................................................................................................................[done]
Building repository 'SLE-Module-Basesystem15-SP1-Updates' cache ......................................................................................................................[done]
Building repository 'SLE-Module-Desktop-Applications15-SP1-Pool' cache ...............................................................................................................[done]
Building repository 'SLE-Module-Desktop-Applications15-SP1-Updates' cache ............................................................................................................[done]
Building repository 'SLE-Module-DevTools15-SP1-Pool' cache ...........................................................................................................................[done]
Building repository 'SLE-Module-DevTools15-SP1-Updates' cache ........................................................................................................................[done]
Building repository 'SLE-Module-Legacy15-SP1-Pool' cache .............................................................................................................................[done]
Building repository 'SLE-Module-Legacy15-SP1-Updates' cache ..........................................................................................................................[done]
Building repository 'SLE-Module-Public-Cloud15-SP1-Pool' cache .......................................................................................................................[done]
Retrieving repository 'SLE-Module-Public-Cloud15-SP1-Updates' metadata................................................................................................................[done]
Building repository 'SLE-Module-Public-Cloud15-SP1-Updates' cache ....................................................................................................................[done]
Building repository 'SLE-Module-Python2-15-SP1-Pool' cache ...........................................................................................................................[done]
Building repository 'SLE-Module-Python2-15-SP1-Updates' cache ........................................................................................................................[done]
Building repository 'SLE-Product-SLES15-SP1-Pool' cache ..............................................................................................................................[done]
Building repository 'SLE-Product-SLES15-SP1-Updates' cache ...........................................................................................................................[done]
Building repository 'SLE-Module-Server-Applications15-SP1-Pool' cache ................................................................................................................[done]
Building repository 'SLE-Module-Server-Applications15-SP1-Updates' cache .............................................................................................................[done]
Building repository 'SLE-Module-Web-Scripting15-SP1-Pool' cache ......................................................................................................................[done]
Building repository 'SLE-Module-Web-Scripting15-SP1-Updates' cache ...................................................................................................................[done]
Loading repository data...
Reading installed packages...

Considering 0 out of 1 applicable patches:
0 patches needed (0 security patches)


Executing 'zypper  refresh'

Repository 'SLE-Module-Basesystem15-SP1-Pool' is up to date.                                                                                                                                                                                                  
Repository 'SLE-Module-Basesystem15-SP1-Updates' is up to date.                                                                                                                                                                                                 
Repository 'SLE-Module-Desktop-Applications15-SP1-Pool' is up to date.                                                                                                                                                                                          
Repository 'SLE-Module-Desktop-Applications15-SP1-Updates' is up to date.                                                                                                                                                                                       
Repository 'SLE-Module-DevTools15-SP1-Pool' is up to date.                                                                                                                                                                                                      
Repository 'SLE-Module-DevTools15-SP1-Updates' is up to date.                                                                                                                                                                                                   
Repository 'SLE-Module-Legacy15-SP1-Pool' is up to date.                                                                                                                                                                                                        
Repository 'SLE-Module-Legacy15-SP1-Updates' is up to date.                                                                                                                                                                                                     
Repository 'SLE-Module-Public-Cloud15-SP1-Pool' is up to date.                                                                                                                                                                                                  
Repository 'SLE-Module-Public-Cloud15-SP1-Updates' is up to date.                                                                                                                                                                                               
Repository 'SLE-Module-Python2-15-SP1-Pool' is up to date.                                                                                                                                                                                                      
Repository 'SLE-Module-Python2-15-SP1-Updates' is up to date.                                                                                                                                                                                                   
Repository 'SLE-Product-SLES15-SP1-Pool' is up to date.                                                                                                                                                                                                         
Repository 'SLE-Product-SLES15-SP1-Updates' is up to date.                                                                                                                                                                                                      
Repository 'SLE-Module-Server-Applications15-SP1-Pool' is up to date.                                                                                                                                                                                           
Repository 'SLE-Module-Server-Applications15-SP1-Updates' is up to date.                                                                                                                                                                                        
Repository 'SLE-Module-Web-Scripting15-SP1-Pool' is up to date.                                                                                                                                                                                                 
Repository 'SLE-Module-Web-Scripting15-SP1-Updates' is up to date.                                                                                                                                                                                              
All repositories have been refreshed.
Available migrations:

    1 | SUSE Linux Enterprise Server 15 SP4 x86_64
        Basesystem Module 15 SP4 x86_64
        Containers Module 15 SP4 x86_64
        Desktop Applications Module 15 SP4 x86_64
        Server Applications Module 15 SP4 x86_64
        Development Tools Module 15 SP4 x86_64
        Legacy Module 15 SP4 x86_64
        Public Cloud Module 15 SP4 x86_64
        Web and Scripting Module 15 SP4 x86_64

    2 | SUSE Linux Enterprise Server 15 SP3 x86_64
        Basesystem Module 15 SP3 x86_64
        Containers Module 15 SP3 x86_64
        Desktop Applications Module 15 SP3 x86_64
        Python 2 Module 15 SP3 x86_64
        Server Applications Module 15 SP3 x86_64
        Development Tools Module 15 SP3 x86_64
        Legacy Module 15 SP3 x86_64
        Public Cloud Module 15 SP3 x86_64
        Web and Scripting Module 15 SP3 x86_64

    3 | SUSE Linux Enterprise Server 15 SP2 x86_64
        Basesystem Module 15 SP2 x86_64
        Containers Module 15 SP2 x86_64
        Desktop Applications Module 15 SP2 x86_64
        Python 2 Module 15 SP2 x86_64
        Server Applications Module 15 SP2 x86_64
        Development Tools Module 15 SP2 x86_64
        Legacy Module 15 SP2 x86_64
        Public Cloud Module 15 SP2 x86_64
        Web and Scripting Module 15 SP2 x86_64    

[num/q]: 
:::

As you can see, it is supported to migrate the instance to one of the next 3 Service Packs.

Select Option 1 (in this example to move from SLES15 SP1 to SLES 15 SP4)

:::code{}
[num/q]: 1 
:::

The system will run through the migration preparation on screen and present with a message similar to the following.

::alert[If you are asked about switching vendors to replace an obsolete package, confirm to switch to the newer version.]

**📌 NOTE**\
Your output may be different, and display a different number of packages to upgrade, depending on the original AMI used to  build the instance and available packages in the update channels.

:::code{}
519 packages to upgrade, 5 to downgrade, 77 new, 20 to remove.
Overall download size: 594.4 MiB. Already cached: 0 B. After the operation, additional 384.2 MiB will be used.

    Note: System reboot required.

Continue? [y/n/v/...? shows all options] (y):


Select 'y' to continue the upgrade.

Continue? [y/n/v/...? shows all options] (y): y

The system will begin downloading packages.
:::

**💡 TIP**\
Select 'y' to resolve any file conflicts, ensuring the version from the newer channels are installed.

Once all the packages have been updated, reboot the system.

:::code{}
...

(596/611) Installing: yast2-kdump-4.3.4-1.4.x86_64 ..................................................................................[done]
(597/611) Installing: yast2-iscsi-lio-server-4.2.5-1.19.noarch ......................................................................[done]
(598/611) Installing: yast2-iscsi-client-4.3.3-1.1.noarch ...........................................................................[done]
(599/611) Installing: yast2-http-server-4.3.1-1.115.noarch ..........................................................................[done]
(600/611) Installing: yast2-audit-laf-4.3.1-1.99.noarch .............................................................................[done]
(601/611) Installing: yast2-dns-server-4.3.3-1.1.noarch .............................................................................[done]
(602/611) Installing: yast2-nis-server-4.3.1-1.116.noarch ...........................................................................[done]
(603/611) Installing: yast2-dhcp-server-4.3.1-1.116.noarch ..........................................................................[done]
(604/611) Installing: yast2-isns-4.3.0-1.99.noarch ..................................................................................[done]
(605/611) Installing: yast2-samba-client-4.3.3-3.3.1.noarch .........................................................................[done]
(606/611) Installing: yast2-ftp-server-4.3.3-3.3.1.noarch ...........................................................................[done]
(607/611) Installing: autoyast2-installation-4.3.86-3.17.1.noarch ...................................................................[done]
(608/611) Installing: yast2-samba-server-4.3.4-1.5.noarch ...........................................................................[done]
(609/611) Installing: yast2-add-on-4.3.8-1.4.noarch .................................................................................[done]
(610/611) Installing: yast2-registration-4.3.23-3.6.1.noarch ........................................................................[done]
(611/611) Installing: yast2-migration-4.2.5-3.3.1.noarch ............................................................................[done]

...
...

    dracut: *** Generating early-microcode cpio image ***
    dracut: *** Store current command line parameters ***
    dracut: Stored kernel commandline:
    dracut:  root=UUID=8e653a0c-f0ab-42e8-88b9-3d2097fe1a79 rootfstype=xfs rootflags=rw,relatime,attr2,inode64,noquota
    dracut: *** Creating image file '/boot/initrd-5.3.18-59.16-default' ***
    dracut: *** Creating initramfs image file '/boot/initrd-5.3.18-59.16-default' done ***


There are running programs which still use files and libraries deleted or updated by recent upgrades. They should be restarted to benefit from the latest updates. Run 'zypper ps -s' to list these programs.

Core libraries or services have been updated.
Reboot is required to ensure that your system benefits from these updates.
ec2-user@ip-172-31-18-31:~> 
:::



#### Step 3: Reboot the instance and reconnect

To let the migration take effect, reboot the instance.
    
:::code{showCopyAction=true showLineNumbers=true} 
    sudo reboot
:::

Re-attach the terminal session to the instance and confirm the Service Pack migration has been successful: 

:::code{showCopyAction=true showLineNumbers=true} 
cat /etc/os-release
:::

:::code{}
ec2-user@ip-172-31-18-31:~> cat /etc/os-release 
NAME="SLES"
VERSION="15-SP4"
VERSION_ID="15.4"
PRETTY_NAME="SUSE Linux Enterprise Server 15 SP4"
ID="sles"
ID_LIKE="suse"
ANSI_COLOR="0;32"
CPE_NAME="cpe:/o:suse:sles:15:sp3"
DOCUMENTATION_URL="https://documentation.suse.com/"
ec2-user@ip-172-31-18-31:~> 
:::

The instance is now at v15 SP4.

::alert[Remember, if you are using a personal account and are unable to complete the labs for any reason, please clean up/terminate any resources from the AWS console by following the instructions in [Lab 05. Cleanup](../../lab05-cleanup)]

**End of Lab**
