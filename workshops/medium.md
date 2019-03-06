## Introduction

Building and deploying applications in a cloud can be time-saving and easy, but requires finding the best tools for the particular problem you are trying to solve. Nearly two years ago, I found tools from Hashicorp- Packer and Terraform, which were easy to pick up and have high levels of utility.

Terraform is another article and workshop (there is a workshop that can walk you through building packer images underway), but about Packer.

Packer let's you define images as code, simple JSON.  

From that  JSON, you can build custom images in various environments (cloud, virtualbox, VMWare) which will include the same additions and configuration defined in the JSON.

In today's cloud environment, deploying container-based images - specifically Docker images running in Kubernetes, is an attractive deployment model for new projects.  Unfortunately for many of the customers who I speak with, they are forced to run Virtual Machine images on VMWare ESX, KVM or or similar hypervisor based platforms.  For business reasons including:

  - Vendor Certification of 3rd-party apps
  - Vendor now Defunct/unsupported version, etc.
  - System requires Accreditation of the Stack
  - VM provided directly by Application Vendor


## Fried vs. Baked

While researching the automation of building of Compute images, tools fall into two different build-patterns.  Frying can be thought of as traditional "bootstrapping" of images so that when they startup, they run a recipe which installs and configures the image.  This has the advantage of ensuring the latest possible versions of software packages.  The analogy of starting is an empty pan - the Operating System, then following a recipe, adding the ingredients and orchestrating the startup, result in images that follow the same process.  The downside with Frying is that if/when things change (a newer package is available for download from a public repo, new application installed from git repo, etc.) you get different outcomes from the same process.  Additionally, in cases where you start multiple copies of the same image, if each is started using the frying/bootstrap approach, each image must download a copy of each package.  With larger installations that can lead to networking load and heavy start-time costs.

Baking, on the other had, builds an instance of the image, but then saves it as a "custom images" allowing that to be the source for additional testing, certification and sharing of that image.  When the image is needed in production, it can be quickly booted without the load of the installation process.  Our developer desktop includes about 1gb of downloaded software.  If we need 5 copies, that means each download that 1gb footprint for a total of 5gb of network traffic. Baking that same image will incur the 1gb download at build-time, using no network for installs at boot-time.  Baking also give us better speed of boot-to-useful. 


By changing the "builder", you choose where you want to place the resulting asset (image/VM).

You define your "Builder" :

- where you want it to build and deploy (virtualbox,aws,oci,etc.)  
- what image you'd like to build from (RedHat, CentOS, anything you can reach)
- your cloud specifics (availability, datacenter, credential info)

and you define a "Provisioner" which can be shared across Builder environments:

