# ROSA Classic with shared VPC example

## Introduction

This is a Terraform manifest example for creating a Red Hat OpenShift Service on AWS (ROSA) cluster. This example provides a structured configuration template that demonstrates how to deploy a ROSA cluster within your AWS environment by using Terraform.

This example includes:
- A ROSA cluster with public access.
- A pre-existing shared VPC within a separate AWS account.

## Prerequisites

* You have installed the [Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli) (1.4.6+).
* You have an [AWS account](https://aws.amazon.com/free/?all-free-tier) and [associated credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/security-creds.html) that you can use to create resources. The credentials configured for the AWS provider (see the [Authentication and Configuration](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#authentication-and-configuration) section in the AWS Terraform provider documentation).
* You have a secondary AWS account for the shared VPC resources. You must supply the credentials for this account through the appropriate input variables in the example.
* You have completed the [ROSA getting started AWS prerequisites](https://console.redhat.com/openshift/create/rosa/getstarted).
* You have a valid [OpenShift Cluster Manager API Token](https://console.redhat.com/openshift/token) configured (see [Authentication and configuration](https://registry.terraform.io/providers/terraform-redhat/rhcs/latest/docs#authentication-and-configuration) for more info).
* Recommended: You have installed the following CLI tools:
    * [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
    * [ROSA CLI](https://docs.openshift.com/rosa/cli_reference/rosa_cli/rosa-get-started-cli.html)
    * [Openshift CLI (oc)](https://docs.openshift.com/rosa/cli_reference/openshift_cli/getting-started-cli.html)

For more info about shared VPC, see [Configuring a shared VPC for ROSA clusters](https://docs.openshift.com/rosa/rosa_install_access_delete_clusters/rosa-shared-vpc-config.html).

<!-- BEGIN_AUTOMATED_TF_DOCS_BLOCK -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.0 |
| <a name="requirement_aws"></a> [aws](#requirement\_aws) | >= 4.0 |
| <a name="requirement_null"></a> [null](#requirement\_null) | >= 3.0.0 |
| <a name="requirement_random"></a> [random](#requirement\_random) | >= 2.0 |
| <a name="requirement_rhcs"></a> [rhcs](#requirement\_rhcs) | >= 1.6.2 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_aws"></a> [aws](#provider\_aws) | >= 4.0 |
| <a name="provider_aws.shared-vpc"></a> [aws.shared-vpc](#provider\_aws.shared-vpc) | >= 4.0 |
| <a name="provider_null"></a> [null](#provider\_null) | >= 3.0.0 |
| <a name="provider_random"></a> [random](#provider\_random) | >= 2.0 |
| <a name="provider_rhcs"></a> [rhcs](#provider\_rhcs) | >= 1.6.2 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_account_iam_resources"></a> [account\_iam\_resources](#module\_account\_iam\_resources) | ../../modules/account-iam-resources | n/a |
| <a name="module_oidc_config_and_provider"></a> [oidc\_config\_and\_provider](#module\_oidc\_config\_and\_provider) | ../../modules/oidc-config-and-provider | n/a |
| <a name="module_operator_policies"></a> [operator\_policies](#module\_operator\_policies) | ../../modules/operator-policies | n/a |
| <a name="module_operator_roles"></a> [operator\_roles](#module\_operator\_roles) | ../../modules/operator-roles | n/a |
| <a name="module_rosa_cluster_classic"></a> [rosa\_cluster\_classic](#module\_rosa\_cluster\_classic) | ../../modules/rosa-cluster-classic | n/a |
| <a name="module_shared-vpc-policy-and-hosted-zone"></a> [shared-vpc-policy-and-hosted-zone](#module\_shared-vpc-policy-and-hosted-zone) | ../../modules/shared-vpc-policy-and-hosted-zone | n/a |
| <a name="module_vpc"></a> [vpc](#module\_vpc) | ../../modules/vpc | n/a |

## Resources

| Name | Type |
|------|------|
| [null_resource.validations](https://registry.terraform.io/providers/hashicorp/null/latest/docs/resources/resource) | resource |
| [random_password.password](https://registry.terraform.io/providers/hashicorp/random/latest/docs/resources/password) | resource |
| [rhcs_dns_domain.dns_domain](https://registry.terraform.io/providers/terraform-redhat/rhcs/latest/docs/resources/dns_domain) | resource |
| [aws_caller_identity.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/caller_identity) | data source |
| [aws_caller_identity.shared_vpc](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/caller_identity) | data source |
| [aws_partition.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/partition) | data source |
| [aws_region.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/region) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_cluster_name"></a> [cluster\_name](#input\_cluster\_name) | Name of the cluster. After the creation of the resource, it is not possible to update the attribute value. | `string` | n/a | yes |
| <a name="input_openshift_version"></a> [openshift\_version](#input\_openshift\_version) | The required version of Red Hat OpenShift for the cluster, for example '4.1.0'. If version is greater than the currently running version, an upgrade will be scheduled. | `string` | `"4.14.9"` | no |
| <a name="input_shared_vpc_aws_access_key_id"></a> [shared\_vpc\_aws\_access\_key\_id](#input\_shared\_vpc\_aws\_access\_key\_id) | The access key provides access to AWS services and is associated with the shared-vpc AWS account. | `string` | `""` | no |
| <a name="input_shared_vpc_aws_profile"></a> [shared\_vpc\_aws\_profile](#input\_shared\_vpc\_aws\_profile) | The name of the AWS profile configured in the AWS credentials file (typically located at ~/.aws/credentials). This profile contains the access key, secret key, and optional session token associated with the shared-vpc AWS account. | `string` | `""` | no |
| <a name="input_shared_vpc_aws_secret_access_key"></a> [shared\_vpc\_aws\_secret\_access\_key](#input\_shared\_vpc\_aws\_secret\_access\_key) | The secret key paired with the access key. Together, they provide the necessary credentials for Terraform to authenticate with the shared-vpc AWS account and manage resources securely. | `string` | `""` | no |
| <a name="input_shared_vpc_aws_shared_credentials_files"></a> [shared\_vpc\_aws\_shared\_credentials\_files](#input\_shared\_vpc\_aws\_shared\_credentials\_files) | List of files path to the AWS shared credentials file. This file typically contains AWS access keys and secret keys and is used when authenticating with AWS using profiles (default file located at ~/.aws/credentials). | `list(string)` | `null` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_account_role_prefix"></a> [account\_role\_prefix](#output\_account\_role\_prefix) | The prefix used for all generated AWS resources. |
| <a name="output_account_roles_arn"></a> [account\_roles\_arn](#output\_account\_roles\_arn) | A map of Amazon Resource Names (ARNs) associated with the AWS IAM roles created. The key in the map represents the name of an AWS IAM role, while the corresponding value represents the associated Amazon Resource Name (ARN) of that role. |
| <a name="output_api_url"></a> [api\_url](#output\_api\_url) | URL of the API server. |
| <a name="output_cluster_admin_password"></a> [cluster\_admin\_password](#output\_cluster\_admin\_password) | The password of the admin user. |
| <a name="output_cluster_admin_username"></a> [cluster\_admin\_username](#output\_cluster\_admin\_username) | The username of the admin user. |
| <a name="output_cluster_id"></a> [cluster\_id](#output\_cluster\_id) | Unique identifier of the cluster. |
| <a name="output_console_url"></a> [console\_url](#output\_console\_url) | URL of the console. |
| <a name="output_current_version"></a> [current\_version](#output\_current\_version) | The currently running version of OpenShift on the cluster, for example '4.11.0'. |
| <a name="output_domain"></a> [domain](#output\_domain) | DNS domain of cluster. |
| <a name="output_infra_id"></a> [infra\_id](#output\_infra\_id) | The ROSA cluster infrastructure ID. |
| <a name="output_oidc_config_id"></a> [oidc\_config\_id](#output\_oidc\_config\_id) | The unique identifier associated with users authenticated through OpenID Connect (OIDC) generated by this OIDC config. |
| <a name="output_oidc_endpoint_url"></a> [oidc\_endpoint\_url](#output\_oidc\_endpoint\_url) | Registered OIDC configuration issuer URL, generated by this OIDC config. |
| <a name="output_operator_role_prefix"></a> [operator\_role\_prefix](#output\_operator\_role\_prefix) | Prefix used for generated AWS operator policies. |
| <a name="output_operator_roles_arn"></a> [operator\_roles\_arn](#output\_operator\_roles\_arn) | List of Amazon Resource Names (ARNs) for all operator roles created. |
| <a name="output_password"></a> [password](#output\_password) | n/a |
| <a name="output_path"></a> [path](#output\_path) | The arn path for the account/operator roles as well as their policies. |
| <a name="output_private_hosted_zone_id"></a> [private\_hosted\_zone\_id](#output\_private\_hosted\_zone\_id) | ID assigned by AWS to private Route 53 hosted zone associated with intended shared VPC |
| <a name="output_state"></a> [state](#output\_state) | The state of the cluster. |
<!-- END_AUTOMATED_TF_DOCS_BLOCK -->
