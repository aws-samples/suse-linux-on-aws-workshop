---
title: "Reading 01. Difference between OS and AMI version"
weight: 10
---

<!--
Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
SPDX-License-Identifier: MIT-0
-->

We will briefly cover the differences between the AMI version of your instance(s), and the OS that the instance(s) is actually running. The AMI is the product that you use, and will remain the same as it was when you purchased it, whereas the OS will change as you perform OS and SP upgrades. For example, after an upgrade to OS SLES 15 SP1 on the instance AMI will still show SLES12 SP5.

When looking for support in managing your instance(s), you will use the version listed in /etc/os-release. This version is what your system is currently running, so any questions or support needed will be specific what is listed there. For example, if you ran into trouble while trying to enable a specific extension (such as the Live Patching extension) on a SLES instance, you would look for help with the version listed in /etc/os-release. 

When creating Annual Agreements, Private Offers, or other similar deals, you will need to use the AMI when creating Annual Agreements, Private Offers, or other similar deals. For example, if you wanted to create an annual agreement with the owner of a specific SLES image, you would use the AMI.
