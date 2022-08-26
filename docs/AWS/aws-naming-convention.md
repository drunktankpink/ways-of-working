# Naming Conventions

The purpose of this document is not for it to be adhered to by the letter, but to act as a guide which can be used to support the process of defining naming conventions as well as highlight the reason why you should set and follow them.


## Placeholders
The following placeholders can be used to define a consistent naming convention.
| Placeholder            | Description                                      | Value | Short Value |
| ------------           | ------------ | ------------ | ------------ |
| \<org_name>            | Organisation that owns the resources             | Capgemini <br> Dunder Mifflin Paper Company <br> Genco Pura Olive Oil Company | cg <br> dmpc <br> genco |
| \<aws_account>         | AWS Account containing the resources <br> As the name of an account must be a single word, I would recommend setting a value that combines \<org_name> & \<environment>            | Capgemini Production Account <br> Dunder Mifflin Paper Company Network Account <br> Genco Pura Olive Oil Company Development Account  | cg-prod <br> dmpc-network <br> genco-dev   |
| \<region>              | AWS Region used                                  | eu-west-1 <br> eu-west-2  | euw1 <br> euw2  |
| \<availability_zone>   | AWS Availability Zone                            | eu-west-2a <br> eu-west-2b <br> eu-west-2c  | euw2a <br> euw2b <br> euw2c |
| \<aws_resource>        | AWS Resource abbreviation                        | Virtual Private Cloud <br> Subnet <br> Route Table <br> Network ACL <br> Transfer Gateway <br> Security Group <br> EC2 Instance <br> Auto Scaling Group <br> ECS Cluster <br> ECS Task <br> EKS Cluster <br> S3 Bucket <br> KMS Key <br> KMS Policy <br> IAM Role <br> IAM Policy  | vpc <br> subnet <br> rtb <br> nacl <br> tgw <br> sg <br> ec2 <br> asg <br> ecs-cluster <br> ecs-task <br> eks-cluster <br> s3 <br> kms <br> kms-policy <br> iam-role <br> iam-policy |
| \<environment>         | The environment being used for the resources     | Development <br> Test <br> Staging <br> UAT <br> Production <br> Shared Services <br> Sandbox <br> | dev <br> test <br> staging <br> uat <br> prod <br> shared <br> sandbox |
| \<business_unit>       | The business unit of the provisioned resources   |   |   |
| \<team>                | The team owning the provisioned resources        |   |   |
| \<resource_identifier> | Free form used to describe the resource <br> This could be the name of an application, the function, the client or a combination of all. <br> For example: |   |   |
| \<client>              | If applicable, the client for provisioned resources |   |   |

By using placeholders like above, prefixes can be specified for account level/wide resources, such as VPCs, subnets etc. <br>
Prefixes can also be specified for application level resources, which are specific to particular applications/teams/clients, such as EC2 instances, ECS clusters or Lambda functions.

## Naming prefixes

| Type | Convention | Comment | Example |
| ---- | ---------- | ------- | ------- |
| Account Naming Prefix | \<org_name>-\<environment>-\<context> <br> \<aws_account>-<context> | `Context` should be used if necessary, for example for multiple VPCs in an AWS account, this can be used to describe each  | cp-prod  |
| Team Naming Prefix | \<team>-\<environment>-\<scope> | `Context` should be used if necessary, for example this could be used to describe a service or sub division within a team | cp-cis-pe |

## AWS Examples

Below are examples which can be used as inspiration. <br>
Again, these are suggestions that be adapted for individual use cases. For example, depending on your landing zone set up, if you only deploy into one region, you could choose to omit `<region>` from the naming convention.

### AWS Accounts

| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| AWS Account Name  | \<org_name>-\<environment>  |  | cg-prod <br> dmpc-network <br> genco-dev  |

### VPC Resources
| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| VPC | {{account_naming_prefix}}-<region>-<aws_resource>  |  |  |
| Subnets | {{account_naming_prefix}}-<availability_zone>-{{subnet_type}}-<aws_resource>   | `{{subnet_type}}` should describe the purpose of the subnet. For example, it could be one of: <ul><li>public</li><li>private</li><li>app</li><li>data</li></ul>  |  |
| Route Tables | {{account_naming_prefix}}-<region>-{{route_type}}-<aws_resource>   | `{{route_type}}` should describe the purpose. It could be one of: <ul><li>public</li><li>private</li></ul>  |  |
| Network ACL | {{account_naming_prefix}}-<region>-{{nacl_type}}-<aws_resource>  | `{{nacl_type}}` should describe the NACL. It could be one of <ul><li>public</li><li>private</li></ul> |  |
| Transit Gateway | {{account_naming_prefix}}-<region>-<aws_resource>   |  | dmpc-network-euw1-tgw  |
| Transit Gateway Attachment | {{account_naming_prefix}}-<aws_resource>  |  | dmpc-network-euw1-tga |
| NAT Gateway | {{account_naming_prefix}}-<region>-<aws_resource>  |  | dmpc-network-euw1-ngw  |
| Endpoint | {{account_naming_prefix}}-<region>-{{endpoint_type}}-<aws_resource>  | `{{endpoint_type}}` should describe the type of endpoint: <ul><li>ec2</li><li>ecr</li><li>ecs</li><li>s3</li><li>ssm</li></ul> |  |

### IAM Resources
| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| IAM User | Users: <ul><li>{{username}}</li></ul> <br> Service Accounts: <ul><li>{{account_naming_prefix}}-{{service_name}}-<aws_resource></li></ul> <br> Third Party Accounts: <ul><li>{{account_naming_prefix}}-{{client}}-{{identifier}}-<aws_resource></li></ul> | **Users:** <br> `{{username}}` can vary based upon how you authenticate users within AWS. <br> You could adopt the first part of email addresses, or import users from third party access management tools, such as Okta or Active Directory <br><br> **Service Accounts:** <br> `{{service_name}}` should describe the service, for example: <ui><li>terraform</li><li>n2ws-backup</li></ul> <br> **Third Party Accounts:** <br> `{{client}}` & `{{identifier}}` should be used to clearly define the user. For example: <ui><li></li></ul>  | User: <ui><li>mscott</li><li>tyrell.wellick</li></ul> <br> Service Account: <ui><li></li></ul> <br> Third Party Accounts:  |
| IAM Role | {{account_naming_prefix}}-{{role_purpose}}-<aws_resource> <br> {{team_naming_prefix}}-{{role_purpose}}-<aws_resource> | `{{role_purpose}}` should explain what the role is for: <ui><li>vpc-flow-logs</li><li>atlantis-deployment</li><li>devops-admin</li></ul> | User: <ui><li></li></ul> <br> |
| IAM Group | {{account_naming_prefix}}-{{group_purpose}} | `{{group_purpose}}` should describe the group: <ul><li>administrators</li><li>management</li><li>billing</li></ul> |  |
| IAM Policy |  |  |  |
| KMS |  |  |  |
| SSL Certificate |  |  |  |

### EC2 Resources
| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| Instances |  |  | User: <ui><li></li></ul> <br> |
| Security Groups |  |  |  |
| Auto Scaling Groups |  |  |  |
| Elastic Load Balancers |  |  |  |
| Launch Configuration |  |  |  |
| AMI |  |  |  |
| Key Pairs |  |  |  |

### AWS Serverless Resources
| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| Lambda |  |  | User: <ui><li></li></ul> <br> |
| Step Functions |  |  |  |

### Container Resources
| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| ECS Cluster |  |  | User: <ui><li></li></ul> <br> |
| ECS Task |  |  |  |
| ECR Repository |  |  |  |
| EKS Cluster |  |  |  |
| EKS Node |  |  |  |

### S3 Resources
| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| S3 Bucket |  |  | User: <ui><li></li></ul> <br> |
| S3 Bucket Policy |  |  |  |

### ElastiCache Resources
| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| ElastiCache |  |  |  |

### CloudWatch Resources
| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| CloudWatch Alarm |  |  |  |

### RDS Resources
| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| RDS Instance |  |  |  |
| Parameter Group |  |  |  |

### Secrets Resources
| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| Secrets Manager |  |  |  |
| Parameter Store |  |  |  |

<ul><li>item1</li><li>item2</li></ul>

#### Additional Resources
https://confluence.huit.harvard.edu/display/CLA/Cloud+Resource+Naming+Conventions
