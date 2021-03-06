# AWS::Greengrass::Group<a name="aws-resource-greengrass-group"></a>

The `AWS::Greengrass::Group` resource represents a group in AWS IoT Greengrass\. In the AWS IoT Greengrass API, groups are used to organize your group versions\.

Groups can reference multiple group versions\.

![\[A group hierarchy with associated group versions.\]](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/images/greengrass/gg-group.png)

All group versions must be associated with a group\. A group version references a device definition version, subscription definition version, and other version types that contain the components you want to deploy to a Greengrass core device\.

![\[A group version that references other version types.\]](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/images/greengrass/gg-groupversion.png)

To deploy a group version, the group version must reference a core definition version that contains one core\. Other version types are optionally included, depending on your business need\.

**Note**  
When you create a group, you can optionally include an initial group version\. To associate a group version later, create a [AWS::Greengrass::GroupVersion](aws-resource-greengrass-groupversion.md) resource and specify the ID of this group\.  
To change group components \(such as devices, subscriptions, or functions\), you must create new versions\. This is because versions are immutable\. For example, to add a function, you create a function definition version that contains the new function \(and all other functions that you want to deploy\)\. Then you create a group version that references the new function definition version \(and all other version types that you want to deploy\)\.

**Deploying a Group Version**

After you create the group version in your AWS CloudFormation template, you can deploy it using the [https://docs.aws.amazon.com/greengrass/latest/apireference/createdeployment-post.html](https://docs.aws.amazon.com/greengrass/latest/apireference/createdeployment-post.html) command in the AWS CLI or by choosing **Greengrass** in the AWS IoT console\. To deploy a group version, you must have a Greengrass service role associated with your AWS account\. For more information, see [ AWS CloudFormation Support for AWS IoT Greengrass](https://docs.aws.amazon.com/greengrass/latest/developerguide/cloudformation-support.html) in the *AWS IoT Greengrass Developer Guide*\.

## Syntax<a name="aws-resource-greengrass-group-syntax"></a>

To declare this entity in your AWS CloudFormation template, use the following syntax:

### JSON<a name="aws-resource-greengrass-group-syntax.json"></a>

```
{
  "Type" : "AWS::Greengrass::Group",
  "Properties" : {
    "[InitialVersion](#cfn-greengrass-group-initialversion)" : [*GroupVersion*](aws-properties-greengrass-group-groupversion.md),
    "[RoleArn](#cfn-greengrass-group-rolearn)" : String,
    "[Name](#cfn-greengrass-group-name)" : String
  }
}
```

### YAML<a name="aws-resource-greengrass-group-syntax.yaml"></a>

```
Type: "AWS::Greengrass::Group"
Properties:
  [InitialVersion](#cfn-greengrass-group-initialversion): 
    [*GroupVersion*](aws-properties-greengrass-group-groupversion.md)
  [RoleArn](#cfn-greengrass-group-rolearn): String
  [Name](#cfn-greengrass-group-name): String
```

## Properties<a name="aws-resource-greengrass-group-properties"></a>

`InitialVersion`  <a name="cfn-greengrass-group-initialversion"></a>
The group version to include when the group is created\. A group version references the Amazon Resource Name \(ARN\) of a core definition version, device definition version, subscription definition version, and other version types\.  
To associate a group version after the group is created, create an [AWS::Greengrass::GroupVersion](aws-resource-greengrass-groupversion.md) resource and specify the ID of this group\.
 *Required*: No  
 *Type*: [GroupVersion](aws-properties-greengrass-group-groupversion.md)  
 *Update requires*: [Replacement](using-cfn-updating-stacks-update-behaviors.md#update-replacement) 

`RoleArn`  <a name="cfn-greengrass-group-rolearn"></a>
The ARN of the IAM role attached to the group\. This role contains the permissions that Lambda functions and connectors use to interact with other AWS services\.  
 *Required*: No  
 *Type*: String  
 *Update requires*: [No interruption](using-cfn-updating-stacks-update-behaviors.md#update-no-interrupt) 

`Name`  <a name="cfn-greengrass-group-name"></a>
The name of the group\.  
 *Required*: Yes  
 *Type*: String  
 *Update requires*: [No interruption](using-cfn-updating-stacks-update-behaviors.md#update-no-interrupt) 

## Return Values<a name="aws-resource-greengrass-group-returnvalues"></a>

### Ref<a name="aws-resource-greengrass-group-ref"></a>

When you pass the logical ID of an `AWS::Greengrass::Group` resource to the intrinsic `Ref` function, the function returns the ID of the group, such as `1234a5b6-78cd-901e-2fgh-3i45j6k178l9`\. 

For more information about using the `Ref` function, see [Ref](intrinsic-function-reference-ref.md)\. 

### Fn::GetAtt<a name="aws-resource-greengrass-group-getatt"></a>

 `Fn::GetAtt` returns a value for a specified attribute of this type\. The following are the available attributes and sample return values\. 

`RoleAttachedAt`  
The time \(in milliseconds since the epoch\) when the group role was attached to the `Group`\. 

`LatestVersionArn`  
The ARN of the last `GroupVersion` that was added to the `Group`, such as `arn:aws:greengrass:us-east-1:123456789012:/greengrass/definition/groups/1234a5b6-78cd-901e-2fgh-3i45j6k178l9/versions/9876ac30-4bdb-4f9d-95af-b5fdb66be1a2`\. 

`Id`  
The ID of the `Group`, such as `1234a5b6-78cd-901e-2fgh-3i45j6k178l9`\. 

`Arn`  
The ARN of the `Group`, such as `arn:aws:greengrass:us-east-1:123456789012:/greengrass/definition/groups/1234a5b6-78cd-901e-2fgh-3i45j6k178l9`\. 

`RoleArn`  
The ARN of the IAM role that's attached to the `Group`, such as `arn:aws:iam::123456789012:role/role-name`\. 

`Name`  
The name of the `Group`, such as `MyGroup`\. 

For more information about using `Fn::GetAtt`, see [Fn::GetAtt](intrinsic-function-reference-getatt.md)\. 

## Examples<a name="aws-resource-greengrass-group-examples"></a>

### Create a Group<a name="aws-resource-greengrass-group-example1"></a>

The template defines a core, device, function, logger, subscription, and two resources, and then references them from the group version\.

The template includes parameters that let you specify the certificate ARNs for the core and device and the ARN of the source Lambda function \(which is an AWS Lambda resource\)\. It uses the `Ref` and `GetAtt` intrinsic functions to reference IDs, ARNs, and other attributes that are required to create Greengrass resources\.

**Note**  
After you create the group version in your AWS CloudFormation template, you can deploy it using the [https://docs.aws.amazon.com/greengrass/latest/apireference/createdeployment-post.html](https://docs.aws.amazon.com/greengrass/latest/apireference/createdeployment-post.html) command in the AWS CLI or by choosing **Greengrass** in the AWS IoT console\. To deploy a group version, you must have a Greengrass service role associated with your AWS account\. For more information, see [ AWS CloudFormation Support for AWS IoT Greengrass](https://docs.aws.amazon.com/greengrass/latest/developerguide/cloudformation-support.html) in the *AWS IoT Greengrass Developer Guide*\.

#### JSON<a name="aws-resource-greengrass-group-example1.json"></a>

```
{
    "Description": "AWS IoT Greengrass example template that creates a group version with a core, device, function, logger, subscription, and resources.",
    "Parameters": {
        "CoreCertificateArn": {
            "Type": "String"
        },
        "DeviceCertificateArn": {
            "Type": "String"
        },
        "LambdaVersionArn": {
            "Type": "String"
        }
    },
    "Resources": {
        "TestCore1": {
            "Type": "AWS::IoT::Thing",
            "Properties": {
                "ThingName": "TestCore1"
            }
        },
        "TestCoreDefinition": {
            "Type": "AWS::Greengrass::CoreDefinition",
            "Properties": {
                "Name": "DemoTestCoreDefinition"
            }
        },
        "TestCoreDefinitionVersion": {
            "Type": "AWS::Greengrass::CoreDefinitionVersion",
            "Properties": {
                "CoreDefinitionId": {
                    "Ref": "TestCoreDefinition"
                },
                "Cores": [
                    {
                        "Id": "TestCore1",
                        "CertificateArn": {
                            "Ref": "CoreCertificateArn"
                        },
                        "SyncShadow": "false",
                        "ThingArn": {
                            "Fn::Join": [
                                ":",
                                [
                                    "arn:aws:iot",
                                    {
                                        "Ref": "AWS::Region"
                                    },
                                    {
                                        "Ref": "AWS::AccountId"
                                    },
                                    "thing/TestCore1"
                                ]
                            ]
                        }
                    }
                ]
            }
        },
        "TestDevice1": {
            "Type": "AWS::IoT::Thing",
            "Properties": {
                "ThingName": "TestDevice1"
            }
        },
        "TestDeviceDefinition": {
            "Type": "AWS::Greengrass::DeviceDefinition",
            "Properties": {
                "Name": "DemoTestDeviceDefinition"
            }
        },
        "TestDeviceDefinitionVersion": {
            "Type": "AWS::Greengrass::DeviceDefinitionVersion",
            "Properties": {
                "DeviceDefinitionId": {
                    "Fn::GetAtt": [
                        "TestDeviceDefinition",
                        "Id"
                    ]
                },
                "Devices": [
                    {
                        "Id": "TestDevice1",
                        "CertificateArn": {
                            "Ref": "DeviceCertificateArn"
                        },
                        "SyncShadow": "true",
                        "ThingArn": {
                            "Fn::Join": [
                                ":",
                                [
                                    "arn:aws:iot",
                                    {
                                        "Ref": "AWS::Region"
                                    },
                                    {
                                        "Ref": "AWS::AccountId"
                                    },
                                    "thing/TestDevice1"
                                ]
                            ]
                        }
                    }
                ]
            }
        },
        "TestFunctionDefinition": {
            "Type": "AWS::Greengrass::FunctionDefinition",
            "Properties": {
                "Name": "DemoTestFunctionDefinition"
            }
        },
        "TestFunctionDefinitionVersion": {
            "Type": "AWS::Greengrass::FunctionDefinitionVersion",
            "Properties": {
                "FunctionDefinitionId": {
                    "Fn::GetAtt": [
                        "TestFunctionDefinition",
                        "Id"
                    ]
                },
                "DefaultConfig": {
                    "Execution": {
                        "IsolationMode": "GreengrassContainer"
                    }
                },
                "Functions": [
                    {
                        "Id": "TestLambda1",
                        "FunctionArn": {
                            "Ref": "LambdaVersionArn"
                        },
                        "FunctionConfiguration": {
                            "Pinned": "true",
                            "Executable": "run.exe",
                            "ExecArgs": "argument1",
                            "MemorySize": "512",
                            "Timeout": "2000",
                            "EncodingType": "binary",
                            "Environment": {
                                "Variables": {
                                    "variable1": "value1"
                                },
                                "ResourceAccessPolicies": [
                                    {
                                        "ResourceId": "ResourceId1",
                                        "Permission": "ro"
                                    },
                                    {
                                        "ResourceId": "ResourceId2",
                                        "Permission": "rw"
                                    }
                                ],
                                "AccessSysfs": "false",
                                "Execution": {
                                    "IsolationMode": "GreengrassContainer",
                                    "RunAs": {
                                        "Uid": "1",
                                        "Gid": "10"
                                    }
                                }
                            }
                        }
                    }
                ]
            }
        },
        "TestLoggerDefinition": {
            "Type": "AWS::Greengrass::LoggerDefinition",
            "Properties": {
                "Name": "DemoTestLoggerDefinition"
            }
        },
        "TestLoggerDefinitionVersion": {
            "Type": "AWS::Greengrass::LoggerDefinitionVersion",
            "Properties": {
                "LoggerDefinitionId": {
                    "Ref": "TestLoggerDefinition"
                },
                "Loggers": [
                    {
                        "Id": "TestLogger1",
                        "Type": "AWSCloudWatch",
                        "Component": "GreengrassSystem",
                        "Level": "INFO"
                    }
                ]
            }
        },
        "TestResourceDefinition": {
            "Type": "AWS::Greengrass::ResourceDefinition",
            "Properties": {
                "Name": "DemoTestResourceDefinition"
            }
        },
        "TestResourceDefinitionVersion": {
            "Type": "AWS::Greengrass::ResourceDefinitionVersion",
            "Properties": {
                "ResourceDefinitionId": {
                    "Ref": "TestResourceDefinition"
                },
                "Resources": [
                    {
                        "Id": "ResourceId1",
                        "Name": "LocalDeviceResource",
                        "ResourceDataContainer": {
                            "LocalDeviceResourceData": {
                                "SourcePath": "/dev/TestSourcePath1",
                                "GroupOwnerSetting": {
                                    "AutoAddGroupOwner": "false",
                                    "GroupOwner": "TestOwner"
                                }
                            }
                        }
                    },
                    {
                        "Id": "ResourceId2",
                        "Name": "LocalVolumeResourceData",
                        "ResourceDataContainer": {
                            "LocalVolumeResourceData": {
                                "SourcePath": "/dev/TestSourcePath2",
                                "DestinationPath": "/volumes/TestDestinationPath2",
                                "GroupOwnerSetting": {
                                    "AutoAddGroupOwner": "false",
                                    "GroupOwner": "TestOwner"
                                }
                            }
                        }
                    }
                ]
            }
        },
        "TestSubscriptionDefinition": {
            "Type": "AWS::Greengrass::SubscriptionDefinition",
            "Properties": {
                "Name": "DemoTestSubscriptionDefinition"
            }
        },
        "TestSubscriptionDefinitionVersion": {
            "Type": "AWS::Greengrass::SubscriptionDefinitionVersion",
            "Properties": {
                "SubscriptionDefinitionId": {
                    "Ref": "TestSubscriptionDefinition"
                },
                "Subscriptions": [
                    {
                        "Id": "TestSubscription1",
                        "Source": {
                            "Fn::Join": [
                                ":",
                                [
                                    "arn:aws:iot",
                                    {
                                        "Ref": "AWS::Region"
                                    },
                                    {
                                        "Ref": "AWS::AccountId"
                                    },
                                    "thing/TestDevice1"
                                ]
                            ]
                        },
                        "Subject": "TestSubjectUpdated",
                        "Target": {
                            "Ref": "LambdaVersionArn"
                        }
                    }
                ]
            }
        },
        "TestGroup": {
            "Type": "AWS::Greengrass::Group",
            "Properties": {
                "Name": "DemoTestGroupNewName",
                "RoleArn": {
                    "Fn::Join": [
                        ":",
                        [
                            "arn:aws:iam:",
                            {
                                "Ref": "AWS::AccountId"
                            },
                            "role/TestUser"
                        ]
                    ]
                },
                "InitialVersion": {
                    "CoreDefinitionVersionArn": {
                        "Ref": "TestCoreDefinitionVersion"
                    },
                    "DeviceDefinitionVersionArn": {
                        "Ref": "TestDeviceDefinitionVersion"
                    },
                    "FunctionDefinitionVersionArn": {
                        "Ref": "TestFunctionDefinitionVersion"
                    },
                    "SubscriptionDefinitionVersionArn": {
                        "Ref": "TestSubscriptionDefinitionVersion"
                    },
                    "LoggerDefinitionVersionArn": {
                        "Ref": "TestLoggerDefinitionVersion"
                    },
                    "ResourceDefinitionVersionArn": {
                        "Ref": "TestResourceDefinitionVersion"
                    }
                }
            }
        }
    }
}
```

#### YAML<a name="aws-resource-greengrass-group-example1.yaml"></a>

```
Description: >-
  AWS IoT Greengrass example template that creates a group version with a core,
  device, function, logger, subscription, and resources.
Parameters:
  CoreCertificateArn:
    Type: String
  DeviceCertificateArn:
    Type: String
  LambdaVersionArn:
    Type: String
Resources:
  TestCore1:
    Type: 'AWS::IoT::Thing'
    Properties:
      ThingName: TestCore1
  TestCoreDefinition:
    Type: 'AWS::Greengrass::CoreDefinition'
    Properties:
      Name: DemoTestCoreDefinition
  TestCoreDefinitionVersion:
    Type: 'AWS::Greengrass::CoreDefinitionVersion'
    Properties:
      CoreDefinitionId: !Ref TestCoreDefinition
      Cores:
        - Id: TestCore1
          CertificateArn: !Ref CoreCertificateArn
          SyncShadow: 'false'
          ThingArn: !Join 
            - ':'
            - - 'arn:aws:iot'
              - !Ref 'AWS::Region'
              - !Ref 'AWS::AccountId'
              - thing/TestCore1
  TestDevice1:
    Type: 'AWS::IoT::Thing'
    Properties:
      ThingName: TestDevice1
  TestDeviceDefinition:
    Type: 'AWS::Greengrass::DeviceDefinition'
    Properties:
      Name: DemoTestDeviceDefinition
  TestDeviceDefinitionVersion:
    Type: 'AWS::Greengrass::DeviceDefinitionVersion'
    Properties:
      DeviceDefinitionId: !GetAtt 
        - TestDeviceDefinition
        - Id
      Devices:
        - Id: TestDevice1
          CertificateArn: !Ref DeviceCertificateArn
          SyncShadow: 'true'
          ThingArn: !Join 
            - ':'
            - - 'arn:aws:iot'
              - !Ref 'AWS::Region'
              - !Ref 'AWS::AccountId'
              - thing/TestDevice1
  TestFunctionDefinition:
    Type: 'AWS::Greengrass::FunctionDefinition'
    Properties:
      Name: DemoTestFunctionDefinition
  TestFunctionDefinitionVersion:
    Type: 'AWS::Greengrass::FunctionDefinitionVersion'
    Properties:
      FunctionDefinitionId: !GetAtt 
        - TestFunctionDefinition
        - Id
      DefaultConfig:
        Execution:
          IsolationMode: GreengrassContainer
      Functions:
        - Id: TestLambda1
          FunctionArn: !Ref LambdaVersionArn
          FunctionConfiguration:
            Pinned: 'true'
            Executable: run.exe
            ExecArgs: argument1
            MemorySize: '512'
            Timeout: '2000'
            EncodingType: binary
            Environment:
              Variables:
                variable1: value1
              ResourceAccessPolicies:
                - ResourceId: ResourceId1
                  Permission: ro
                - ResourceId: ResourceId2
                  Permission: rw
              AccessSysfs: 'false'
              Execution:
                IsolationMode: GreengrassContainer
                RunAs:
                  Uid: '1'
                  Gid: '10'
  TestLoggerDefinition:
    Type: 'AWS::Greengrass::LoggerDefinition'
    Properties:
      Name: DemoTestLoggerDefinition
  TestLoggerDefinitionVersion:
    Type: 'AWS::Greengrass::LoggerDefinitionVersion'
    Properties:
      LoggerDefinitionId: !Ref TestLoggerDefinition
      Loggers:
        - Id: TestLogger1
          Type: AWSCloudWatch
          Component: GreengrassSystem
          Level: INFO
  TestResourceDefinition:
    Type: 'AWS::Greengrass::ResourceDefinition'
    Properties:
      Name: DemoTestResourceDefinition
  TestResourceDefinitionVersion:
    Type: 'AWS::Greengrass::ResourceDefinitionVersion'
    Properties:
      ResourceDefinitionId: !Ref TestResourceDefinition
      Resources:
        - Id: ResourceId1
          Name: LocalDeviceResource
          ResourceDataContainer:
            LocalDeviceResourceData:
              SourcePath: /dev/TestSourcePath1
              GroupOwnerSetting:
                AutoAddGroupOwner: 'false'
                GroupOwner: TestOwner
        - Id: ResourceId2
          Name: LocalVolumeResourceData
          ResourceDataContainer:
            LocalVolumeResourceData:
              SourcePath: /dev/TestSourcePath2
              DestinationPath: /volumes/TestDestinationPath2
              GroupOwnerSetting:
                AutoAddGroupOwner: 'false'
                GroupOwner: TestOwner
  TestSubscriptionDefinition:
    Type: 'AWS::Greengrass::SubscriptionDefinition'
    Properties:
      Name: DemoTestSubscriptionDefinition
  TestSubscriptionDefinitionVersion:
    Type: 'AWS::Greengrass::SubscriptionDefinitionVersion'
    Properties:
      SubscriptionDefinitionId: !Ref TestSubscriptionDefinition
      Subscriptions:
        - Id: TestSubscription1
          Source: !Join 
            - ':'
            - - 'arn:aws:iot'
              - !Ref 'AWS::Region'
              - !Ref 'AWS::AccountId'
              - thing/TestDevice1
          Subject: TestSubjectUpdated
          Target: !Ref LambdaVersionArn
  TestGroup:
    Type: 'AWS::Greengrass::Group'
    Properties:
      Name: DemoTestGroupNewName
      RoleArn: !Join 
        - ':'
        - - 'arn:aws:iam:'
          - !Ref 'AWS::AccountId'
          - role/TestUser
      InitialVersion:
        CoreDefinitionVersionArn: !Ref TestCoreDefinitionVersion
        DeviceDefinitionVersionArn: !Ref TestDeviceDefinitionVersion
        FunctionDefinitionVersionArn: !Ref TestFunctionDefinitionVersion
        SubscriptionDefinitionVersionArn: !Ref TestSubscriptionDefinitionVersion
        LoggerDefinitionVersionArn: !Ref TestLoggerDefinitionVersion
        ResourceDefinitionVersionArn: !Ref TestResourceDefinitionVersion
```

## See Also<a name="aws-resource-greengrass-group-seealso"></a>
+ [CreateGroup](https://docs.aws.amazon.com/greengrass/latest/apireference/creategroup-post.html) in the *AWS IoT Greengrass API Reference*
+ [AWS IoT Greengrass Developer Guide](https://docs.aws.amazon.com/greengrass/latest/developerguide/)