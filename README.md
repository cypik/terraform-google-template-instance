# terraform-gcp-template-instance
# Terraform-gcp-Template-Instance

## Table of Contents

- [Introduction](#introduction)
- [Usage](#usage)
- [Module Inputs](#module-inputs)
- [Module Outputs](#module-outputs)
- [Examples](#examples)
- [License](#license)


## Introduction

This Terraform module provides infrastructure configuration for creating a Virtual Private Cloud (VPC) in Google Cloud Platform (GCP) along with subnets, firewall rules, and compute instances. It's designed to be used for managing network resources in a GCP environment.


## Usage

To use this module, include it in your Terraform configuration. Below is an example of how to call the VPC module and its dependencies.

- # complete-instance-from-template
```hcl
data "google_compute_instance_template" "generic" {
  name = "template-test-020230919082713685100000001"
}

module "compute_instance" {
  source                   = "git::git@github.com:opz0/terraform-gcp-template-instance.git?ref=master"
  name                     = "instance"
  environment              = "test"
  region                   = "asia-northeast1"
  project_id               = "opz0-397319"
  zone                     = "asia-northeast1-a"
  subnetwork               = module.subnet.subnet_id
  num_instances            = 1
  source_instance_template = data.google_compute_instance_template.generic.self_link
  deletion_protection      = false
  service_account          = null

  access_config = [
    {
      nat_ip       = ""
      network_tier = ""
    }
  ]
}
```

- # instance-template
```hcl
# Instance Template module call
module "instance_template" {
  source = "git::git@github.com:opz0/terraform-gcp-template-instance.git?ref=master"
  instance_template = true
  name = "template"
  environment = "test"
  region = "asia-northeast1"
  project_id = "opz0-397319"
  source_image = "ubuntu-2204-jammy-v20230908"
  source_image_family = "ubuntu-2204-lts"
  source_image_project = "ubuntu-os-cloud"
  subnetwork = module.subnet.subnet_id
  service_account = null
  metadata = {
    ssh-keys = <<EOF
      dev:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCnRpyyDQHM2KPJ+j/FmgC27u/ohglMoWsJLsXSqfms5fWTW7YOm6WU89HlyWIJkQRPx4pIxpGFvDZwrFu0u3uTKLDtlfjs7KG5pH7q2RzIDq7spvrLJZ5VX2hJxveP9+L6ihYrPhcx5/0YqTB2cIkD1/R0qwnOWlNBUpDL9/GcLH54hjJLjPcMLfVfJwAa9IZ8jDGbMbFYLRazk78WCaYVe3BIBzFpyhwYcLL4YVolO6l450rsARENBq7ObXrP3AW1O/+I3fLaKGVZB7VXA7I0rj3MKU4qzD5L6tZLn5Lq3aUPcerhDgsiCY0X4nSJygxYX2Lxc3YKmJ/1PvrR9eJJ585qkRE25Z7URiICm45kFVfqf5Wn56PxzA+nOlPpV2QeNspI/6wih87hbyvUsE0y1fyds3kD9zVWQNzLd2BW4QZ/ZoaYRVY365S8LEqGxUVRbuyzni+51lj99yDW8X8W/zKU+lCBaggRjlkx4Q3NWS1gefgv3k/3mwt2y+PDQMU= suresh@suresh

    EOF
  }
}

# Compute Instance module call
module "compute_instance" {
  source = "git::git@github.com:opz0/terraform-gcp-template-instance.git?ref=master"
  instance_from_template = true
  name = "instance"
  environment = "test"
  project_id = "opz0-397319"
  region = "asia-northeast1"
  zone = "asia-northeast1-a"
  subnetwork = module.subnet.subnet_id
  num_instances = "1"
  source_instance_template = module.instance_template.self_link_unique
  deletion_protection = false
  service_account = null

  access_config = [
    {
      nat_ip = ""
      network_tier = ""
    },
  ]
}
```

## Module Inputs

Here are the inputs accepted by this module:

- `provider`: Configuration for the Google Cloud provider.
- `vpc`: Configuration for the Virtual Private Cloud (VPC).
- `subnet`: Configuration for subnets.
- `firewall`: Configuration for firewall rules.
- `compute_instance`: Configuration for compute instances.
- `instance_template`: Set to `true` to create an instance template.
- `name`: The name of the instance template.
- `environment`: The environment (e.g., "test").
- `region`: The GCP region.
- `project_id`: The GCP project ID.
- `source_image`: Source image for the instances.
- `source_image_family`: Image family for the instances.
- `source_image_project`: Project for the source image.
- `subnetwork`: ID of the subnet to associate with instances.
- `service_account`: Service account configuration.
- `metadata`: Metadata configuration, including SSH keys.
- `project_id`: The GCP project ID.
- `zone`: The GCP zone.
- `num_instances`: Number of instances to create.
- `source_instance_template`: Link to the instance template.
- `deletion_protection`: Enable or disable deletion protection.
- `access_config`: List of access configurations.


## Module Outputs

This module provides the following outputs:

- `vpc_id`: The ID of the created VPC.
- `subnet_id`: The ID of the created subnet.
- `firewall_id`: The ID of the created firewall rule.
- `compute_instance_id`: The ID of the created compute instance.

## Examples
For detailed examples on how to use this module, please refer to the 'examples' directory within this repository.

## License
This Terraform module is provided under the '[License Name]' License. Please see the [LICENSE](https://github.com/opz0/terraform-gcp-template-instance/blob/readme/LICENSE) file for more details.

## Author
Your Name
Replace '[License Name]' and '[Your Name]' with the appropriate license and your information. Feel free to expand this README with additional details or usage instructions as needed for your specific use case.
