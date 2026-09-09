# Evidence Index

These selected screenshots came from my own AWS account and terminal during the September 8, 2026 labs. The repository copies have descriptive filenames; their image bytes are unchanged.

| Repository file | Original screenshot timestamp, Pacific | What it shows |
| --- | --- | --- |
| [S3 upload](labs/01-s3-private-access/images/01-object-uploaded.png) | 19:51:51 | One successful 38-byte upload |
| [S3 ordinary URL](labs/01-s3-private-access/images/02-access-denied.png) | 19:53:30 | AccessDenied response |
| [S3 permissions](labs/01-s3-private-access/images/03-private-permissions.png) | 20:01:09 | Block all public access On; no bucket policy |
| [S3 cleanup](labs/01-s3-private-access/images/04-bucket-deleted.png) | 20:02:33 | Successful bucket deletion and empty list |
| [EC2 root disk](labs/02-ec2-launch-and-ssh/images/01-encrypted-root-volume.png) | 20:37:20 | Encrypted 8 GiB gp3 volume; Delete on termination Yes |
| [EC2 SSH verification](labs/02-ec2-launch-and-ssh/images/02-ssh-and-linux-verification.png) | 20:55:43 | Successful login and Linux command results |
| [EBS cleanup](labs/02-ec2-launch-and-ssh/images/03-ebs-volume-deleted.png) | 21:07:59 | No matching volumes for the lab disk |
| [Key-pair cleanup](labs/02-ec2-launch-and-ssh/images/04-key-pair-deleted.png) | 21:09:25 | Successful deletion; no key pairs displayed |

Additional screenshots reviewed during the session confirmed presigned access in Incognito, instance termination, security-group deletion, and the completed EC2 activity. The presigned-access screenshot is excluded because the address bar contains a signed URL. Full console screenshots containing account identifiers were also omitted from this selected set. No private-key contents are included.
