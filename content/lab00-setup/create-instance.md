---
title: "Task 01 a). Set up SUSE Linux Instance"
weight: 20
---

<!--
Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
SPDX-License-Identifier: MIT-0
-->

In this lab we will walk through the process of provisioning a Pay-as-you-go (PAYG) SUSE Linux Enterprise Server through the AWS EC2 console. We will then ssh into our instance for the first time, and check to make sure that the appropriate repositories and packages are installed and enabled.

### Activity 1: Deploy and Configure a SUSE Linux instance in AWS (PAYG)

#### Step 1: Launch the Instance

From the [AWS EC2 dashboard](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Home:) click the button that says **Launch instance**. 

Name the instance `SUSE Linux Enterprise Server`.

For the OS Image, we will choose an Amazon Machine Image (AMI) for a SUSE Linux Enterprise Server from the Community Marketplace.
- Search for `suse-sles-12-sp5` AMIs
- From the results, look under "Community AMIs". The image number can vary, so select the most recent AMI that matches the following pattern: `suse-sles-12-sp5-vXXXXXXXX-hvm-ssd-x86_64` option.

Instance type specifications: `t2.micro`

#### Step 2: Security Key
Use the Security Key created for these labs, or use an existing one that have already created.  You will need to SSH into the server once it has launched.
- Select of an existing key pair that you wish to use. If you do not already have a key pair, you will need to create one through the AWS console. 
    - If you need to create and download your own key pair, you can follow this guide: [How to Create a Key Pair in the AWS Console](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/create-key-pairs.html#having-ec2-create-your-key-pair)

#### Step 3: Network Settings
You can choose to use the default VPC, but make sure that you enable "Auto-assign public IP".

The security group attached to the SUSE Linux instance should allow the following inbound traffic:
- inbound SSH 22
- restrict the traffic to your IP address by selecting the option "My IP"

#### Step 4: Storage

Under the storage section, click advanced in the top right. Then give it the following capabilities and leave everything else as is:
- 20GiB Disk for root ("/") Volume 
- Throughput of 125

#### Step 5: Launch Instance

After reviewing that all settings are in order, launch the instance by clicking **Launch instance**.

**Proceed to Task 2**