# Supply Chain Hub Installation Guide

## Introduction

This document guides you through the steps required to install the Supply Chain Hub for Oracle E-Business Suite R12.

Supply Chain Hub is an add-on to Blitz Report designed specifically for discrete manufacturing customers. It provides a unified view of supply chain data by combining ERP information with Advanced Supply Chain Planning (ASCP) data, giving users a powerful single-screen interface for planning and analysis.

### Key Features

- **Unified Data View:** Combines data from ERP and ASCP (if using a separate planning server) through database link integration
- **Advanced Item Search:** Search for inventory items based on item type, sales order, project/task, or planning exceptions
- **Bill of Materials Explosion:** Explode BOM levels up to 5 levels deep from the item search results
- **Quick Navigation:** Right-click on items to navigate directly to Oracle standard screens (Bill of Materials, On-Hand Quantities, etc.)
- **Hyperlink Access:** Click hyperlinks to access PO supply, work orders, and other related documents
- **Blitz Report Integration:** Highlight items and run Blitz Reports (such as the Horizontal Plan report) directly from the interface
- **Planned Order Release:** Release planned orders to work orders or purchase requisitions directly from the Supply/Demand grid

### Installed Objects

Supply Chain Hub installs the following objects on your Oracle E-Business Suite server:

- Database tables, sequences, synonyms, indexes and views
- Database packages
- User interface form
- Application setups such as crm spreadtables, profile options, lookups, form functions, menus

All installed objects are prefixed with `XXEN_SCHUB`.

The basic installation steps are:

1. Run the installation shell script on the application node using the application owner user.
2. Perform additional manual application setup steps from the 'System Administrator' responsibility.

## Table of Contents

| Section | Description |
|---------|-------------|
| [1. Prerequisites](part1_prerequisites.md) | Install Blitz Report requirement |
| [2. Installation](part2_installation.md) | Step-by-step installation process |
| [3. Application Setup](part3_application_setup.md) | Menu entry, planning source, WIP setup |
| [4. Profile Options](part4_profile_options.md) | Configurable profile options |
| [5. Upgrade](part5_upgrade.md) | Upgrade procedures |
| [6. Troubleshooting](part6_troubleshooting.md) | Common issues and solutions |

*Next: [1. Prerequisites](part1_prerequisites.md)*
