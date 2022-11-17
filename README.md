## Automate the sending of AWS Audit Manager Assessment Reports

# Overview

Reference solution to automated the sending of AWS Audit Manager assessment report suammry to stakeholde's email inbox

# Architect Diagaram

![ArchitectureDiagram](./images/AuditManagerBlog.jpeg)

The architecture workflow is described as follow:

1. An [assessment report](https://docs.aws.amazon.com/audit-manager/latest/userguide/assessment-reports.html) summarizes your assessment and providers links to the evidences. Compliance Specialist is the persona using with AWS Audit Manager to simplify capturing evidences for be compliant with a specific framework 
2. For generate the assessment report refer to this [documentation](https://docs.aws.amazon.com/audit-manager/latest/userguide/generate-assessment-report.html) and it will be uploaded in the Amazon S3 bucket of your choice
3. Using [Event Notification](https://docs.aws.amazon.com/AmazonS3/latest/userguide/NotificationHowTo.html) on that S3 bucket, an AWS Lambda function will be trigger
4. The AWS Lambda function will capture the assessment report uploaded, and sent using [Amazon SES](https://aws.amazon.com/ses/) to the recipients assigned
5. The email addresses used as parameters will receive the assessment report as an attachment through email


# Prerequistes

You must first complete the following pre-requisites to operationalize configuration for the reports:

* AWS Audit Manager needs to be deployed - [Getting started with AWS Audit Manager](https://docs.aws.amazon.com/audit-manager/latest/userguide/getting-started.html)
* Amazon S3 bucket should be set to receive the assessment report summary
* The solution providers 3 email address that can receive the assessment report, fill in at least Email Address 1 to receive the email containing the report
* In this blog, we are assuming that your Amazon SES are in [sandbox mode](https://docs.aws.amazon.com/ses/latest/dg/request-production-access.html). That’s why once the solution is deployed, you need to make sure to go to your email inbox and click on the validation link sent by SES


# Walkthrough

Go to [AWS Cloudformation console](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks?filteringStatus=active&filteringText=&viewNested=true&hideStacks=false), select **With new resources (standard)**, select **Upload a template file**,  upload the [auditmanager-ses-notification.yaml](https://github.com/aws-samples/automated-email-assessment-report/blob/main/auditmanager-ses-notification.yaml) file from our  GitHub repository and follow steps in the console to launch the template The template takes the following parameters: 

* **Source Email Address**: The sender email address that appears in the From address where the email will be sent
* **Register Source Email Addres**: Default is true. The source email address will be registered in Amazon SES that email address will be register in Amazon SES
* **Email Address 1**:  Email address of the 1st recipient
* **Register Email Address 1**: Choose true to register the email address of the 1st recipient in Amazon SES
* **Email Address 2**: Email address of the 2nd recipient 
* **Register Email Address 2**: Choose true to register the email address of the 2nd recipient in Amazon SES 
* **Email Address 3**: Email address of the 3rd recipient
* **Register Email Address 3**: Choose true to register the email address of the 3rd recipient in Amazon SES; 
* **Report S3 Bucket Name**: S3 bucket name to store the assessment


### Source Blog

Blog Post - https://aws.amazon.com/blogs/mt/automate-the-sending-of-aws-audit-manager-assessment-reports/


## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

