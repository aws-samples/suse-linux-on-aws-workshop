---
title: "Reading 02. Troubleshooting"
weight: 30
---

<!--
Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
SPDX-License-Identifier: MIT-0
-->

## Common Issues

If you run into issues, here are some of the most common errors that users face and how to address them

### Monitoring

If you want to monitor the progress, you can perform the following to get a play by play:

:::code{showCopyAction=true} 
    ssh -i ~/.ssh/example_keypair.pem migration@IP_of_instance

    tail -f /system-root/var/log/distro_migration.log
:::
### Repositories not defined

If there is no upgrade path available for defined repos:

:::code{showCopyAction=false} 
i.e., Advanced Systems Manager (obsoleted in SLE15)
i.e., HPC module (paid for in 15)
:::

To address this, you need to uninstall the packages associated with any module that is not available in the upgrade you are targeting.

### Filesystems mounted

For best results, only have ‘/’ root and system filesystems mounted, and comment out NFS and virtual filesystems (i.e., /proc).

### Use Debug mode

:::code{showCopyAction=false} 
/etc/sle-migration-service.yml
debug: true|false
:::

## Rare Issue

For issues not addressed by the above, here is a less common scenario.

### Cloud-Init failing in initialize Adapter after update

Systems boots (no network access)

If this is a Xen based instance type then the customer may fall victim to an issue that is related to the interaction between the ENA driver and Xen.

If the customer moves to a Nitro based instance type the issue is expected to go away. Bottom line is the ENA driver fails to initialize and as such there is no network connection. All network connections in AWS are ENA based.

The only option customers have at this point is, do not update/upgrade or switch to a nitro-based instance type.
