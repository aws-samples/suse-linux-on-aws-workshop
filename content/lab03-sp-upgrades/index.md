---
title: "Lab 03. Performing Service Pack Upgrades"
weight: 600
---

<!--
Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
SPDX-License-Identifier: MIT-0
-->

## Introduction to the SLES (for SAP) Lifecycle for Service Packs

Both SLES and SLES for SAP Lifecycle use Service Packs which are released approximately 12 months apart.

From the time SUSE releases new Service Packs and there is a delay before rollout of those service packs on AWS. This allows SAP and AWS to perform validation ensuring the new Service Packs are compatible with their respective arrays of instances and products. [Extended Service Pack Overlap Support (ESPOS)](https://www.suse.com/support/policy/#extended) is part of SLES for SAP and makes it easier for customers to align upgrades with application needs, giving them 4.5 years of total support.

To migrate between Service Packs, instances can be moved from one Service Pack to another while online with the command `zypper migration` which we will use in the next task.
