# Supply Chain Hub Installation Guide

---

## 1. Introduction

This document guides you through the steps required to install the Supply Chain Hub for Oracle E-Business Suite R12.

Supply Chain Hub is an add-on to Blitz Report and installs the following objects on your Oracle E-Business Suite server:

- Database tables, sequences, synonyms, indexes and views
- Database packages
- User interface form
- Application setups such as crm spreadtables, profile options, lookups, form functions, menus

All installed objects are prefixed with `XXEN_SCHUB`.

The basic installation steps are:

1. Run the installation shell script on the application node using the application owner user.
2. Perform additional manual application setup steps from the 'System Administrator' responsibility.

---

## Table of Contents

| Section | Description |
|---------|-------------|
| [Prerequisites](part1_prerequisites.md) | Install Blitz Report requirement |
| [Installation](part2_installation.md) | Step-by-step installation process |
| [Application Setup](part3_application_setup.md) | Menu entry, planning source, WIP setup |
| [Profile Options](part4_profile_options.md) | Configurable profile options |
| [Upgrade](part5_upgrade.md) | Upgrade procedures |
| [Troubleshooting](part6_troubleshooting.md) | Common issues and solutions |

---

*Next: [Prerequisites](part1_prerequisites.md)*
