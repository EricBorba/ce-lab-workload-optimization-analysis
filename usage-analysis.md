# Workload Usage Pattern Analysis

## Observed Data (2026-06-03)
- dev-backend: 0.65% CPU average (single data point, instance just launched)
- dev-frontend: 0.20% CPU average (single data point, instance just launched)
- Both instances measured at 10:47 UTC+2 — effectively idle since launch

## Observations
- Dev instances show near-zero CPU utilization (<1%) since launch
- No business-hours vs off-hours pattern detectable yet (insufficient history)
- In a real scenario, 2-4 weeks of data would show ~25-40% CPU during business hours and <2% off-hours

## Active Window (projected based on dev workload pattern)
- **Business hours:** Monday-Friday 8:00 AM - 7:00 PM (local time)
- **Idle hours:** Weeknights + full weekends = ~76% of total hours


## Scheduling Savings Calculator

Instance: dev-frontend (t3.micro)
  On-Demand rate: $0.0104/hr
  Hours/month (24x7): 730
  Monthly cost (always on): $7.59

  Business hours only: Mon-Fri 8AM-7PM = 11hrs x 22 days = 242 hrs
  Monthly cost (scheduled): $2.52
  Monthly SAVINGS: $5.07 (67%)

Instance: dev-backend (t3.micro)
  On-Demand rate: $0.0104/hr
  Hours/month (24x7): 730
  Monthly cost (always on): $7.59

  Business hours only: 242 hrs
  Monthly cost (scheduled): $2.52
  Monthly SAVINGS: $5.07 (67%)

TOTAL MONTHLY SAVINGS: $10.14
ANNUAL SAVINGS: $121.68
