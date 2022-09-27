# Resource Tagging

The purpose of this document is to outline key concepts of resource tagging and highlight the importance in having a tagging strategy.
The ideal tagging strategy will change per organisation, client or project; therefore this document is not intended to be a provide a ready made & reusable strategy, but instead to provide insight and guidance on best practices that will enable you to adopt a strategy that meets individual requirements or challenges.

Amazon Web Services (AWS) allows up to 50 tags per AWS resource. Each cloud provider has its own limits, as well as its own defined tagging guidelines. This document may be useful for other cloud providers, but its intended use case is to provide guidance for the AWS platform.

### What are Tags?
Think of tags as labels which can be added to resources such as instances, storage volumes and databases. Metadata in the form of key value pairs, which provide additional information & context about that specific resource. Tags can assist in the identification & management of deployed resources, providing information on who owns a resource, the environment it is used, as well as any other technical or business attributes based on requirements.

In order to get the most out of tags, it pays to have a set tagging strategy and to ensure all team members are aware which tags to use and when to use them. 
Without a defined strategy, it can become an unsustainable mess.
For example, without a standard, you could have over 20 variables of a "Cost Centre" tag. 

### What are Use Cases for Tags?
Tags can be used to categorise resources by owner, environment, purpose etc. This can help to understand AWS costs & usage. Tools such as AWS Cost Explorer allow you to use tags to breakdown resource usage.

Provides possibilities for security & access control

Provides possibilities for automation

---

## AWS Best Practices for Tagging AWS Resources
To assist users in developing an optimal tagging strategy, Amazon recommends the following best practices:

- Do not add personally identifiable information (PPI) or other confidential or sensitive information in tags. Tags are not intended to be used for private or sensitive data
- Always use a standardised, case-sensitive format for tags, and implement it consistently across all resource types
- Consider tag guidelines that support multiple purposes, like managing resource access control, cost tracking, automation and organisation
- Aim towards too many tags, rather than too few tags. AWS allows for up to 50 tags per resource
- Implement automated tools to help manage resource tags. The tools should be capable of reallocating existing tags names to comply with a new strategy. [AWS Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/resource-groups.html) and the [Resource Groups Tagging API](https://docs.aws.amazon.com/resourcegroupstagging/latest/APIReference/overview.html) enable programmatic control of tags, making it easier to automatically manage, search and filter tags and resources.
- Remember that it is easy to modify tags to accommodate changing business requirements, however, consider the ramifications of future changes, especially in relation to tag-based access control, automation or upstream billing reports
- You can automatically enforce the tagging standards your organisation chooses to adopt by creating and deploying [tag policies](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_tag-policies.html) using AWS Organizations. 

---

## Tagging Categories

The following categories and tags are not exhaustive. But serve to give an idea on the types of tags which can be used within your tagging strategy:

- **Technical -** Allow engineers to identify and work with resources
- **Automation -** Allow automated tasks to be performed against resources
- **Business -** Allow stakeholders to identify responsibility and analyse costs 
- **Security -** Allow security and compliance standards to be adhered, or to provide access to resources


| Technical | Automation | Business | Security |
| --------- | ---------- | -------- | -------- |
| **Name -** Identify resources with a unique name <br><br> **Application ID -** Identify resources that belong to a specific application <br><br> **Application role / service -** Describe the function of of a particular resource (web server, bastion, database, message broker) <br><br> **Cluster -** Identify resource groups that share a common configuration & perform a specific function for an application <br><br> **Environment -** Distinguish between development, test & production environments <br><br> **Version -** Distinguish between versions of applications or resources | **Date/Time -** Identify the date or time a resources should be started, stopped, deleted or rotated <br><br> **Opt in/Opt out -** Indicate whether a resource should be included in an automated activity such as starting, stopping or resizing instances <br><br> **Security -** Determine requires such as encryption or enabling of Amazon VPC flow logs; identify resources that need extra security (security groups or route tables) | **Project -** Identify the project that the resources support <br><br> **Owner -** Identify who is responsible for resource <br><br> **Cost Center/Business Unit -** Identify the cost center or business unit associated with a resource, typically for cost allocation <br><br> **Customer -** Identify a client that resources serve | **Confidentiality -** Identify specific data confidentiality levels for a resource <br><br> **Compliance -** Identify if resources need to adhere to specific compliance requirements |


!!! note
    Tags can be added manually to any resource, however it is best practice to define mandatory tags within common templates (AWS CloudFormation) or shared modules (Terraform). This ensures consistency in adhering to the agreed tagging strategy.
    In addition to this, policies can be set within landing zones to ensure resources are provisioned with required tags

---

## Considerations

Limits & requirements can be found at the following [AWS page](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html#tag-conventions)

The following considerations should be taken into account:

- Each resource can have a maximum of 50 user created tags.
- System created tags that begin with aws: are reserved for AWS use, and do not count against this limit. You can't edit or delete a tag that begins with the aws: prefix.
- For each resource, each tag key must be unique, and each tag key can have only one value.
- The tag key must be a minimum of 1 and a maximum of 128 Unicode characters in UTF-8.
- The tag value must be a minimum of 0 and a maximum of 256 Unicode characters in UTF-8.
- Allowed characters can vary by AWS service. For information about what characters you can use to tag resources in a particular AWS service, see its documentation. In general, the allowed characters are letters, numbers, spaces representable in UTF-8, and the following characters: `_ . : / = + - @`.
- Agree on the naming of tags. Decide how tags will be named and stick to it. For example `business_unit`, `BusinessUnit`, `businessUnit` & `business-unit` are all valid tag names, however as tags are case sensitive, only one format should be agreed and used across all resources. Avoid using similar tags with inconsistent case treatment.
- Use tooling to ensure tagging policies are adhered. Or go one step further and use policy as code to ensure resources are not provisioned without mandatory tags. Tools such as [AWS Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/resource-groups.html) can be used to tag instances, and [Cloud Custodian](https://cloudcustodian.io/) can be used to monitor for non-compliant resources.
- Restrict tag permissions. If you use tags to control resource access, then you may want to restrict who has permissions to update or remove tags
- Consider the use of prefixes for tags. For example:

There are many tag categories which can be applied to resources; because of this, a truly comprehensive tagging strategy might need input and buy in from several parts of an organisation. This itself brings many challenges, and as such, any agreed tagging strategy will be prone to change. It is important to identify and set a process which allows for any changes to be recorded and visible. It is important to ensure you have a method to keep strategy in place as organisations scale and change.

---

## Common Tagging Strategies

Using the above mentioned categories, the following strategies can be used to manage AWS resources

### Tags for resource organisation
Use tags to organise AWS resources within the AWS Management Console. You can configure tags to be displayed with resources and can search and filter against specific tags. AWS Resource Groups can be used to create groups based on one or more tags. You can also create groups of their occurrence in an AWS CloudFormation stack. With Resource Groups and Tag Editor, you can view data for applications that consist of multiple services, resources & regions in one place

### Tags for cost allocation
Using AWS Cost Explorer and detailed billing reports, you can break down AWS costs by tag. You can use business tags such as cost center/business unit, customer or project for traditional cost-allocation dimensions, however any tag can be used for a cost allocation report. This enabled you to associate costs with a technical or security dimension, such as specific applications or environments.

### Tags for automation
Use tags to filter resources during automation activities. Tags can be used to opt in or out of automated tasks, or to identify resources to archive, update or delete. For example, if you run automated `start` or `stop` scripts to turn off instances during non-business hours to reduce costs, you can use tags on EC2 instances to opt out of this task. In this use case you can even be more specific and use tags to set the stop and start times for instances. Scripts can also be used to delete out of date or rolling Amazon EBS snapshots. In this case, tags on snapshots can be used to add an extra dimension of search criteria 

### Tags for access control
Tags can be used in conjunction with IAM Policies. Tag-based conditions can be used to constrain permissions based on specific tags or tag values. For example, you could restrict an IAM user or an IAM Role permissions so that API calls to a specific environment (such as production) are allow or denied. Again, when you use tags for access control, it is important do define and restrict who has access to modify tags. 

---

## Mandatory Tags
Define mandatory tags which are to be applied to all resources.

| Tag | Format | Example | Description |
| --- | ------ | ------- | ----------- |
| application_role/service_name | lowercase alphanumerical | "backupdb" or "bastion" | Describe the service of the resource in question |
| cost_centre | numerical | "384786" | The cost centre for the resources |
| environment | lowercase alphanumerical | "prod" , "test", "dev", "uat" or "sandbox" | Follow environment naming standards |
| owner | lowercase alphanumerical | username or team | Identify who the owner is for any contact |
| project_id | lowercase alphanumerical | "cis-pe-gap" | project id for the resources |
| id | numerical | "01" | identifier |
| name | programmatic | publiccloud-prod-backup-01 | Depending on your naming convention, this should ideally be handled by your IaC deployment |


**service_name** - Allow for grouping of resources by service, across environments or projects.

**cost_centre** - Breakdown costs by business unit or cost centre. 

**environment** - Identify the environment. Can be used in conjunction with conditions within IAM policies to restrict access (to production for example). Can also be used with VPC networks. 

**owner** - Assist with service ownership and accountability of resources.

**project_id** - SOmetimes a single cost code can have multiple projects associated. Therefore an additional project ID tag can be used.

**id** - can be used to identify multiple resources of the same name 01 & 02

**name** - The resource name. This should follow a pre-defined naming convention and handles via IaC tooling

---

## Tagging Governance

For any tagging strategy to be effective, you need to apply it consistently and programmatically across AWS resources. Two approaches to governing tags are reactive and proactive

- **Reactive -** find resources that are not correctly tagged.
- **Proactive -** ensure standardised tags are consistently applied at resource creation. 

<br> 

The following tools can be used to assist with tagging governance:

- Resource Groups Tagging API
- AWS Config Rules
- Custom Scripts
- Tag Editor - can be used manually with detailed billing reports
- AWS Service Catalogue - add portfolio and product tags to apply tags to resources automatically when launched
- AWS Organisations
- Infrastructure as Code - tools such as AWS CloudFormation or Terraform support resource tagging. Best practice would be to ensure these are used when provisioning resources
- [Cloud Custodian](https://cloudcustodian.io/) - a third party tool which can be used to enforce tagging strategies using custom policies

A stronger approach can also be adopted, whereby automated tasks are run against non-conformant resources. For example, you can decide to quarantine or terminate resources which are improperly tagged. Tools such as Resource Group Tagging API or Cloud Custodian can be used to achieve this

---

## Additional Resources
[AWS - Tagging AWS resources](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html)

[Lucidchart - AWS tagging best practices](https://www.lucidchart.com/blog/aws-tagging-best-practices)

[Harvard - AWS Tagging strategies](https://confluence.huit.harvard.edu/display/CLA/Cloud+Resource+Tagging)

[CloudZero - AWS tagging best practices](https://www.cloudzero.com/blog/aws-tagging-strategy)

[Cloudforecast - AWS Tags Best Practices and Strategies](https://www.cloudforecast.io/blog/aws-tagging-best-practices/)

[Terraform vs AWS CloudFormation for AWS Tags](https://www.cloudforecast.io/blog/aws-tagging-best-practices-guide-part-2/?utm_source=blog&utm_medium=crosspostlink&utm_campaign=tagging_part1)