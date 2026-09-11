# Lab 03 — Amazon VPC Network Foundations

**Author:** DevGarza
**Date:** September 10, 2026 (Pacific)
**Region:** US East (Ohio), `us-east-2`
**Status:** Completed; all temporary resources removed

## Goal and configuration

Build a two-AZ VPC, distinguish public/private routing, add private S3 connectivity, validate the public subnet with EC2, and prove cleanup.

| Setting | Value |
| --- | --- |
| VPC | `devgarza-vpc-lab-vpc` |
| CIDR | `10.0.0.0/16` |
| AZs | `us-east-2a`, `us-east-2b` |
| Subnets | 2 public, 2 private |
| NAT gateways | None |
| Endpoint | S3 gateway |
| DNS hostnames / resolution | Enabled / enabled |
| Public route | `0.0.0.0/0` → internet gateway |

Creation settings (evidence reviewed privately) · Resource map (evidence reviewed privately)

## Validation

I launched `devgarza-vpc-demo-ec2` in the `us-east-2a` public subnet using Amazon Linux 2023, `t3.micro`, no key pair, the default security group, and an 8 GiB root disk. It received private address `10.0.5.80`, temporary public address `18.220.22.211`, and passed 3/3 status checks.

Validation instance (evidence reviewed privately)

## Cleanup

I terminated the instance, verified its EBS volume was absent, deleted the custom VPC, and confirmed deletion of the VPC plus nine related resources. Only the default VPC remained.

Cleanup proof (evidence reviewed privately)

## Lessons

- Public/private status comes from routing, not the subnet name.
- Public internet traffic used the internet gateway.
- Private route tables gained an S3 prefix-list route through the gateway endpoint without a NAT gateway.
- Cleanup required checking the instance disk and VPC dependencies separately.

[Back to lab index](../../README.md)
