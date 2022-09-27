# Terraform best practices

The standards and guide-lines detailed in these pages are a set of working best practices and principles for writing modules that should be followed for all projects. They are based on input from many experienced Terraform users, including those from AWS, HashiCorp, and community grown conventions.

Terms mentioned throughout reference the official [Terraform Glossary](https://www.terraform.io/docs/glossary) <br>
If you see unfamiliar terms, such as [Root module](https://www.terraform.io/docs/glossary#root-module), check the external glossary.

---

HashiCorp (company behind Terraform Project) do not point to specific patterns and best practices in their documentation.
There is possibility where you can store whole code in single `.tf` file, however this is highly discouraged, as navigation in thousands lines of code is usually hard.
For small Terraform code and [modules](modules), we can observe established pattern, where the code containing resource definitions is put on `main.tf` file, with  variables and outputs separated into `variables.tf` and `outputs.tf` files.

```
.
├── LICENSE
├── README.md
├── main.tf
├── variables.tf
├── outputs.tf
```

As Terraform configuration usually includes `terraform{}` block (i.e. for adding required providers and for pinning them to particular version), this is often split from `main.tf` and put into `terraform.tf` for better readability.

Whenever you write Terraform code don't assume it's self explanatory. Different people applies different thinking. Comment your code whenever you feel it might need few words of explanation.

## Security

Since your Terraform state may contain [sensitive data](https://www.terraform.io/language/state/sensitive-data), remember to use encryption at rest whenever you're storing state on S3 bucket or terraform cloud. Whenever your `variable` or `output` contains sensitive value, use `sensitive = true` flag, so it will be masked on terraform log (so also, may be included within logs of CI/CD pipeline).

## Terragrunt

[Terragrunt](https://terragrunt.gruntwork.io/) is a thin wrapper that provides extra tools for keeping your configurations DRY (Don't Repeat Yourself), working with multiple Terraform modules, and managing remote state.
It's also helpful when you work with multiple environments within same project, as you may need different resource names, settings, etc.

## AWS Resources

Part of AWS resources that will be required for successful build of infrastructure may require resources, which could be `regional` or `global`. While writing IaC in modules, you can distinguish which part of is not region locked (i.e IAM roles, policies, etc), and which is (i.e EC2 instances, S3 buckets, etc).