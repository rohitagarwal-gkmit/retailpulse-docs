# **RetailPulse**

A lightweight, data-driven Inventory dashboard for a single store with real-world data formats.

---

## Problem Statement

A small retail store, needs a simple way to analyze its sales and inventory. The store generates daily sales reports as PDFs or CSVs from its POS machine and receives supplier invoices in various formats.

Currently, there is no centralized system to:
*   Track daily profit margins.
*   Identify fast-moving vs. slow-moving products.
*   Manage inventory levels.

The goal is to build a web application that allows a store operator to upload these files and provides an admin dashboard to view key business insights.

---

## The Solution – RetailPulse

> A React + FastAPI + PostgreSQL + PySpark powered web dashboard
> that automates daily retail analytics for a single store,
> by processing POS sales files (PDF/CSV) and supplier bills,
> then storing them in a data warehouse, with S3 tracking for file lineage.

---

## Core Concept

| Component                  | Role              | Description                                                  |
| -------------------------- | ----------------- | ------------------------------------------------------------ |
| **Frontend (React)**       | Web + PWA         | Operator uploads POS & supplier files, Admin views analytics |
| **Backend (FastAPI)**      | API + ETL Trigger | Handles file upload, triggers PySpark ETL, serves analytics  |
| **Database (PostgreSQL)**  | Structured Store  | Sales, Products, Suppliers, Inventory                        |
| **Processing (PySpark)**   | ETL Jobs          | Parse, transform, and enrich uploaded data                   |
| **Storage (AWS S3)**       | Data Lake         | Stores uploaded files & tracking table                       |
| **Tracking Table (in S3)** | Metadata          | Records file name, upload time, status (Processed/Failed)    |