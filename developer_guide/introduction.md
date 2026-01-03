# Blitz Report Developer Guide

## Introduction

Blitz Report is an Oracle Forms based software, fully integrated with Oracle E-Business Suite. It enables your IT team to easily store and edit SQL scripts for reports, and to make them available to your business users. Blitz Report runs as a concurrent process and generates output files in XLSX or text delimited CSV format. Upon completion, reports automatically download and open in Excel.

### Why Blitz Report is Fast

Blitz Report generates Excel output files directly in the database using optimized PL/SQL code, creating binary XLSX files without intermediate XML conversion steps. This eliminates the performance bottlenecks common in other reporting tools that use the Output Post Processor. Additionally, Blitz Report uses dynamic SQL by default, enabling the Oracle optimizer to use all available indexes for better query performance.

### Pre-Built Reports and Import Tools

Blitz Report includes approximately 600 pre-built reports covering all major Oracle EBS modules, with enhanced multi-organization capabilities. The tool also provides import functionality to migrate reports from:

- **Oracle Discoverer:** Automatic import of existing Discoverer workbooks
- **BI Publisher:** Import BI Publisher report queries
- **Enterprise Command Centers:** Import ECC queries for use in Blitz Report
- **Third-party tools:** Migration paths from various reporting solutions

With Blitz Report, we created the most efficient and easy to use operational reporting solution for Oracle EBS. Optimized for skilled IT professionals to better organize and maintain their reporting queries, and for business users to quickly access EBS data in a format they love without having to learn new skills.

We hope that you will enjoy working with Blitz Report as much as we do, and we welcome your feedback to [support@enginatics.com](mailto:support@enginatics.com).