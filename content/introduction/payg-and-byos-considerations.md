---
title: "PAYG and BYOS Considerations"
weight: 60
---

<!--
Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
SPDX-License-Identifier: MIT-0
-->

Pay-as-you-go (PAYG) and Bring Your Own Subscription (BYOS) are the two methods you can use to deploy SUSE offerings on AWS. 

With BYOS instances you only pay AWS for the instance itself, and purchase the subscription (whether SUSE Linux Enterprise Server or another offering) from SUSE themselves. Next, you connect to your deployed instance and manually register it with the [SUSE Customer Center (SCC)](https://scc.suse.com/) using the subscription to gain access to the necessary modules. 

With PAYG, you do not need to purchase a subscription from SUSE or manually connect to the SCC. PAYG instances automatically enable all the modules for your offering, without requiring you to manually purchase and register your subscription for the individual instances. Instead, all repositories are configured and any updates are immediately available. This makes PAYG instances great for production workloads that are hosted on AWS, and lets customers only pay the time that they are actively using the servers.

## Support

Level 1 - Equivalent to the first person you talk to when calling customer service. This
person has a general knowledge and may be able to answer your questions. If not, they
will direct you to Level 2 support.

Level 2 - Someone with deeper knowledge on the subject. This may be an engineer who
knows about the product and its implementation and can answer specific questions.

Level 3 - The question is escalated to the vendor of the product itself. In this instance,
the customer would be directed to speak with SUSE.

| Usage Model | Level 1 & 2 | Level 3 |
| :---: | :---: | :---: |
| PAYG | AWS | SUSE |
| BYOS | SUSE | SUSE |

## Commercial

| Usage Model | Infrastructure | SLES Subscription |
| :---: | :---: | :---: |
| PAYG | AWS | AWS |
| BYOS | AWS | SUSE |

## Entitlements

| Usage Model | Product | Lifecycle Management Client | Live Kernel Patching |
| :---: | :---: | :---: | :---: |
| PAYG | SLES for SAP Applications | ✅ | ✅ |
| BYOS | SLES for SAP Applications | Requires Additional Subscription | Requires Additional Subscription |

## Deployment
<!-- One of the primary differences between using BYOS and PAYG instances was the sources from which you deploy them. PAYG is offered in the AWS Marketplace, while BYOS used to only be offered through community AMIs. This provided a level of comfort in knowing that instances launched from the AWS Marketplace have already gone through testing and meet the security and access policies defined by AWS. BYOS images launched from community AMIs did not have that same level of security. However, this has now changed, as all SUSE has moved all of their AWS BYOS images to the Marketplace. This means that you can now find both BYOS and PAYG subscriptions in the Marketplace, with BYOS AMIs being tagged as such. -->
<!-- https://www.suse.com/c/suse-byos-images-and-the-aws-marketplace-2/ -->
It is important to choose the instance subscription model that best fits your needs. For most customers and use cases, PAYG is the right choice. It does not require a separate process to get your subscription nor require the user to register the instance using that subscription code after deployment, and consequently works much better with automation and deploying multiple instances. Customers who need to use BYOS, are typically migrating their on-premises architecture to AWS, and have existing subscriptions to SLES. Once the existing subscriptions expire, the best solution is to use PAYG instances which they can shift to using [AWS MGN (Application Migration Service)](https://aws.amazon.com/application-migration-service/). Instead of needing to manually reinstall and redeploy all of their servers, you can use MGN for a fully automated process!


## Summary
When you use BYOS, the instance is created on AWS infrastructure, while the SUSE functionality is enabled through a subscription that you purchase from SUSE directly and then use to register your instance. This means that all levels of support are through SUSE. With PAYG both the infrastructure and subscription are purchased through AWS, meaning that the first two levels of support are done through AWS, with only the third level of support going through SUSE.

When choosing how best to deploy your servers, PAYG requires less hands-on operations and no external subscriptions, making it the best choice for most customers.
