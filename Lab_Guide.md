# Automated Deployment of Detective Controls


## Table of Contents

1. [Deployment](#deployment)
2. [Knowledge Check](#knowledge_check)

## 1. AWS CloudFormation to configure AWS CloudTrail, AWS Config, and Amazon GuardDuty <a name="deployment"></a>

[AWS CloudTrail](https://aws.amazon.com/cloudtrail/) is a service that enables governance, compliance, operational auditing, and risk auditing of your AWS account. With CloudTrail, you can log, continuously monitor, and retain account activity related to actions across your AWS infrastructure. CloudTrail provides event history of your AWS account activity, including actions taken through the AWS Management Console, AWS SDKs, command line tools, and other AWS services.

[Amazon GuardDuty](https://aws.amazon.com/guardduty/) is a threat detection service that continuously monitors for malicious or unauthorized behavior to help you protect your AWS accounts and workloads. It monitors for activity such as unusual API calls or potentially unauthorized deployments that indicate a possible account compromise. GuardDuty also detects potentially compromised instances or reconnaissance by attackers.

Using [AWS CloudFormation](https://aws.amazon.com/cloudformation/), we are going to configure GuardDuty, and configure alerting to your email address.

[AWS Config](https://aws.amazon.com/config/) is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources. AWS Config continuously monitors and records your AWS resource configurations and allows you to automate the evaluation of recorded configurations against desired configurations.

1. Launch the CloudFormation Stack [Here](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks/new?stackName=Detective-Controls&templateURL=https://securityroadshowbucket.s3-ap-southeast-2.amazonaws.com/cloudtrail-config-guardduty.yaml) to setup the  environment

2. Enter the following details for each section:
  **General**
  * Stack name: The name of this stack. For this lab, use `DetectiveControls`.
  * AWS CloudTrail: Enable CloudTrail Yes/No. If you already have CloudTrail enabled select No.
  * AWS Config: Enable Config Yes/No. If you already have Config enabled select No.
  * Amazon GuardDuty: Enable GuardDuty Yes/No. If you already have GuardDuty enabled select No. Note that GuardDuty will create and leave an AWS IAM role the first time its enabled.
  * Amazon S3BucketPolicyExplicitDeny: (Optional) Explicitly deny destructive actions to the bucket. AWS root user will be required to modify this bucket if configured.
  * Amazon S3AccessLogsBucketName: (Optional) The name of an existing S3 bucket for storing S3 access logs.
  
  **AWS CloudTrail**
  * CloudTrailBucketName: The name of the new S3 bucket to create for CloudTrail to send logs to.  **IMPORTANT** Specify a bucket name that is unique.
  * CloudTrailCWLogsRetentionTime: Number of days to retain logs in CloudWatch Logs.
  * CloudTrailS3RetentionTime: Number of days to retain logs in the S3 bucket before they are automatically deleted.
  * CloudTrailEncryptS3Logs: (Optional) Use AWS KMS to encrypt logs stored in Amazon S3. A new KMS key will be created.
  * CloudTrailLogS3DataEvents: (Optional) These events provide insight into the resource operations performed on or within S3.
  
  **AWS Config**
  * ConfigBucketName: The name of the new S3 bucket to create for Config to save config snapshots to.  **IMPORTANT** Specify a bucket name that is unique.
  * ConfigSnapshotFrequency: AWS Config configuration snapshot frequency
  * ConfigS3RetentionTime: Number of days to retain logs in the S3 bucket before they are automatically deleted.
  
  **Amazon GuardDuty**
  * GuardDutyEmailAddress: The email address you own that will receive the alerts, you must have access to this address for testing.

  * Once you have finished entering the details for the template continue to the bottom of the page and click **Next**.
  * In this lab, we won't add any tags or other options. Click Next. Tags, which are key-value pairs, can help you identify your stacks. For more information, see [Adding Tags to Your AWS CloudFormation Stack](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide//cfn-console-add-tags.html).
  * Review the information for the stack. When you're satisfied with the configuration, check **I acknowledge that AWS CloudFormation might create IAM resources with custom names** then click **Create stack**.

![cloudformation-createstack-final](Images/cloudformation-createstack-final.png)

  * After a few minutes the stack status should change from *CREATE_IN_PROGRESS* to *CREATE_COMPLETE*.
You have now set up detective controls to log to your buckets and retain events, giving you the ability to search history and later enable pro-active monitoring of your AWS account!
  * You should receive an email to confirm the SNS email subscription, you must confirm this. Note as the email is directly from GuardDuty via SNS is will be JSON format.

## 2. Knowledge Check <a name="knowledge_check"></a>

The security best practices followed in this lab are: <a name="best_practices"></a>

* [Automate alerting on key indicators](https://wa.aws.amazon.com/wat.question.SEC_4.en.html) AWS Cloudtrail, AWS Config and Amazon GuardDuty provide insights into your environment.
* [Implement new security services and features:](https://wa.aws.amazon.com/wat.question.SEC_5.en.html) New features such as Amazon GuardDuty have been adopted.
* [Automate configuration management:](https://wa.aws.amazon.com/wat.question.SEC_6.en.html) CloudFormation is being used to configure AWS CloudTrail, AWS Config and Amazon GuardDuty.
* [Implement managed services:](https://wa.aws.amazon.com/wat.question.SEC_7.en.html) Managed services are utilized to increase your visibility and control of your environment.

***

## References & useful resources

[AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
[AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)
[Amazon GuardDuty User Guide](https://docs.aws.amazon.com/guardduty/latest/ug/what-is-guardduty.html)
[AWS Config User Guide](https://docs.aws.amazon.com/config/latest/)

***

## License

Licensed under the Apache 2.0 and MITnoAttr License.

Copyright 2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

Licensed under the Apache License, Version 2.0 (the "License"). You may not use this file except in compliance with the License. A copy of the License is located at

    https://aws.amazon.com/apache2.0/

or in the "license" file accompanying this file. This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
