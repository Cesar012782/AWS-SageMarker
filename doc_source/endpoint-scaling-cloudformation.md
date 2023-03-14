# Use AWS CloudFormation to update auto scaling policies<a name="endpoint-scaling-cloudformation"></a>

The following example shows how to activate Application Auto Scaling on an endpoint using AWS CloudFormation\.

```
  Endpoint:
    Type: "AWS::SageMaker::Endpoint"
    Properties:
      EndpointName: yourEndpointName
      EndpointConfigName: yourEndpointConfigName

  ScalingTarget:
    Type: "AWS::ApplicationAutoScaling::ScalableTarget"
    Properties:
      MaxCapacity: 10
      MinCapacity: 2
      ResourceId: endpoint/MyEndPoint/variant/MyVariant
      RoleARN: arn
      ScalableDimension: sagemaker:variant:DesiredInstanceCount
      ServiceNamespace: sagemaker

  ScalingPolicy:
    Type: "AWS::ApplicationAutoScaling::ScalingPolicy"
    Properties:
      PolicyName: myscalablepolicy
      PolicyType: TargetTrackingScaling
      ScalingTargetId:
        Ref: ScalingTarget
      TargetTrackingScalingPolicyConfiguration:
        TargetValue: 75.0
        ScaleInCooldown: 600
        ScaleOutCooldown: 30
        PredefinedMetricSpecification:
          PredefinedMetricType: SageMakerVariantInvocationsPerInstance
```

For more details, refer to AWS CloudFormation [AutoScalingPlans API reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-autoscalingplans-scalingplan.html)\.