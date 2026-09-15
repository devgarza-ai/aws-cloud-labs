# Lab 02 — Amazon EC2 Launch, SSH, and Cleanup

## Overview

This project provisions an Amazon Linux instance, restricts administrative access, validates encrypted block storage, connects from a local Linux environment, and verifies the full resource lifecycle. It also records a practical SSH troubleshooting case caused by an incorrect private-key filename.

## Project profile

| Item | Configuration |
| --- | --- |
| Region | US East (Ohio), `us-east-2` |
| AMI | Amazon Linux 2023, 64-bit x86 |
| Instance type | `t3.micro` |
| Network | Existing default VPC; public IPv4 assigned |
| Security group | SSH on TCP 22 from one administrator `/32` address |
| Root volume | 8 GiB gp3; encrypted with the default EBS key |
| Volume lifecycle | Delete on termination enabled |
| Authentication | RSA key pair; PEM format |

No HTTP or HTTPS inbound rules were added.

## Implementation

1. Launched one Amazon Linux 2023 instance in the default VPC.
2. Created a dedicated security group with SSH restricted to the current administrator address.
3. Enabled encryption on the 8 GiB gp3 root volume and retained delete-on-termination behavior.
4. Waited for all EC2 status checks to pass.
5. Moved the private key into the local WSL SSH directory and limited it to owner-read permissions.
6. Connected as `ec2-user` and inspected the login identity, hostname, and operating-system release.

```bash
chmod 400 ~/.ssh/INSTANCE_KEY.pem
ssh -i ~/.ssh/INSTANCE_KEY.pem ec2-user@INSTANCE_PUBLIC_IP
```

## Troubleshooting

The first SSH command referenced a misspelled key filename (`ket` instead of `key`). SSH reported that the identity file did not exist and then failed public-key authentication.

The diagnostic sequence was:

1. Read the first specific error instead of treating the final authentication message as the root cause.
2. Compared the command path with the output of `ls -l`.
3. Corrected the filename and retried the connection.
4. Matched the server's ED25519 host fingerprint to the EC2 system log before accepting the host key.

The corrected command opened the Amazon Linux shell successfully.

## Validation

```bash
whoami
hostname
cat /etc/os-release
```

| Check | Result |
| --- | --- |
| EC2 health | All status checks passed |
| Remote identity | `ec2-user` |
| Operating system | Amazon Linux 2023 |
| Private-key permissions | Owner read only (`400`) |
| Host verification | SSH fingerprint matched the EC2 system log |

## Cleanup

The instance was terminated, and the root EBS volume was verified as absent on the independent Volumes page. The lab security group and EC2 key-pair registration were deleted, leaving only the account's default network resources.

Deleting the EC2 key-pair registration does not remove a private-key file stored locally.

## Key takeaways

- The AMI defines the machine image, while the instance type defines compute capacity.
- A `/32` security-group source limits SSH access to one IPv4 address.
- The earliest concrete error is often the fastest path to the root cause.
- Instance termination and EBS deletion should be verified separately.

[AWS documentation: EC2 connection prerequisites](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connection-prereqs-general.html)  
[AWS documentation: Preserving EBS volumes on termination](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/preserving-volumes-on-termination.html)

[Back to project index](../../README.md)
