---
tags: [data_analysis, mysql, sql_query, mlm_pyramid, hierarchy_analysis, financial_forensics, website_reconstruction]
tools: [mysql, honglian_netju, sql, excel]
category: data_analysis
difficulty: medium
source: 2024FIC_finals
date: 2026-05-05
verified: true
---
# Title: 2024FIC Finals - Data Analysis (9 Questions)

## Problem
After reconstructing the "鲸易元MALL管理系统" website, analyze the database to answer questions about members, hierarchy, orders, and financial transactions in an MLM (传销) scheme.

## Prerequisites
- Website must be reconstructed first (see server forensics writeup)
- MySQL database accessible via Docker container on port 13306
- Admin backend accessible via browser

## Solution Steps

### Q1: Members with "总代" (Top Agent) level count
Admin backend → Member Management → filter by level "总代".
→ **248**

### Q2: Total hierarchy depth (using 推荐人id as parent)
Export member data from admin backend. Use 弘联网钜 (Honglian NetJu) data analysis tool:
1. Import as organizational structure
2. Select **strict mode**
3. Tool calculates max depth automatically
→ **53**

### Q3: Downstream member count for member sgl01
Filter member ID = sgl01 in 网钜, check downstream count.
→ **18001**

### Q4: Total recharge amount for sgl01's downstream members
From MySQL `member_money` table (contains recharge totals). Cross-reference with `member` table for hierarchy.
Export both tables → import into 网钜 with organizational template.
→ **8704119** (元/RMB, note: database stores without decimal places)

### Q5: Paid order count
Admin backend → Order Management → filter status = "已支付" (paid).
→ **31760**

### Q6: Total payment amount for paid orders
```sql
SELECT SUM(pay_money) FROM `doing_order` WHERE is_pay=1
```
→ **71979976** (元/RMB, note two decimal places in raw data)

### Q7: Bank card records in withdrawal account management
Admin backend → Withdrawal Management → Account Management → bank card records.
→ **6701**

### Q8: Successful withdrawal record count
Withdrawal Management → filter status = "打款成功".
→ **8403**

### Q9: Total withdrawal amount for successful payouts
```sql
SELECT SUM(need_give_money) FROM `member_deal` WHERE deal_status = 4
```
(deal_status = 4 means successful payout)
→ **10067655** (元/RMB)

## Key Takeaways
- **弘联网钜 (NetJu)**: Purpose-built for MLM hierarchy analysis — imports member data, calculates depth, downstream counts, financial aggregations automatically
- **Strict mode in NetJu**: Required for accurate hierarchy analysis with 推荐人id as parent
- **SQL for financial queries**: When admin UI doesn't show totals, query database directly
- **Key tables**:
  - `sys_user` — admin accounts
  - `member` — member info, hierarchy (推荐人id)
  - `member_money` — recharge/balance data
  - `doing_order` — orders (is_pay=1 for paid)
  - `member_deal` — withdrawal records (deal_status=4 for success, need_give_money for amount)
- **Decimal precision**: Database may store amounts without trailing zeros; verify decimal places
- **Member count**: Total 52908 members visible in admin backend after reconstruction

## Answer
Q1: 248
Q2: 53
Q3: 18001
Q4: 8704119
Q5: 31760
Q6: 71979976
Q7: 6701
Q8: 8403
Q9: 10067655
