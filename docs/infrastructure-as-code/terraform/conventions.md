# Conventions

## Naming conventions

### General conventions

!!! note
    Beware that some cloud resources have restrictions in allowed names. For example, can't contain dashes, some must be camel-cased. The conventions on this page refer to Terraform names themselves.

1. Resource names should be `snake_cased`. Use `_` (underscore) instead of `-` (dash).
2. Use lowercase letters and numbers

### Resource and data source arguments
1. Do not repeat resource type in resource name.

    !!! success "Good practice"
        ```resource "aws_route_table" "public" {}```
    
    !!! failure "Bad practice"
        ```resource "aws_route_table" "public_route_table" {}```
    
    !!! failure "Bad practice"
        ```resource "aws_route_table" "public_aws_route_table" {}```

2. Resources should be called `this` if there is not more descriptive and general name available, or if the resource module creates a single resource of this type. <br>
   eg. in an [AWS VPC Module](https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws/3.14.0) there is a single resource of type `aws_nat_gateway`, and multiple resources of type `aws_route_table`. Therefore `aws_nat_gateway` should be named `this` and each `aws_route_table` should have more descriptive names such as `private`, `public` or `database`.
3. Use singluar nounds for names.
4. Use `-` inside argument values and in places where the value will be exposed to a human
5. Any `count` / `for_each` arguments inside a resource or data block should be defined as the first argument and separated by a newline after it.
6. Always favour `for_each` over `count`
7. `tags` should be the last real argument of a resource, followed by `depends_on` and `lifecycle`, if necessary. Each of these arguments should be separated by a single empty line.

## Dynamic resources
It is possible to dynamically create resources in Terraform using either [count](https://www.terraform.io/language/meta-arguments/count#the-count-meta-argument) or [for_each](https://www.terraform.io/language/meta-arguments/for_each).<br>
Unless count = o or 1, `for_each` should always be preferred over `count`

Since `for_each` requires a map, you may find a situation where you have a list you want to create resources from dynamically. In this situation, you can convert your list to a set using the `toset` [function](https://www.terraform.io/language/functions/toset). In this case, by passing `toset(var.mylist)`, Terraform will use each entry as a key.

??? example
    ```hcl
    resource "aws_ssm_parameter" "params_from_list" {
      for_each = toset(["item1", "item2", "item3"])

      name  = each.key
      type  = "String"
      value = each.value
    }

    $ terraform state list
    aws_ssm_parameter.params_from_list["item1"]
    aws_ssm_parameter.params_from_list["item2"]
    aws_ssm_parameter.params_from_list["item3"]

    $ terraform state show aws_ssm_parameter.params_from_list["item1"]
    { ...
    name  = "item1"
    value = "item1"
    ... }
    ```

## Default tags

You should ensure that all resources that can accept tags, have tags defined.

For the Terraform `aws` provider, it is possible to use `default_tags`. This feature should not be used inside the modules block, but instead inside the root module. Doing so will add default tags to all supported resources being deployed

## Attachment over embedded resources

Some resources allow pseudo resources embedded as attributes in them. Where possible, it is best practice to avoid using these. Instead you should opt to create the resource type and then attach the psuedo-resource. This will reduce chicken/egg issues that are unique per resource. 

!!! failure "Avoid"
    ```hcl
    resource "aws_security_group" "allow_tls" {
      ...
      ingress {
        description      = "TLS from VPC"
        from_port        = 443
        to_port          = 443
        protocol         = "tcp"
        cidr_blocks      = [aws_vpc.main.cidr_block]
        ipv6_cidr_blocks = [aws_vpc.main.ipv6_cidr_block]
      }

      egress {
        from_port        = 0
        to_port          = 0
        protocol         = "-1"
        cidr_blocks      = ["0.0.0.0/0"]
        ipv6_cidr_blocks = ["::/0"]
      }
    }
    ```

!!! success "Preferred"
    ```hcl
    resource "aws_security_group" "allow_tls" {
      ...
    }

    resource "aws_security_group_rule" "example" {
      type              = "ingress"
      description      = "TLS from VPC"
      from_port        = 443
      to_port          = 443
      protocol         = "tcp"
      cidr_blocks      = [aws_vpc.main.cidr_block]
      ipv6_cidr_blocks = [aws_vpc.main.ipv6_cidr_block]
      security_group_id = aws_security_group.allow_tls.id
    }
    ```