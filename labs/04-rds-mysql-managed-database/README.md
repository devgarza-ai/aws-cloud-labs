# Lab 04 — Amazon RDS for MySQL

**Author:** DevGarza
**Date:** September 10, 2026 (Pacific)
**Region:** US East (Ohio), `us-east-2`
**Status:** Completed; all temporary resources removed

## Goal and configuration

Create a small managed MySQL database, inspect its managed configuration, confirm availability, and delete it without retaining database copies.

| Setting | Value |
| --- | --- |
| Method / engine | Easy create / MySQL Community |
| Identifier | `devgarza-rds-demo-mysql` |
| Instance | `db.t4g.micro`; 2 vCPUs, 1 GiB |
| Storage | 20 GiB |
| Username | `admin` |
| Credentials | Auto-generated self-managed password; never published |
| AZ / VPC | `us-east-2c` / default VPC |
| Multi-AZ | No |
| Internet gateway / IAM DB auth | Disabled / disabled |

Creation configuration (evidence reviewed privately)

The console displayed an estimated `$0.019 USD/hour` rate at creation; this was not a final bill. The database reached **Available** with zero connections. Its live endpoint screenshot is excluded.

## Cleanup

I cleared **Create final snapshot** and **Retain automated backups**, entered `delete me`, and deleted the database. I then verified the database, manual snapshot, current backup, and retained-backup lists; removed the DB subnet group; and deleted `rds-monitoring-role`.

Database deleted (evidence reviewed privately) · Snapshots empty (evidence reviewed privately) · Subnet group deleted (evidence reviewed privately) · Monitoring role deleted (evidence reviewed privately)

## Lessons

- RDS manages infrastructure, while I still choose connectivity, credentials, backups, and deletion behavior.
- **Available** means ready, not that an application connected.
- Snapshots and retained backups can outlive a DB instance, so cleanup needs independent checks.

[Back to lab index](../../README.md)
