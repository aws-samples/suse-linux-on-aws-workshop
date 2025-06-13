---
title: "Task 01. SLES 12 to 15 Migration"
weight: 20
---

<!--
Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
SPDX-License-Identifier: MIT-0
-->

Lab Objectives: Use DMS to upgrade a SLES 12 instance to SLES 15.

**💡 TIP**\
It is worth noting that with SLES instances that actively being used in a commercial setting, it is important to [secure backups](https://documentation.suse.com/sles/15-SP6/single-html/SLES-upgrade/index.html#sec-update-preparation-backup) of any applications and OS binaries running on the instances before performing upgrades such as the one we will carry out in this lab. In this lab, we are performing the upgrade on an instance launched specifically for this workshop, and thus will not perform any backups.

### Activity 1: Connect to the instance

#### Step 1: Establish an SSH Session
If not already connected, establish a ssh terminal session to your running Lab instance using your preferred method.

### Activity 2: Prepare the instance to use DMS
The distribution migration system is available from the Public Cloud module. Therefore, the Public Cloud module needs to be enabled on the system prior to upgrade. For on-demand instances this module is already enabled, and you previously checked the connection to the Public Cloud module in the PINT lab. If you are starting with a fresh instance, however, check the connection in step 1.

#### Step 1: Check Public Cloud Module and other registered extensions
:::code{showCopyAction=true showLineNumbers=true} 
sudo SUSEConnect --list-extensions | grep -i Public
:::
The Public Cloud Modules should be shown as **(Activated)**.

```
ec2-user@ip-172-31-18-31:~> sudo SUSEConnect --list-extensions | grep -i Public
    Public Cloud Module 12 x86_64 (Activated)
    Deactivate with: SUSEConnect -d -p sle-module-public-cloud/12/x86_64
ec2-user@ip-172-31-18-31:~> 
```

#### Step 2: Check Filesystem Layout

It is also advised to check /etc/fstab and remove any NFS, Network or data filesystem that will not be needed during the upgrade process.
:::code{showCopyAction=true showLineNumbers=true} 
cat /etc/fstab
:::

```
ec2-user@ip-172-31-18-31:~> cat /etc/fstab
LABEL=ROOT / xfs defaults 0 0
LABEL=EFI /boot/efi vfat defaults 0 0
ec2-user@ip-172-31-18-31:~> 
```

As shown above, we have only the root file system and a boot file system mounted which is ideal.

#### Step 3: Workarounds

By default, a PAYG instance has all SLES modules connected and available, when running DMS, there may be issues when attempting to upgrade modules which no longer exist on the target version.

View the registered modules with:

:::code{showCopyAction=true showLineNumbers=true} 
sudo SUSEConnect --list-extensions
:::
In this case, the HPC module was included with SLES 12, but is no longer included in the base SLES offering for SLES 15 as seen in the [SLES 15 release notes](https://www.suse.com/releasenotes/x86_64/SUSE-SLES/15/index.html#Intro.Module).

::alert[If a customer who wishes to upgrade to SLES 15 needs to use the HPC module, they will need to purchase the SUSE Linux Enterprise Server for HPC subscription. This is beyond the scope of this workshop, however, so we will assume that we are good to disable/remove the module.]

If left enabled, the upgrade will fail. Normally, it is possible to use SUSEConnect to deactivate modules, but with recent changes to PAYG images, this option is no longer available and de-registration is disabled. Instead, we will uninstall the package in the next step.

#### Step 4: Uninstall the HPC release package
:::code{showCopyAction=true showLineNumbers=true} 
sudo zypper rm sle-module-hpc-release-POOL sle-module-hpc-release
:::

:::code{}
ec2-user@ip-172-31-84-252:~> sudo zypper rm sle-module-hpc-release-POOL sle-module-hpc-release
Loading repository data...
Warning: No repositories defined. Operating only with the installed resolvables. Nothing can be installed.
Reading installed packages...
Resolving package dependencies...

The following 2 packages are going to be REMOVED:
  sle-module-hpc-release sle-module-hpc-release-POOL

The following product is going to be REMOVED:
  "HPC Module"

2 packages to remove.
After the operation, 2.0 KiB will be freed.
Continue? [y/n/...? shows all options] (y): y
(1/2) Removing sle-module-hpc-release-12-3.3.1.x86_64 .......................................................................................................................................................[done]
(2/2) Removing sle-module-hpc-release-POOL-12-3.3.1.x86_64 ..................................................................................................................................................[done]
:::

With the HPC module (and advanced systems management module) removed, proceed to the next activity.


### Activity 3: Install DMS Packages

#### Step 1: Install DMS

To install the distribution migration system run:

:::code{showCopyAction=true showLineNumbers=true} 
sudo zypper in SLES15-Migration
:::


```
ec2-user@ip-172-31-18-31:~> sudo zypper in SLES15-Migration
Refreshing service 'Public_Cloud_Module_12_x86_64'.
Refreshing service 'SUSE_Linux_Enterprise_Server_12_SP5_x86_64'.
Loading repository data...
Reading installed packages...
Resolving package dependencies...

The following NEW package is going to be installed:
  SLES15-Migration

1 new package to install.
Overall download size: 352.1 MiB. Already cached: 0 B. After the operation, additional 391.6 MiB will be used.
Continue? [y/n/...? shows all options] (y): y
Retrieving package SLES15-Migration-2.0.24-6.x86_64                                                                                  (1/1), 352.1 MiB (391.6 MiB unpacked)
Retrieving: SLES15-Migration-2.0.24-6.x86_64.rpm ......................................................................................................[done (40.9 MiB/s)]

Checking for file conflicts: .......................................................................................................................................[done]
(1/1) Installing: SLES15-Migration-2.0.24-6.x86_64 .................................................................................................................[done]
ec2-user@ip-172-31-18-31:~> 

```
::alert[The distribution migration process can be started using different methods. One method to run the migration is the 'run_migration' included with the SLES15-Migration package. The second is to invoke the migration process via reboot after installing the suse-migration-sle15-activation package. We will be using the reboot method.]

:::code{showCopyAction=true showLineNumbers=true} 
sudo zypper in suse-migration-sle15-activation
:::

```
ec2-user@ip-172-31-18-31:~> sudo zypper in suse-migration-sle15-activation
Refreshing service 'Public_Cloud_Module_12_x86_64'.
Refreshing service 'SUSE_Linux_Enterprise_Server_12_SP5_x86_64'.
Loading repository data...
Reading installed packages...
Resolving package dependencies...

The following NEW package is going to be installed:
  suse-migration-sle15-activation

1 new package to install.
Overall download size: 10.1 KiB. Already cached: 0 B. After the operation, additional 3.0 KiB will be used.
Continue? [y/n/...? shows all options] (y): y
Retrieving package suse-migration-sle15-activation-2.0.24-6.20.1.noarch                                                              (1/1),  10.1 KiB (  3.0 KiB unpacked)
Retrieving: suse-migration-sle15-activation-2.0.24-6.20.1.noarch.rpm ...............................................................................................[done]

Checking for file conflicts: .......................................................................................................................................[done]
(1/1) Installing: suse-migration-sle15-activation-2.0.24-6.20.1.noarch .............................................................................................[done]
Additional rpm output:
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-4.12.14-122.71-default
Found initrd image: /boot/initrd-4.12.14-122.71-default
done

ec2-user@ip-172-31-18-31:~> 
```

During installation of the suse-migration-sle15-activation package the bootloader configuration is modified such that on the next boot the system will boot using the new, upgraded image. This in turns starts the automated distribution migration process.


### Activity 4: Run the 12-15 Upgrade.

#### Step 1: Activate Migration

As the suse-migration-sle15-activation packages was installed, the instance only needs a reboot to start the migration process.

:::code{showCopyAction=true} 
sudo reboot
:::
**OR**
:::code{showCopyAction=true} 
sudo run_migration
:::

The upgrade process can take around 15 minutes to complete.

#### Step 2: Follow the Upgrade Process

You can follow the progress of the running migration.

From your chosen SSH Terminal you can start a session to the instance being migrated by with the user 'migration':

e.g.
:::code{showCopyAction=true} 
ssh -i ~/.ssh/example_keypair.pem migration@IP_of_instance
:::
It may take a few minutes for the instance to reboot and become available for ssh.

When attached, there is a log file 'distro-migration.log' that contains all information about the upgrade.
:::code{showCopyAction=true} 
sudo tail -f /system-root/var/log/distro_migration.log
:::
Follow the upgrade, the process should take around 15-30 minutes, the instance will reboot once the upgrade has completed.


### Activity 5: Check the state of the instance.

#### Step 1: Reconnect to the instance.

After 15 minutes attempt to reconnect to the instance via SSH (ec2-user)

Whether the upgrade succeeded or not, a log file is available in /var/log/distro_migration.log and it will contain information about the upgrade process. If the upgrade failed, the file /etc/issue may contain a pointer to the respective log file.

:::code{showCopyAction=true} 
sudo cat /var/log/distro_migration.log
:::

```
==> /var/log/distro_migration.log &lt;==
Running ssh keys service
Getting keys from /system-root/home/ec2-user/.ssh/authorized_keys
Getting keys from /system-root/root/.ssh/authorized_keys
Save keys to /home/migration/.ssh/authorized_keys
Copying host ssh keys
Restarting sshd
Calling: ['systemctl', 'restart', 'sshd']
Empty /system-root/etc/resolv.conf, continuing without copying it
Running setup host network service
Device path devtmpfs not found and skipped
...
...
...

```




#### Step 2: Check the Upgrade Status

Check your upgrade has completed.
:::code{showCopyAction=true} 
cat /etc/os-release
:::

```
ec2-user@ip-172-31-18-31:~> cat /etc/os-release 
NAME="SLES"
VERSION="15-SP1"
VERSION_ID="15.1"
PRETTY_NAME="SUSE Linux Enterprise Server 15 SP1"
ID="sles"
ID_LIKE="suse"
ANSI_COLOR="0;32"
CPE_NAME="cpe:/o:suse:sles:15:sp1"
ec2-user@ip-172-31-18-31:~> 

```

If the os-release file still states SLES12 SP5, the upgrade has failed, troubleshoot the issue with the instructor.


Note that the instance is now connected to a series of SLES 15 repositories on SCC

:::code{showCopyAction=true} 
zypper lr
:::

```
ec2-user@ip-172-31-18-31:~> zypper lr
Repository priorities are without effect. All enabled repositories share the same priority.

#  | Alias                                                                                             | Name                                                    | Enabled | GPG Check | Refresh
---+---------------------------------------------------------------------------------------------------+---------------------------------------------------------+---------+-----------+--------
 1 | Basesystem_Module_15_SP1_x86_64:SLE-Module-Basesystem15-SP1-Debuginfo-Pool                        | SLE-Module-Basesystem15-SP1-Debuginfo-Pool              | No      | ----      | ----
 2 | Basesystem_Module_15_SP1_x86_64:SLE-Module-Basesystem15-SP1-Debuginfo-Updates                     | SLE-Module-Basesystem15-SP1-Debuginfo-Updates           | No      | ----      | ----
 3 | Basesystem_Module_15_SP1_x86_64:SLE-Module-Basesystem15-SP1-Pool                                  | SLE-Module-Basesystem15-SP1-Pool                        | Yes     | (r ) Yes  | No
 4 | Basesystem_Module_15_SP1_x86_64:SLE-Module-Basesystem15-SP1-Source-Pool                           | SLE-Module-Basesystem15-SP1-Source-Pool                 | No      | ----      | ----
 5 | Basesystem_Module_15_SP1_x86_64:SLE-Module-Basesystem15-SP1-Updates                               | SLE-Module-Basesystem15-SP1-Updates                     | Yes     | (r ) Yes  | Yes
 6 | Desktop_Applications_Module_15_SP1_x86_64:SLE-Module-Desktop-Applications15-SP1-Debuginfo-Pool    | SLE-Module-Desktop-Applications15-SP1-Debuginfo-Pool    | No      | ----      | ----
 7 | Desktop_Applications_Module_15_SP1_x86_64:SLE-Module-Desktop-Applications15-SP1-Debuginfo-Updates | SLE-Module-Desktop-Applications15-SP1-Debuginfo-Updates | No      | ----      | ----
 8 | Desktop_Applications_Module_15_SP1_x86_64:SLE-Module-Desktop-Applications15-SP1-Pool              | SLE-Module-Desktop-Applications15-SP1-Pool              | Yes     | (r ) Yes  | No
 9 | Desktop_Applications_Module_15_SP1_x86_64:SLE-Module-Desktop-Applications15-SP1-Source-Pool       | SLE-Module-Desktop-Applications15-SP1-Source-Pool       | No      | ----      | ----
10 | Desktop_Applications_Module_15_SP1_x86_64:SLE-Module-Desktop-Applications15-SP1-Updates           | SLE-Module-Desktop-Applications15-SP1-Updates           | Yes     | (r ) Yes  | Yes
11 | Development_Tools_Module_15_SP1_x86_64:SLE-Module-DevTools15-SP1-Debuginfo-Pool                   | SLE-Module-DevTools15-SP1-Debuginfo-Pool                | No      | ----      | ----
12 | Development_Tools_Module_15_SP1_x86_64:SLE-Module-DevTools15-SP1-Debuginfo-Updates                | SLE-Module-DevTools15-SP1-Debuginfo-Updates             | No      | ----      | ----
13 | Development_Tools_Module_15_SP1_x86_64:SLE-Module-DevTools15-SP1-Pool                             | SLE-Module-DevTools15-SP1-Pool                          | Yes     | (r ) Yes  | No
14 | Development_Tools_Module_15_SP1_x86_64:SLE-Module-DevTools15-SP1-Source-Pool                      | SLE-Module-DevTools15-SP1-Source-Pool                   | No      | ----      | ----
15 | Development_Tools_Module_15_SP1_x86_64:SLE-Module-DevTools15-SP1-Updates                          | SLE-Module-DevTools15-SP1-Updates                       | Yes     | (r ) Yes  | Yes
16 | Legacy_Module_15_SP1_x86_64:SLE-Module-Legacy15-SP1-Debuginfo-Pool                                | SLE-Module-Legacy15-SP1-Debuginfo-Pool                  | No      | ----      | ----
17 | Legacy_Module_15_SP1_x86_64:SLE-Module-Legacy15-SP1-Debuginfo-Updates                             | SLE-Module-Legacy15-SP1-Debuginfo-Updates               | No      | ----      | ----
18 | Legacy_Module_15_SP1_x86_64:SLE-Module-Legacy15-SP1-Pool                                          | SLE-Module-Legacy15-SP1-Pool                            | Yes     | (r ) Yes  | No
19 | Legacy_Module_15_SP1_x86_64:SLE-Module-Legacy15-SP1-Source-Pool                                   | SLE-Module-Legacy15-SP1-Source-Pool                     | No      | ----      | ----
20 | Legacy_Module_15_SP1_x86_64:SLE-Module-Legacy15-SP1-Updates                                       | SLE-Module-Legacy15-SP1-Updates                         | Yes     | (r ) Yes  | Yes
21 | Public_Cloud_Module_15_SP1_x86_64:SLE-Module-Public-Cloud15-SP1-Debuginfo-Pool                    | SLE-Module-Public-Cloud15-SP1-Debuginfo-Pool            | No      | ----      | ----
22 | Public_Cloud_Module_15_SP1_x86_64:SLE-Module-Public-Cloud15-SP1-Debuginfo-Updates                 | SLE-Module-Public-Cloud15-SP1-Debuginfo-Updates         | No      | ----      | ----
23 | Public_Cloud_Module_15_SP1_x86_64:SLE-Module-Public-Cloud15-SP1-Pool                              | SLE-Module-Public-Cloud15-SP1-Pool                      | Yes     | (r ) Yes  | No
24 | Public_Cloud_Module_15_SP1_x86_64:SLE-Module-Public-Cloud15-SP1-Source-Pool                       | SLE-Module-Public-Cloud15-SP1-Source-Pool               | No      | ----      | ----
25 | Public_Cloud_Module_15_SP1_x86_64:SLE-Module-Public-Cloud15-SP1-Updates                           | SLE-Module-Public-Cloud15-SP1-Updates                   | Yes     | (r ) Yes  | Yes
26 | Python_2_Module_15_SP1_x86_64:SLE-Module-Python2-15-SP1-Debuginfo-Pool                            | SLE-Module-Python2-15-SP1-Debuginfo-Pool                | No      | ----      | ----
27 | Python_2_Module_15_SP1_x86_64:SLE-Module-Python2-15-SP1-Debuginfo-Updates                         | SLE-Module-Python2-15-SP1-Debuginfo-Updates             | No      | ----      | ----
28 | Python_2_Module_15_SP1_x86_64:SLE-Module-Python2-15-SP1-Pool                                      | SLE-Module-Python2-15-SP1-Pool                          | Yes     | (r ) Yes  | No
29 | Python_2_Module_15_SP1_x86_64:SLE-Module-Python2-15-SP1-Source-Pool                               | SLE-Module-Python2-15-SP1-Source-Pool                   | No      | ----      | ----
30 | Python_2_Module_15_SP1_x86_64:SLE-Module-Python2-15-SP1-Updates                                   | SLE-Module-Python2-15-SP1-Updates                       | Yes     | (r ) Yes  | Yes
31 | SUSE_Linux_Enterprise_Server_15_SP1_x86_64:SLE-Product-SLES15-SP1-Debuginfo-Pool                  | SLE-Product-SLES15-SP1-Debuginfo-Pool                   | No      | ----      | ----
32 | SUSE_Linux_Enterprise_Server_15_SP1_x86_64:SLE-Product-SLES15-SP1-Debuginfo-Updates               | SLE-Product-SLES15-SP1-Debuginfo-Updates                | No      | ----      | ----
33 | SUSE_Linux_Enterprise_Server_15_SP1_x86_64:SLE-Product-SLES15-SP1-Pool                            | SLE-Product-SLES15-SP1-Pool                             | Yes     | (r ) Yes  | No
34 | SUSE_Linux_Enterprise_Server_15_SP1_x86_64:SLE-Product-SLES15-SP1-Source-Pool                     | SLE-Product-SLES15-SP1-Source-Pool                      | No      | ----      | ----
35 | SUSE_Linux_Enterprise_Server_15_SP1_x86_64:SLE-Product-SLES15-SP1-Updates                         | SLE-Product-SLES15-SP1-Updates                          | Yes     | (r ) Yes  | Yes
36 | Server_Applications_Module_15_SP1_x86_64:SLE-Module-Server-Applications15-SP1-Debuginfo-Pool      | SLE-Module-Server-Applications15-SP1-Debuginfo-Pool     | No      | ----      | ----
37 | Server_Applications_Module_15_SP1_x86_64:SLE-Module-Server-Applications15-SP1-Debuginfo-Updates   | SLE-Module-Server-Applications15-SP1-Debuginfo-Updates  | No      | ----      | ----
38 | Server_Applications_Module_15_SP1_x86_64:SLE-Module-Server-Applications15-SP1-Pool                | SLE-Module-Server-Applications15-SP1-Pool               | Yes     | (r ) Yes  | No
39 | Server_Applications_Module_15_SP1_x86_64:SLE-Module-Server-Applications15-SP1-Source-Pool         | SLE-Module-Server-Applications15-SP1-Source-Pool        | No      | ----      | ----
40 | Server_Applications_Module_15_SP1_x86_64:SLE-Module-Server-Applications15-SP1-Updates             | SLE-Module-Server-Applications15-SP1-Updates            | Yes     | (r ) Yes  | Yes
41 | Web_and_Scripting_Module_15_SP1_x86_64:SLE-Module-Web-Scripting15-SP1-Debuginfo-Pool              | SLE-Module-Web-Scripting15-SP1-Debuginfo-Pool           | No      | ----      | ----
42 | Web_and_Scripting_Module_15_SP1_x86_64:SLE-Module-Web-Scripting15-SP1-Debuginfo-Updates           | SLE-Module-Web-Scripting15-SP1-Debuginfo-Updates        | No      | ----      | ----
43 | Web_and_Scripting_Module_15_SP1_x86_64:SLE-Module-Web-Scripting15-SP1-Pool                        | SLE-Module-Web-Scripting15-SP1-Pool                     | Yes     | (r ) Yes  | No
44 | Web_and_Scripting_Module_15_SP1_x86_64:SLE-Module-Web-Scripting15-SP1-Source-Pool                 | SLE-Module-Web-Scripting15-SP1-Source-Pool              | No      | ----      | ----
45 | Web_and_Scripting_Module_15_SP1_x86_64:SLE-Module-Web-Scripting15-SP1-Updates                     | SLE-Module-Web-Scripting15-SP1-Updates                  | Yes     | (r ) Yes  | Yes
ec2-user@ip-172-31-18-31:~> 

```

The registered extensions can also be checked:

:::code{showCopyAction=true} 
sudo SUSEConnect --list-extensions
:::

::alert[Remember, if you are using a personal account and are unable to complete the labs for any reason, please clean up/terminate any resources from the AWS console by following the instructions in [Lab 05. Cleanup](../../lab05-cleanup)]

**End of Lab**