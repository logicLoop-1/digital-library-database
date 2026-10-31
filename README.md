# 📚 Digital Library Database

A relational database for a **Digital Library** system, as a project for my Database Management Systems (DBMS) course.

The database manages digital items (eBooks, eJournals, journal articles, software tools), tiered memberships with discounts, guest access, and access/revenue tracking. It is fully normalized to **3NF** and comes pre-loaded with a large sample dataset.

---

## ✨ Features

- **Membership tiers** (Bronze / Silver / Gold) with different annual fees, discount percentages, and free-item limits
- **Four access types:** Public, Member View Only, Member Download, Member Charged
- **Four item types:** eBook, eJournal, Journal Article, Software Tool
- **Tier-based pricing:** the same item can cost a different amount for each membership level
- **Hierarchical categories:** categories with sub-categories (self-referencing table)
- **Guest access** to public items only
- **Access tracking** with charged amount and money saved for each access
- **Sample queries** for search, usage statistics, revenue, and member savings

---

## 🗂️ Database Schema

The database has **11 tables**:

| # | Table | Purpose |
|---|-------|---------|
| 1 | `Membership` | Membership tiers: fee, discount %, free item limit |
| 2 | `Member` | Registered library members |
| 3 | `Guest` | Guest visitors |
| 4 | `Access_Type` | Access rules (public, chargeable, download, view-only) |
| 5 | `Item_Type` | eBook, eJournal, Journal Article, Software Tool |
| 6 | `Digital_Item` | Core table of all library items |
| 7 | `Item_Membership_Price` | Price of each item per membership tier (resolves M:M) |
| 8 | `Category` | Categories and sub-categories (self-referencing) |
| 9 | `Item_Category` | Links items to categories (resolves M:M) |
| 10 | `Access_Record` | Member access history, charges, and savings |
| 11 | `Guest_Access` | Guest access history |

> 💡 See `docs/ER_Diagram.png` for the entity-relationship diagram.

---

## 📊 Sample Data

| Table | Rows |
|-------|-----:|
| Membership | 3 |
| Member | 1,000 |
| Guest | 50 |
| Access_Type | 4 |
| Item_Type | 4 |
| Digital_Item | 2,000 |
| Item_Membership_Price | 1,500 |
| Category | 30 |
| Item_Category | 2,796 |
| Access_Record | 20,000 |
| Guest_Access | 200 |

---

## 🚀 How to Run

### Requirements
- Microsoft SQL Server (Express, Developer, or any edition)
- SQL Server Management Studio (SSMS), Azure Data Studio, or the `sqlcmd` command-line tool

### Option 1: Command line (sqlcmd)

```bash
sqlcmd -S localhost -U sa -P YourPassword -i digital_library_final.sql
```

### Option 2: SSMS / Azure Data Studio

1. Open `digital_library_final.sql`
2. Click **Execute** (or press `F5`)
3. The script creates the `DigitalLibrary` database, all tables, and inserts the sample data

> ⚠️ The script runs `CREATE DATABASE DigitalLibrary`. If the database already exists, drop it first or rename it in the script.

---

## 🔍 Sample Queries

The end of the script includes 5 ready-to-run queries:

| # | Query | Description |
|---|-------|-------------|
| 1 | **Keyword search** | Find items whose title matches a keyword (e.g. `Physics`) |
| 2 | **Access count** | Number of times an item was accessed within a date range |
| 3 | **Category revenue** | Paid accesses and total revenue per category |
| 4 | **Member savings** | Total paid vs. total saved by each member thanks to their membership discount |
| 5 | **Item details** | Full details of an item: type, access rules, category, and description |

Example: search the library by keyword

```sql
SELECT DI.Item_ID, DI.Title, IT.Type_Name, AT.Access_Name, DI.Publish_Date
FROM Digital_Item DI
JOIN Item_Type   IT ON DI.Item_Type_ID   = IT.Item_Type_ID
JOIN Access_Type AT ON DI.Access_Type_ID = AT.Access_Type_ID
WHERE DI.Title LIKE '%Physics%';
```

---

## 🧠 Design Highlights

- **3NF normalization:** no repeating groups or transitive dependencies
- **Junction tables** (`Item_Membership_Price`, `Item_Category`) with composite primary keys to resolve many-to-many relationships
- **Self-referencing foreign key** in `Category` for unlimited sub-category levels
- **Foreign key constraints** across all related tables to keep data consistent
- Access rules stored as `BIT` flags (`Is_Public`, `Is_Chargeable`, `Download_Allowed`, `View_Only`)

---

## 📁 Repository Structure

```
digital-library-database/
├── README.md
├── digital_library_final.sql
├── docs/
│   └── ER_Diagram.png
└── .gitignore
```

---

## 🛠️ Tech Stack

- **Database:** Microsoft SQL Server
- **Language:** T-SQL

---

## 🔮 Possible Future Improvements

- Add stored procedures for borrowing/accessing items and calculating discounts
- Add triggers to automatically enforce free-item limits
- Add indexes to speed up search queries
- Build a simple front-end (web or desktop) on top of the database

---

## 👤 Author

**Your Name**
- GitHub: [@your-username](https://github.com/your-username)
- Course: Database Management Systems (DBMS)

---

## 📄 License

This project is for educational purposes. Licensed under the [MIT License](LICENSE).