# Terraform

- "Terraform is an open-source infrastructure as code software tool that provides a consistent CLI workflow to manage hundreds of cloud services. Terraform codifies cloud APIs into declarative configuration files."
- [official documentation](https://www.terraform.io/docs/index.html)
- [official learn stuff](https://learn.hashicorp.com/terraform)
- you can also [import existing infrastructure](https://www.terraform.io/docs/import/index.html) into terraform


## Wording

- a ``provider`` creates and manages resources. They wrap the API of a service (provider) like AWS, Azure or GCP.
- If you use multiple providers you can qualifiy which provider uses which resource.
- a ``resource`` might be a physical reosurce like a aws EC2 instance or a logical resource like a application.
- A resource has a type and a name (e.g. ``resource "aws_instance" "example"`{...}``)and can be configured inside the curly brackets.

## CLI


```shell
terraform init # initializes various local settings 
terraform fmt # format all files in directory
terraform validate # validate file

terraform apply # checks diff between config file and real infrastructure and creates execution plan to eliminate this diff

terraform state # The state of the infrastructure is saved in terraform.tfstate file. It can be manually modified by this command.

terraform destroy # completely destroys the Terraform-managed infrastructure
```

## Resource Dependencies

There are explicit and implicit dependencies for creating a order of actions.

- explicit: with the ``depends_on`` field of a resource.
- implicit: e.g. usage of ``instance = aws_instance.example.id``

Resources which are not dependant on others can be build in parallel.

## Provisioning

- Only necessary if you do not use image-based infrastructure.
- provisioner are defined inside a resource and have a type like: ``local-exec`` or ``remote-exec``
- Are for bootstrapping components (on creation) not to change software on a running component.
- **You need to destroy the infrastructure if the reosurces arleady exist, so the reosurces will be recreated and the bootstrapping logic of the provisioner can be done.**
- If a resource successfully creates but fails during provisioning, Terraform will error and mark the resource as "tainted".
- Terraform tries to destroy and recreate tainted resources every time *apply* ist called.
- ``terraform taint <resource.id>`` manually marks a resource as tainted.


## Input Variables

### In a nutshell

- preset in variables.tf file via the *default* field
- overwrite console (``-var 'var_name=var_value'``)
- overwrite in terraform.tfvars file
- overwrite in *.tfvars file and  (``-var-file 'production.tfvars'``)
- overwrite in environmental variables (TF_VARS_)


### A bit more extensive

- variables are defined in a *variables.tf* file and may be assigned a default value
- variables are accessed with a `var.<variable_name>` notation.
- variables can be overwritten in console: `terraform apply -var 'region=us-east-2'`
- variables can also be overwritten from file. Terraform search automatically for *terraform.tfvars* or *.auto.tfvars* files. Alternatively you can pass a file in console `terraform apply -var-file 'production.tfvars'`.
- variables can be overwritten by environment variables. They need to start with **TF_VAR_***<name of variable to overwrite>*. Environment variables are limited to string-type variables (can not use List and map type variables).
- variables which are unspecified are asked for after executing ``terraform apply``

### Variable Types

#### Lists

```hcl
# define
variable "cidrs" { type = list }`

# init
cidrs = [ "10.0.0.0/16", "10.1.0.0/16" ]
```

### Maps


```hcl
# define and init
variable "amis" {
  type = "map"
  default = {
    "us-east-1" = "ami-b374d5a5"
    "us-west-2" = "ami-4b32be2b"
  }
}

# use
resource "aws_instance" "example" {
  ami           = var.amis["us-east-1"]
  instance_type = "t2.micro"
}

```

## Output Variables
`
Output blocks can be pasted in any of the *.tf files. Output are printed after ``terraform output`` or ``terraform apply`` are run.

Example:
```hcl
output "ip" {
  value = aws_eip.ip.public_ip
}
```

## modules

Modules are self-contained packages of Terraform configurations that are managed as a group. Any set of Terraform configuration files in a folder can be a module.



- [Terraform Registry ](https://registry.terraform.io/) includes a directory of ready-to-use modules for various common purposes
- you can use local 

### use module

- After adding new modules you need to rerun ``terraform init`` not only ``terraform apply``
- ``terraform get`` will download the modules
- Modules reside under ~/.terraform/modules/<the module alias given in root tf script>
- You can define [from where to download the module](https://www.terraform.io/docs/modules/sources.html) via the `source` attribute of `module` 
    - local: `./my-module`
    - git: 'github.com/crowdsalat/examplemodule'
    - Terraform Registry: `source = 'hashicorp/consul/aws'`

Example for module usage via registry:

```hcl
module "consul" {
  source      = "hashicorp/consul/aws"
  version = "0.7.3" # optional
  num_servers = "3"
}
```

### create module

- Providers should be configured by the user of the module and not by the module itself.
- input parameters for the module are defined in variables.tf
- return values of the module are defined in outputs.tf
- outputs.tf: return values of the module



## Remote State Storage

- Use a **remote backend** to store state-data on a server.
- There are different backends. Terraform Cloud is one of such.

Google cloud backend example:

```hcl
 backend "gcs" {
    bucket = "existing-bucket-name"
  }
```