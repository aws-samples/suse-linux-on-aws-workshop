---
title: "Task 02. Check Repository Connectivity"
weight: 60
---

<!--
Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
SPDX-License-Identifier: MIT-0
-->

### Activity 1: Check Repository Connectivity

As this lab uses a PAYG instance (Pay-as-you-go Subscription) type from SUSE, it should already have access to fixes and updates. In this activity we check to ensure these are available.

#### Step 1: Establish an SSH Session

Get the public IP address of the instance from the AWS Console or AWS CLI.
Connect to the instance via public IP address using the terminal of your choice (ssh, PuTTY etc.)

#### Step 2: Check the connected repositories
Using `zypper`, list the available repositories.

:::code{showCopyAction=true showLineNumbers=true}
sudo zypper lr
:::

The output should be similar to below.

```

ec2-user@ip-172-31-17-181:~> sudo zypper lr
Repository priorities are without effect. All enabled repositories share the same priority.

#  | Alias                                                            | Name                                  | Enabled | GPG Check | Refresh
---+------------------------------------------------------------------+---------------------------------------+---------+-----------+--------
 1 | Legacy_Module_x86_64:SLE-Module-Legacy12-Debuginfo-Pool          | SLE-Module-Legacy12-Debuginfo-Pool    | No      | ----      | ----   
 2 | Legacy_Module_x86_64:SLE-Module-Legacy12-Debuginfo-Updates       | SLE-Module-Legacy12-Debuginfo-Updates | No      | ----      | ----   
 3 | Legacy_Module_x86_64:SLE-Module-Legacy12-Pool                    | SLE-Module-Legacy12-Pool              | Yes     | ( p) Yes  | No     
 4 | Legacy_Module_x86_64:SLE-Module-Legacy12-Source-Pool             | SLE-Module-Legacy12-Source-Pool       | No      | ----      | ----   

...
...

```

If the following error is seen ...

`Warning: No repositories defined.`

Wait 60 seconds and try the zypper command again.

#### Step 3: Explore Update infrastructure client packages and configuration

How does the SUSE instance connect to the update infrastructure?

There are 3 packages installed and enabled by default on PAYG instances.

* cloud-regionsrv-client 
* cloud-regionsrv-client-plugin-ec2 
* regionServiceClientConfigEC2

Run the following commands to show the packages installed.

:::code{showCopyAction=true showLineNumbers=true}
sudo zypper search --details --installed-only
:::
**OR**
:::code{showCopyAction=true showLineNumbers=true}
sudo zypper se -s -i region
:::

::alert[The two commands above have the same effect, the second is simply the abbreviated version]
    
If there is the need to install / re-install these packages, then focus on the 'cloud-regionsrv-client' package.  The other two  are dependencies and should already be installed with the base package.

It is also worth looking at the '/etc/hosts' file.

:::code{showCopyAction=true showLineNumbers=true}
sudo cat /etc/hosts
:::

You will find these lines added at the bottom of the hosts file.  This IP address has been returned but the SUSE Region Servers and is used to connect to the region local repository for updates.

```
# Added by SMT registration do not remove, retain comment as well
54.197.240.216	smt-ec2.susecloud.net	smt-ec2

```

Finally, take a look at the log files used by the client software, it’s useful for troubleshooting.

:::code{showCopyAction=true showLineNumbers=true}
sudo less /var/log/cloudregister
:::

Following the output from this should show which Region Server the instance connected to, and which IP address was assigned.

```
2022-05-12 12:48:08,766 INFO:	Using region server: 54.223.148.145
2022-05-12 12:48:09,669 INFO:No current registration server set.
2022-05-12 12:48:10,041 INFO:Modified /etc/hosts, added: 54.225.105.144	smt-ec2.susecloud.net	smt-ec2
```

**End of Lab**