# Resource Tagging

The purpose of this document is to outline key concepts of resource tagging and how to apply these identify your tagging strategy. 
The ideal tagging strategy will change per organisation, client or project; therefore this document is not intended to be a provide a ready made & resusable strategy, but instead to provide insight and guidance on best practices that will enable you to adopt a stratgy that meets individual requirements or challenges


## What Are Tags?
Think of tags as labels which can be added to resources. Metadata in the form of key value pairs, which can assist in the identification & management of deployed resources.

In order to get the most out of tags, it pays to have a set tagging strategy and to ensure all team members are aware which tags to use and when to use them. 
Without a definted strategy, it can become an unsustainable mess.

## What Are Use Cases For Tags
Tags can be used to categoise resources by owner, environment, purpose etc. 

Understand AWS costs & usage. Tools such as AWS Cost Explorer allow you to use tags to breakdown resource usage.
### Technical

### Automation
Tags can be used in conjunction with tooling to provide automation. 
Example

### Security
Tags can be used to observe security levels, for compliance or to provide access to resources.

### Resource access
You can incorporate tagging into your role based access conrol (RBAC) model. Restricting certain principals access only to resources with a specific tag.

### Business
Using business and departmental tags. You can run reports against specific resources, as well as assign budgets.

## Tagging Categories
The following categories and tags are not exhaustive. But serve to give an idea on the types of tags which can be used within your tagging strategy. 
- Technical - allow engineers to identify and work with resources
  - Name - Unique resource name
  - Application ID - 
  - Cluster - Indentify resource groups
  - Application role - 
  - Version - Identify versions of applications
  - Environment - Identify environment
- Automation
  - Start/Stop - Identify when a resource should be started, stopped or terminated
- Business - Allow steakholders to identify responsibility and analyse costs 
  - Owner - Identify who is responsible for resources
  - Cost Center - Identify which cost center is allocated for resources
  - Business Unit - Identify which business unit reources belong to
  - Customer - Identify a client that resources serve
  - Project - Identify the project resources are supporting
- Security & Compliance
  - Compliance - Identify if resources need to adhere to specific compliance requirements
  - Confidentiality


Tags can be added manually to any resource, however it is best practice to define mandatory tags within common templates (AWS CloudFormation) or shared modules (Terraform). This ensures consistency in adhering to the agreed tagging strategy.
In addition to this, policies can be set within landing zones to ensure resources are provisioned with required tags


### Considerations & Best Practices
There are many tag categories which can be applied to resources; because of this, a truly comprehensive tagging strategy might need input and buy in from several parts of an organisation. This itself brings many challenges, and as such, any agreed tagging stragegy will be prone to change. It is important to identify and set a process which allows for any changes to be recorded and visable. It is important to ensure you have a method to keep strategy in place as organisations scale and change.

Use tooling to get notified when tags are missing. Or go one step further and use policy as code to ensure resources are not provisioned without mandatory tags.

AWS Resource Groups for tag automation

Agree on the naming of tags. Decide how tags will be named and stick to it. For example 'business_unit', 'BusinessUnit' & 'businessUnit' are all valid tag names, however as tags are case sensitive, only one format should be agreed and used across all resources

Consider the use of prefixes for tags. For example

Restrict tag permissions
If you use tags to control resource access, then you may want to restrict who has permissions to update tags

It should go without saying, but ensure that tags are not used to store confidential data

## Additional Resources
https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html
https://www.lucidchart.com/blog/aws-tagging-best-practices
https://confluence.huit.harvard.edu/display/CLA/Cloud+Resource+Tagging
https://www.cloudzero.com/blog/aws-tagging-strategy
https://www.cloudforecast.io/blog/aws-tagging-best-practices/
https://www.cloudforecast.io/blog/aws-tagging-best-practices-guide-part-2/?utm_source=blog&utm_medium=crosspostlink&utm_campaign=tagging_part1