# Terraform-gcp-template-instance
# Google Cloud Infrastructure Provisioning with Terraform

## Table of Contents

- [Introduction](#introduction)
- [Usage](#usage)
- [Module Inputs](#module-inputs)
- [Module Outputs](#module-outputs)
- [Examples](#examples)
- [License](#license)


## Introduction

This Terraform module provides infrastructure configuration for creating a **template-instance** in Google Cloud Platform (GCP) along with vpc subnets, firewall rules . It's designed to be used for managing network resources in a GCP environment.


## Usage

To use this module, include it in your Terraform configuration. Below is an example of how to call the template-instance module and its dependencies.
### Examples

## Example: _instance-from-existing-template_
```hcl
data "google_compute_instance_template" "generic" {
  name = "template-test-020230919082713685100000001"
}

module "compute_instance" {
  source                 = "git::https://github.com/cypik/terraform-gcp-template-instance.git?ref=v1.0.0"
  name                   = "instance"
  environment            = "test"
  region                 = "asia-northeast1"
  zone                   = "asia-northeast1-a"
  subnetwork             = module.subnet.subnet_id
  instance_from_template = true
  deletion_protection    = false
  service_account        = null

  ## public IP if enable_public_ip is true
  enable_public_ip         = true
  source_instance_template = data.google_compute_instance_template.generic.self_link
}
```

## Example: instance-with-template
```hcl
module "instance_template" {
  source               = "git::https://github.com/cypik/terraform-gcp-template-instance.git?ref=v1.0.0"
  name                 = "template"
  environment          = "test"
  region               = "asia-northeast1"
  source_image         = "ubuntu-2204-jammy-v20230908"
  source_image_family  = "ubuntu-2204-lts"
  source_image_project = "ubuntu-os-cloud"
  disk_size_gb         = "20"
  subnetwork           = module.subnet.subnet_id
  instance_template    = true
  service_account      = null
  ## public IP if enable_public_ip is true
  enable_public_ip     = true
  metadata = {
    ssh-keys = <<EOF
      dev:ssh-rsa AAAAB3NzaC1yc2EAA/3mwt2y+PDQMU= suresh@suresh
    EOF
  }
}

# compute-instance
module "compute_instance" {
  source                 = "git::https://github.com/cypik/terraform-gcp-template-instance.git?ref=v1.0.0"
  name                   = "instance"
  environment            = "test"
  region                 = "asia-northeast1"
  zone                   = "asia-northeast1-a"
  subnetwork             = module.subnet.subnet_id
  instance_from_template = true
  deletion_protection    = false
  service_account        = null

  ## public IP if enable_public_ip is true
  enable_public_ip         = true
  source_instance_template = module.instance_template.self_link_unique
}
```
This example demonstrates how to create various GCP resources using the provided modules. Adjust the input values to suit your specific requirements.

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
- `enable_public_ip` : Set to `true` to create public ip.


## Module Outputs

This module provides the following outputs:

- `vpc_id`: The ID of the created VPC.
- `subnet_id`: The ID of the created subnet.
- `firewall_id`: The ID of the created firewall rule.
- `compute_instance_id`: The ID of the created compute instance.
- `template_id` : An identifier for the resource with format.
- `metadata_fingerprint` : The unique fingerprint of the metadata.
- `template_tags_fingerprint` :The unique fingerprint of the tags.
- `template_self_link` : The URI of the created resource.
- `self_link_unique` : A special URI of the created resource that uniquely identifies this instance template with the following format.
- `instances_self_links` : List of self-links for compute instances

## Examples
For detailed examples on how to use this module, please refer to the [EXAMPLES](https://github.com/cypik/terraform-gcp-template-instance/tree/master/example) directory within this repository.

## License
This Terraform module is provided under the **'[License Name]'** License. Please see the [LICENSE](https://github.com/cypik/terraform-gcp-template-instance/blob/master/LICENSE) file for more details.

## Author
Your Name
Replace **'[License Name]'** and **'[Your Name]'** with the appropriate license and your information. Feel free to expand this README with additional details or usage instructions as needed for your specific use case.
