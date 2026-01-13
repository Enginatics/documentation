# Documentation Map

This document provides a detailed map of the project's documentation. Use it to quickly identify which file contains information on a specific topic and its inner structure.

## 1. User Guide (`user_guide/`)

Contains information for end-users on how to effectively use Blitz Report to run, manage, and customize reports.

-   **`introduction.md`**
    -   **Topics**: High-level introduction to Blitz Report, its integration with Oracle E-Business Suite, output formats (XLSX, CSV), and general benefits for business users.
    -   **Keywords**: Overview, What is Blitz Report, Oracle Forms, E-Business Suite, SQL scripts, XLSX, CSV, operational reporting solution.
    -   **Common Queries**: "What is Blitz Report?", "How does Blitz Report integrate with Oracle EBS?"

-   **`part1_running_blitz_report.md`**
    -   **Topics**: Detailed steps for running a Blitz Report, including selecting reports, entering parameter values, viewing output, customizing runtime options, managing templates, scheduling reports, and using Blitz Upload for data entry.
    -   **Keywords**: Running reports, report selection, parameter values, multiple values, default parameters, output viewing, options, email delivery, output format, row limit, time limit, disable column translations, exclude column headers, custom postprocess, output filename, additional output directory, tokens, templates, template layout, pivot table, Excel upload, datasheet, template sharing, excluded parameters, scheduling a report, delivery options, output distribution, Blitz Upload, create mode, create or update mode, color-coded columns, gray columns, yellow columns, read-only, required columns, LOV validation, bulk upload, error handling, upload results.
    -   **Inner Structure**:
        -   `1.1 Selecting a report`: Text search, LOV options, responsibility filtering.
        -   `1.2 Parameter values`: Multiple values, saving user/template defaults.
        -   `1.3 Running and viewing the output`: Concurrent process, re-download output.
        -   `1.4 Options`: User/Developer view, Email (placeholders, custom messages), Output Format, Row/Time Limit, Column Translations, Postprocess, Filename, Additional Directories, Tokens, Freeze flag.
        -   `1.5 Templates`: Select/create, layout (columns, aggregation, sort, sheet break), pivot table (filters, columns, rows, values), Excel upload, datasheet, template sharing (site, responsibility, user), excluded parameters.
        -   `1.6 Scheduling a report`: Copy existing request, manual submission, delivery options (email, FTP, WebDAV), suppress empty file delivery.
        -   `1.7 Using Blitz Upload`: Upload modes (create, create or update), color-coded Excel columns (gray/yellow/white), LOV validation, data entry, bulk processing, error handling and reprocessing.
    -   **Common Queries**: "How to run a report?", "How to schedule a Blitz Report?", "What are runtime options?", "How to use templates?", "Email report output.", "How to use Blitz Upload?", "What do Excel column colors mean?", "How to fix upload errors?"

-   **`part2_gl_financial_statement.md`**
    -   **Topics**: Comprehensive guide to the GL Financial Statement and Drilldown (FSG) feature, covering its capabilities, accessing it, responsibility/ledger/segment selection, Discover tools, List of Values, Insert Functions, Refresh options, Drilldown, and conversion tools.
    -   **Keywords**: FSG, GL Financial Statement, Drilldown, Balance Reporting, Oracle Data Integration, Migration Tools, Accessing FSG, Running Default Report, Uploaded Templates, Locally Stored Templates, Login, Exit, Responsibility Selector, Ledger, Segment, Discover Tools, Expand, Explode, List of Values (LOV), Single Value, Multiple Value, Segment Values, Add Range, Delete LOV, Insert Functions, Balance, Segment Description, Period Offset, Period by Date, Period Name, Daily Rate, Refresh (Pending, Sheet, Range, Workbook), Drilldown Criteria (Balance, Journal Extract), Next Level Drilldown, Layout Options, Tools (Snapshot, Hide Zeros), Converter (Oracle FSG, GL Wand, Spreadsheet Server).
    -   **Inner Structure**:
        -   `2.1 Accessing Blitz FSG`: Running default/uploaded/local templates, login, exit.
        -   `2.2 Responsibility, Ledger and Segment Selector`: Select Responsibility, Ledger, Segment dropdowns.
        -   `2.3 Discover`: Key Functionality (Discover, Expand, Explode).
        -   `2.4 List of Values`: Create, Show, Single Value, Multiple Value, Segment Values, Adding Range, Delete LOV.
        -   `2.5 Insert Functions`: Using Function Form, Balance (parameters), Segment Description, Period Offset, Period by Date, Period Name, Daily Rate.
        -   `2.6 Refresh`: Pending, Sheet, Range, Workbook refresh.
        -   `2.7 Drilldown`: Drill Criteria (Balance, Journal Extract), Next Level Drilldown, Layout Options.
        -   `2.8 Tools`: Snapshot, Hide Zeros, Unhide Zeros.
        -   `2.9 Converter`: Oracle FSG, GL Wand & Spreadsheet Server.
    -   **Common Queries**: "How to use FSG?", "What are the FSG drilldown options?", "How to convert FSG reports?", "Refresh GL balances."

## 2. Developer Guide (`developer_guide/`)

Contains technical, in-depth information for developers on how to create reports, integrate with APIs, and leverage advanced features of Blitz Report.

-   **`introduction.md`**
    -   **Topics**: High-level introduction to Blitz Report for IT professionals, its role in Oracle EBS, and core functionalities for report creation and maintenance.
    -   **Keywords**: Overview, Developer Guide, Oracle Forms, E-Business Suite, SQL scripts, IT team, operational reporting solution.
    -   **Common Queries**: "What is the developer guide about?", "Blitz Report for developers."

-   **`part1_creating_report.md`**
    -   **Topics**: Detailed instructions on creating a Blitz Report, including SQL definition, anchor types (`n=n`, `&lexical`, `:bind`), dynamic SQL examples, report header properties, parameter setup (display sequence, types, LOVs, default values, dependencies), assignments, multi-language support, and security aspects like access profiles and VPD.
    -   **Keywords**: Report Creation Steps, SQL query, Anchors, `n=n`, `&lexical`, Bind variables, `:sheet_name`, Dynamic SQL Example, Pivot Table in SQL, Report Header (Name, Description, Search, Category, Enabled, Version, Type, BIP Code, Number Format, Report Options, DB Package, Author email, Output Format, Row/Time Limit, Custom Postprocess, Output Filename, Additional Out. Directory), 'Blitz Report Information' Descriptive Flexfield, Parameters (Display Sequence, SQL Text, Multiple Values, Anchor, Parameter Type, LOV Name/Query, Matching Value, Default Value, Required, Advanced Required, Dependent Parameters, Dynamic Parameter SQL Text), Assignments (Levels, Form Assignment, Default Assignments, Mass Assignments), Categories, Multi-language Support, Security and User Profiles, Access Profile Functionality Matrix, Data Access Security, Securing Sensitive Information with Oracle Virtual Private Database (VPD).
    -   **Inner Structure**:
        -   `1.1 Report Creation Steps`: Basic steps, dynamic SQL concept, bind variables.
        -   `1.2 Anchors and Binds`: `n=n`, `&lexical` (WHERE clauses, dynamic tables, ORDER BY, HINTS), `:bind` (reserved words), `:sheet_name`, Examples.
        -   `1.3 Dynamic SQL Example`: `n=n` anchor, Pivot Table in SQL.
        -   `1.4 Report Header`: Name, Description, Search, Category, Enabled, Version, Type, BIP Code, Number Format, Report Options (list of 14 options), DB Package, Author email, Email, Output Format, Row Limit, Time Limit, Custom Postprocess, Output Filename, Additional Out. Directory (APPS/DB Server), Additional Out. Filename, Request type, Target Database, SQL, Descriptive Flexfield.
        -   `1.5 Parameters`: Display Sequence, Parameter Name, SQL Text, Multiple Values, Anchor, Parameter Type (Char, Date, DateTime, Number, LOV, LOV Custom, LOV Oracle), LOV Name, LOV Query, Matching Value, Default Value (SQL examples), Description, Required, Advanced Required Parameters, Dependent Parameters, Dynamic Parameter SQL Text.
        -   `1.6 Assignments`: Levels (Site, Application, Operating Unit, Request Group, Responsibility, User, Form), Form Assignment, Default Assignments, Mass Assignments.
        -   `1.7 Categories`: Definition and usage.
        -   `1.8 Multi-language Support`: Report Data, User Messages, User Interface Translations.
        -   `1.9 Security and User Profiles`: Levels of security, Access Profile Functionality Matrix.
        -   `1.10 Data Access Security`: `_all` tables, Operating Unit parameter.
        -   `1.11 Securing Sensitive Information with Oracle Virtual Private Database`: Steps to secure data (lookup setup, update/remove VPD policies, profile option).
    -   **Common Queries**: "How to create a new report?", "Explain `&lexical` parameters.", "How to secure a report?", "Set up dependent parameters.", "What are profile options for reports?"

-   **`part2_tools_menu.md`**
    -   **Topics**: Detailed explanation of functionalities available in the "Tools" menu within the Blitz Report setup, including managing LOVs, assignments, categories, copying reports/LOVs, export/import features (XML, BI Publisher, Discoverer, etc.), uploading large SQLs, and column/dynamic column translations.
    -   **Keywords**: Tools Menu, LOVs (setup, name, description, validate from list, filter before display, used by, LOV query, version history), Assignments (mass-assign, review), Categories (definition, migration), Copy Report, Copy LOV, Export (XML files, SQL scripts, options, API, Blitz Report Library, notes), Import (XML Upload, BI Publisher, Concurrent Program, Discoverer Worksheet/Folders, Excel4apps Reports Wand, Enterprise Command Center, Polaris Reporting Workbench, Import APIs, prerequisites), Upload Large SQL, Column Translations, Dynamic Column Translations, Resequence Parameters, License Key, User License Assignment, Mass Change.
    -   **Inner Structure**:
        -   `2.1 LOVs`: Name, Description, Validate From List, Filter Before Display, Used By, LOV Query, Version.
        -   `2.2 Tools > Assignments`: Mass assignment, review.
        -   `2.3 Tools > Categories`: Definition and use.
        -   `2.4 Copy Report`: Purpose, notes on seeded reports.
        -   `2.5 Copy LOV`: Purpose, notes on seeded LOVs.
        -   `2.6 Export`: Menu options, Export Checkboxes, API (export_file_data, export_report_), Blitz Report Library, Notes, Import API.
        -   `2.7 Import`: XML Upload, BI Publisher, Concurrent Program, Discoverer Worksheet/Folders (prerequisites), Excel4apps Reports Wand, Enterprise Command Center, Polaris Reporting Workbench.
        -   `2.8 Upload Large SQL`: SQL files > 32K characters, browser upload.
        -   `2.9 Column Translations`: Multi-language support, number formats.
        -   `2.10 Dynamic Column Translations`: Rules, SQL output, parameters/header columns.
        -   `2.11 Resequence Parameters`: Automatic resequencing.
        -   `2.12 License Key`: Company name, license key, active users.
        -   `2.13 User License Assignment`: Automatic vs. Manual.
        -   `2.14 Mass Change`: SQL text, LOV query updates.
    -   **Common Queries**: "How to import BI Publisher reports?", "Export reports to XML.", "Manage LOVs.", "What's in the Tools menu?"

-   **`part3_tips_tricks.md`**
    -   **Topics**: Practical tips and advanced configurations for debugging, browser setup (Firefox), creating incremental outbound interfaces, using Blitz Report as a data warehouse, handling Excel/CSV files, and production deployment strategies.
    -   **Keywords**: Debugging (SQL execution errors, logfile), Firefox with Oracle EBS (32bit, NPAPI, Mozilla Maintenance Service, disable updates, Java plugin), Incremental Outbound Interface (last_update_date, scheduled reports, suppress empty file delivery, protected type), Data Warehouse (additional output directory, filename convention, Power BI, Qlik Sense, Tableau, OBIEE), MS Excel and CSV Files (delimiter, column type detection, retain leading zeroes, macro), Blitz Report Production Deployment (upgrade, export metadata, profile options, import XML), MS Excel Blocked Macros Warning (Trusted Sites).
    -   **Inner Structure**:
        -   `3.1 Debugging`: Steps (logfile, SQL execution).
        -   `3.2 Using Firefox with Oracle EBS`: Historic version, remove Mozilla Maintenance Service, disable automatic updates, check Java plugin.
        -   `3.3 Incremental Outbound Interface`: SQL example, profile option.
        -   `3.4 Data Warehouse`: Configuration (profile options), time series.
        -   `3.5 MS Excel and CSV Files`: Default Delimiter, Column Type Detection (retain leading zeroes), Macro to Filter and Freeze Top Row.
        -   `3.6 Blitz Report Production Deployment`: High level steps (upgrade, export, install, import).
        -   `3.7 MS Excel Blocked Macros Warning`: Solution (trusted sites).
    -   **Common Queries**: "How to debug report errors?", "Best practices for Firefox and EBS?", "Create a data warehouse with Blitz Report?", "Deployment steps for production."

-   **`part4_profile_options.md`**
    -   **Topics**: A comprehensive reference list of all Blitz Report profile options, categorized for easier navigation, detailing their descriptions and default values.
    -   **Keywords**: Profile Options, Configuration, Settings, Core, Column, Translation, Discoverer Import, Email, Output, File, Log, Retention, User Interface, Security, XLSX Format, Blitz Upload, Blitz FSG.
    -   **Inner Structure**: Categorized tables for:
        -   `Core Profile Options`
        -   `Column and Translation Options`
        -   `Discoverer Import Options`
        -   `Email Options`
        -   `Output and File Options`
        -   `Log and Retention Options`
        -   `User Interface Options`
        -   `Security Options`
        -   `XLSX Format Options`
        -   `Blitz Upload Options`
        -   `Blitz FSG Options`
    -   **Common Queries**: "List all profile options.", "How to configure email settings?", "What are the security profile options?"

-   **`part5_apis_integration.md`**
    -   **Topics**: Integrating Blitz Report with other systems programmatically using various APIs for export, import, and report submission, along with useful database functions.
    -   **Keywords**: APIs, Integration, Export API, Import API, Submitting Blitz Report from PLSQL, `fnd_request.submit_request`, `xxen_api.report_submit_concurrent_`, `set_parameter_value_`, `set_runtime_option`, Runtime Option Names, `run_report`, BLOB output, Opening Reports from Custom Forms, `xxen_utils.download_file`, Useful DB Functions, `XXEN_UTIL.CLIENT_TIME`, `SERVER_TIME`, `TIME`, `USER_NAME`, `USER_ID`, `DFF_COLUMNS`.
    -   **Inner Structure**:
        -   `5.1 Export`: Blitz Report Library Export API (`export_file_data`, `export_report_`, examples).
        -   `5.2 Import`: XML Upload Import API, BI Publisher Import API, Discoverer Worksheet Import API, Discoverer Folders Import API, Excel4apps Reports Wand Import API, Enterprise Command Center Import API, Polaris Reporting Workbench Import API.
        -   `5.3 Submitting Blitz Report from PLSQL`: `FND Concurrent Program API` (`fnd_request.submit_request`), `Blitz Report Concurrent Program API` (`xxen_api.report_submit_concurrent_`, `set_parameter_value_`, `set_runtime_option`, Runtime Option Names, `create_run_id_`, `run_report`), `PLSQL API Returning a BLOB`, `Opening Reports from Custom Forms`.
        -   `5.4 Useful DB Functions`: `XXEN_UTIL.CLIENT_TIME`, `XXEN_UTIL.SERVER_TIME`, `XXEN_UTIL.TIME`, `XXEN_UTIL.USER_NAME` (overloaded), `XXEN_UTIL.USER_ID`, `XXEN_UTIL.DFF_COLUMNS`.
    -   **Common Queries**: "How to submit a report from PL/SQL?", "List Blitz Report APIs.", "Convert server time to client time.", "Export reports programmatically."

-   **`part6_upload_glossary.md`**
    -   **Topics**: A guide to the "Blitz Upload" feature, including creation steps, SQL requirements, column validations, API integration, and result reporting, followed by a glossary of key terms.
    -   **Keywords**: Blitz Upload, Creating a Blitz Upload, Basic Steps, Header, SQL Requirements (API based, Interface Table based), Upload Columns (Data type validations, LOV validations, Value to Id queries, Defaulting, Comments, Required, Read Only, Hidden, Group Validation, Column Properties, LOV Query Example with Dependencies), Upload API (Type, Create Only, Name, API Wrapper Procedure Requirements, Upload Parameters, Post Procedure, Excel Validation), Upload Results (Error SQL, Success SQL, Data View, Pre-built Functions for Result SQL), Glossary.
    -   **Inner Structure**:
        -   `6.1 Header`: Name, Description, Type (Upload).
        -   `6.2 SQL Requirements`: For API Based Uploads, For Interface Table Based Uploads.
        -   `6.3 Upload Columns`: Features, Column Properties, LOV Query Example with Dependencies.
        -   `6.4 Upload API`: Type, Create Only, Name, API Wrapper Procedure Requirements, Upload Parameters, Post Procedure, Excel Validation.
        -   `6.5 Upload Results`: Error SQL, Success SQL, Data View, Pre-built Functions for Result SQL.
        -   `7. Glossary`: List of common terms (LOV, Anchor, Bind Variable, Lexical Parameter, VPD, EUL, DFF, API, BLOB, CLOB, EBS, SSO, SMTP, CSV, TSV, XLSX).
    -   **Common Queries**: "How to create a data upload?", "Blitz Upload API requirements.", "What are Blitz Upload column validations?", "Glossary of terms."

## 3. Installation Guide (`installation_guide/`)

Contains comprehensive instructions for installing, configuring, upgrading, and troubleshooting Blitz Report on Oracle E-Business Suite.

-   **`README.md`**
    -   **Topics**: Introduction to the Installation Guide, table of contents, quick start summary, and links to related resources.
    -   **Keywords**: Installation overview, quick start, support, related resources.
    -   **Common Queries**: "How to install Blitz Report?", "Installation guide overview."

-   **`part1_prerequisites.md`**
    -   **Topics**: All requirements that must be met before installing Blitz Report, including required tools, access credentials, space requirements, creating a custom application via adsplice, configuring the Integrated SOA Gateway (ISG), and other database prerequisites.
    -   **Keywords**: Prerequisites, Required tools, Kitty, Putty, WinSCP, SQL Developer, Toad, APPS password, SYSTEM password, applmgr, Space requirements, APPS_TS_TX_DATA, APPS_TS_TX_IDX, Custom application, adsplice, XXEN, XXEN_TOP, newprods.txt, topfile.txt, AutoConfig, APPL_TOP, shared APPL_TOP, FNDCPASS, ISG, Integrated SOA Gateway, SELECT ANY DICTIONARY, RESOURCE role, ctxsys.context.
    -   **Inner Structure**:
        -   `1.1 Required tools and access credentials`: Tools list, access requirements, space requirements.
        -   `1.2 Creating a custom application in Oracle E-Business Suite`: adsplice steps 1-12, shared APPL_TOP considerations, EBS R12.2 and 11i notes.
        -   `1.3 Configure Oracle EBS Integrated SOA Gateway`: Links to ISG configuration for R12.1.3 and R12.2.
        -   `1.4 Other prerequisites`: EBS versions supported, database version, SELECT ANY DICTIONARY grant.
    -   **Common Queries**: "What are the prerequisites for Blitz Report?", "How to create XXEN custom application?", "Configure ISG for Blitz Report.", "Required database grants."

-   **`part2_installation.md`**
    -   **Topics**: Step-by-step instructions for installing Blitz Report on the application server, including running the install script, handling multi-node environments, and configuring the OAF user interface.
    -   **Keywords**: Installation, install.sh, unzip, APPS_PWD, CUSTOM_APP, CUSTOM_TABLESPACE, CUSTOM_INDEXSPACE, skip_db, generate_webservice_artifacts.sh, adcgnjar, customall.jar, adapcctl.sh, admanagedsrvctl.sh, OACORE_SERVER, R12.1.3, R12.2.
    -   **Inner Structure**:
        -   `2.1 Installation Steps`: Steps 1-7, non-interactive parameters, multi-node installation, OAF UI setup for R12.1.3 and R12.2.
    -   **Common Queries**: "How to install Blitz Report?", "Run installation script.", "Multi-node installation.", "Configure OAF user interface."

-   **`part3_application_setup.md`**
    -   **Topics**: Post-installation configuration steps including setting up access profiles, default assignments, menu entries, concurrent manager settings, license key, GL summary accounts, and testing ISG connectivity.
    -   **Keywords**: Application setup, Blitz Report Access profile, User, Developer, System access levels, Default assignment load, Blitz Report Load Default Assignments during Upgrades, Update Menu Entries, Menu Entry Prompt, Include Web User Interface, Add to Navigator Top10 List, Compile Security, Concurrent manager sleep time, Work Shifts, Blitz Report Monitor, License key, trial license, free version, GL Summary Accounts, Update GL Indexes, ISG connectivity, GL Daily Rates Upload, Excel Trust Center, VBA macros.
    -   **Inner Structure**:
        -   `3.1 Access profile option`: Profile levels, User/Developer/System access.
        -   `3.2 Default assignment load profile option`: Profile setting, custom assignments.
        -   `3.3 Update Menu Entries concurrent program`: Parameters (Update Mode, Menu Entry Prompt, Include Web UI, Update Manually Created, Add to Navigator Top10), Compile Security.
        -   `3.4 Concurrent manager sleep time`: Work Shifts, Blitz Report Create Manager.
        -   `3.5 Monitor concurrent program`: Tasks performed, scheduling recommendations.
        -   `3.6 License key`: Trial license, free version limitations.
        -   `3.7 GL Summary Accounts`: Summary accounts setup, Update GL Indexes.
        -   `3.8 Test Integrated SOA Gateway connectivity`: GL Daily Rates Upload test, Excel Trust Center settings.
    -   **Common Queries**: "How to setup Blitz Report access?", "Configure concurrent manager for Blitz Report.", "Enter license key.", "Test ISG connectivity."

-   **`part4_upgrade.md`**
    -   **Topics**: Instructions for upgrading Blitz Report using the automatic concurrent program or manual terminal session, including handling blocking sessions and CUSTOM.pll updates.
    -   **Keywords**: Upgrade, Blitz Report Upgrade concurrent program, Maintenance Mode, Manual upgrade, install.sh, skip_db, generate_webservice_artifacts.sh, ORA-00955, Blocking sessions, active sessions query, disconnect_db_session, kill_db_server_process, CUSTOM.pll warning.
    -   **Inner Structure**:
        -   `Manual Upgrade Steps`: Steps 1-6 for manual upgrade.
        -   `Additional guidelines`: ORA-00955 errors, blocking sessions (query to identify active users), CUSTOM.pll update.
    -   **Common Queries**: "How to upgrade Blitz Report?", "Manual upgrade steps.", "Handle blocking sessions during upgrade.", "CUSTOM.pll warning."

-   **`part5_optional_configurations.md`**
    -   **Topics**: Optional configurations for specific environments including Windows installation, standby database setup, mod_plsql for EBS 11i, SSO configuration, advanced email delivery, manual menu/request group entries, dedicated concurrent manager, and manual custom application creation.
    -   **Keywords**: Optional configurations, Windows, MKS Toolkit, Cygwin, Standby database, Active Data Guard, DML Redirection, xxen_adg database link, Event 10946, mod_plsql, EBS 11i, SSO Enabled profile, Blitz Report SSO Enabled, Blitz Upload Use mod_plsql, Advanced Email Delivery, SMTP Host, SMTP Port, SMTP SSL, Email Body Message, Email Subject Message, Menu entry, Request group entry, Blitz Report Create Manager, Custom application manually, CREATE USER XXEN, Register Application, DataGroup, Alert Manager Installations, AutoConfig custom parameters.
    -   **Inner Structure**:
        -   `5.1 Installation on Windows`: MKS Toolkit bash.
        -   `5.2 Running blitz reports on a standby database`: Prerequisites (Oracle 19c+, Active Data Guard, xxen_adg link, Event 10946).
        -   `5.3 Configure mod_plsql (EBS 11i only)`: Link to blog post.
        -   `5.4 SSO enabled profile option`: Blitz Report SSO Enabled profile.
        -   `5.5 Mod plsql profile option`: Blitz Upload Use mod_plsql profile.
        -   `5.6 Setup Blitz Report Advanced Email Delivery`: Profile options list.
        -   `5.7 Menu entry`: Manual menu assignment.
        -   `5.8 Request group entry`: Adding to request groups.
        -   `5.9 Blitz Report Create Manager program`: Dedicated concurrent manager.
        -   `5.10 Creating a custom application manually`: Steps 1-6 (CREATE USER, Register Application, Register Oracle User, DataGroup, Alert Manager, AutoConfig).
    -   **Common Queries**: "Install Blitz Report on Windows.", "Configure standby database for Blitz Report.", "Setup SSO for Blitz Report.", "Create custom application without adsplice."

-   **`part6_troubleshooting.md`**
    -   **Topics**: Comprehensive troubleshooting guide covering common installation errors, form errors, ISG connectivity issues, Excel/macro problems, and database errors with detailed solutions.
    -   **Keywords**: Troubleshooting, ORA-12154, TNS_ADMIN, frmcmp_batch.sh, CUSTOM_SCHEMA error, Alert Manager Installations, Concurrent Manager stops, ORA-01843, date format, ORA-29855, DRG-11446, DRG-10700, Oracle Text Knowledge Base, droldUS.dat, DEFAULT_LEXER, FRM-40654, character set mismatch, NLS_CHARACTERSET, FRM-99999 Error 408, CTXSYS synchronization, PLS-00306 SYNCRN, APP-FND-02901, MO_ORG_ACCESS_NO_DATA_FOUND, Security Profile, Function not available, Compile Security, Clear All Cache, CUSTOM.pll library attachment, ORA-28003, password verification, ISGADMIN authentication, ISG_SERVICE_EXECUTION_ERROR, ISGDatasource reset, ORA-04088 trigger error, ISG_SERVICE_AUTH_FAILURE, GLOBAL user, ISG_INVALID_ALIAS, ISG_FILE_ACCESS_ERROR, ASADMIN password, FND_SOA_AUTHORIZATION_FAILURE, Run-time error 1004, Protected View, Trusted Sites, ORA-00001 I_OBJAUTH1, Macros disabled, Trust Publisher, 504 Gateway Time-out, OAFM Java Heap Size, FRM-30085, package recompilation.
    -   **Inner Structure**:
        -   `6.1 ORA-12154`: TNS_ADMIN variable fix.
        -   `6.2 CUSTOM_SCHEMA error`: Alert Manager link setup.
        -   `6.3 Concurrent Manager stops`: Restart after adsplice.
        -   `6.4 ORA-01843`: Missing Oracle patch 12678526.
        -   `6.5 ORA-29855 DRG-11446`: Oracle Text Knowledge Base files.
        -   `6.6 ORA-29855 DRG-10700`: DEFAULT_LEXER preference.
        -   `6.7 FRM-40654`: Character set mismatch.
        -   `6.8 FRM-99999 Error 408`: CTXSYS synchronization.
        -   `6.9 PLS-00306 SYNCRN`: Same as 6.8.
        -   `6.10 APP-FND-02901`: Security profile issues.
        -   `6.11 Function not available`: Compile Security, clear cache.
        -   `6.22 CUSTOM library attachment`: Library attachment steps.
        -   `6.30 ORA-28003`: Password verification function.
        -   `6.31 ISGADMIN authentication`: Reset ISGADMIN password.
        -   `6.32 ISG_SERVICE_EXECUTION_ERROR`: Clear cache, reset ISGDatasource.
        -   `6.33 ORA-04088 trigger`: Disable investigation trigger.
        -   `6.34 ISG_SERVICE_AUTH_FAILURE`: GLOBAL user, clear cache.
        -   `6.35 ISG_INVALID_ALIAS`: Configure ISG.
        -   `6.36 FND_SOA_AUTHORIZATION_FAILURE`: Clear cache, reboot servers.
        -   `6.37 Run-time error 1004`: Trusted Sites, Trust Center.
        -   `6.38 ORA-00001 I_OBJAUTH1`: Recreate package.
        -   `6.39 Macros disabled`: Trust Publisher.
        -   `6.40 504 Gateway Time-out`: Increase OAFM heap size.
        -   `6.41 FRM-30085`: Recompile packages and form.
    -   **Common Queries**: "ORA-12154 during installation.", "ISG connection error.", "Excel macro blocked.", "Form compilation error.", "Character set mismatch.", "Blitz Report function not available."

## 4. Supply Chain Hub Installation Guide (`supply_chain_guide/`)

Contains comprehensive instructions for installing, configuring, and troubleshooting the Supply Chain Hub add-on for Oracle E-Business Suite.

-   **`README.md`**
    -   **Topics**: Introduction to Supply Chain Hub, overview of installed objects, basic installation steps, and table of contents.
    -   **Keywords**: Supply Chain Hub, SCHUB, Oracle E-Business Suite R12, Blitz Report add-on, XXEN_SCHUB prefix, database objects, user interface form, application setups.
    -   **Common Queries**: "What is Supply Chain Hub?", "Supply Chain Hub overview.", "SCHUB installation summary."

-   **`part1_prerequisites.md`**
    -   **Topics**: Prerequisites for Supply Chain Hub installation, primarily requiring Blitz Report to be installed first.
    -   **Keywords**: Prerequisites, Blitz Report, decentralized ASCP, Action Release, Reschedule, Cancel recommendations.
    -   **Inner Structure**:
        -   `1.1 Install Blitz Report in Oracle E-Business Suite`: Blitz Report dependency, decentralized ASCP considerations.
    -   **Common Queries**: "What are the prerequisites for Supply Chain Hub?", "Do I need Blitz Report for SCHUB?", "ASCP integration requirements."

-   **`part2_installation.md`**
    -   **Topics**: Step-by-step installation instructions for Supply Chain Hub, including running the install script and handling errors.
    -   **Keywords**: Installation, install.sh, unzip, supply_chain_hub_*.zip, APPS_PWD, APPS password, non-interactive installation, installation log, supply_chain_hub_install_logs_*.zip, decentralized ASCP.
    -   **Inner Structure**:
        -   `Installation Steps`: Steps 1-3 for installation, non-interactive mode, error review.
    -   **Common Queries**: "How to install Supply Chain Hub?", "Run SCHUB installation script.", "Non-interactive installation."

-   **`part3_application_setup.md`**
    -   **Topics**: Post-installation application setup including menu entries, choosing ASCP or MRP as planning source, and WIP requirement operations configuration.
    -   **Keywords**: Application setup, Menu entry, XXEN_SCHUB form function, sub-menu, System Administrator, ASCP, MRP, Planning Source, Supply Chain Hub: Use MRP for Planning Source profile, WIP Requirement Operations Closed Flag, wip_requirement_operations table, DFF, Component Information flexfield, WRO Closed Flag.
    -   **Inner Structure**:
        -   `3.1 Menu Entry`: Manual menu assignment for SCHUB.
        -   `3.2 Choosing ASCP or MRP as the Planning Source`: Default ASCP, profile option for MRP.
        -   `3.3 Set WIP Requirement Operations Closed Flag Concurrent Program`: WIP Demand KPI, DFF setup, scheduling recommendations.
    -   **Common Queries**: "How to add Supply Chain Hub to menu?", "Configure MRP instead of ASCP.", "Setup WIP Demand KPI.", "WRO Closed Flag flexfield."

-   **`part4_profile_options.md`**
    -   **Topics**: Comprehensive reference of all Supply Chain Hub profile options controlling system behavior, UI availability, and Oracle application integration.
    -   **Keywords**: Profile Options, Supply Chain Hub: Enable Release, Enable Reschedule and Cancel Actions, Calculate Price, Demand Price List, Drilldown after Release, Exclude Complete Work Orders, Exclude Inventory Master Organizations, Item Attachment Category, Show KPIs in Item Details Tab, Show KPIs in Item Search Tab, Show Release Button, Show Tooltip Help, Use MRP for Planning Source, Use Quick Sales Orders Form, Use Purchasing Buyer Work Center, Best Practices, Performance tuning.
    -   **Inner Structure**:
        -   `Profile Options Reference`: Complete table of all profile options with descriptions and defaults.
        -   `Best Practices`: Review and set explicitly, performance tuning, drilldown configurations, testing.
        -   `Notes`: Blank values, environment-specific options.
    -   **Common Queries**: "List all Supply Chain Hub profile options.", "How to enable planned order release?", "Configure drilldown forms.", "Performance tuning for SCHUB."

-   **`part5_upgrade.md`**
    -   **Topics**: Instructions for upgrading Supply Chain Hub using the same installation process.
    -   **Keywords**: Upgrade, supply_chain_hub_*.zip, ORA-00955, existing object errors.
    -   **Inner Structure**:
        -   `Upgrade Steps`: Download and deploy latest installation file, handle ORA-00955 errors.
    -   **Common Queries**: "How to upgrade Supply Chain Hub?", "ORA-00955 during upgrade.", "SCHUB upgrade steps."

-   **`part6_troubleshooting.md`**
    -   **Topics**: Troubleshooting guide covering form compilation errors, index synchronization issues, and planned order release problems for MRP and ASCP.
    -   **Keywords**: Troubleshooting, PL/SQL ERROR 302, XXEN_SCHUB_WIPDJMDF.fmb, WIPDJMDF, XXEN_SCHUB_FORMS lookup, Index synchronization, xxen_mtl_sys_items_tl_t1, xxen_mtl_sys_items_tl_t2, intermedia index, Planned Orders, MRP on-prem, Work Orders, Purchase Requisitions, ASCP centralized, ASCP distributed, APS_RELEASE responsibility, SCP Workbench, Release Order Transaction ID, Make vs Buy, Plan Organization, Pegging Info, Cross-Org Fulfillment.
    -   **Inner Structure**:
        -   `6.1 PL/SQL ERROR 302`: Form compilation workaround using standard WIP Discrete Jobs form.
        -   `6.2 Index Synchronization Performance Issues`: Reduce intermedia index sync frequency.
        -   `6.3 Planned Orders`: MRP vs ASCP requirements, Release Order Transaction ID, Make vs Buy confusion, Cross-Org fulfillment visibility, known gaps and upcoming enhancements.
    -   **Common Queries**: "SCHUB form compilation error.", "Index synchronization performance.", "Planned order release not working.", "MRP vs ASCP differences.", "Make vs Buy attribute.", "Pegging info drilldown."