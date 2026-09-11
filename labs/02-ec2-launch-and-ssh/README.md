# Lab 02 — Amazon EC2 Launch, SSH, and Cleanup

**Author:** DevGarza  
**Date:** September 8, 2026 (Pacific)  
**Method:** Guided practice in my own AWS account using the AWS Management Console and WSL Ubuntu  
**Status:** Completed; cloud resources created for the lab cleaned up

## Goal and learning context

Launch a Linux virtual server, configure access and storage, connect from my computer using SSH, verify the operating system, troubleshoot a connection failure, and remove the lab resources.

I watched the EC2 demonstration in AWS Educate Introduction to Cloud 101, Module 4: AWS Core Services. For my own exercise, I used Amazon Linux 2023, a `t3.micro` instance, the existing default VPC, and encrypted gp3 storage. The instructor's custom VPC was part of the demonstration; I did not create a custom VPC during this lab.

## Configuration

| Setting | Value used |
| --- | --- |
| Instance name | `devgarza-ec2-lab` |
| Region | US East (Ohio), `us-east-2` |
| Instance count | 1 |
| AMI | Amazon Linux 2023, 64-bit x86 |
| Instance type | `t3.micro`; 2 vCPUs and 1 GiB memory |
| VPC | Existing default VPC; `172.31.0.0/16` |
| Subnet / AZ selection | No preference at launch |
| Assigned Availability Zone | `us-east-2b` |
| Auto-assign public IPv4 | Enabled |
| Security group | `devgarza-ec2-sg` |
| Inbound rule | SSH, TCP port 22, My IP only (`/32`) |
| HTTP / HTTPS inbound rules | Not added |
| Key pair | `devgarza-ec2-key`; RSA; PEM format selected |
| Actual local key filename | `devgarza-ec2-key.pem.txt` |
| Root disk | EBS, 8 GiB, gp3, `/dev/xvda` |
| Disk performance settings | 3,000 IOPS; 125 MiB/s throughput |
| EBS encryption | Enabled using the default `aws/ebs` key |
| Delete on termination | Yes |
| Additional volumes / file systems | None |

## Launch and connection

1. Opened EC2, chose Launch instance, entered the instance name, and selected the AMI and instance type.
2. Created the RSA key pair and saved the private key locally.
3. Used the default VPC and enabled an automatically assigned public IP.
4. Created the lab security group with SSH access limited to My IP.
5. Configured the root EBS volume and enabled encryption before launch. Delete on termination was set to Yes.
6. Launched the instance and waited for all **3/3 status checks** to pass.
7. Placed the saved private key in my WSL `~/.ssh` directory and restricted its permissions.
8. Connected as `ec2-user`, then checked the login identity, hostname, and operating system.

The saved file had an extra `.txt` suffix. SSH successfully used it once the command referenced its exact filename.

```bash
chmod 400 ~/.ssh/devgarza-ec2-key.pem.txt
ls -l ~/.ssh/devgarza-ec2-key.pem.txt
```

The permission listing showed `-r--------`. AWS documents this owner-read-only permission setting for the private key. [AWS connection prerequisites](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connection-prereqs-general.html)

The connection command below substitutes `INSTANCE_PUBLIC_IP` for the address used during the lab. The original instance has been terminated.

```bash
ssh -i ~/.ssh/devgarza-ec2-key.pem.txt ec2-user@INSTANCE_PUBLIC_IP
```

## Troubleshooting: SSH could not find my key

The first command used `devgarza-ec2-ket.pem.txt`, with **ket** instead of **key**. SSH reported that the identity file was not accessible because there was no such file, then ended with:

```text
Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
```

I checked the filename against `ls`, corrected the typo, and retried. The Amazon Linux banner and remote shell prompt appeared.

I also compared the server's ED25519 SHA256 host fingerprint in the SSH prompt with the corresponding entry in the AWS system log. They matched. The public IP used for connecting and the private IP embedded in the server's hostname described different addresses for the same instance; that difference did not cause this failure.

The host fingerprint identifies the server's host key. The RSA key pair selected at launch was used to authenticate my login. AWS explains how to locate host fingerprints under `BEGIN SSH HOST KEY FINGERPRINTS` in the system log. [AWS fingerprint verification](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connection-prereqs-general.html)

**Troubleshooting lesson:** Read the first specific error and verify the file path. Correcting the missing-key path resolved this connection attempt.

## Linux verification

I ran these commands after the successful login:

```bash
whoami
hostname
cat /etc/os-release
```

| Command | Observed output |
| --- | --- |
| `whoami` | `ec2-user` |
| `hostname` | `ip-172-31-28-136.us-east-2.compute.internal` |
| `cat /etc/os-release` | `PRETTY_NAME="Amazon Linux 2023.12.20260831"` |

Successful SSH session and command results (evidence reviewed privately)

## Cleanup and proof

1. Inspected the attached EBS volume and recorded its volume ID.
2. Terminated `devgarza-ec2-lab` and confirmed the state **Terminated**.
3. Opened the separate EBS Volumes page in Ohio. The unfiltered list showed no volumes in the region, and searching for the recorded volume ID returned no matching volumes.
4. Deleted `devgarza-ec2-sg`. The success banner appeared; after refresh, only the default security group remained.
5. Deleted the EC2 key-pair registration `devgarza-ec2-key`. The console confirmed one deletion and no key pairs to display.

| Resource or check | Final observed state | Evidence |
| --- | --- | --- |
| Encrypted root disk configuration | 8 GiB gp3; encryption enabled; Delete on termination Yes | Storage configuration (evidence reviewed privately) |
| EC2 instance | Terminated | Console screenshot reviewed during the session |
| Root EBS volume | No matching volumes on the independent EBS Volumes page | Volume search (evidence reviewed privately) |
| Lab security group | Deleted; default security group remained | Success banner and refreshed list reviewed during the session |
| EC2 key-pair registration | Deleted; list empty | Key-pair cleanup (evidence reviewed privately) |

The empty Storage tab on a terminated instance only showed that no disks were attached. Checking the separate Volumes page provided additional evidence that the root disk itself was gone. EBS deletion at instance termination is controlled by each volume's `DeleteOnTermination` setting. [AWS EBS persistence documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/preserving-volumes-on-termination.html)

Deleting the EC2 key-pair registration does not delete the local private-key file. Local key-file removal was not recorded; the private key is excluded from this repository.

## Activity milestone

At the end of the session, the AWS Console Home **Explore AWS** widget marked **Launch an instance using EC2** as **Completed**. It showed **1 of 5 activities completed** and **$20 of $100 credits earned**. The Cost and usage widget showed **$120.00 credits remaining** and **$0.00 current-month cost** at that time. These are the values displayed in my screenshot, not a calculation of the final billed lab cost.

## Reflection

This exercise connected AWS console configuration with Linux commands on a running cloud server. I practiced choosing an AMI, restricting SSH access, enabling disk encryption, checking status checks, and verifying a remote session. Resolving the filename typo gave me a concrete troubleshooting example. Verifying the separate EBS list helped me understand why cleanup requires checking related resources as well as the instance.

## Review questions

1. What did the AMI determine, and what did the instance type determine?
2. What did the SSH rule's My IP `/32` source restrict?
3. Why did the first SSH attempt fail?
4. What is the difference between the server's host fingerprint and my login key pair?
5. Why did I check the EBS Volumes page after terminating the instance?

[Back to lab index](../../README.md)
