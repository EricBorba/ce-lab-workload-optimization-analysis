# Lab M7.04 - Workload Optimization Analysis

![Amazon EC2](https://img.shields.io/badge/Amazon%20EC2-%23FF9900.svg?style=for-the-badge&logo=amazonec2&logoColor=white)
![AWS Lambda](https://img.shields.io/badge/AWS%20Lambda-%23FF9900.svg?style=for-the-badge&logo=awslambda&logoColor=white)
![Amazon EventBridge](https://img.shields.io/badge/Amazon%20EventBridge-%23FF9900.svg?style=for-the-badge&logo=amazonaws&logoColor=white)
![Amazon CloudWatch](https://img.shields.io/badge/Amazon%20CloudWatch-%23FF9900.svg?style=for-the-badge&logo=amazoncloudwatch&logoColor=white)
![Amazon EBS](https://img.shields.io/badge/Amazon%20EBS-%23FF9900.svg?style=for-the-badge&logo=amazonaws&logoColor=white)
![AWS IAM](https://img.shields.io/badge/AWS%20IAM-%23FF9900.svg?style=for-the-badge&logo=amazonaws&logoColor=white)
![Python](https://img.shields.io/badge/Python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![Bash](https://img.shields.io/badge/Bash-%23121011.svg?style=for-the-badge&logo=gnubash&logoColor=white)

## What I Did
- Set up a development environment with two tagged EC2 instances (t3.micro)
- Analyzed CPU utilization — confirmed near-zero usage (<1%) on freshly launched dev instances
- Built a Lambda + EventBridge scheduler for automatic start/stop during business hours
- Audited EBS volumes and snapshots for orphaned resources
- Created a prioritized optimization report with projected savings

## Key Findings
- Scheduling dev instances saves $10.14/month (67% reduction) with zero performance impact
- Found 1 unattached EBS volume (50 GB) wasting $4.00/month
- No snapshots older than 90 days found
- Total annual savings opportunity: $169.68

## Architecture

```
EventBridge (cron MON-FRI)
  └── StopDevInstances  (23:00 UTC / 7 PM EST)  ──┐
  └── StartDevInstances (12:00 UTC / 8 AM EST)  ──┤
                                                   ▼
                                         Lambda: InstanceScheduler
                                                   │
                                    tag:Schedule=business-hours
                                                   ▼
                                     EC2: dev-frontend, dev-backend
```

## Files

| File | Description |
|------|-------------|
| `scheduler.py` | Lambda function that starts/stops EC2 instances by tag |
| `usage-analysis.md` | CPU utilization analysis and scheduling savings calculation |
| `optimization-report.md` | Full prioritized workload optimization report |
| `response.json` | Lambda manual test response |

## Screenshots

| File | Description |
|------|-------------|
| `screenshots/cloudwatch-cpu-metrics.png` | CloudWatch CPU metrics loop — dev-backend (0.65%) and dev-frontend (0.20%) near-zero utilization |
| `screenshots/lambda-function-overview.png` | Lambda console — InstanceScheduler function overview with code source |
| `screenshots/lambda-eventbridge-triggers.png` | Lambda configuration — both EventBridge triggers (StopDevInstances, StartDevInstances) enabled |
| `screenshots/lambda-invoke-stop-test.png` | Lambda manual test — stop action successfully targeted both instances |
| `screenshots/ebs-unattached-volumes.png` | EBS audit — orphaned volume vol-021ed2e5f96ac6a0c (50 GB, gp3) identified |
| `screenshots/ebs-snapshots-audit.png` | Snapshot audit — no snapshots older than 90 days found |
| `screenshots/ebs-storage-waste-calculation.png` | Storage waste calculation — 50 GB unattached at $4.00/month |

## Resources Created

| Resource | ID / Name |
|----------|-----------|
| EC2 instance | i-055fff4339182c05b (dev-frontend) |
| EC2 instance | i-09ecc983e448deb14 (dev-backend) |
| EBS volume | vol-021ed2e5f96ac6a0c (orphaned-data-volume) |
| Lambda function | InstanceScheduler |
| IAM role | InstanceSchedulerRole |
| EventBridge rule | StopDevInstances |
| EventBridge rule | StartDevInstances |
