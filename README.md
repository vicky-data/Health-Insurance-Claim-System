<h1 align="center">🏥 Health Insurance Claim System</h1>

<p align="center">
  <b>Relational Database Design • SQL • MySQL Workbench • Healthcare Analytics</b><br>
  Designing and implementing a real-world Health Insurance Claims Database for analytics and reporting.
</p>

---

## 📘 Overview

This project focuses on building a **real-world health insurance claims database** that stores information about members, providers, claims, addresses, claim statuses, claim payments, and coverage.  
We designed and implemented a **relational database using MySQL Workbench**, generating seven interrelated tables, each populated with approximately **100 rows** of realistic healthcare data.

---

## 🎯 Project Goal

The main goal of this project is to:

- Enable **healthcare insurance officials** to generate analytical reports and insights.  
- Build a **normalized, efficient relational database** for claim management and performance reporting.  
- Understand the **end-to-end database design process**, from conceptual ER modeling to SQL implementation and optimization.  
- Create a foundation for **data visualization and business intelligence** use cases in healthcare insurance.

---

## 🧩 Entity Relationship (ER) Diagram

<p align="center">
  <img src="https://github.com/user-attachments/assets/6290ed49-7d1d-4ead-8e1a-50d33d02a93c" width="900" alt="ER Diagram" />
</p>

---

## 🗄️ Database Schema

### 📂 Database Structure — Health Insurance Claim System

---

#### 🏠 **Address**
| Column | Data Type | Key | Description |
|:--------|:-----------|:---:|:-------------|
| `address_id` | INT | PK | Unique ID for each address |
| `street_address` | VARCHAR(45) |  | Street information |
| `apartment_no` | INT |  | Apartment or flat number |
| `city` | VARCHAR(50) |  | City name |
| `county` | VARCHAR(45) |  | County or district |
| `country` | VARCHAR(45) |  | Country name |
| `zipcode` | INT |  | Zip or postal code |

---

#### 🩺 **Provider**
| Column | Data Type | Key | Description |
|:--------|:-----------|:---:|:-------------|
| `provider_id` | INT | PK | Unique provider identifier |
| `provider_first_name` | VARCHAR(45) |  | Provider’s first name |
| `provider_last_name` | VARCHAR(45) |  | Provider’s last name |
| `degree` | VARCHAR(45) |  | Medical qualification |
| `network` | VARCHAR(45) |  | Insurance network |
| `practice_name` | VARCHAR(45) |  | Practice or clinic name |
| `claim_id` | INT | FK → Claim | Associated claim record |
| `address_id` | INT | FK → Address | Linked address |
| `gender` | VARCHAR(45) |  | Provider’s gender |

---

#### 💰 **Claim_Payment**
| Column | Data Type | Key | Description |
|:--------|:-----------|:---:|:-------------|
| `claim_payment_id` | INT | PK | Payment record ID |
| `billed_amount` | VARCHAR(45) |  | Total billed amount |
| `approved_amount` | VARCHAR(45) |  | Amount approved by insurer |
| `copay_amount` | VARCHAR(45) |  | Member copayment |
| `coinsurance_amount` | VARCHAR(45) |  | Coinsurance share |
| `deductible_amount` | VARCHAR(45) |  | Deductible applied |
| `net_payment` | VARCHAR(45) |  | Final payable amount |
| `claim_id` | INT | FK → Claim | Related claim record |

---

#### 📑 **Coverage**
| Column | Data Type | Key | Description |
|:--------|:-----------|:---:|:-------------|
| `coverage_id` | INT | PK | Coverage plan ID |
| `member_id` | INT | FK → Member | Covered member |
| `coverage_name` | VARCHAR(45) |  | Plan name |
| `effective_date` | DATE |  | Start date of coverage |
| `term_date` | DATE |  | End date of coverage |

---

#### 👨‍👩‍👧 **Member**
| Column | Data Type | Key | Description |
|:--------|:-----------|:---:|:-------------|
| `member_id` | INT | PK | Member unique ID |
| `member_first_name` | VARCHAR(45) |  | First name |
| `member_last_name` | VARCHAR(45) |  | Last name |
| `address_id` | INT | FK → Address | Member address |
| `gender` | VARCHAR(45) |  | Gender |
| `member_dob` | DATE |  | Date of birth |
| `claim_id` | INT | FK → Claim | Associated claim |
| `coverage_id` | INT | FK → Coverage | Linked coverage |

---

#### 🧾 **Claim**
| Column | Data Type | Key | Description |
|:--------|:-----------|:---:|:-------------|
| `claim_id` | INT | PK | Claim unique ID |
| `status_id` | INT | FK → Status | Claim status |
| `date_of_service` | DATE |  | Date of service rendered |
| `received_date` | DATE |  | Date received by insurer |
| `add_by` | VARCHAR(45) |  | User who created claim |

---

#### 📊 **Status**
| Column | Data Type | Key | Description |
|:--------|:-----------|:---:|:-------------|
| `status_id` | INT | PK | Status unique ID |
| `claim_status` | VARCHAR(45) |  | Claim status (e.g., Paid, Denied, Pending) |
| `type` | VARCHAR(45) |  | Status category |

---

### 🔗 Relationship Summary

| Relationship | Type | Description |
|:--------------|:------:|:-------------|
| Address → Provider | 1 → Many | One address can belong to multiple providers |
| Address → Member | 1 → Many | One address can have multiple members |
| Member → Claim | 1 → 1 | Each member can have one claim |
| Claim → Claim_Payment | 1 → 1 | Each claim has one payment record |
| Claim → Status | 1 → 1 | Each claim has a single status |
| Member → Coverage | 1 → 1 | Each member has one coverage plan |

---

✨ **Design Principle:**  
This database is **normalized to 3NF**, ensuring minimal redundancy and efficient querying for analytical purposes in healthcare insurance claim management.


---

## 📊 Data Visualizations (Exploratory Data Analysis)

<p align="center">
  <img src="https://github.com/user-attachments/assets/35b28339-d6d1-48e5-a0fe-7b30bc3efa15" width="750" alt="EDA Visualization 1" />
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/7170fc46-874f-4289-8e5a-6fe31908636c" width="700" alt="EDA Visualization 2" />
</p>

---

## 🧾 SQL Queries and Reports

### 1️⃣ Top 20 Members by Approved Claim Amount (View)
Reports top 20 members with provider details and approved amounts greater than $2200.

```sql
USE health_insurance_claims_database;

CREATE VIEW top_20_claim_approved_customers AS
SELECT 
    m.member_first_name,
    m.member_last_name,
    s.claim_status,
    p.provider_first_name,
    p.provider_last_name,
    p.network,
    p.practice_name,
    cp.billed_amount,
    cp.approved_amount,
    cp.net_payment
FROM ((((member m
INNER JOIN claim c ON m.claim_id = c.claim_id)
INNER JOIN status s ON s.status_id = c.status_id)
INNER JOIN provider p ON p.claim_id = m.claim_id)
INNER JOIN claim_payment cp ON cp.claim_id = c.claim_id)
WHERE cp.approved_amount > 2200
ORDER BY cp.approved_amount DESC
LIMIT 20;

SELECT * FROM top_20_claim_approved_customers;
# 2️⃣ Members with Claims in Progress (Stored Procedure)
Lists all members whose claim status matches the provided status input.

sql
Copy code
USE health_insurance_claims_database;

DELIMITER //

CREATE PROCEDURE get_member_claim_status_data(IN claim_status_in VARCHAR(50))
BEGIN
  SELECT 
    m.member_first_name,
    m.member_last_name,
    m.member_dob,
    m.gender,
    m.claim_id,
    s.claim_status,
    s.type
  FROM ((member m
  INNER JOIN claim c ON m.claim_id = c.claim_id)
  INNER JOIN status s ON s.status_id = c.status_id)
  WHERE s.claim_status = claim_status_in
  ORDER BY m.member_first_name;
END //

DELIMITER ;

CALL get_member_claim_status_data('paid');
# 3️⃣ Members by Address Range (Indexing)
Fetches members with coverage plans and address IDs between 2000 and 5000.

sql
Copy code
USE health_insurance_claims_database;

SELECT DISTINCT 
    m.member_first_name,
    m.member_last_name,
    cp.coverage_name,
    m.address_id
FROM coverage cp
INNER JOIN member m ON m.member_id = cp.member_id
WHERE m.address_id BETWEEN 2000 AND 5000;

CREATE INDEX member_index ON member(address_id);

SELECT DISTINCT 
    m.member_first_name,
    m.member_last_name,
    cp.coverage_name,
    m.address_id
FROM coverage cp
INNER JOIN member m ON m.member_id = cp.member_id
WHERE m.address_id BETWEEN 2000 AND 5000;

DROP INDEX member_index ON member;


# ⚡ Performance Optimization Techniques
This project applies several SQL optimization techniques to improve query performance:

✅ Views — Simplify complex multi-join queries

✅ Stored Procedures — Centralize business logic and improve reusability

✅ Indexes — Speed up table lookups and filtering operations

# 🧠 Learnings & Takeaways
Deep understanding of healthcare insurance data relationships

Proficiency in MySQL Workbench schema design and normalization (3NF)

Hands-on experience with views, indexing, and stored procedures

Ability to generate business insights from healthcare claims data

# 🧑‍💻 Author
Vigneshwaran Palanisamy
Data Analyst | SQL Developer | Healthcare Data Enthusiast

