# Create a Human Review Workflow<a name="a2i-create-flow-definition"></a>

Use an Amazon Augmented AI \(Amazon A2I\) *human review workflow*, or *flow definition*, to specify the following:
+ For the Amazon Textract and Amazon Rekognition built\-in task types, the conditions under which your human loop is called
+ The workforce to which your tasks are sent
+ The set of instructions that your workforce receives, which is called a *worker task template*
+ The configuration of your worker tasks, including the number of workers that receive a task and time limits to complete tasks 
+ Where your output data is stored 

You can create a human review workflow in the SageMaker console or using the SageMaker [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateFlowDefinition.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateFlowDefinition.html) operation\. You can build a worker task template using the console for Amazon Textract and Amazon Rekognition task types while creating your flow definition\.

**Important**  
Human loop activation conditions, which initiate the human loop—for example, confidence thresholds—aren't available for Amazon A2I custom task types\. When using the console to create a flow definition for a custom task type, you can't specify activation conditions\. When using the Amazon A2I API to create a flow definition for a custom task type, you can't set the `HumanLoopActivationConditions` attribute of the `HumanLoopActivationConditionsConfig` parameter\. To control when human reviews are initiated, specify conditions under which `StartHumanLoop` is called in your custom application\. In this case, every `StartHumanLoop` invocation results in a human review\. For more information, see [Use Amazon Augmented AI with Custom Task Types](a2i-task-types-custom.md)\.

**Prerequisites**

To create a human review workflow definition, you must have completed the prerequisites described in [Prerequisites to Using Augmented AI](a2i-getting-started-prerequisites.md)\. 

If you use the API to create a flow definition for any task type, or if you use a custom task type when creating a flow definition in the console, first create a worker task template\. For more information, see [Create and Manage Worker Task Templates](a2i-instructions-overview.md)\.

If you want to preview your worker task template while creating a flow definition for a built\-in task type in the console, ensure that you grant the role that you use to create the flow definition permission to access the Amazon S3 bucket that contains your template artifacts using a policy like the one described in [Enable Worker Task Template Previews ](a2i-permissions-security.md#permissions-for-worker-task-templates-augmented-ai)\.



**Topics**
+ [Create a Human Review Workflow \(Console\)](#a2i-create-human-review-console)
+ [Create a Human Review Workflow \(API\)](#a2i-create-human-review-api)
+ [JSON Schema for Human Loop Activation Conditions in Amazon Augmented AI](a2i-human-fallback-conditions-json-schema.md)

## Create a Human Review Workflow \(Console\)<a name="a2i-create-human-review-console"></a>

Use this procedure to create a Amazon Augmented AI \(Amazon A2I\) human review workflow using the SageMaker console\. If you are new to Amazon A2I, we recommend that you create a private work team using people in your organization, and use this work team's ARN when creating your flow definition\. To learn how to set up a private workforce and create a work team, see [Create a Private Workforce \(Amazon SageMaker Console\)](sms-workforce-create-private-console.md)\. If you have already set up a private workforce, see [Create a Work Team Using the SageMaker Console](sms-workforce-management-private-console.md#create-workteam-sm-console) to learn how to add a work team to that workforce\.

If you are using Amazon A2I with one of the built\-in task types, you can create worker instructions using a default worker task template provided by Augmented AI while creating a human review workflow in the console\. To see samples of the default templates provided by Augmented AI, see the built\-in task types in [Use Cases and Examples Using Amazon A2I](a2i-task-types-general.md)\.

**To create flow definition \(console\)**

1. Open the SageMaker console at [https://console.aws.amazon.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. In the navigation pane, under the **Augmented AI** section, choose **Human review workflows** and then choose **Create human review workflow**\.

1. In **Overview**, do the following:

   1. For **Name**, enter a unique workflow name\. The name must be lowercase, unique within the AWS Region in your account, and can have up to 63 characters\. Valid characters include: a\-z, 0\-9, and \- \(hyphen\)\.

   1. For **S3 location for output**, enter the S3 bucket where you want to store the human review results\. The bucket must be located in the same AWS Region as the workflow\.

   1. For **IAM role**, choose the role that has the required permissions\. If you choose a built\-in task type and want to preview your worker template in the console, provide a role with the type of policy described in [Enable Worker Task Template Previews ](a2i-permissions-security.md#permissions-for-worker-task-templates-augmented-ai) attached\.

1. For **Task type**, choose the task type that you want the human worker to perform\. 

1. If you chose the Amazon Rekognition or Amazon Textract task type, specify the conditions that invoke human review\.
   + For Amazon Rekognition image moderation tasks, choose an inference confidence score threshold interval that initiates human review\. 
   + For Amazon Textract tasks, you can initiate a human review when specific form keys are missing or when form key detection confidence is low\. You can also initiate a human review if, after evaluating all of the form keys in the text, confidence is lower than your required threshold for any form key\. Two variables specify your confidence thresholds: **Identification confidence** and **Qualification confidence**\. To learn more about these variables, see [Use Amazon Augmented AI with Amazon Textract](a2i-textract-task-type.md)\.
   + For both task types, you can randomly send a percentage of data objects \(images or forms\) and their labels to humans for review\. 

1. Configure and specify your worker task template:

   1. If you are using the Amazon Rekognition or Amazon Textract task type:

      1. In the **Create template** section: 
        + To create instructions for your workers using the Amazon A2I default template for Amazon Rekognition and Amazon Textract task types, choose **Build from a default template**\.
          + If you choose **Build from a default template**, create your instructions under **Worker task design**: 
            + Provide a **Template name** that is unique in the AWS Region you are in\. 
            + In the **Instructions** section, provide detailed instructions on how to complete your task\. To help workers achieve greater accuracy, provide good and bad examples\. 
            + \(Optional\) In **Additional instructions**, provide your workers with additional information and instructions\. 

              For information on creating effective instructions, see [Creating Good Worker Instructions](a2i-creating-good-instructions-guide.md)\.
        + To select a custom template that you've created, choose it from the **Template** menu and provide a **Task description** to briefly describe the task for your workers\. To learn how to create a custom template, see [Create a Worker Task Template](a2i-worker-template-console.md#a2i-create-worker-template-console)\.

   1. If you are using the custom task type:

      1. In the **Worker task template** section, select your template from the list\. All of the templates that you have created in the SageMaker console appear in this list\. To learn how to create a template for custom task types, see [Create and Manage Worker Task Templates](a2i-instructions-overview.md)\.

1. \(Optional\) Preview your worker template: 

   For Amazon Rekognition and Amazon Textract task types, you have the option to choose **See a sample worker task** to preview your worker task UI\.

   If you are creating a flow definition for a custom task type, you can preview your worker task UI using the `RenderUiTemplate` operation\. For more information, see [Preview a Worker Task Template](a2i-custom-templates.md#a2i-preview-your-custom-template)\.

1. For **Workers**, choose a workforce type\.

1. Choose **Create**\.

### Next Steps<a name="a2i-next-step-createflowdefinition-console"></a>

After you've created a human review workflow, it appears in the console under **Human review workflows**\. To see your flow definition's Amazon Resource Name \(ARN\) and configuration details, choose the workflow by selecting its name\. 

If you are using a built\-in task type, you can use the flow definition ARN to start a human loop using that AWS service's API \(for example, the Amazon Textract API\)\. For custom task types, you can use the ARN to start a human loop using the Amazon Augmented AI Runtime API\. To learn more about both options, see [Create and Start a Human Loop](a2i-start-human-loop.md)\.

## Create a Human Review Workflow \(API\)<a name="a2i-create-human-review-api"></a>

To create a flow definition using the SageMaker API, you use the `CreateFlowDefinition` operation\. After you complete the [Prerequisites to Using Augmented AI](a2i-getting-started-prerequisites.md), use the following procedure to learn how to use this API operation\. 

For an overview of the `CreateFlowDefinition` operation, and details about each parameter, see [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateFlowDefinition.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateFlowDefinition.html)\. 

**To create a flow definition \(API\)**

1. For `FlowDeﬁnitionName`, enter a unique name\. The name must be unique within the AWS Region in your account, and can have up to 63 characters\. Valid characters include: a\-z, 0\-9, and \- \(hyphen\)\.

1. For `RoleArn`, enter the ARN of the role that you configured to grant access to your data sources\.

1. For `HumanLoopConfig`, enter information about the workers and what they should see\. For information about each parameter in `HumanLoopConfig`, see [HumanLoopConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateFlowDefinition.html#sagemaker-CreateFlowDefinition-request-HumanLoopActivationConfig)\.

1. \(Optional\) If you are using a built\-in task type, provide conditions that initiate a human loop in `HumanLoopActivationConﬁg`\. To learn how to create the input required for the `HumanLoopActivationConﬁg` parameter, see [JSON Schema for Human Loop Activation Conditions in Amazon Augmented AI](a2i-human-fallback-conditions-json-schema.md)\. If you do not specify conditions here, when you provide a flow definition to the AWS service associated with a built\-in task type \(for example, Amazon Textract or Amazon Rekognition\), that service sends every task to a human worker for review\. 

   If you are using a custom task type, `HumanLoopActivationConfig` is disabled\. To learn how to control when tasks are sent to human workers using a custom task type, see [Use Amazon Augmented AI with Custom Task Types](a2i-task-types-custom.md)\.

1. \(Optional\) If you are using a built\-in task type, specify the integration source \(for example, Amazon Rekognition or Amazon Textract\) in the [HumanLoopRequestSource](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HumanLoopRequestSource.html) parameter\.

1. For `OutputConfig`, indicate where in Amazon Simple Storage Service \(Amazon S3\) to store the output of the human loop\.

1. \(Optional\) Use `Tags` to enter key\-value pairs to help you categorize and organize a flow definition\. Each tag consists of a key and a value, both of which you define\.

------
#### [ Amazon Textract – Key\-value pair extraction ]

The following is an example of a request to create an Amazon Textract human review workflow \(flow definition\) using the AWS SDK for Python \(Boto3\)\. You must use `'AWS/Textract/AnalyzeDocument/Forms/V1'` to create a Amazon Textract human loop\. Only include `PublicWorkforceTaskPrice` if you are using the Mechanical Turk workforce\.

```
sagemaker_client = boto3.client('sagemaker', aws_region)

response = sagemaker_client.create_flow_definition(
    FlowDefinitionName='ExampleFlowDefinition',
    HumanLoopRequestSource={
         'AwsManagedHumanLoopRequestSource': 'AWS/Textract/AnalyzeDocument/Forms/V1'
    }, 
    HumanLoopActivationConfig={
        'HumanLoopActivationConditionsConfig': {
            'HumanLoopActivationConditions': '{...}'
        }
    },
    HumanLoopConfig={
        'WorkteamArn': 'arn:aws:sagemaker:aws_region:aws_account_number:workteam/private-crowd/workteam_name',
        'HumanTaskUiArn': 'arn:aws:sagemaker:aws_region:aws_account_number:human-task-ui/template_name',
        'TaskTitle': 'Example task title',
        'TaskDescription': 'Example task description.',
        'TaskCount': 123,
        'TaskAvailabilityLifetimeInSeconds': 123,
        'TaskTimeLimitInSeconds': 123,
        'TaskKeywords': [
            'Keyword1','Keyword2'
        ],
        'PublicWorkforceTaskPrice': {
            'AmountInUsd': {
                'Dollars': 123,
                'Cents': 123,
                'TenthFractionsOfACent': 123
            }
        }
    },
    OutputConfig={
        'S3OutputPath': 's3://bucket/path/',
        'KmsKeyId': '1234abcd-12ab-34cd-56ef-1234567890ab'
    },
    RoleArn='arn:aws:iam::aws_account_number:role/role_name',
    Tags=[
        {
            'Key': 'KeyName',
            'Value': 'ValueName'
        },
    ]
)
```

------
#### [ Amazon Rekognition – Image moderation ]

The following is an example of a request to create an Amazon Rekognition human review workflow \(flow definition\) using the AWS SDK for Python \(Boto3\)\. You must use `'AWS/Rekognition/DetectModerationLabels/Image/V3'` to create an Amazon Rekognition flow definition\. Only include `PublicWorkforceTaskPrice` if you are using the Mechanical Turk workforce\.

```
sagemaker_client = boto3.client('sagemaker', aws_region)

response = sagemaker_client.create_flow_definition(
    FlowDefinitionName='ExampleFlowDefinition',
    HumanLoopRequestSource={
         'AwsManagedHumanLoopRequestSource': 'AWS/Rekognition/DetectModerationLabels/Image/V3'
    }, 
    HumanLoopActivationConfig={
        'HumanLoopActivationConditionsConfig': {
            'HumanLoopActivationConditions': '{...}'
        }
    },
    HumanLoopConfig={
        'WorkteamArn': 'arn:aws:sagemaker:aws_region:aws_account_number:workteam/private-crowd/workteam_name',
        'HumanTaskUiArn': 'arn:aws:sagemaker:aws_region:aws_account_number:human-task-ui/template_name',
        'TaskTitle': 'Example task title',
        'TaskDescription': 'Example task description.',
        'TaskCount': 123,
        'TaskAvailabilityLifetimeInSeconds': 123,
        'TaskTimeLimitInSeconds': 123,
        'TaskKeywords': [
            'Keyword1','Keyword2'
        ],
        'PublicWorkforceTaskPrice': {
            'AmountInUsd': {
                'Dollars': 123,
                'Cents': 123,
                'TenthFractionsOfACent': 123
            }
        }
    },
    OutputConfig={
        'S3OutputPath': 's3://bucket/path/',
        'KmsKeyId': '1234abcd-12ab-34cd-56ef-1234567890ab'
    },
    RoleArn='arn:aws:iam::aws_account_number:role/role_name',
    Tags=[
        {
            'Key': 'KeyName',
            'Value': 'ValueName'
        },
    ]
)
```

------
#### [ Custom Workflow ]

The following is an example of a request to create a human review workflow \(flow definition\) for a custom integration\. To create this type of human review workflow, omit `HumanLoopRequestSource` from the flow definition request\. You only need to include `PublicWorkforceTaskPrice` if you are using the Mechanical Turk workforce\.

```
sagemaker_client = boto3.client('sagemaker', aws_region)

response = sagemaker_client.create_flow_definition(
    FlowDefinitionName='ExampleFlowDefinition',
    HumanLoopActivationConfig={
        'HumanLoopActivationConditionsConfig': {
            'HumanLoopActivationConditions': '{...}'
        }
    },
    HumanLoopConfig={
        'WorkteamArn': 'arn:aws:sagemaker:aws_region:aws_account_number:workteam/private-crowd/workteam_name',
        'HumanTaskUiArn': 'arn:aws:sagemaker:aws_region:aws_acount_number:human-task-ui/template_name',
        'TaskTitle': 'Example task title',
        'TaskDescription': 'Example task description.',
        'TaskCount': 123,
        'TaskAvailabilityLifetimeInSeconds': 123,
        'TaskTimeLimitInSeconds': 123,
        'TaskKeywords': [
            'Keyword1','Keyword2'
        ],
        'PublicWorkforceTaskPrice': {
            'AmountInUsd': {
                'Dollars': 123,
                'Cents': 123,
                'TenthFractionsOfACent': 123
            }
        }
    },
    OutputConfig={
        'S3OutputPath': 's3://bucket/path/',
        'KmsKeyId': '1234abcd-12ab-34cd-56ef-1234567890ab'
    },
    RoleArn='arn:aws:iam::account_number:role/role_name',
    Tags=[
        {
            'Key': 'KeyName',
            'Value': 'ValueName'
        },
    ]
)
```

------

### Next Steps<a name="a2i-next-step-createflowdefinition-api"></a>

The return value of a successful call of the `CreateFlowDefinition` API operation is a flow definition Amazon Resource Name \(ARN\)\.

If you are using a built\-in task type, you can use the flow definition ARN to start a human loop using that AWS service's API \(i\.e\. the Amazon Textract API\)\. For custom task types, you can use the ARN to start a human loop using the Amazon Augmented AI Runtime API\. To learn more about both of these options, see [Create and Start a Human Loop](a2i-start-human-loop.md)\.