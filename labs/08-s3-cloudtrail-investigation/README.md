# Lab 08 — S3 Management-Event Investigation with AWS CloudTrail

## Overview

This project used AWS CloudTrail Event history to investigate management activity against a private Amazon S3 bucket. The recorded events identified which principal created and deleted the bucket, the service and Region involved, and whether each API operation modified resources. The investigation also led to verification and simplification of the account's IAM security baseline.

## Project profile

| Item | Configuration |
| --- | --- |
| Region | US East (Ohio), `us-east-2` |
| Services | Amazon S3, AWS CloudTrail, AWS IAM |
| S3 resource | General-purpose bucket used only for the investigation |
| Public access | Block Public Access enabled |
| Object Ownership | Bucket owner enforced; ACLs disabled |
| Versioning | Disabled |
| Bucket policy | None |
| CloudTrail source | Built-in Event history; no custom trail or event data store |
| Events investigated | `CreateBucket`, `DeleteBucket` |

## Implementation

1. Created a private S3 bucket with Block Public Access enabled and added a purpose tag.
2. Located the corresponding `CreateBucket` management event in CloudTrail Event history.
3. Reviewed the event identity, timestamp, source service, Region, read-only status, and referenced S3 resource.
4. Confirmed that the creation event was associated with the AWS account root identity.
5. Ended root-user activity and continued administration through the MFA-protected IAM user `devgarza`.
6. Deleted the empty S3 bucket through the IAM user session.
7. Located the `DeleteBucket` event and verified that CloudTrail attributed it to `devgarza`.
8. Generated an IAM credential report, verified MFA coverage, checked for long-term access keys, and removed redundant policies from the administrator group.

## Event validation

| Evidence | `CreateBucket` | `DeleteBucket` |
| --- | --- | --- |
| Event source | `s3.amazonaws.com` | `s3.amazonaws.com` |
| AWS Region | `us-east-2` | `us-east-2` |
| Resource type | `AWS::S3::Bucket` | `AWS::S3::Bucket` |
| Read-only | `false` | `false` |
| Principal observed | AWS account root identity | IAM user `devgarza` |
| Result | Bucket creation recorded | Bucket deletion recorded |

Both operations appeared as write-oriented management events. No S3 objects were uploaded, and the project did not configure or test object-level data-event logging.

## IAM security verification

The credential report showed MFA enabled for both the root identity and `devgarza`. It also showed no active long-term access keys for either identity.

The `Administrators` group initially contained `AdministratorAccess` plus eight narrower AWS-managed policies. Because the broad administrator policy already provided the effective permissions required by this personal lab account, the redundant policies were detached. The final group retained one user and one attached policy: `AdministratorAccess`.

This configuration is specific to a controlled learning account. Production environments should prefer temporary credentials, role-based access, and permissions scoped to job responsibilities.

## Security decisions

- Root access was reserved for tasks that specifically require the root identity after CloudTrail exposed its use during bucket creation.
- MFA was verified for both identities.
- No long-term access keys were created or retained.
- The S3 bucket remained private and contained no objects.
- Account IDs, ARNs, IP addresses, event IDs, request IDs, and temporary credential identifiers were excluded from repository documentation.
- CloudTrail findings were documented as management-event evidence without claiming a persistent trail or S3 data-event coverage.

## Cleanup

The empty S3 bucket was deleted, and the `DeleteBucket` event confirmed the cleanup action. No custom CloudTrail trail, event data store, or additional billable monitoring resource was created. The IAM user and administrator group were retained as account-management resources after their security settings were verified.

## Key takeaways

- CloudTrail answers who performed an API action, what action occurred, when it occurred, and where the request was processed.
- S3 bucket creation and deletion are management events; object-level activity requires separate data-event configuration.
- The `readOnly` field helps distinguish read activity from operations that change resource state.
- An IAM credential report provides account-wide evidence for MFA and long-term access-key status.
- Effective permission sets should be understandable and free of redundant policy attachments.
- Audit evidence can reveal unsafe operating habits and verify that corrective action changed the acting principal.

[AWS documentation: Viewing CloudTrail events with Event history](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)

[AWS documentation: IAM credential reports](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html)

[Back to project index](../../README.md)
