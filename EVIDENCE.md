# Evidence Index

This index records the AWS console and terminal evidence reviewed during each session. Screenshots remain private; the public repository keeps only the verification result and timestamp. Times are Pacific.

## September 8, 2026

| Evidence | Time | Result |
| --- | ---: | --- |
| S3 upload (evidence reviewed privately) | 19:51:51 | One successful 38-byte upload |
| Ordinary S3 URL (evidence reviewed privately) | 19:53:30 | `AccessDenied` |
| S3 permissions (evidence reviewed privately) | 20:01:09 | Block Public Access on; no bucket policy |
| S3 cleanup (evidence reviewed privately) | 20:02:33 | Bucket deleted; list empty |
| EC2 root disk (evidence reviewed privately) | 20:37:20 | Encrypted 8 GiB gp3; delete on termination |
| SSH verification (evidence reviewed privately) | 20:55:43 | Successful login and Linux checks |
| EBS cleanup (evidence reviewed privately) | 21:07:59 | No matching volume |
| Key-pair cleanup (evidence reviewed privately) | 21:09:25 | Registration deleted; list empty |

## September 10, 2026

### VPC

| Evidence | Time | Result |
| --- | ---: | --- |
| Configuration (evidence reviewed privately) | 18:43:04 | Ohio, `10.0.0.0/16`, 2 AZs, 2 public + 2 private subnets, no NAT, S3 endpoint |
| Resource map (evidence reviewed privately) | 18:45:39 | VPC, subnets, route tables, and internet gateway |
| EC2 validation (evidence reviewed privately) | 19:02:22 | Instance running; 3/3 checks |
| Cleanup (evidence reviewed privately) | 19:10:19 | VPC and nine related resources deleted |

### RDS

| Evidence | Time | Result |
| --- | ---: | --- |
| MySQL configuration (evidence reviewed privately) | 19:26:30 | Easy-create `db.t4g.micro` configuration |
| Database deleted (evidence reviewed privately) | 19:44:35 | Database list empty |
| Snapshots empty (evidence reviewed privately) | 19:44:42 | No manual snapshots |
| Subnet group deleted (evidence reviewed privately) | 19:46:22 | Subnet-group list empty |
| Monitoring role deleted (evidence reviewed privately) | 20:20:52 | Role deletion confirmed |

### IAM

| Evidence | Time | Result |
| --- | ---: | --- |
| Initial list denial (evidence reviewed privately) | 20:15:20 | `s3:ListAllMyBuckets` permission unavailable |
| Authorized list (evidence reviewed privately) | 20:17:11 | Bucket list loaded; no buckets |
| Membership (evidence reviewed privately) | 20:17:35 | User belongs to lab group |
| Policy (evidence reviewed privately) | 20:17:43 | `AmazonS3ReadOnlyAccess` attached |
| User cleanup (evidence reviewed privately) | 20:21:31 | User list empty |
| Group cleanup (evidence reviewed privately) | 20:21:46 | Group list empty |

### Serverless

| Evidence | Time | Result |
| --- | ---: | --- |
| Lambda configuration (evidence reviewed privately) | 20:36:04 | HTTP blueprint, Node.js 22, x86_64 |
| DynamoDB stream (evidence reviewed privately) | 20:41:24 | New and old images enabled |
| Trigger (evidence reviewed privately) | 20:46:40 | Event source mapping |
| Logs (evidence reviewed privately) | 21:04:07 | Successful INIT/START/END/REPORT |
| Alarm + items (evidence reviewed privately) | 21:23:33 | Alarm in ALARM; two proof items |
| Alarm deleted (evidence reviewed privately) | 21:25:15 | Alarm list empty |
| SNS deleted (evidence reviewed privately) | 21:25:37 | Topic list empty |
| Trigger deleted (evidence reviewed privately) | 21:26:35 | Trigger removed |
| Table deleted (evidence reviewed privately) | 21:27:58 | Table list empty |
| Backups empty (evidence reviewed privately) | 21:28:03 | Backup list empty |
| Function deleted (evidence reviewed privately) | 21:28:22 | Function list empty |
| Log groups deleted (evidence reviewed privately) | 21:30:06 | Log-group list empty |
| Role deleted (evidence reviewed privately) | 21:32:16 | Execution-role deletion confirmed |
| Policy deleted (evidence reviewed privately) | 21:33:11 | Generated policy deletion confirmed |

No screenshots are published. Credentials, email addresses, signed S3 URLs, SNS subscription links, account details, the Lambda Function URL, and the live RDS endpoint remain outside this repository.
