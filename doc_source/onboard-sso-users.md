# Onboard to Amazon SageMaker Domain Using IAM Identity Center<a name="onboard-sso-users"></a>

This topic describes how to onboard to Amazon SageMaker Domain using authentication using IAM Identity Center from the SageMaker console or the AWS CLI\. For information about setting up IAM Identity Center for use with a Domain, see [Set Up IAM Identity Center for use with Amazon SageMaker Domain](onboard-sso-setup.md)\. For information on how to onboard using AWS Identity and Access Management \(IAM\) authentication, see [Onboard Using Quick setup](onboard-quick-start.md) or [Onboard Using IAM](onboard-iam.md)\.

## Onboard from the console<a name="onboard-sso-users-console"></a>

**To onboard to Domain using IAM Identity Center**

1. Open the [SageMaker console](https://console.aws.amazon.com/sagemaker/)\.

1. Choose **Domains** on the left of the page\.

1. From the **Domains** page, choose **Create domain**\.

1. On the **Setup SageMaker Domain** page, choose **Standard setup**\.

1. Select **Configure**\.

**Step 1: General settings**

1. For **Domain Name**, enter a unique name for your Domain\.

1. For **Authentication**, choose **AWS IAM Identity Center \(successor to AWS Single Sign\-On\)**\.

1. If you don't have a group in IAM Identity Center in the same Region as your SageMaker Domain, you must create a group in IAM Identity Center in the same Region as your SageMaker Domain before proceeding\. To continue to onboard without IAM Identity Center, choose the **AWS Identity and Access Management \(IAM\)** authentication method or the **Quick setup** procedure, which also uses IAM\.

   For information about setting up IAM Identity Center for use with Domain, see [Set Up IAM Identity Center for use with Amazon SageMaker Domain](onboard-sso-setup.md)\.

1. Under **Permission**, for **Default execution role**, choose an option from the role selector\.

   If you choose **Enter a custom IAM role ARN**, the role must have at a minimum, an attached trust policy that grants SageMaker permission to assume the role\. For more information, see [SageMaker Roles](sagemaker-roles.md)\.

   If you choose **Create a new role**, the **Create an IAM role** dialog opens:

   1. For **S3 buckets you specify**, specify additional Amazon S3 buckets that users of your notebooks can access\. If you don't want to add access to more buckets, choose **None**\.

   1. Choose **Create role**\. SageMaker creates a new IAM `AmazonSageMaker-ExecutionPolicy` role with the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy attached\.

1. For **Space default execution role**, choose an option from the role selector\.

   If you choose **Enter a custom IAM role ARN**, the role must have at a minimum, an attached trust policy that grants SageMaker permission to assume the role\. For more information, see [SageMaker Roles](sagemaker-roles.md)\.

   If you choose **Create a new role**, the **Create an IAM role** dialog opens:

   1. For **S3 buckets you specify**, specify additional Amazon S3 buckets that users of your notebooks can access\. If you don't want to add access to more buckets, choose **None**\.

   1. Choose **Create role**\. SageMaker creates a new IAM `AmazonSageMaker-ExecutionPolicy` role with the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy attached\.

1. Under **Network and storage**, specify the following:
   + Your Amazon Virtual Private Cloud \(Amazon VPC\) information – For more information, see [Choose an Amazon VPC](onboard-vpc.md)\.
   + \(Optional\) **Encryption key** – SageMaker uses an AWS KMS key to encrypt your Amazon Elastic File System \(Amazon EFS\) and Amazon Elastic Block Store \(Amazon EBS\) file systems\. By default, it uses an AWS managed key\. To use a customer managed key, enter its key ID or Amazon Resource Name \(ARN\)\. For more information, see [Protect Data at Rest Using Encryption](encryption-at-rest.md)\.
**Note**  
Encryption in transit is only available for Amazon SageMaker Studio\.

1. Select **Next**\. 

**Step 2: Studio settings**

1. Under **Default JupyterLab version**, select a JupyterLab version from the dropdown to use as the default for your Domain\. For information on selecting a JupyterLab version, see [JupyterLab Versioning](studio-jl.md)\.

1. Under **Notebook Sharing Configuration**, accept the default notebook sharing configuration or customize the options\.

1. Under **SageMaker Projects and JumpStart**, accept the default Project and JumpStart settings, or customize whether administrators and users can create projects and use Jumpstart\. For more information, see [SageMaker Studio Permissions Required to Use Projects](sagemaker-projects-studio-updates.md)\.

1. Select **Next**\. 

**Step 3: RStudio settings**

1. Under **RStudio Workbench**, verify that your RStudio license is automatically detected\. For more information about getting an RStudio license and activating it with SageMaker, see [RStudio license](rstudio-license.md)\.

1. Select an instance type to launch your RStudio Server on\. For more information, see [RStudioServerPro instance type](rstudio-select-instance.md)\.

1. Under **Permission**, create your role or select an existing role\. The role must have the following permissions policy\. This policy allows the RStudioServerPro app to access necessary resources and allows Amazon SageMaker to automatically launch an RStudioServerPro app when the existing RStudioServerPro app is in a `Deleted` or `Failed` status\. For information about adding permissions to a role, see [Modifying a role permissions policy \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-managingrole-editing-console.html#roles-modify_permissions-policy)\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "VisualEditor0",
               "Effect": "Allow",
               "Action": [
                   "license-manager:ExtendLicenseConsumption",
                   "license-manager:ListReceivedLicenses",
                   "license-manager:GetLicense",
                   "license-manager:CheckoutLicense",
                   "license-manager:CheckInLicense",
                   "logs:CreateLogDelivery",
                   "logs:CreateLogGroup",
                   "logs:CreateLogStream",
                   "logs:DeleteLogDelivery",
                   "logs:Describe*",
                   "logs:GetLogDelivery",
                   "logs:GetLogEvents",
                   "logs:ListLogDeliveries",
                   "logs:PutLogEvents",
                   "logs:PutResourcePolicy",
                   "logs:UpdateLogDelivery",
                   "sagemaker:CreateApp"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1. Under **RStudio Connect**, add the URL for your RStudio Connect server\. RStudio Connect is a publishing platform for Shiny applications, R Markdown reports, dashboards, plots, and more\. When you onboard to RStudio on SageMaker, an RStudio Connect server is not created\. For more information, see [RStudio Connect URL](rstudio-configure-connect.md)\.

1. Under **RStudio Package Manager**, add the URL for your RStudio Package Manager\. SageMaker creates a default package repository for the Package Manager when you onboard RStudio\. For more information about RStudio Package Manager, see [RStudio Package Manager](rstudio-configure-pm.md)\. 

1. Select **Next**\. 

**Step 4: SageMaker Canvas settings**

1. For the **Canvas base permissions configuration**, leave the **Enable Canvas base permissions** option turned on \(it is turned on by default\)\. This establishes the minimum required permissions to use the SageMaker Canvas app\.

1. \(Optional\) For the **Time series forecasting configuration**, leave the **Enable time series forecasting** option turned on to give your users permissions to do time series forecasting in SageMaker Canvas \(it is turned on by default\)\.

1. \(Optional\) If you left **Enable time series forecasting** turned on, select **Create and use a new execution role**\. However, if you already have an IAM role with the required Amazon Forecast permissions attached, select **Use an existing execution role**\. For more information, see the [IAM role setup method](canvas-set-up-forecast.md#canvas-set-up-forecast-iam)\.

1. Use the default IAM role suffix or provide a custom suffix for the role\.

1. For **Local file upload configuration**, select **Enable local file upload** to enable users to upload local files into their SageMaker Canvas application \(it's already checked by default\)\.

1. Choose **Submit**\.

## Onboard from the AWS CLI<a name="onboard-sso-users-cli"></a>

Use the following commands to onboard to a Domain using authentication using IAM Identity Center from the AWS CLI\.

1. Create an execution role that is used to create a Domain and attach the [AmazonSageMakerFullAccess](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AmazonSageMakerFullAccess) policy\. You can also use an existing role that has, at a minimum, an attached trust policy that grants SageMaker permission to assume the role\. For more information, see [SageMaker Roles](sagemaker-roles.md)\.

   ```
   aws iam create-role --role-name execution-role-name
   aws iam attach-role-policy --role-name execution-role-name --policy-arn arn:aws:iam::aws:policy/AmazonSageMakerFullAccess
   ```

1. Get the default Amazon Virtual Private Cloud \(Amazon VPC\) of your account\.

   ```
   aws --region region ec2 describe-vpcs --filters Name=isDefault,Values=true --query "Vpcs[0].VpcId" --output text
   ```

1. Get the list of subnets in the default Amazon VPC\.

   ```
   aws --region region ec2 describe-subnets --filters Name=vpc-id,Values=default-vpc-id --query "Subnets[*].SubnetId" --output json
   ```

1. Create a Domain by passing the default Amazon VPC ID, subnets, and execution role ARN\. You must also pass a SageMaker image ARN\. For information on the available JupyterLab version ARNs, see [Setting a default JupyterLab version](studio-jl.md#studio-jl-set)\.

   ```
   aws --region region sagemaker create-domain --domain-name domain-name --vpc-id default-vpc-id --subnet-ids subnet-ids --auth-mode SSO --default-user-settings "ExecutionRole=arn:aws:iam::account-number:role/execution-role-name,JupyterServerAppSettings={DefaultResourceSpec={InstanceType=system,SageMakerImageArn=image-arn}}" \ --query DomainArn --output text
   ```

1. Verify that the Domain has been created\.

   ```
   aws --region region sagemaker  list-domains
   ```

**To access the Domain after onboarding**

After you are given access to the Domain, you are sent an email inviting you to create a password and use IAM Identity Center\. The email also contains the URL to sign in to the Domain\. For more information about signing in and session duration, see [How to sign in to the user portal](https://docs.aws.amazon.com/singlesignon/latest/userguide/howtosignin.html)\.

After you activate your account, go to the Domain URL, sign in, and wait for your user profile to be created\. On subsequent visits, you only need to wait for the Studio or RStudio app to load\.

Bookmark the URL\. The URL is also available on the **Domain settings** page\.

For information about using Studio, see [SageMaker Studio](studio.md)\.

For information about using RStudio, see [RStudio on Amazon SageMaker](rstudio.md)\.