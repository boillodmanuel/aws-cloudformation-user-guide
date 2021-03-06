# AWS::ElasticBeanstalk::Application ApplicationResourceLifecycleConfig<a name="aws-properties-elasticbeanstalk-application-applicationresourcelifecycleconfig"></a>

The resource lifecycle configuration for an application\. Defines lifecycle settings for resources that belong to the application, and the service role that Elastic Beanstalk assumes in order to apply lifecycle settings\. The version lifecycle configuration defines lifecycle settings for application versions\.

 `ApplicationResourceLifecycleConfig` is a property of the [AWS::ElasticBeanstalk::Application](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-beanstalk.html) resource\.

## Syntax<a name="aws-properties-elasticbeanstalk-application-applicationresourcelifecycleconfig-syntax"></a>

To declare this entity in your AWS CloudFormation template, use the following syntax:

### JSON<a name="aws-properties-elasticbeanstalk-application-applicationresourcelifecycleconfig-syntax.json"></a>

```
{
  "[ServiceRole](#cfn-elasticbeanstalk-application-applicationresourcelifecycleconfig-servicerole)" : String,
  "[VersionLifecycleConfig](#cfn-elasticbeanstalk-application-applicationresourcelifecycleconfig-versionlifecycleconfig)" : [ApplicationVersionLifecycleConfig](aws-properties-elasticbeanstalk-application-applicationversionlifecycleconfig.md)
}
```

### YAML<a name="aws-properties-elasticbeanstalk-application-applicationresourcelifecycleconfig-syntax.yaml"></a>

```
﻿  [ServiceRole](#cfn-elasticbeanstalk-application-applicationresourcelifecycleconfig-servicerole) : String
﻿  [VersionLifecycleConfig](#cfn-elasticbeanstalk-application-applicationresourcelifecycleconfig-versionlifecycleconfig) : [ApplicationVersionLifecycleConfig](aws-properties-elasticbeanstalk-application-applicationversionlifecycleconfig.md)
```

## Properties<a name="aws-properties-elasticbeanstalk-application-applicationresourcelifecycleconfig-properties"></a>

`ServiceRole`  <a name="cfn-elasticbeanstalk-application-applicationresourcelifecycleconfig-servicerole"></a>
The ARN of an IAM service role that Elastic Beanstalk has permission to assume\.  
*Required*: No  
*Type*: String  
*Update requires*: [No interruption](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-update-behaviors.html#update-no-interrupt)

`VersionLifecycleConfig`  <a name="cfn-elasticbeanstalk-application-applicationresourcelifecycleconfig-versionlifecycleconfig"></a>
Defines lifecycle settings for application versions\.  
*Required*: No  
*Type*: [ApplicationVersionLifecycleConfig](aws-properties-elasticbeanstalk-application-applicationversionlifecycleconfig.md)  
*Update requires*: [No interruption](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-update-behaviors.html#update-no-interrupt)