# Modules

## Why use modules?

A Terraform module allows you to create logical abstraction on the top of a resource set. In other words, modules allows you to group resources together and reuse multiple times without the need to duplicate code.

For example, we have a requirement to create a virtual server in the cloud. We may need the following resources:
 - the virtual machine, created from an image
 - attached block device, with a specified size for additional storage
 - a static IP address mapped to the servers virtual network interface
 - a set firewall rules attached to the server

By creating a module for your virtual machine, it will allow you to create this server multiple times - all without having to repeat the same configuration code over and over.

Below is an example of how to call the virtual machine module <br>
"To call a module" means to use it in the configuration file.

!!! example
    ```hcl
    module "server" {
    
    count         = 5
    
    source        = "./module_server"
    some_variable = some_value
    }
    ```

!!! info
    Terraform supports "count" for modules starting from version 0.13

### Organising modules

It's best practice to create a module for any distinct logical component of infrastructure. This could be:

- a network like a virtual private cloud/virtual network
- static content hosting (i.e. buckets)
- virtual machines
- databases
- load balancers
- logging configuration

### Module sources

Once you have several custom modules, these can be referred to as "child" modules. The configuration where you call the child modules is the "root" module
The child module can be sourced from a number of places:

- local paths
- the official Terraform Registry
- a Git repository
- an HTTP URL to a `.zip` archive containing the module

## Structure

Modules should look to maintain a structure that contains at least the below files:

```
├── README.md
├── main.tf
├── variables.tf
├── outputs.tf
```

- `main.tf` - contains Terraform resources
- `outputs.tf` - contains Module outputs
- `provider.tf` - contains the [terraform block](https://www.terraform.io/language/settings) with `required_providers`
- `variables.tf` - contains variable declarations

**Other files:**

- `data.tf` - includes `locals` & `data` declarations. Note: it is common for these declarations to be included in the `main.tf` file instead
- `alias.tf`- can be used if you have aliased providers to declare
- `versions.tf` - alternative name for `provider.tf`. 

    !!! info
        This convention comes from the `terraform0.12-upgrade` command where once terraform code was upgraded from v0.11.x to v0.12.x a `versions.tf` file was create to enforce `terraform { required_version = ">= 0.12.0}"`

**Service named files:**<br>
It's common to create several files and separate terraform resources by service. This should be stifled as much as possible in favour of defining resources inside the `main.tf`. If a collection of resources, for example IAM Roles and Policies, exceed 150 lines then it is reasonable to break that into its own files such as `iam.tf`. Otherwise all resource code should be defined in the `main.tf`.

!!! note
    As best practice, you should look to maintain module code in addition to examples & functional tests.

## Examples & Testing

Its good practice to include at least one working deployment example, and multiple examples to cover various usage patterns. By convention, it is best practice to call this test `basic` 

Modules should provide tests to guarantee provided functionality. These tests can be done via [Terratest](https://terratest.gruntwork.io/docs/getting-started/quick-start/). These tests should verify each deployment example in addition to any other functionality.<br>
Example specific tests should be named `examples_<example_name>_test.go`. Tests that are generic to the module should be named `<module_name>_tests.go`

```
$ tree
├── examples
│   ├── basic
│   │   ├── main.tf
│   │   ├── outputs.tf
│   │   └── variables.tf
│   └── formatted_tags
│       ├── main.tf
│       └── variables.tf
├── modules
│   └── my_sub_module/
├── test
│   ├── examples_basic_test.go
│   ├── examples_formatted_tags_test.go
│   └── label_test.go
├── LICENSE
├── README.md
├── main.tf
├── variables.tf
├── outputs.tf

```