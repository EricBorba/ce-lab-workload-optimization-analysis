# Lab M7.04 - Workload Optimization Analysis

![Python](https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white)
![Bash](https://img.shields.io/badge/Bash-5.2-4EAA25?logo=gnubash&logoColor=white)
![AWS EC2](https://img.shields.io/badge/AWS-EC2%20t3.micro-FF9900?logo=amazonec2&logoColor=white)
![AWS Lambda](https://img.shields.io/badge/AWS-Lambda-FF9900?logo=awslambda&logoColor=white)
![Amazon EventBridge](https://img.shields.io/badge/AWS-EventBridge-FF4F8B?logo=amazonaws&logoColor=white)
![Amazon CloudWatch](https://img.shields.io/badge/AWS-CloudWatch-FF4F8B?logo=amazonaws&logoColor=white)
![Amazon EBS](https://img.shields.io/badge/AWS-EBS%20gp3-FF9900?logo=amazonaws&logoColor=white)
![AWS IAM](https://img.shields.io/badge/AWS-IAM-DD344C?logo=amazonaws&logoColor=white)

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
