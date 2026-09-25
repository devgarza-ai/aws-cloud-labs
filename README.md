# AWS Cloud Labs

A portfolio of hands-on AWS projects covering storage, compute, networking, databases, identity, serverless architecture, observability, and layered network security. Each project was built in a personal AWS account, validated against a defined objective, and cleaned up after verification.

## Projects

| Lab | Project | AWS services | Engineering focus | Final state |
| ---: | --- | --- | --- | --- |
| 01 | [Private S3 object access](labs/01-s3-private-access/README.md) | Amazon S3 | Private-by-default storage and presigned access | Object and bucket deleted |
| 02 | [EC2 launch and SSH](labs/02-ec2-launch-and-ssh/README.md) | Amazon EC2, Amazon EBS, VPC security groups | Linux provisioning, encrypted storage, SSH troubleshooting | Instance and related lab resources removed |
| 03 | [VPC network foundations](labs/03-vpc-network-foundations/README.md) | Amazon VPC, Amazon EC2, Amazon S3 | Multi-AZ subnetting, routing, gateway endpoints | Validation instance and custom VPC removed |
| 04 | [RDS for MySQL](labs/04-rds-mysql-managed-database/README.md) | Amazon RDS | Managed database provisioning and lifecycle controls | Database and related resources removed |
| 05 | [IAM S3 read-only access](labs/05-iam-s3-read-only/README.md) | AWS IAM, Amazon S3 | Group-based permissions and access verification | Lab identities removed |
| 06 | [Serverless event observability](labs/06-serverless-event-observability/README.md) | AWS Lambda, Amazon DynamoDB, Amazon CloudWatch, Amazon SNS | Event-driven processing, logs, metrics, alarms, notifications | All lab resources removed |
| 07 | [Multi-AZ VPC security architecture](labs/07-multi-az-vpc-security/README.md) | Amazon VPC | Public/private routing, tiered security groups, custom network ACLs | Custom VPC and dependent resources removed |
| 08 | [S3 management-event investigation](labs/08-s3-cloudtrail-investigation/README.md) | Amazon S3, AWS CloudTrail, AWS IAM | Management-event auditing, identity verification, account hardening | Bucket deleted; IAM controls verified and simplified |

## Documentation standard

Each lab documents the project objective, architecture or configuration, implementation decisions, validation results, and cleanup state. Claims are limited to actions that were directly verified; design-only components and untested behaviors are identified explicitly.

## Security and privacy

Credentials, private keys, signed URLs, account identifiers, email addresses, and temporary service endpoints are excluded. Console evidence was reviewed during implementation, while the public repository retains only the technical result needed to explain each project.

Labs 01–06 and 08 used US East (Ohio), `us-east-2`, for regional resources. Lab 07 used US East (N. Virginia), `us-east-1`. IAM resources are global.
