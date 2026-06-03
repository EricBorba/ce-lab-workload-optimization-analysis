# Workload Optimization Report

## Executive Summary
Total identified monthly savings: $14.14 across scheduling and storage cleanup.

---

## 1. Instance Scheduling (Priority: HIGH)

| Instance     | Type     | Current Cost/mo | Scheduled Cost/mo | Savings/mo |
|--------------|----------|-----------------|-------------------|------------|
| dev-frontend | t3.micro | $7.59           | $2.52             | $5.07      |
| dev-backend  | t3.micro | $7.59           | $2.52             | $5.07      |
| **Subtotal** |          | **$15.18**      | **$5.04**         | **$10.14** |

- On-Demand rate: $0.0104/hr (t3.micro, us-east-1)
- Business hours schedule: Mon-Fri 8AM-7PM = 11hrs x 22 days = 242 hrs/month
- Always-on hours: 730 hrs/month
- Savings: 67% reduction

**Implementation:** EventBridge + Lambda scheduler (deployed in this lab)

---

## 2. Orphaned EBS Volumes (Priority: MEDIUM)

| Volume ID             | Size (GB) | Monthly Cost | Action                         |
|-----------------------|-----------|--------------|--------------------------------|
| vol-021ed2e5f96ac6a0c | 50        | $4.00        | Delete after backup verification |
| **Subtotal**          |           | **$4.00**    |                                |

- gp3 pricing: $0.08/GB/month
- Volume created 2026-06-03, never attached to any instance

---

## 3. Old Snapshots (Priority: LOW)

| Count              | Total Size (GB) | Monthly Cost | Action |
|--------------------|-----------------|--------------|--------|
| 0 snapshots > 90d  | 0 GB            | $0.00        | None required |

No snapshots older than 90 days found in this account.

---

## Prioritized Recommendations

1. **Enable instance scheduling** — $10.14/mo savings, zero performance impact
2. **Delete orphaned volume** vol-021ed2e5f96ac6a0c — $4.00/mo, verify no dependencies first
3. **Implement snapshot lifecycle policy** — prevent future accumulation going forward

---

## Annual Projected Savings: $169.68
