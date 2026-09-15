# Lab 03 — Amazon VPC Network Foundations

## Overview

This project builds a two-Availability-Zone VPC with public and private subnets, distinct route tables, internet access for public workloads, and private routing to Amazon S3 through a gateway endpoint. A temporary EC2 instance validated the public-subnet path.

## Architecture

| Component | Configuration |
| --- | --- |
| Region | US East (Ohio), `us-east-2` |
| VPC CIDR | `10.0.0.0/16` |
| Availability Zones | `us-east-2a`, `us-east-2b` |
| Subnets | Two public and two private |
| Internet gateway | Attached to the custom VPC |
| Public route | `0.0.0.0/0` to the internet gateway |
| Private service route | Amazon S3 prefix list to an S3 gateway endpoint |
| NAT gateways | None |
| DNS | DNS resolution and DNS hostnames enabled |

## Implementation

1. Created the VPC and four subnets across two Availability Zones.
2. Associated the public subnets with a route table containing the internet-gateway default route.
3. Kept the private subnets on route tables without a default internet route.
4. Added an S3 gateway endpoint to the private route tables.
5. Launched a temporary Amazon Linux 2023 `t3.micro` instance in a public subnet with an 8 GiB root volume.

## Validation

| Check | Result |
| --- | --- |
| VPC resource map | Four subnets distributed across two Availability Zones |
| Public routing | Public subnet associated with the internet-gateway route |
| Private routing | No NAT gateway or general internet route |
| S3 private connectivity | Gateway-endpoint prefix-list route present on private route tables |
| EC2 validation | Instance received private and public IPv4 addresses and passed all status checks |

## Design decisions

- Public and private behavior was defined by route-table associations, not by subnet names.
- The S3 gateway endpoint provided service-specific private routing without a NAT gateway.
- The validation instance was temporary and existed only to confirm the public-subnet configuration.

## Cleanup

The validation instance was terminated, its EBS volume was verified as deleted, and the custom VPC was removed with its dependent networking resources. The default VPC remained intact.

## Key takeaways

- A public subnet needs a route to an internet gateway, and an internet-facing instance also needs a public address.
- Gateway endpoints can provide private access to supported AWS services without a NAT gateway.
- Route-table associations are a critical part of validating subnet intent.

[Back to project index](../../README.md)
