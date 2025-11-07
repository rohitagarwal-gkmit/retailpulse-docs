# **RetailPulse**

A lightweight, data-driven Inventory dashboard for a single store with real-world data formats.

---

## Problem Statement

A small retail store, needs a simple way to analyze its sales and inventory. The store generates daily sales reports as PDFs or CSVs from its POS machine and receives supplier invoices in various formats.

Currently, there is no centralized system to:

1. Track daily profit margins.
2. Identify fast-moving vs. slow-moving products.
3. Manage inventory levels.

The goal is to build a web application that allows a store operator to upload these files and provides an admin dashboard to view key business insights.

---

## The Solution – RetailPulse

A powered web dashboard that automates daily retail analytics for a single store, by processing POS sales files (PDF/CSV) and supplier bills, then storing them in a data warehouse, with S3 tracking for file lineage.

---

## Core Concept

| Component                  | Role              | Description                                                  |
| -------------------------- | ----------------- | ------------------------------------------------------------ |
| **Frontend**               | Web + PWA         | Operator uploads POS & supplier files, Admin views analytics |
| **Backend**                | API + ETL Trigger | Handles file upload, triggers PySpark ETL, serves analytics  |
| **Database**               | Structured Store  | Sales, Products, Suppliers, Inventory                        |
| **Processing**             | ETL Jobs          | Parse, transform, and enrich uploaded data                   |
| **Storage**                | Data Warehouse    | Stores uploaded files & tracking table                       |
| **Tracking Table**         | Tracking File     | Records file name, upload time, status (Processed/Failed)    |