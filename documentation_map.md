# Documentation Map

This document provides a detailed map of the project's documentation. Use it to quickly identify which file contains information on a specific topic and its inner structure.

## 1. User Guide (`user_guide/`)

Contains information for end-users on how to effectively use Blitz Report to run, manage, and customize reports.

-   **`introduction.md`**
    -   **Topics**: High-level introduction to Blitz Report, its integration with Oracle E-Business Suite, output formats (XLSX, CSV), and general benefits for business users.
    -   **Keywords**: Overview, What is Blitz Report, Oracle Forms, E-Business Suite, SQL scripts, XLSX, CSV, operational reporting solution.
    -   **Common Queries**: "What is Blitz Report?", "How does Blitz Report integrate with Oracle EBS?"

-   **`part1_running_blitz_report.md`**
    -   **Topics**: Detailed steps for running a Blitz Report, including selecting reports, entering parameter values, viewing output, customizing runtime options, managing templates, and scheduling reports.
    -   **Keywords**: Running reports, report selection, parameter values, multiple values, default parameters, output viewing, options, email delivery, output format, row limit, time limit, disable column translations, exclude column headers, custom postprocess, output filename, additional output directory, tokens, templates, template layout, pivot table, Excel upload, datasheet, template sharing, excluded parameters, scheduling a report, delivery options, output distribution.
    -   **Inner Structure**:
        -   `1.1 Selecting a report`: Text search, LOV options, responsibility filtering.
        -   `1.2 Parameter values`: Multiple values, saving user/template defaults.
        -   `1.3 Running and viewing the output`: Concurrent process, re-download output.
        -   `1.4 Options`: User/Developer view, Email (placeholders, custom messages), Output Format, Row/Time Limit, Column Translations, Postprocess, Filename, Additional Directories, Tokens, Freeze flag.
        -   `1.5 Templates`: Select/create, layout (columns, aggregation, sort, sheet break), pivot table (filters, columns, rows, values), Excel upload, datasheet, template sharing (site, responsibility, user), excluded parameters.
        -   `1.6 Scheduling a report`: Copy existing request, manual submission, delivery options (email, FTP, WebDAV), suppress empty file delivery.
    -   **Common Queries**: "How to run a report?", "How to schedule a Blitz Report?", "What are runtime options?", "How to use templates?", "Email report output."

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