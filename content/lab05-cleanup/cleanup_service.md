---
title: "Task 01 c). Workshop Cleanup from Service Catalog Setup"
weight: 40
---

<!--
Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
SPDX-License-Identifier: MIT-0
-->

### Activity 1: Cleanup

#### Step 1: Delete Lab Infrastructure CloudFormation Stack

If you provisioned your infrastructure through the Service Catalog option, cleanup should be clean and simple. Navigate to the [AWS Service Catalog Dashboard](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks?filteringText=&filteringStatus=active&viewNested=true) in the console, and then select "Provisioned products" from the left-hand menu. You will then select the products that you provisioned for this workshop, choose "Terminate" from the dropdown menu in the top right, and then confirm that you want to terminate the product when it asks you to. This will automatically roll back all resources that were created for the lab by the Service Catalog.

![Delete Product Success](../../static/cleanup/delete_product.jpg)

#### Step 2: Delete Key Pair
If you wish to delete the keypair that you used for the lab, navigate to the [Key pairs](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#KeyPairs:) listed on the EC2 homepage. From the list of key pairs, select the key pair that you used for the lab, go to the "Actions" dropdown in the top right, select "Delete" and confirm.

#### Step 3: Delete Patching Logs S3 Bucket

To delete the S3 bucket that you used to store patching logs, navigate to [S3](https://s3.console.aws.amazon.com/s3/home?region=us-east-1) in the AWS console. From the list of buckets, find the one you created for storing patching logs and select it. 

Before we can delete the bucket, we first need to empty it, so click the "Empty" button from the top menu and confirm by typing "permanently delete".

![Emtpy S3 Bucket](../../static/cleanup/empty_S3.jpg)

Once it has been successfully emptied, select the bucket again from the list of buckets and this time choose the "Delete" button from the top menu and confirm by typing the name of the bucket.

![Delete S3 Bucket](../../static/cleanup/delete_S3.jpg)

#### Step 4: Delete Patch Manager Patch Policy

Next, we will delete the patch policy that we set up in patch manager. To do so, we will navigate to the [CloudFormation Dashboard](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks?filteringText=&filteringStatus=active&viewNested=true), and then select "Stacks" from the left-hand menu. You will then select the stack that Patch Manager automatically created when you set up your patch policy. It should be named something similar to "StackSet-AWS-QuickSetup-PatchPolicy-...". Choose "Delete" from the menu in the top right, and then confirm that you want to delete the stack when it asks you to. This will automatically roll back all resources that were created for the lab, with the CloudFormation template.

![Policy Delete Success](../../static/cleanup/catalog_delete_success.jpg)

#### Step 5: Delete Patch Manager Patch Baseline

Navigate to the [Patch Manager](https://us-east-1.console.aws.amazon.com/systems-manager/patch-manager?region=us-east-1) feature of AWS Systems manager. Once there, select "Patch baselines" from the middle menu.

Once there, select the patch baseline(s) that you created. Then to remove the patch baseline, simply choose Delete and confirm when the system prompts you to. It will then confirm that it has been successfully deleted at the top of the screen!

::alert[If they are the default, you will need to select a new patch baseline and make the default first. To do this, you can search for the "AWS-SuseDefaultPatchBaseline", select it, and then make it the default patch baseline by clicking on "Set default patch baseline" in the Actions dropdown menu.]

![Baseline Delete Success](../../static/cleanup/baseline_delete_success.jpg)