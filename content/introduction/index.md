---
title: "Introduction to SUSE Linux on AWS"
weight: 100
---

<!--
Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
SPDX-License-Identifier: MIT-0
-->

[SUSE Linux Enterprise Server for SAP](https://www.suse.com/products/sles-for-sap/) forms a critical part of any SAP landscape deployed on AWS.  Effectively maintaining this requires skill and knowledge around multiple topic areas, one of which is architecting OS lifecycle management solutions on AWS. The goal of this workshop is to educate and improve capability and understanding of how to maintain and patch SUSE environments on AWS. 
::alert[SAP Landscape is defined as an architecture of SAP servers. In an SAP environment, this is typically a three-system landscape consisting of a Development Server, a Production Server, and a Quality Assurance server.]{header="SAP Landscape"}

By the end of the session delegates will:
- Learn about the SUSE OS lifecycle in AWS
- Gain familiarity with [SUSE’s Public Cloud Update Infrastructure](./public_cloud_infra.md)
- Execute both a major OS upgrade and service pack upgrade on a SUSE Linux Enterprise Server (SLES) on AWS
- Set up patch management on a SLES instance with AWS Systems Manager (SSM)

::alert[The Public Cloud Update Infrastructure (PCUI) is a system designed to provide low latency updates to SUSE customers. It is comprised of region and update servers, and is maintained by SUSE on the AWS cloud.]{header="Public Cloud Update Infrastructure"}

Target Audience:
- Users who are seeking to better understand the relationship between AWS and SUSE, and how to work with SUSE products on AWS.
- Users who work with SUSE environments on AWS and wish to learn how to best maintain those environments.

Level of Experience Needed:
- Workshop users should know basic Linux commands, including how to connect to a Linux instance through SSH
- Workshop users should be able to launch an EC2 instance through the AWS Management Console

Estimated Duration:
- To complete the primary four labs, as well as setup and cleanup, it will take approximately four hours.
- If users wish to complete the optional BYOS and Marketplace labs as well, it will take an additional four hours.

Infrastructure Costs:
- Regardless of whether users are participating in this workshop with an individual account or as part of an event, the services in the four primary labs of this workshop should cost less than ten dollars in total.
- For users wishing to complete the optional BYOS and Marketplace labs, costs will be incurred according to the subscription costs, but should be less than five dollars if the labs are completed in their suggested timeframes.

