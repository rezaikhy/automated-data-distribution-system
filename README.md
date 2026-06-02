# Automated Data Distribution System

## Overview
Automated Data Distribution System is a Google Apps Script-based automation project designed to streamline WhatsApp campaign data distribution across multiple operational areas.

The system automatically processes campaign links, extracts target IDs, distributes customer data, and generates distribution logs, significantly reducing manual workload and minimizing human error.

This project was developed to support campaign execution management within a telecommunications operational environment.

---

## Key Features

### Automated Link Processing
- Extracts Google Spreadsheet IDs automatically from campaign links.
- Supports multiple campaign categories and distribution areas.

### Target Data Distribution
- Automatically distributes customer data (MSISDN) into target campaign sheets.
- Supports area-based segmentation:
  - Ambon
  - Seram Bagian Barat (SBB)
  - Kepulauan Tual

### Campaign Management
- Supports multiple campaign materials.
- Handles High Priority and Low Priority campaign categories.
- Simplifies campaign scheduling and execution.

### Logging System
- Generates distribution logs automatically.
- Provides execution tracking and monitoring.
- Helps maintain data integrity.

### Google Workspace Integration
- Google Sheets
- Google Apps Script
- Google Drive

---

## Workflow

1. User inputs campaign links into the master sheet.
2. System extracts Spreadsheet IDs automatically.
3. Campaign materials are mapped to target areas.
4. Customer data is distributed to destination sheets.
5. Distribution activity is recorded in logs.
6. Campaign execution becomes faster and more reliable.

---

## Technologies Used

- Google Apps Script (JavaScript)
- Google Sheets
- Google Drive
- SpreadsheetApp API

---

## Project Structure
