# Lab 05 — IAM Group-Based S3 Read-Only Access

**Author:** DevGarza
**Date:** September 10, 2026 (Pacific)
**Scope:** IAM global; access verified in S3
**Status:** Completed; lab identities deleted

## Goal and configuration

Create a user and group, grant S3 read-only access at the group level, verify inheritance, and remove the identities.

| Resource | Value |
| --- | --- |
| Group | `devgarza-s3-readonly-lab` |
| User | `devgarza-iam-demo-user` |
| Group policy | AWS managed `AmazonS3ReadOnlyAccess` |

## Verification

The first bucket-list attempt named a missing `s3:ListAllMyBuckets` permission. After the group permission became effective, the list loaded successfully and showed no buckets. The user belonged to the group and the policy was attached there.

Initial denial (evidence reviewed privately) · Authorized list (evidence reviewed privately) · Membership (evidence reviewed privately) · Policy (evidence reviewed privately)

This lab did **not** record a bucket-creation attempt, so no write-denial test is claimed.

## Cleanup and lessons

I deleted the user and group and verified both lists were empty: user (evidence reviewed privately) · group (evidence reviewed privately).

- Group policies avoid duplicating permissions on each user.
- A successful empty list still proves list authorization.
- Evidence should identify the exact tested action.

[Back to lab index](../../README.md)
