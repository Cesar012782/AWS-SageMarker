# Using the role manager<a name="role-manager-tutorial"></a>

You can use the Amazon SageMaker Role Manager from the following locations on the left\-hand navigation of the Amazon SageMaker console:
+ **Getting started** – Quickly add permissions policies for your users\.
+ **Domains** – Add permissions policies for users within a Amazon SageMaker Domain\.
+ **Notebooks** – Add least permissions for users who create and run notebooks\.
+ **Training** – Add least permissions for users who create and manage training jobs\.
+ **Inference** – Add least permissions for users who deploy and manage models for inference\.

You can use the following are procedures to start the process of creating a role from different locations in the SageMaker console\.

## Getting started<a name="role-manager-tutorial-getting-started"></a>

If you're using SageMaker for the first time, we recommend creating a role from the **Getting started** section\.

To create a role using Amazon SageMaker Role Manager, do the following\.

1. Open the Amazon SageMaker console\.

1. On the left\-hand navigation, choose **Getting started**\.

1. Choose **Create a role**\.

## Domains<a name="role-manager-tutorial-domain"></a>

You can create a role using Amazon SageMaker Role Manager when you start the process of creating a Amazon SageMaker Domain\.

To create a role using Amazon SageMaker Role Manager, do the following\.

1. Open the Amazon SageMaker console\.

1. On the left\-hand navigation, choose **Domains**\.

1. Choose **Create domain**\.

1. Choose **Create role using the role creation wizard**\.

## Notebook<a name="role-manager-tutorial-notebook"></a>

You can create a role using Amazon SageMaker Role Manager when you start the process of creating a notebook\.

To create a role using Amazon SageMaker Role Manager, do the following\.

1. Open the Amazon SageMaker console\.

1. On the left\-hand navigation, select **Notebook**\.

1. Choose **Notebook instances**\.

1. Choose **Create notebook instance**\.

1. Choose **Create role using the role creation wizard**\.

## Training<a name="role-manager-tutorial-training"></a>

You can create a role using Amazon SageMaker Role Manager when you start the process of creating a training job\.

To create a role using Amazon SageMaker Role Manager, do the following\.

1. Open the Amazon SageMaker console\.

1. On the left\-hand navigation, choose **Training**\.

1. Select **Training jobs**\.

1. Choose **Create training job**\.

1. Choose **Create role using the role creation wizard**\.

## Inference<a name="role-manager-tutorial-inference"></a>

You can create a role using Amazon SageMaker Role Manager when you start the process of deploying a model for inference\.

To create a role using Amazon SageMaker Role Manager, do the following\.

1. Open the Amazon SageMaker console\.

1. On the left\-hand navigation, choose **Inference**\.

1. Select **Models**\.

1. Choose **Create model**\.

1. Choose **Create role using the role creation wizard**\.

After you've completed one of the preceding procedures, use the information in the following sections to help you create the role\.

## Prerequisites<a name="role-manager-tutorial-prerequisites"></a>

To use Amazon SageMaker Role Manager, you must have permission to create an IAM role\. This permission is usually available to ML administrators and roles with least\-privilege permissions for ML practitioners\. 

You can temporarily assume an IAM role in the AWS Management Console by [switching roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-console.html)\. For more information about methods for using roles, see [Using IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use.html) in the *IAM User Guide*\.

## Step 1\. Enter role information<a name="role-manager-tutorial-enter-role-information"></a>

Provide a name to use as the unique suffix of your new SageMaker role\. By default, the prefix `"sagemaker-"` is added to every role name for easier search in the IAM console\. For example, if you name your role `test-123` during role creation, your role shows up as `sagemaker-test-123` in the IAM console\. You can optionally add a description of your role to provide additional details\. 

Then, choose from one of the available personas to get suggested permissions for personas such as data scientists, data engineers, or machine learning operations \(MLOps\) engineers\. For information on available personas and their suggested permissions, see [Persona reference](role-manager-personas.md)\. To create a role without any suggested permissions to guide you, choose **Custom Role Settings**\.

**Note**  
We recommend that you first use the role manager to create a SageMaker Compute Role so that SageMaker compute resources have the ability to perform tasks such as training and inference\. Use the SageMaker Compute Role persona to create this role with the role manager\. After creating a SageMaker Compute Role, take note of its ARN for future use\.

### Network and encryption conditions<a name="role-manager-tutorial-enter-role-information-network-and-encryption"></a>

We recommend that you activate VPC customization to use VPC configurations, subnets, and security groups with IAM policies associated with your new role\. When VPC customization is activated, IAM policies for ML activities that interact with VPC resources are scoped down for least\-privilege access\. VPC customization is not activated by default\. For more details on recommended networking architecture, see [Networking architecture](https://docs.aws.amazon.com/whitepapers/latest/build-secure-enterprise-ml-platform/networking-architecture.html) in the *AWS Technical Guide*\.

You can also use a KMS key to encrypt, decrypt, and re\-encrypt data for regulated workloads with highly sensitive data\. When AWS KMS customization is activated, IAM policies for ML activities that support custom encryption keys are scoped down for least\-privilege access\. For more information, see [Encryption with AWS KMS](https://docs.aws.amazon.com/whitepapers/latest/build-secure-enterprise-ml-platform/encryption-with-kms.html) in the *AWS Technical Guide*\.

## Step 2\. Configure ML activities<a name="role-manager-tutorial-configure-ml-activities"></a>

Each Amazon SageMaker Role Manager ML activity includes suggested IAM permissions to provide access to relevant AWS resources\. Some ML activities require that you add service role ARNs to complete setup\. For information on predefined ML activities and their permissions, see [ML activity reference](role-manager-ml-activities.md)\. For information on adding service roles, see [Service roles](#role-manager-tutorial-configure-ml-activities-service-roles)\.

Based on the chosen persona, certain ML activities are already selected\. You can deselect any suggested ML activities or select additional activities to create your own role\. If you selected the Custom Role Settings persona, then no ML activities are preselected in this step\. 

You can add any additional AWS or customer\-managed IAM policies to your role in [Step 3: Add additional policies and tags](#role-manager-tutorial-add-policies-and-tags)\.

### Service roles<a name="role-manager-tutorial-configure-ml-activities-service-roles"></a>

Some AWS services require a service role to perform actions on your behalf\. If the ML activity that you selected requires you to pass a service role, then you must provide the ARN for that service role\. 

You can either create a new service role or use an existing one, such as a service role created with the SageMaker Compute Role persona\. You can find the ARN of an existing role by selecting the role name in the Roles section of the [IAM console](https://console.aws.amazon.com/iamv2/)\. To learn more about service roles, see [Creating a role for an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html)\.

## Step 3: Add additional policies and tags<a name="role-manager-tutorial-add-policies-and-tags"></a>

You can add any existing AWS or customer\-managed IAM policies to your new role\. For information on existing SageMaker policies, see [AWS Managed Policies for Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/security-iam-awsmanpol.html)\. You can also check your existing policies in the **Roles** section of the [IAM console](https://console.aws.amazon.com/iamv2/)\. 

Optionally, use tag\-based policy conditions to assign metadata information to categorize and manage AWS resources\. Each tag is represented by a key\-value pair\. For more information, see [Controlling access to AWS resources using tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html)\.

## Review role<a name="role-manager-tutorial-review-role"></a>

Take the time to review all of the information associated with your new role\. Choose **Previous** to go back and edit any of the information\. When you are ready to create your role, choose **Create role**\. This generates a role with permissions for your selected ML activities\. You can view your new role in the **Roles** section of the [IAM console](https://console.aws.amazon.com/iamv2/)\. 