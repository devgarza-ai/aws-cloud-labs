# Lab 05 — IAM Group-Based S3 Read-Only Access

## Overview

This project demonstrates group-based permission management in AWS Identity and Access Management. An IAM user inherited Amazon S3 read-only permissions through group membership, and access was verified against a specific S3 list operation.

## Configuration

| Resource | Configuration |
| --- | --- |
| IAM group | `devgarza-s3-readonly-lab` |
| IAM user | `devgarza-iam-demo-user` |
| Group policy | AWS managed `AmazonS3ReadOnlyAccess` |
| Scope | IAM global; permissions exercised in Amazon S3 |

## Implementation

1. Created a dedicated IAM group and user.
2. Added the user to the group.
3. Attached `AmazonS3ReadOnlyAccess` to the group rather than directly to the user.
4. Tested the user's ability to list S3 buckets.

## Validation

The first list attempt reported that `s3:ListAllMyBuckets` was unavailable. After the group policy became effective, the same view loaded successfully and returned an empty bucket list. The empty result still proved authorization for the list operation.

The user's group membership and the group's attached policy were verified independently. This project did not perform a write operation, so it does not claim a tested write-denial result.

## Security decisions

- Permissions were assigned to a group to support centralized access management.
- The AWS-managed policy limited the exercise to S3 read-only actions.
- Verification was limited to the exact action observed rather than inferring untested permissions.

## Cleanup

The temporary IAM user and group were deleted, and both resource lists were verified as empty.

## Key takeaways

- Group membership allows consistent permissions without duplicating policies across users.
- Authorization tests should identify the exact API action being exercised.
- A successful empty list is evidence of access even when no resources are returned.

[Back to project index](../../README.md)
