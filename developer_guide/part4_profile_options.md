# 4. Profile Options

The profile option 'Blitz Report Access' should be set to 'Developer' for users with the SQL skills required for creating Blitz Reports.

The following optional profile options can be set to configure Blitz Report functionality according to your business and user requirements:

## Core Profile Options

| Profile Option | Description | Default if not set |
|---------------|-------------|-------------------|
| **Blitz Report Access** | Controls access to Blitz Report.<br>**User:** run assigned reports or uploads only<br>**User Admin:** access and run all reports, create or modify report assignments, view only access to report SQL and setup<br>**Developer:** full access, except uploads, system and protected reports<br>**System:** full access, including uploads, system and protected reports | No access to run reports |
| **Blitz Report Additional Output Directory APPS Server** | Directory path on the applications server to write output files to (in addition to writing them to $APPLCSF/out) | |
| **Blitz Report Additional Output Directory DB Server** | Directory name on the database server to write output files to (in addition to writing them to $APPLCSF/out) | |
| **Blitz Report Additional Output Filename** | Filename for output files written to a directory on the DB or apps server | |
| **Blitz Report CSV File Delimiter** | Delimiting character in Blitz Report CSV output files | `,` |
| **Blitz Report Debug** | Set to 'Yes' to write additional debug information, for example in the upload processing logs | No |
| **Blitz Report Default Email Address** | Default email address for Blitz Reports | |

## Column and Translation Options

| Profile Option | Description | Default if not set |
|---------------|-------------|-------------------|
| **Blitz Report Disable Column Translations** | Set to 'Yes' to disable automated column header translations | No |
| **Blitz Report Disable Copied From Tracking** | Disables population of the Copied From field when copying reports | No |
| **Blitz Report Disable GL Flex Value Security** | Allows disabling the GL flex value security for customers experiencing slow performance of GL Balance or GL Account Analysis reports | No |
| **Blitz Report Disable Run Button after Submission** | After report submission, the run button is disabled to prevent accidental resubmission | Yes |
| **Blitz Report Disable SQL Text Double Click Editor Window** | Disables the default behavior to open an editor window when double clicking on the report SQL text | No |

## Discoverer Import Options

| Profile Option | Description | Default if not set |
|---------------|-------------|-------------------|
| **Blitz Report Discoverer Default EUL** | Sets the default Discoverer end user layer | Most recently created EUL |
| **Blitz Report Discoverer Folder Import Include Columns** | 'Active' imports columns used by active worksheets only; 'All' includes all columns | Active |
| **Blitz Report Discoverer Import Literals as Binds** | When set to 'Yes', automatically replaces literals with binds during Discoverer import | No |
| **Blitz Report Discoverer Import LOV Access History Days** | Number of access history days for the Discoverer worksheet import LOV | 90 days |
| **Blitz Report Discoverer Import Preserve Original SQL** | Set to 'Yes' to turn off additional enhancements to imported Discoverer SQLs | No |
| **Blitz Report Discoverer Import Report Name Prefix** | Prefix for imported Discoverer reports | |

## Email Options

| Profile Option | Description | Default if not set |
|---------------|-------------|-------------------|
| **Blitz Report Email Attachment Filename Length Limit** | Truncates report attachment filenames to the specified limit | 1000 characters |
| **Blitz Report Email Attachment Multibyte Filenames** | When set to 'Yes', email attachment filenames are sent including UTF-8 multibyte characters | Yes for Linux, No for other OSs |
| **Blitz Report Email Attachment Size Limit (bytes)** | Maximum attachment size for Blitz Reports sent via email | no limit |
| **Blitz Report Email Body Message** | Message name for Blitz Report outbound emails bodies (must start with XXEN_REPORT_EMAIL_BODY%) | XXEN_REPORT_EMAIL_BODY |
| **Blitz Report Email Subject Message** | Message name for Blitz Report outbound emails subjects (must start with XXEN_REPORT_EMAIL_SUBJECT%) | XXEN_REPORT_EMAIL_SUBJECT |
| **Blitz Report From Email Address** | Email address that Blitz Report emails are sent from. Format: `Enginatics GmbH <support@enginatics.com>` | Email address from user or employee record |
| **Blitz Report SMTP Host** | SMTP Host for Blitz Report advanced email delivery | |
| **Blitz Report SMTP Port** | SMTP Port for Blitz Report advanced email delivery | |
| **Blitz Report SMTP use SSL** | Use SSL for Blitz Report advanced email delivery | No |
| **Blitz Report Use Advanced Email Delivery** | Set to "Yes" to use Blitz Report Advanced Email Delivery instead of the standard Oracle EBS Delivery Option | No |

## Output and File Options

| Profile Option | Description | Default if not set |
|---------------|-------------|-------------------|
| **Blitz Report File Name Convention** | Defines the format for output filenames (report name, template name or both) | Report Name – Template Name |
| **Blitz Report Filter Reports by Responsibility** | Yes: LOV filtered to current login responsibility only | Yes for users, No for developers |
| **Blitz Report Include SQL in Log** | When set to 'No', the report SQL is not included in the log file | Yes |
| **Blitz Report Include SQL in XLSX Output** | When set to 'No', the report SQL is not included on the parameter tab | Yes |
| **Blitz Report Output Button Refresh Interval** | Blitz Report output button refresh interval in seconds | 1 |
| **Blitz Report Output Filename Length Limit** | Output file name full path length limit | 160 |
| **Blitz Report Output Format** | Blitz Report output file format (CSV, TSV or XLSX) | XLSX |

## Log and Retention Options

| Profile Option | Description | Default if not set |
|---------------|-------------|-------------------|
| **Blitz Report License Expiration Warning Days** | Number of days warning is shown before license expiration | 14 |
| **Blitz Report Load Default Assignments during Upgrades** | When set to 'Yes' default assignments are loaded during upgrades | No |
| **Blitz Report Log Retention Days Error** | Number of days to keep log data for errored or cancelled report runs | no purge |
| **Blitz Report Log Retention Days Standard** | Number of days to keep log data for successful standard report runs | no purge |
| **Blitz Report Log Retention Days System** | Number of days to keep log data for successful system type report runs | no purge |
| **Blitz Report LOV History Days** | Number of days to identify users most recently run reports for 'favourites on top' | 365 |

## User Interface Options

| Profile Option | Description | Default if not set |
|---------------|-------------|-------------------|
| **Blitz Report Maintenance Mode** | Set to 'Yes' by install.sh during upgrades | No |
| **Blitz Report Row Limit** | Default limit for maximum number of rows returned | no limit |
| **Blitz Report Show All Templates** | Set to 'Yes' to show all templates for all reports | No |
| **Blitz Report Show Hidden Parameters** | Allows Developers to show hidden parameters for debugging | No |
| **Blitz Report Show Log Instead of Output** | Set to 'Yes' to open the logfile instead of report output | No |
| **Blitz Report Show Tooltip Help** | Enable or disable tooltip help display | Yes |
| **Blitz Report Show User Description** | When set to 'No', additional FND user description is not shown | Yes |
| **Blitz Report Skip Column Translations during Upgrades** | When set to 'Yes', column translation imports are skipped during upgrades | No |
| **Blitz Report Start Tab** | Allows developers to directly open the specified start tab | SQL |
| **Blitz Report Suppress Empty File Delivery** | Suppress file delivery if report is empty | Yes |

## Security Options

| Profile Option | Description | Default if not set |
|---------------|-------------|-------------------|
| **Blitz Report SSO Enabled** | Set to 'Yes' for SSO (Single Sign-on) enabled EBS instances | No |
| **Blitz Report Target Database** | Allows running reports on a standby database. Specify TNS descriptor | |
| **Blitz Report Template Access** | Controls template creation/modification rights:<br>**No Access:** view and use only<br>**Private only:** create private templates only<br>**Private and shared:** create private and shared templates<br>**Super User:** modify other owner's templates | Private and shared |
| **Blitz Report Template Excel Upload Size Limit** | Size limit in MB for warning when uploading large template files | 5 MB |
| **Blitz Report Time Limit in minutes** | Default maximum execution time limit | no limit |
| **Blitz Report Trace Level** | Trace level for the Blitz Report database session | off (null) |
| **Blitz Report Use Ledger Security** | When 'Yes', also restrict by ledger access set | No |
| **Blitz Report Use Operating Unit Security** | When 'Yes', also restrict by operating unit | No |
| **Blitz Report VPD Policy Rule** | Allows setting up access to sensitive data protected through VPD policies | No access |

## XLSX Format Options

| Profile Option | Description | Default if not set |
|---------------|-------------|-------------------|
| **Blitz Report XLSX Column Header Color** | Hexadecimal code for column header background color | eaeff5 |
| **Blitz Report XLSX Column Width Mode** | Column width alignment (headers, data or both) | both |
| **Blitz Report XLSX Column Width Scale Percentage** | Adjust automatically calculated column width | 100 |
| **Blitz Report XLSX Date Format** | Date format mask for XLSX output files | value from 'ICX: Date format mask' |
| **Blitz Report XLSX Font** | XLSX output file font | Calibri |
| **Blitz Report XLSX Font Size** | XLSX output file font size | 10 |
| **Blitz Report XLSX Max Column Width** | Maximum column width | 60 |
| **Blitz Report XLSX Min Column Width** | Minimum column width | 2 |
| **Blitz Report XLSX Number Format** | Numeric column format string | General |
| **Blitz Report XLSX Orientation** | Page layout orientation | Landscape |
| **Blitz Report XLSX Sheet Footer** | Footer in XLSX sheets | |
| **Blitz Report XLSX Sheet Header** | Header in XLSX sheets | report name only |
| **Blitz Report XLSX Sheet Row Limit** | Maximum number of rows per excel sheet | 1048576 |
| **Blitz Report XLSX Use Long Date Format** | When 'Yes', show full year and leading zeroes (e.g. 05-Jan-2018) | No |

## Blitz Upload Options

| Profile Option | Description | Default if not set |
|---------------|-------------|-------------------|
| **Blitz Upload Custom Sheet Parsing** | Use Enginatics custom code instead of Oracle's buildin xml functions | No |
| **Blitz Upload Data Retention Days** | Number of days to keep data in table xxen_upload_data | 0 |
| **Blitz Upload Maximum LOV Size** | Maximum upload list of values size in MB | 2 MB |
| **Blitz Upload Maximum Request Payload Size** | Maximum payload size for data upload webservice requests in MB | 0.4 MB |
| **Blitz Upload Maximum Rows** | Maximum number of rows in a Blitz Upload excel sheet | 100000 |
| **Blitz Upload Round to Decimal Places** | Number of decimal places used for rounding number values | 5 |
| **Blitz Upload Use mod_plsql** | Use mod_plsql for establishing server connections from the upload excel | No |

## Blitz FSG Options

| Profile Option | Description | Default if not set |
|---------------|-------------|-------------------|
| **Blitz FSG Disable Summary Templates** | Use query on detail balances instead of summary templates | Summary templates used |
| **Blitz FSG Doubleclick To Balance Function** | Double click on balance opens balance function form or executes balance drilldown | Balance drilldown |
| **Blitz FSG Drilldown Report GFJ** | Report Name for GL Full Journal drilldown | GL Journals (Drilldown) |
| **Blitz FSG Drilldown Report GJ** | Report Name for GL Journal drilldown | GL Journals (Drilldown) |
| **Blitz FSG Drilldown Report GJA** | Report Name for GL Journal with Attachments drilldown | GL Journals (Drilldown) |
| **Blitz FSG Drilldown Report SD** | Report Name for Subledger details drilldown | GL Account Analysis (Drilldown) |
| **Blitz FSG Drilldown Template GFJ** | Template Name for GL Full Journal drilldown | GL Full Journal Drilldown |
| **Blitz FSG Drilldown Template GJ** | Template Name for GL Journal drilldown | GL Journal Drilldown |
| **Blitz FSG Drilldown Template GJA** | Template Name for GL Journal with Attachments drilldown | GL Journal Drilldown (Attachment) |
| **Blitz FSG Drilldown Template SD** | Template Name for Subledger details drilldown | FSG Subledger Details |
| **Blitz FSG Drilldown to same workbook** | Balance drilldown opens in same or new workbook | Same workbook |
| **Blitz FSG Drilldown View Transaction** | Responsibility used to view subledger transaction | |
| **Blitz FSG Oracle to Blitz Report Converter** | Report Name for Oracle FSG to Blitz converter | GL Oracle FSG Converter |
| **Blitz FSG Webservice Functions Upper Limit** | Upper limit for maximum functions via webservice call | 10000 |
| **Blitz FSG Webservice Maximum Functions** | Maximum functions via webservice call | 1000 |
| **Blitz FSG Webservice Timeout** | Webservice timeout threshold in seconds | 120 |



*Source: [Enginatics Blitz Report Developer Guide](https://www.enginatics.com/blitz-report-developer-guide/)*
