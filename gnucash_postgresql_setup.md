# GnuCash PostgreSQL Setup: Understanding Unix vs Database Users

## Date
March 20, 2026

## Author
Thomas Boimah

---

## Overview

This document explains the distinction between Unix shell users and PostgreSQL database users, and applies those concepts to our GnuCash setup on webschool.sjcompute.org.

This demonstrates understanding of database concepts and system architecture.

---

## System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    WEBSCHOOL SERVER                         │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────────────┐      ┌─────────────────────────┐   │
│  │   UNIX USERS        │      │   POSTGRESQL USERS      │   │
│  │   (SSH - Port 22)   │      │   (Port 5432)           │   │
│  ├─────────────────────┤      ├─────────────────────────┤   │
│  │ • jelkner           │      │ • jelkner (login)       │   │
│  │ • tboimah           │      │ • tboimah (login)       │   │
│  │ • dcammue           │      │ • dcammue (login)       │   │
│  │ • zOnny             │      │ • svaye (login)         │   │
│  │ • svaye             │      │ • ved (login)           │   │
│  | • jkollie           |      │ • devesh (login)        │   │
│  | • freena            |      │ • vrishin (login)       │   │
│  | • kthomas           |      │ • klarios (login)       │   │
│  | • gabriel           |      │ • gnucash (NO LOGIN!)   │   │
│  | • jejemplo          |      └─────────────────────────┘   │
│  └─────────────────────┘                                    │
│                               ┌─────────────────────────┐   │
│                               │   DATABASES             │   │
│                               │   Owned by: gnucash     │   │
│                               ├─────────────────────────┤   │
│                               │ • jetroweb              │   │
│                               │ • sjcompute             │   │
│                               │ • novaweb               │   │
│                               │ • secosol               │   │
│                               │                         │   │
│                               └─────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

---

## Core Concept

Unix users and PostgreSQL users are completely separate systems.

| Unix Users | PostgreSQL Users |
|-----------|-----------------|
| Access OS | Access database |
| SSH (port 22) | PostgreSQL (port 5432) |
| Stored in system files | Stored in pg_roles |
| Use SSH keys/passwords | Use DB credentials |

---

## What is a "User"?

A user is not a person.

A user is:
> A set of credentials and permissions stored in a system.

---

## Applying Database Knowledge

### 1. PostgreSQL is the DBMS
- GnuCash is an application
- PostgreSQL stores the data

### 2. Users are Roles
- Stored in `pg_roles`
- May have LOGIN or NOLOGIN

### 3. Role-Based Access Control (RBAC)
```sql
GRANT gnucash TO username;
```

Users inherit permissions from roles.

### 4. Client-Server Model
- GnuCash = client
- PostgreSQL = server

---

## Key Discovery

The recommended setup is:

> Use a NON-LOGIN role to own the database.

---

## Why gnucash is NOLOGIN

- Owns all databases
- Cannot log in
- Has no password
- Improves security
- Separates ownership from access

---

## How Access Works

1. User opens GnuCash
2. Connects to PostgreSQL (port 5432)
3. Enters DB username/password
4. PostgreSQL authenticates user
5. User inherits permissions from gnucash

---

## Common Mistake

Using SSH authentication for GnuCash is incorrect.

- SSH → Unix (port 22)
- PostgreSQL → Database (port 5432)

---

## Correct Connection

Host: webschool.sjcompute.org  
Port: 5432  
Database: jetroweb  
Username: PostgreSQL user  
Password: PostgreSQL password  

---

## Final Understanding

- Unix users ≠ PostgreSQL users
- GnuCash ≠ Database
- PostgreSQL = DBMS
- gnucash = non-login owner role
- Users inherit permissions

---

## Conclusion

This setup is:
- Secure
- Scalable
- Based on database best practices

It demonstrates correct use of PostgreSQL roles and system separation.

---
