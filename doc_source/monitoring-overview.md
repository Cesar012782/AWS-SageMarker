# Monitoring Amazon SageMaker<a name="monitoring-overview"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of Amazon SageMaker and your other AWS solutions\. AWS provides the following monitoring tools to watch Amazon SageMaker, report when something is wrong, and take automatic actions when appropriate:

+ *Amazon CloudWatch* monitors your AWS resources and the applications that you run on AWS in real time\. You can collect and track metrics, create customized dashboards, and set alarms that notify you or take actions when a specified metric reaches a threshold that you specify\. For example, you can have CloudWatch track CPU usage or other metrics of your Amazon EC2 instances and automatically launch new instances when needed\. For more information, see the [Amazon CloudWatch User Guide](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

+ *Amazon CloudWatch Logs* enables you to monitor, store, and access your log files from EC2 instances, AWS CloudTrail, and other sources\. CloudWatch Logs can monitor information in the log files and notify you when certain thresholds are met\. You can also archive your log data in highly durable storage\. For more information, see the [Amazon CloudWatch Logs User Guide](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/)\.

+ *AWS CloudTrail* captures API calls and related events made by or on behalf of your AWS account and delivers the log files to an Amazon S3 bucket that you specify\. You can identify which users and accounts called AWS, the source IP address from which the calls were made, and when the calls occurred\. For more information, see the [AWS CloudTrail User Guide](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.