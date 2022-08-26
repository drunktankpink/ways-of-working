# Installation

Terraform can be installed a variety of different ways for multiple operating systems. 

Consult the [Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli) website for installation steps

## TF Switch

[TF switch](https://tfswitch.warrensbox.com/) is a command line tool that allows you to switch between different versions of Terraform (useful when testing upgrades)

!!! info
    `tfswitch` is only available for Linux and MacOS systems<br>
    If using Windows, this can be installed within WSL/WSL2

### Installation

The installation steps for `tfswitch` can be found [here](https://tfswitch.warrensbox.com/Install/)

!!! tip
    When installing in WSL2, you may encounter an error which can be resolved using the solution provided [here](https://tfswitch.warrensbox.com/Troubleshoot/).<br>
    However when running this fix, you are told to add `$HOME/.bin` to your PATH, but `tfswitch` installs `terraform` to `$HOME/bin` by default. This can be fixed by editing the path export to add `$HOME/bin` to PATH instead