# 📝 GnuCash PostgreSQL Setup: Understanding Unix vs Database Users

## 📅 Date
March 20, 2026

## ✍️ Author
Thomas Boimah

---

## 🎯 Overview

This document explains the critical distinction between Unix shell users and PostgreSQL database users, and how this applies to our GnuCash setup on webschool.sjcompute.org.

---

## 🎯 The Core Distinction

| Unix Shell Users | PostgreSQL Users |
|----------------|-----------------|
| Access the operating system | Access the database |
| Use SSH (port 22) | Use PostgreSQL (port 5432) |
| Authenticate with SSH keys/passwords | Authenticate with DB password |
| Stored in /etc/passwd, /etc/shadow | Stored in pg_roles |
| Run system commands | Perform DB operations only |

---

## 🧠 Key Insight

PostgreSQL users are not people — they are roles with permissions.

---

## 🏠 Analogy (Refined)

- Unix user = Key to the front door (SSH, port 22)
- PostgreSQL user = Key to a window (port 5432)

---

## 🏢 Our Current Setup

### Unix Users (SSH)
- tboimah
- jelkner
- dcammue
- zOnny
- postgres

### PostgreSQL Roles

#### Login Roles
- tboimah
- jelkner
- dcammue
- svaye
- ved
- devesh
- vrishin
- klarios

#### Non-Login Role
- gnucash (NOLOGIN)

---

## 🎯 The Recommended GnuCash Setup

| Role | Type | Purpose |
|------|------|--------|
| gnucash | NOLOGIN | Owns all databases |
| Users | LOGIN | Access data via membership |

---

## 🔍 Why gnucash Must Be NOLOGIN

- Security: Cannot login, cannot be hacked
- Separation: Ownership ≠ usage
- Flexibility: Users can change without affecting data
- Best Practice: RBAC model

---

## 🔗 How Access Works

1. User logs in with PostgreSQL credentials
2. PostgreSQL checks pg_hba.conf
3. Verifies password
4. Checks role membership
5. Inherits permissions from gnucash

---

## ❌ Common Mistake

Using SSH keys for GnuCash authentication is incorrect.

---

## ✅ Correct GnuCash Connection

Host: webschool.sjcompute.org  
Port: 5432  
Database: jetroweb  
Username: your_postgres_user  
Password: your_postgres_password  

---

## 🎯 Key Takeaways

- Unix users ≠ Database users
- PostgreSQL roles control access
- gnucash is a non-login owner role
- Users inherit permissions
- GnuCash connects to PostgreSQL, not Unix

---

## 🚀 Status

Ready to share with team
