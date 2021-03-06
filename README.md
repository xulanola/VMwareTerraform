# VMware Terraform 

**References**


**Tooling**
- Terraform
- Terragrunt
- test-kitchen
- bats

## VMware Project Notes
These are the projects that are allocated to the Xavier environment


terraform-admin is the project with the service account that we will use for all the terraform operations.

 Reference the `test.tfvars` envars file for a reference

## Rules of the Road

**Being a good Citizen**
- When you see something that needs to be fixed, fix it, if you can't, add a `@TODO` with ample information to come back and fix it
- When fixing a `@TODO` remove the `@TODO` so the codebase is clean
- All folders should have a `README.md` explaining the nature of what the code is supposed to be doing
- Be friendly to the people that are coming into the codebase, comment your code.

**Terraform**
- You will need to source the environment `.env` files into your shell be executing `source envars/<environment>.env` to put you in to the proper environment execution
- Leverage `terraform validate` before committing to make sure your code is good
- Try to make sure anything that can be pulled into variable is pulled into it
- Folder Layout
  * Persistent = parts of the application stack that have persistence tied to them ie Database servers
  * Ephemeral = parts of the application stack that can be blown away ie webservers / k8s compute nodes
  * Dev = development code base. Should NOT house any systems or code that cannot be destroyed on a regular basis. Promotes only to "Test".
  * Test = Systems promoted from development (dev) - used for validating changes made in development and user testing
  * Prod = Production systems that are user facing - requires tight change control
- If your code is not ready for terraform runs name your `terraform.tfvars` to `terraform.tfvars.skip`
- Using underscore `_` vs dash `-` : for variable names use underscores but for values inside of the variable use dashes. E.g:
```
GOOD....
project_name       = {
  "shared_vpc"      = "shared-vpc-host-project"
}

BAD...
project_name       = {
  "shared-vpc"      = "shared-vpc-host-project"
}
```

Underscores must be used for resource names as well, an example is shown below:
```
GOOD...
module "db_server" {
  source                     = "../../../../modules/vault-db-server"
  project_name               = "${var.project_name["vault"]}"
  [...]
}

BAD...
module "db-server" {
  source                     = "../../../../modules/vault-db-server"
  project_name               = "${var.project_name["vault"]}"
  [...]
}
```
```
GOOD...
resource "google_project_iam_binding" "iamserviceAccountActor_shared_vpc" {
  [...]
}

BAD...
resource "google_project_iam_binding" "iamserviceAccountActor-shared-vpc" {
  [...]
}
```

## Directory Structure

```
|____environments
| |____prod
| | |____README.MD
| | |____variables.tf
| | |____main.tf
| | |____outputs.tf
| | |____ephemerial
| | | |____README.md
| | |____persistant
| | | |____README.md
| |____test
| | |____README.MD
| | |____variables.tf
| | |____main.tf
| | |____outputs.tf
| | |____ephemerial
| | | |____README.md
| | |____persistant
| | | |____README.md
| |____dev
| | |____README.MD
| | |____variables.tf
| | |____main.tf
| | |____outputs.tf
| | |____ephemerial
| | | |____README.md
| | |____persistant
| | | |____README.md
| |____README.MD
| |____variables.tf
| |____main.tf
| |____outputs.tf

```


## Troubleshooting


## Pending Issues

## Onboarding 
