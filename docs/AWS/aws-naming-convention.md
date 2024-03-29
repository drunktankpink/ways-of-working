# Naming Conventions

The purpose of this document is not for it to be adhered to by the letter, but to act as a guide which can be used to support the process of defining naming conventions as well as highlight the reason why you should set and follow them.


## Placeholders
The following placeholders can be used to define a consistent naming convention.

| Placeholder | Description | Example Value | Short Value |
| ----------- | ----------- | ------------- | ----------- |
| `<org_name>` | Organisation that owns the resources | Capgemini <br><br> Dunder Mifflin Paper Company <br><br> Genco Pura Olive Oil Company | cg <br><br> dmpc <br><br> genco |
| `<aws_account>` | AWS Account containing the resources <br> As the name of an account must be a single word, I would recommend setting a value that combines `<org_name>` & `<environment>` <br><br> However this may or may not be suitable depending on the hierarchy and set up of AWS Accounts | Capgemini Production Account <br><br> Dunder Mifflin Paper Company Network Account <br><br> Genco Pura Olive Oil Company Development Account | cg-prod <br><br> dmpc-network <br><br> genco-dev |
| `<region>` | AWS Region used | eu-west-1 <br> eu-west-2 <br><br> us-east-1 | euw1 <br> euw2 <br><br> ue1 |
| `<availability_zone>` | AWS Availability Zone | eu-west-2a <br> eu-west-2b <br> eu-west-2c <br><br> us-east-2a <br> us-east-2b <br> us-east-2c | euw2a <br> euw2b <br> euw2c <br><br> ue1a <br> ue1b <br> ue1c |
| `<aws_resource>` | AWS Resource abbreviation | VPC <br> Subnet <br> Route Table <br> Network ACL <br> Transfer Gateway <br> Security Group <br> EC2 Instance <br> Auto Scaling Group <br> ECS Cluster <br> ECS Task <br> EKS Cluster <br> S3 Bucket <br> KMS Key <br> KMS Policy <br> IAM Role <br> IAM Policy  | vpc <br> subnet <br> rtb <br> nacl <br> tgw <br> sg <br> ec2 <br> asg <br> ecs-cluster <br> ecs-task <br> eks-cluster <br> s3 <br> kms <br> kms-policy <br> iam-role <br> iam-policy |
| `<environment>` | The environment being used for the resources | Development <br> Test <br> Staging <br> UAT <br> Production <br> Shared Services <br> Sandbox <br> | dev <br> test <br> staging <br> uat <br> prod <br> shared <br> sandbox |
| `<business_unit>` | The business unit of the provisioned resources | Cloud Infrastructure Services  | cis  |
| `<team>` | The team owning the provisioned resources | Platform Engineering <br> Warehouse <br> Sales  | pe <br> wh <br> sales |
| `<resource_identifier>` | Free form used to describe the resource <br> This could be the name of an application, the function, the client or a combination of all. |   |   |
| `<client>` | If applicable, the client for provisioned resources |   |   |

By using placeholders like above, prefixes can be specified for account level/wide resources, such as VPCs, subnets etc. <br>
Prefixes can also be specified for application level resources, which are specific to particular applications/teams/clients, such as EC2 instances, ECS clusters or Lambda functions.

## Naming prefixes

| Type | Convention | Comment | Example |
| ---- | ---------- | ------- | ------- |
| Account Naming Prefix | `<org_name>`-`<environment>`-`<context>` <br> `<aws_account>`-`<context>` | Account naming prefixes can follow their own structure, or they could begin with the account name, if AWS accounts have a different convention. <br> `<context>` can be used if necessary, for example, if there are multiple VPCs within the same AWS account, this can be used to differentiate and describe each. | cp-prod <br><br> dmpc-network <br><br> genco-dev |
| Team Naming Prefix | `<business_unit>`-`<team>`-`<environment>`-`<context>` | `<context>` can be used if necessary, for example this could be used to describe a service or sub division within a team. <br><br> `<business_unit>` could be used in conjunction with `<team>`, for example `cis-pe-prod` <br><br> `<org_name>` can be appended as a prefix | cp-cis-pe-prod <br><br> dmpc-wh-staging <br><br> genco-sales-dev |

## AWS Examples

Below are examples which can be used for inspiration. <br>
Again, these are suggestions that be adapted for individual use cases. For example, you may chose to omit `<region>` if you only deploy into a single region.

### AWS Accounts

| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| AWS Account Name  | `<org_name>`-`<environment>`  |  | cg-prod <br> dmpc-network <br> genco-dev  |

### VPC Resources
| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| VPC | `{{account_naming_prefix}}-<region>-<aws_resource>`  |  | cg-prod-ue1-sharedservices-vpc <br> dmpc-network-euw1-vpc <br> genco-dev-vpc |
| Subnets | `{{account_naming_prefix}}-<region>-<availability_zone>-{{subnet_type}}-<aws_resource>`   | `{{subnet_type}}` should describe the purpose of the subnet. For example, it could be one of: <ul><li>public</li><li>private</li><li>app</li><li>data</li></ul> <br> `<availability_zone>` can also be used after `<aws_resource>`. The value of this could be a shortened version of the AWS availability zone, or just the letter for the availability zone. | cg-prod-ue1-sharedservices-private-subnet-a <br> dmpc-network-euw1b-public-subnet <br> genco-dev-data-subnet-euw2c |
| Route Tables | `{{account_naming_prefix}}-<region>-{{route_type}}-<aws_resource>` | `{{route_type}}` should describe the purpose. It could be one of: <ul><li>public</li><li>private</li></ul>  | cg-prod-ue1-sharedservices-private-rt <br> dmpc-network-public-rt <br> genco-dev-private-rt |
| Network ACL | `{{account_naming_prefix}}-<region>-{{nacl_type}}-<aws_resource>`  | `{{nacl_type}}` should describe the NACL. It could be one of <ul><li>public</li><li>private</li></ul> | cg-prod-ue1-sharedservices-private-nacl <br> dmpc-network-euw1-public-nacl <br> genco-dev-private-nacl |
| Transit Gateway | `{{account_naming_prefix}}-<region>-<aws_resource>`   |  | cg-prod-ue1-sharedservices-tgw <br> dmpc-network-euw1-tgw <br> genco-dev-tgw |
| Transit Gateway Attachment | `{{account_naming_prefix}}-<region>-<aws_resource>`  |  | cg-prod-ue1-sharedservices-tgwa <br> dmpc-network-euw1-tgw-att <br> genco-dev-tga |
| NAT Gateway | `{{account_naming_prefix}}-<region>-<aws_resource>`  |  | cg-prod-ue1-sharedservices-ngw <br> dmpc-network-euw1-ngw <br> genco-dev-ngw |
| Endpoint | `{{account_naming_prefix}}-<region>-{{endpoint_type}}-<aws_resource>`  | `{{endpoint_type}}` should describe the endpoint type: <ul><li>ec2</li><li>ecr</li><li>ecs</li><li>s3</li><li>ssm</li></ul> | cg-prod-ue1-sharedservices-s3-endpoint <br> dmpc-network-euw1-ec2-endpoint <br> genco-dev-ssm-endpoint |

### IAM Resources
| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| IAM User | **Users:** <ul><li>{{username}}</li></ul> <br> **Service Accounts:** <ul><li>{{account_naming_prefix}}-{{service_name}}-`<aws_resource>`</li></ul> <br> **Third Party Accounts:** <ul><li>{{account_naming_prefix}}-{{client}}-{{identifier}}-`<aws_resource>`</li></ul> | **Users:** <br> `{{username}}` can vary based upon how you authenticate users within AWS. <br> You could adopt the first part of email addresses, or import users from third party access management tools, such as Okta or Active Directory <br><br> **Service Accounts:** <br> `{{service_name}}` should describe the service, for example: <ui><li>terraform</li><li>n2ws-backup</li></ul> <br> **Third Party Accounts:** <br> `{{client}}` & `{{identifier}}` should be used to clearly define the user. For example: <ui><li>Audit</li><li>Support</li><li>S3 Upload</li></ul>  | **User:** <ui><li>mscott</li><li>tyrell.wellick</li></ul> <br> **Service Account:** <ui><li>cg-prod-terraform</li><li>dmpc-network-n2ws-backup</li><li>genco-dev-serviceaccount</li></ul> <br> **Third Party Accounts:** <ui><li>cg-prod-thirdparty-audit-iam-user</li><li>dmpc-network-thirdparty-support-iam-user</li><li>genco-dev-thirdparty-s3upload-iam-user</li></ul> |
| IAM Role | {{account_naming_prefix}}-{{role_purpose}}-`<aws_resource>` <br> {{team_naming_prefix}}-{{role_purpose}}-`<aws_resource>` | `{{role_purpose}}` should explain what the role is for: <ui><li>n2ws-backup</li><li>vpc-flow-logs</li><li>atlantis-deployment</li><li>devops-admin</li><li>ecs-task</li><li>dispatch-lambda</li></ul> <br> For `{{team_naming_prefix}}` the `<context>` can be used to describe a service/project/function | **Account naming:** <br> <ul><li>cg-prod-n2ws-backup-iam-role</li><li>dmpc-network-devops-admin-iam-role</li><li>genco-dev-atlantis-deployment-iam-role</li></ul> <br> **Team naming:** <br> <ul><li>cg-cis-pe-prod-gitlab-ecs-iam-role</li><li>dmpc-wh-staging-dispatch-lambda-iam-role</li><li>genco-dev-sales-monthlyreport-lambda-iam-role</li></ul> |
| IAM Group | {{account_naming_prefix}}-{{group_purpose}} | `{{group_purpose}}` should describe the group: <ul><li>administrators</li><li>management</li><li>billing</li></ul> | cg-prod-administrators <br> dmpc-network-managers <br> genco-dev-billing |
| IAM Policy | {{account_naming_prefix}}-{{policy_purpose}}-`<aws_resource>` <br> {{team_naming_prefix}}-{{policy_purpose}}-`<aws_resource>` | `{{policy_purpose}}` should explain the purpose of the policy: <br> <ul><li>n2ws-backup</li><li>terraform</li><li>S3 admin</li><li>force-mfa</li><li>billing-view</li></ul> <br> `<policy_purpose>` can also match the resource being used, or the naming of the role it will be attached to: <ul><li>devops-admin</li><li>dispatch-lambda</li></ul> | **Account naming:** <br> <ul><li>cg-prod-n2ws-backup-iam-policy</li><li>dmpc-network-devops-admin-iam-policy</li><li>genco-dev-atlantis-deployment-iam-policy</li></ul> <br> **Team naming:** <br> <ul><li>cg-cis-pe-prod-gitlab-ecs-iam-policy</li><li>dmpc-wh-staging-dispatch-lambda-iam-policy</li><li>genco-dev-sales-monthlyreport-lambda-iam-policy</li></ul> |
| KMS | {{account_naming_prefix}}-{{kms_type}}-`<aws_resource>` <br> {{team_naming_prefix}}-{{kms_type}}-`<aws_resource>` | `{{kms_type}}` should reference the AWS resource the KMS key is for: <br> <ul><li>S3</li><li>EBS</li> | cg-prod-ue1-sharedservices-s3-kms-key <br> cg-prod-ue1-sharedservices-s3-kms-policy <br><br> dmpc-network-euw1-ebs-kms-key <br> dmpc-network-euw1-ebs-kms-policy <br><br> genco-dev-ecr-kms-key <br> genco-dev-ecr-kms-policy |
| SSL Certificate | {{naming_prefix}}-`<resource_identifier>`-{{product}}-{{cert_type}}-`<aws_resource>` | `{{product}}` could describe where the certificate is used: <br> <ui><li>alb</li><li>elb</li><li>cloudfront</li></ul> <br> `{{cert_type}}` should describe the certificate: <br> <ui><li>domain</li><li>wildcard</li></ul> <br> `<resource_identifier>` should describe the function of the resource: <br><br> It is also possible match the name of the certificate to the domain name e.g.: <br> <ui><li>portal.capgemini.com</li><li>orders.dmpc.com</li><li>dashboard.genco.info</li></ul> | <br> cg-prod-gitlab-alb-wildcard-cert <br> dmpc-staging-orders-cloudfront-domain-sslcert <br> genco-dev-reports-alb-domain-sslcert |

### EC2 Resources
| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| Instances | {{naming_prefix}}-`<region>`-`<resource_identifier>`-`<aws_resource>` | `<resource_identifier>` should describe the function of the resource: <br> <ui><li>item1</li><li>item2</li><li>item3</li></ul> |  **Account naming:** <br> <ul><li>cg-prod-ue1-gitlab-ec2</li><li>dmpc-network-staging-euw2-ip-analysis-ec2</li><li>genco-dev-reports-scripts-ec2</li></ul> <br> **Team naming:** <br> <ul><li>cg-cis-pe-prod-ue1-gitlab-ec2</li><li>dmpc-wh-staging-euw2-erp-ec2</li><li>genco-dev-sales-reports-ec2</li></ul> |
| Security Groups | {{naming_prefix}}-`<region>`-`<resource_identifier>`-`<aws_resource>` | `<resource_identifier>` should describe the function of the resource: <br> <ui><li>item1</li><li>item2</li><li>item3</li></ul> | **Account naming:** <br> <ul><li>cg-prod-ue1-gitlab-sg</li><li>dmpc-network-staging-euw2-ip-analysis-sg</li><li>genco-dev-reports-scripts-sg</li></ul> <br> **Team naming:** <br> <ul><li>cg-cis-pe-prod-ue1-gitlab-sg</li><li>dmpc-wh-staging-euw2-erp-sg</li><li>genco-dev-sales-reports-sg</li></ul> |
| Auto Scaling Groups | {{naming_prefix}}-`<region>`-`<resource_identifier>`-`<aws_resource>` | `<resource_identifier>` should describe the function of the resource: <br> <ui><li>item1</li><li>item2</li><li>item3</li></ul> |  **Account naming:** <br> <ul><li>cg-prod-ue1-gitlab-asg</li><li>dmpc-network-staging-euw2-ip-analysis-asg</li><li>genco-dev-reports-scripts-asg</li></ul> <br> **Team naming:** <br> <ul><li>cg-cis-pe-prod-ue1-gitlab-asg</li><li>dmpc-wh-staging-euw2-erp-asg</li><li>genco-dev-sales-reports-asg</li></ul> |
| Elastic Load Balancers | {{naming_prefix}}-`<region>`-`<resource_identifier>`-`<aws_resource>` | `<resource_identifier>` should describe the function of the resource: <br> <ui><li>item1</li><li>item2</li><li>item3</li></ul> |  **Account naming:** <br> <ul><li>cg-prod-ue1-gitlab-alb</li><li>dmpc-network-staging-euw2-ip-analysis-nlb</li><li>genco-dev-reports-scripts-lb</li></ul> <br> **Team naming:** <br> <ul><li>cg-cis-pe-prod-ue1-gitlab-alb</li><li>dmpc-wh-staging-euw2-erp-alb</li><li>genco-dev-sales-reports-lb</li></ul> |
| Launch Configuration | {{naming_prefix}}-`<region>`-`<resource_identifier>`-`<aws_resource>` | `<resource_identifier>` should describe the function of the resource: <br> <ui><li>item1</li><li>item2</li><li>item3</li></ul> |  **Account naming:** <br> <ul><li>cg-prod-ue1-gitlab-lc</li><li>dmpc-network-staging-euw2-ip-analysis-lc</li><li>genco-dev-reports-scripts-lc</li></ul> <br> **Team naming:** <br> <ul><li>cg-cis-pe-prod-ue1-gitlab-lc</li><li>dmpc-wh-staging-euw2-erp-lc</li><li>genco-dev-sales-reports-lc</li></ul> |
| AMI | {{naming_prefix}}-`<region>`-`<resource_identifier>`-`<aws_resource>` | `<resource_identifier>` should describe the function of the resource: <br> <ui><li>item1</li><li>item2</li><li>item3</li></ul> |  **Account naming:** <br> <ul><li>cg-prod-ue1-base-ami</li><li>dmpc-network-staging-euw2-prometheus-ami</li><li>genco-dev-scripts-ami</li></ul> <br> **Team naming:** <br> <ul><li>cg-cis-pe-prod-ue1-gitlab-ami</li><li>dmpc-wh-staging-euw2-erp-ami</li><li>genco-dev-sales-stats-dashboard-ami</li></ul> |
| Key Pairs | {{account_naming_prefix}}-`<region>`-`<resource_identifier>`-`<aws_resource>` | `<resource_identifier>` should describe the function of the resource: <br> <ui><li>item1</li><li>item2</li><li>item3</li></ul> | <br> cg-prod-ue1-gitlab-key-pair <br> dmpc-network-staging-euw2-ec2-keypair <br> genco-dev-reports-dashboard-kp |

### AWS Serverless Resources
| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| Lambda | {{naming_prefix}}-`<region>`-`<resource_identifier>`-`<aws_resource>` | `<resource_identifier>` should describe the function of the resource | **Account naming:** <br> <ul><li>cg-prod-ue1-data-validation-lambda</li><li>dmpc-network-staging-euw2-ip-analysis-lambda</li><li>genco-dev-reports-publish-lambda</li></ul> <br> **Team naming:** <br> <ul><li>cg-cis-pe-prod-data-transformation-lambda</li><li>dmpc-wh-staging-euw2-orders-dispatch-lambda</li><li>genco-dev-sales-monthlyreport-publish-lambda</li></ul> |
| Step Functions | {{naming_prefix}}-`<region>`-`<resource_identifier>`-`<aws_resource>` |  | **Account naming:** <br> <ul><li>cg-prod-ue1-data-ingestion-step-function</li><li>dmpc-network-staging-euw2-ip-analysis-step-function</li><li>genco-dev-reports-step-function</li></ul> <br> **Team naming:** <br> <ul><li>cg-cis-pe-prod-data-ingestion-step-function</li><li>dmpc-wh-staging-euw2-orders-step-function</li><li>genco-dev-sales-monthlyreport-step-function</li></ul> |

### Container Resources
| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| ECS Cluster | {{naming_prefix}}-`<region>`-`<resource_identifier>`-`<aws_resource>` | `<resource_identifier>` should describe the function of the resource: <br> <ui><li>gitlab</li><li>dispatch</li><li>reports</li></ul> | **Account naming:** <br> <ul><li>cg-prod-ue1-gitlab-ecs-cluster</li><li>dmpc-network-staging-euw2-ecs-cluster</li><li>genco-dev-ecs-cluster</li></ul> <br> **Team naming:** <br> <ul><li>cg-cis-pe-prod-gitlab-ecs-cluster</li><li>dmpc-wh-staging-euw2-dispatch-ecs-cluster</li><li>genco-dev-sales-reports-ecs-cluster</li></ul> |
| ECS Task | {{naming_prefix}}-`<region>`-`<resource_identifier>`-`<aws_resource>` | `<resource_identifier>` should describe the function of the resource: <br> <ui><li>gitlab-runner</li><li>dispatch</li><li>monthlyreport</li></ul> | **Account naming:** <br> <ul><li>cg-prod-ue1-gitlab-runner-ecs-task</li><li>dmpc-network-staging-euw2-ecs-task</li><li>genco-dev-warehouse-ecs-task</li></ul> <br> **Team naming:** <br> <ul><li>cg-cis-pe-prod-gitlab-runner-ecs-task</li><li>dmpc-wh-staging-euw2-dispatch-ecs-task</li><li>genco-dev-sales-monthlyreport-ecs-task</li></ul> |
| ECR Repository | {{naming_prefix}}-`<region>`-`<resource_identifier>`-`<aws_resource>` | `<resource_identifier>` should describe the function of the resource: <br> <ui><li>item1</li><li>item2</li><li>item3</li></ul> | **Account naming:** <br> <ul><li>cg-prod-ue1-ecr</li><li>dmpc-network-staging-ecr</li><li>genco-dev-ecr</li></ul> <br> **Team naming:** <br> <ul><li>cg-cis-pe-prod-ecr</li><li>dmpc-wh-staging-dispatch-ecr</li><li>genco-dev-sales-ecr</li></ul> |
| EKS Cluster | {{naming_prefix}}-`<region>`-{{cluster_type}}-`<aws_resource>` | `{{cluster_type}}` should describe cluster: <br> <ul><li>main</li><li>replica</li></ul> | **Account naming:** <br> <ul><li>cg-prod-ue1-replica-eks-cluster</li><li>dmpc-network-staging-euw2-main-eks-cluster</li><li>genco-dev-main-eks-cluster</li></ul> <br> **Team naming:** <br> <ul><li>cg-cis-pe-prod-ue1-replica-eks-cluster</li><li>dmpc-wh-staging-euw2-main-eks-cluster</li><li>genco-dev-sales-main-eks-cluster</li></ul> |
| EKS Node | {{naming_prefix}}-`<region>`-{{cluster_type}}-{{node_type}}-`<aws_resource>` | `{{cluster_type}}` should describe cluster the node belongs to: <br> <ul><li>main</li><li>replica</li></ul> <br> `{{node_type}}` should describe the node purpose. This could reference a characteristic or a specific application: <br> <ul><li>highmem</li><li>highcpu</li><li>gitlab</li><li>prometheus</li> | **Account naming:** <br> <ul><li>cg-prod-ue1-replica-highmem-eks-node</li><li>dmpc-network-staging-euw2-main-highcpu-eks-node</li><li>genco-dev-main-prometheus-eks-node</li></ul> <br> **Team naming:** <br> <ul><li>cg-cis-pe-prod-ue1-replica-gitlab-eks-node</li><li>dmpc-wh-staging-euw2-main-highcpu-eks-node</li><li>genco-dev-sales-main-prometheus-eks-node</li></ul> |

### S3 Resources
| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| S3 Bucket | {{naming_prefix}}-`<region>`-{{bucket_purpose}}-`<aws_resource>` | `{{bucket_purpose}}` should describe the bucket function: <br> <ui><li>deployment</li><li>tfstate</li><li>logs</li><li>ingestion</li></ul> | cg-prod-ue1-logs-bucket <br> cg-cis-pe-prod-ue1-deployment-bucket <br><br> dmpc-network-staging-euw2-tfstate-bucket <br> dmpc-wh-staging-euw2-dispatch-bucket <br><br> genco-dev-cloudwatch-logs-bucket <br> genco-sales-dev-monthlyreport-bucket |
| S3 Bucket Policy | {{naming_prefix}}-`<region>`-{{bucket_purpose}}-`<aws_resource>` | `{{bucket_purpose}}` should describe the bucket function: <br> <ui><li>deployment</li><li>tfstate</li><li>logs</li><li>ingestion</li></ul> | cg-prod-ue1-logs-bucket-policy <br> cg-cis-pe-prod-ue1-deployment-bucket-policy <br><br> dmpc-network-staging-euw2-tfstate-bucket-policy <br> dmpc-wh-staging-euw2-dispatch-bucket-policy <br><br> genco-dev-cloudwatch-logs-bucket-policy <br> genco-sales-dev-monthlyreport-bucket-policy |

### ElastiCache Resources
| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| ElastiCache | {{naming_prefix}}-`<region>`-`<resource_identifier>`-{{engine}}-{{deployment_type}}-`<aws_resource>` | `{{engine}}` can be one of: <br> <ui><li>memcached</li><li>redis</li></ul> <br> `{{deployment_type}}` can be one of: <br> <ui><li>standalone</li><li>multiaz</li></ul> <br> `{{db_resource}}` can be one of: <br> <ui><li>cluster</li><li>instance</li></ul> <br> `<resource_identifier>` should describe the function of the resource: <br> <ui><li>logstash</li><li>metrics</li><li>reports</li></ul>  | **Account naming:** <br> <ul><li>cg-prod-ue1-gitlab-redis-standalone-ec</li><li>dmpc-network-staging-euw2-ip-analysis-redis-multiaz-ec</li><li>genco-dev-logstash-memcached-standalone-ec</li></ul> <br> **Team naming:** <br> <ul><li>cg-cis-pe-prod-ue1-gitlab-redis-multiaz-ec</li><li>dmpc-wh-staging-euw2-dispatch-redis-standalone-ec</li><li>genco-dev-sales-reports-memcached-standalone-ec</li></ul> |

### CloudWatch Resources
| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| CloudWatch Alarm | {{naming_prefix}}-`<region>`-`<resource_identifier>`-{{alarm_type}}-`<aws_resource>` | `{{alarm_type}}` should describe the alarm: <br> <ui><li>scaleup</li><li>scaledown</li><li>cpu-high</li><li>cpu-low</li><li>mem-high</li><li>mem-low</li><li>throughput-high</li><li>throughput-warning</li></ul> <br> `<resource_identifier>` should describe the function of the resource: <br> <ui><li>web-server-efs</li><li>image-server</li><li>gitlab-ec2</li></ul> | **Account naming:** <br> <ul><li>cg-prod-ue1-gitlab-scaleup-alarm</li><li>dmpc-network-staging-euw2-image-server-throughput-warning-alarm</li><li>genco-dev-web-server-cpu-high-alarm</li></ul> <br> **Team naming:** <br> <ul><li>cg-cis-pe-prod-ue1-gitlab-runner-scaleup-alarm</li><li>dmpc-wh-staging-euw2-image-server-cpu-high-alarm</li><li>genco-dev-sales-reports-throughput-high-alarm</li></ul> |

### RDS Resources
| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| RDS Instance | {{naming_prefix}}-`<region>`-`<resource_identifier>`-{{db_engine}}-{{deployment_type}}-{{db_resource}} | `{{db_engine}}` can be one of: <br> <ui><li>aurora</li><li>maria</li><li>mysql</li><li>oracle</li><li>postgres</li><li>dynamodb</li></ul> <br> `{{deployment_type}}` can be one of: <br> <ui><li>standalone</li><li>multiaz</li><li>writer</li><li>reader</li></ul> <br> `{{db_resource}}` can be one of: <br> <ui><li>cluster</li><li>instance</li></ul> <br> `<resource_identifier>` should describe the function of the resource: <br> <ui><li>ip-analysis</li><li>metrics</li><li>reports</li></ul> | **Account naming:** <br> <ul><li>cg-prod-ue1-oracle-standalone-instance</li><li>dmpc-network-staging-euw2-aurora-multiaz-cluster</li><li>genco-dev-aurora-reader-instance</li></ul> <br> **Team naming:** <br> <ul><li>cg-cis-pe-prod-ue1-ip-analysis-mysql-standalone-instance</li><li>dmpc-wh-staging-euw2-metrics-postgres-standalone-instance</li><li>genco-dev-sales-reports-mysql-cluster</li></ul> |
| Parameter Group | {{naming_prefix}}-`<region>`-`<resource_identifier>`-{{db_engine}}-{{deployment_type}}-{{db_resource}}-`<aws_resource>` | `{{db_engine}}` can be one of: <br> <ui><li>aurora</li><li>maria</li><li>mysql</li><li>oracle</li><li>postgres</li><li>dynamodb</li></ul> <br> `{{deployment_type}}` can be one of: <br> <ui><li>standalone</li><li>multiaz</li><li>writer</li><li>reader</li></ul> <br> `{{db_resource}}` can be one of: <br> <ui><li>cluster</li><li>instance</li></ul> <br> `<resource_identifier>` should describe the function of the resource: <br> <ui><li>ip-analysis</li><li>metrics</li><li>reports</li></ul> | **Account naming:** <br> <ul><li>cg-prod-ue1-oracle-standalone-paramgroup</li><li>dmpc-network-staging-euw2-aurora-multiaz-paramgroup</li><li>genco-dev-aurora-reader-paramgroup</li></ul> <br> **Team naming:** <br> <ul><li>cg-cis-pe-prod-ue1-ip-analysis-mysql-standalone-paramgroup</li><li>dmpc-wh-staging-euw2-metrics-postgres-standalone-paramgroup</li><li>genco-dev-sales-reports-mysql-paramgroup</li></ul> |

### Secrets Resources
| AWS Resource | Naming Convention | Comment | Example |
| ------------ | ----------------- | ------- | ------- |
| Secrets Manager | `<environment>`/`{{secret_type}}`/`<team>`/`<resource_identifier>`/`{{secret_name}}` | By using a tiered naming approach, you can better control access to secrets using IAM policies. For example, you could deny access within a policy by including `prod/app/team/*`; alternatively, you could grant access to shared secrets by allowing access to `prod/common/*`  | prod/app/cis/pe/gitlab/database <br><br> staging/wh/app/dispatch/rds <br><br> dev/common/sharedcredential |
| Parameter Store | `<environment>`/`{{parameter_type}}`/`<team>`/`<resource_identifier>`/`{{parameter_name}}` |  | prod/app/cis/pe/gitlab/user <br><br> staging/wh/app/dispatch/user <br><br> dev/common/shareduser |

<ui><li>item1</li><li>item2</li><li>item3</li></ul>


## Additional Resources
[Naming conventions](https://confluence.huit.harvard.edu/display/CLA/Cloud+Resource+Naming+Conventions)