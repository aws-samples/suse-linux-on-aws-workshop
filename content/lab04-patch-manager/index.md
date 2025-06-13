---
title: "Lab 04. Patch Management"
weight: 700
---

<!--
Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
SPDX-License-Identifier: MIT-0
-->

An easy way to scan for and install patches for our SLES instances is through the [AWS Patch Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager.html). The patch manager is part of the AWS console, and allows you to set up patch baselines specific to your instances' operating systems. You can then set up Patch Manager to automatically scan for and install necessary patches against that baseline to ensure compliance of your instances. Additionally, you can set up multiple baselines to allow Patch Manager to scan at different frequencies for different severities of patches ranging from low to critical.