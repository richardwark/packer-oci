## Introduction

Building and deploying applications in a cloud can be time-saving and easy, but requires finding the best tools for a particular problem. Nearly two years ago, I found tools from Hashicorp, Packer and Terraform, which were easy to pick up and have high levels of utility.

Terraform is another article and workshop (there is a workshop that can walk you through building packer images underway), but about Packer.

Packer let's you define images as code, simple JSON.  

From that simple JSON, you can build custom images in various environments (cloud, virtualbox, VMWare) which will include the same additions and customizations defined in the JSON.

By changing the "builder", you choose where you want to place the resulting asset (image/VM)

You define your "Builder" :

- where you want it to build and deploy (virtualbox,aws,oci,etc.)  
- what image you'd like to build on (RedHat, CentOS, anything you can reach)
- your cloud specifics (availability, datacenter, credential info)

and you define a "Provisioner" which can be shared across Builder enviroments:
