# Structured SQL Queries

## 1. Gold PPO Deductible

**Question:**
What's the deductible on the Gold PPO plan?

**SQL:**

```sql
SELECT plan_name, annual_deductible
FROM plans
WHERE plan_name = 'Gold PPO';
```

**Output:**

| plan_name | annual_deductible |
| --------- | ----------------: |
| Gold PPO  |              2000 |

---

## 2. Pending Claims for Member M1001

**Question:**
How many claims are pending for member M1001?

**SQL:**

```sql
SELECT COUNT(*) AS pending_claims
FROM claims
WHERE member_id = 'M1001'
  AND status = 'Pending';
```

**Output:**

| pending_claims |
| -------------: |
|              1 |

---

## 3. Plans Under $400

**Question:**
Which plans have a monthly premium under $400?

**SQL:**

```sql
SELECT plan_name, monthly_premium
FROM plans
WHERE monthly_premium < 400;
```

**Output:**

| plan_name  | monthly_premium |
| ---------- | --------------: |
| Silver HMO |             300 |
| Bronze HMO |             150 |

---

## 4. Claims Joined With Plan Information

**Question:**
Show claims along with their associated plan.

**SQL:**

```sql
SELECT
    c.claim_id,
    c.member_id,
    c.procedure,
    c.claim_amount,
    c.status,
    p.plan_name,
    p.coverage_type
FROM claims c
JOIN plans p
    ON c.plan_id = p.plan_id;
```

**Output:**

| claim_id | member_id | procedure | claim_amount | status   | plan_name  | coverage_type |
| -------- | --------- | --------- | -----------: | -------- | ---------- | ------------- |
| C1001    | M1001     | X-ray     |          250 | Pending  | Gold PPO   | PPO           |
| C1002    | M1001     | Surgery   |         1200 | Approved | Gold PPO   | PPO           |
| C1003    | M1002     | X-ray     |          150 | Denied   | Silver HMO | HMO           |
| C1004    | M1002     | Surgery   |          900 | Approved | Silver HMO | HMO           |
| C1005    | M1003     | X-ray     |           50 | Pending  | Bronze HMO | HMO           |

---

## 5. Most Frequently Claimed Procedures

**Question:**
What are the most frequently claimed procedures?

**SQL:**

```sql
SELECT
    procedure,
    COUNT(*) AS claim_count
FROM claims
GROUP BY procedure
ORDER BY claim_count DESC
LIMIT 2;
```

**Output:**

| procedure | claim_count |
| --------- | ----------: |
| X-ray     |           3 |
| Surgery   |           2 |
