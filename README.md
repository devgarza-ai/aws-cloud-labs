# AWS Cloud Labs

My hands-on AWS learning journal, beginning with guided console labs from AWS Educate **Introduction to Cloud 101 — Module 4: AWS Core Services**.

I perform the exercises in my own AWS account, verify the results, record troubleshooting, and clean up the resources. These introductory exercises were completed with guidance from Stratus (ChatGPT). The documentation describes my actual configuration and results.

## Completed labs

| Lab | What I practiced | Status |
| --- | --- | --- |
| [01 — S3 private object access](labs/01-s3-private-access/README.md) | Object upload, private access, presigned URLs, and bucket cleanup | Completed; object and bucket deleted |
| [02 — EC2 launch and SSH](labs/02-ec2-launch-and-ssh/README.md) | Linux instance launch, security groups, encrypted EBS, SSH troubleshooting, and cleanup | Completed; instance, volume, lab security group, and EC2 key-pair registration removed |

Both labs were completed on **September 8, 2026**, using **US East (Ohio), us-east-2**.

## Evidence and reflection

Each lab includes its settings, work completed, verification results, lessons learned, and selected original screenshots. The EC2 write-up includes the actual SSH filename typo and the successful fix.

The [September 8 session log](sessions/2026-09-08.md) records the study session and AWS's completed **Launch an instance using EC2** activity, which awarded **$20 in credits**.

## Next study session

Continue with the **VPC demonstration** and the remaining core-services demos. Choose the next hands-on exercise after reviewing the demonstration.

Private keys and signed access URLs are excluded from this repository. The ignore rules include files ending in `.pem.txt`, the filename format encountered during the EC2 exercise.
