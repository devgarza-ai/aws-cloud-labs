# Lab 01 — Private Amazon S3 Object Access

## Overview

This project demonstrates private-by-default object storage and controlled temporary sharing. An S3 object remained inaccessible through its ordinary URL while a presigned URL provided scoped, time-limited access without changing the bucket's public-access settings.

## Project profile

| Item | Configuration |
| --- | --- |
| Region | US East (Ohio), `us-east-2` |
| Service | Amazon S3 |
| Bucket type | General purpose |
| Object Ownership | Bucket owner enforced; ACLs disabled |
| Public access | Block Public Access enabled; no bucket policy |
| Encryption | SSE-S3 |
| Versioning | Disabled |
| Storage class | S3 Standard |

## Implementation

1. Created a private S3 bucket and confirmed that it contained no objects.
2. Uploaded a 38-byte text object named `s3-practice.txt`.
3. Requested the object's ordinary HTTPS URL and received `AccessDenied`.
4. Generated a presigned URL and confirmed that the object opened in a private browser window.
5. Rechecked the bucket permissions to confirm that Block Public Access remained enabled and no bucket policy had been added.

## Validation

| Check | Result |
| --- | --- |
| Object upload | One object uploaded successfully with zero failures |
| Ordinary object URL | Access denied under the private bucket configuration |
| Presigned URL | Object content loaded successfully |
| Bucket permissions after sharing | Block Public Access remained enabled; bucket policy remained absent |

The test confirmed successful presigned access, but it did not independently observe the URL's eventual expiration.

## Security decisions

- ACLs were disabled through bucket-owner-enforced Object Ownership.
- The bucket was never made public.
- Temporary access was granted with a presigned URL instead of a public bucket policy.
- The signed URL itself was treated as sensitive and was not retained in the repository.

## Cleanup

The object was deleted first, followed by the bucket. The final S3 bucket list contained no lab bucket.

## Key takeaways

- An object URL identifies a resource; it does not grant permission to read it.
- Presigned URLs provide temporary access using the permissions of the signing identity.
- Access validation should include both the expected denial path and the authorized path.

[AWS documentation: Sharing objects with presigned URLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ShareObjectPreSignedURL.html)

[Back to project index](../../README.md)
