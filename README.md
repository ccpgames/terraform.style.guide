# Terraform Style Guide

**Table of Contents**

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
- [Introduction](#introduction)
- [Syntax](#syntax)
  - [Spacing](#spacing)
  - [Resource Block Alignment](#resource-block-alignment)
  - [Comments](#comments)
- [Naming Conventions](#naming-conventions)
  - [File Names](#file-names)
  - [Parameter, Metaparameter, and Variable Naming](#parameter-metaparameter-and-variable-naming)
  - [Resource Naming](#resource-naming)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Introduction

This repository gives coding conventions and general style guidance for Terraform's HashiCorp Configuration Language (HCL).
Terraform allows infrastructure to be described as code. As such, we should adhere to a style guide to ensure readable and high quality code.

## Syntax

- Strings are in double-quotes.

### Spacing

Use two (2) spaces when defining resources except when defining inline policies or other inline resources.

```
resource "aws_iam_role" "iam_role" {
  name = "${var.resource_name}-role"
  assume_role_policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": "sts:AssumeRole",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Effect": "Allow",
      "Sid": ""
    }
  ]
}
EOF
}
```

### Resource Block Alignment

Parameter definitions in a resource block should be aligned. The `terraform fmt` command can do this for you.

```
provider "aws" {
  access_key = "${var.aws_access_key}"
  secret_key = "${var.aws_secret_key}"
  region     = "us-east-1"
}
```


### Comments

When commenting use two "//" and a space in front of the comment.

```
// CREATE ELK IAM ROLE
...
```

## Naming Conventions

### File Names

Create a separate resource file for each type of AWS resource. Similar resources should be defined in the same file and named accordingly. Underscores should be used as word separators.
```
ami.tf
autoscaling_group.tf
cloudwatch.tf
data.tf
iam.tf
main.tf
outputs.tf
providers.tf
s3.tf
security_groups.tf
sns.tf
sqs.tf
user_data.sh
variables.tf
```
Note the special files for **data sources**, **main**, **outputs**, **providers**, and **variables**.

Large resources files of similar type may be split into smaller files by appending a tag relating all resources together.
```
acls.tf
acls_bastion.tf
acls_cache.tf
...
```

### Parameter, Metaparameter, and Variable Naming

Only use an underscore (`_`) as a word separator when naming Terraform resources like TYPE/NAME parameters and variables.

```
resource "aws_security_group" "security_group" {
  ...
}
```

### Resource Naming

Only use a hyphen (`-`) as a word separator when supplying the **name** property within a resource.

```
resource "aws_security_group" "security_group" {
  name = "${var.resource_name}-security-group"
  ...
}
```

A resource's name should contain some reference to its type within its name, either as the full type name or a shortened version.

```
resource "aws_autoscaling_group" "this_is_a_long_name_so_we_can_shorten_the_type_autoscaling_group" {
  ...
}
```
**OR**
```
resource "aws_autoscaling_group" "this_is_a_long_name_so_we_can_shorten_the_type_asg" {
  ...
}
```
Note: Always prefer consistency, if you shorten the type in a resource name, shorten the type in the name in all resources of the same type.
