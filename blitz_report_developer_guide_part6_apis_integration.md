# Blitz Report Developer Guide

## 5. APIs and Integration

---

### 5.1 Export

To make migration tasks easier Blitz Report offers the following Export APIs:

- [Blitz Report Library Export API](https://www.enginatics.com/blitz-report-developer-guide/#export_api)

---

### 5.2 Import

To make migration tasks easier Blitz Report offers the following Import APIs:

- [XML Upload Import API](https://www.enginatics.com/blitz-report-developer-guide/#import_api)
- [BI Publisher Import API](https://www.enginatics.com/blitz-report-developer-guide/#bi_publisher)
- [Discoverer Worksheet Import API](https://www.enginatics.com/blitz-report-developer-guide/#discoverer_worksheet)
- [Discoverer Folders Import API](https://www.enginatics.com/blitz-report-developer-guide/#discoverer_folders)
- [Excel4apps Reports Wand Import API](https://www.enginatics.com/blitz-report-developer-guide/#excel4apps)
- [Enterprise Command Center Import API](https://www.enginatics.com/blitz-report-developer-guide/#ecc)
- [Polaris Reporting Workbench Import API](https://www.enginatics.com/blitz-report-developer-guide/#polaris)

---

### 5.3 Submitting Blitz Report from PLSQL

Blitz reports can be started from PLSQL as a background concurrent program, or through an API call returning the Excel output as a blob.

#### FND Concurrent Program API

Blitz reports can be submitted through the Oracle standard `fnd_request.submit_request` API. Arguments 1-15 specify the report and template names, and other runtime options, such as the email address or output format. The user entered report parameters start from arguments16 onwards.

```sql
declare
  l_request_id number;
begin
  xxen_report.apps_initialize('ENGINATICS','RECEIVABLES_VISION_OPERATIONS','AR');
  --User Name, Responsibility Key, Resp. App. Short Code

  l_request_id:=fnd_request.submit_request(
    application=>'XXEN',           --Application short code of the Blitz Report concurrent program
    program=>'XXEN_REPORT',
    description=>'AR Past Due Invoice', --Request Description
    argument1 =>replace('AR Past Due Invoice','"','\"'), --Blitz Report Name
    argument2 =>null,              --Run Id
    argument3 =>null,              --Template Name
    argument4 =>'user@domain.com', --Email
    argument5 =>null,              --Output Format
    argument6 =>null,              --Row Limit
    argument7 =>null,              --Time Limit
    argument8 =>null,              --Disable Column Translations
    argument9 =>null,              --Exclude Column Headers
    argument10 =>null,             --Custom Postprocess
    argument11 =>null,             --Output file name
    argument12 =>null,             --Additional out dir APPS server
    argument13 =>null,             --Additional out dir DB server
    argument14 =>null,             --Additional out file name
    argument15 =>null,             --Organization Code
    argument16 =>'Vision Operations', --Parameter1
    argument17 =>'Customer',       --Parameter2
    argument18 =>'04-MAY-2007'     --Parameter3
  );

  if l_request_id=0 then
    dbms_output.put_line('Error: '||fnd_message.get);
  else
    dbms_output.put_line('Submitted Request Id: '||l_request_id);
    commit;
  end if;
end;
```

#### Blitz Report Concurrent Program API

You can also use the API `xxen_api.report_submit_concurrent_`, which allows referencing the report, parameter and template names by name or id instead of argument position:

```sql
declare
  l_error_message varchar2(4000);
  l_request_id number;
begin
  xxen_report.apps_initialize(
    p_user_name=>'ENGINATICS',
    p_responsibility_key=>'RECEIVABLES_VISION_OPERATIONS',
    p_application_short_name=>'AR'
  );

  xxen_api.set_parameter_value_(
    p_parameter_name=>'Operating Unit',
    p_value=>'Vision Operations',
    p_report_name=>'AP Suppliers'
  );

  xxen_api.set_runtime_option(
    p_runtime_option_name=>'TEMPLATE_NAME',
    p_value=>'Pivot template'
  );

  l_error_message:=xxen_api.report_submit_concurrent_(
    p_report_name=>'AP Suppliers',
    x_request_id=>l_request_id
  );

  if l_request_id=0 then
    dbms_output.put_line('Error: '||l_error_message);
  else
    dbms_output.put_line('Submitted Request Id: '||l_request_id);
    commit;
  end if;
end;
```

#### Runtime Option Names

You can specify the following runtime option names in procedure `set_runtime_option`:

| Option Name | Description |
|-------------|-------------|
| `TEMPLATE_NAME` | Template name to use |
| `EMAIL` | Email address for output delivery |
| `OUTPUT_FORMAT` | Output format (CSV, TSV, XLSX) |
| `ROW_LIMIT` | Maximum rows to return |
| `TIME_LIMIT` | Maximum execution time |
| `DISABLE_COLUMN_TRANSLATIONS` | Disable column header translations |
| `EXCLUDE_COLUMN_HEADERS` | Exclude column headers from output |
| `CUSTOM_POSTPROCESS` | Custom postprocess script |
| `OUTPUT_FILENAME` | Output file name |
| `ADDITIONAL_OUT_DIR_APPS` | Additional output directory on APPS server |
| `ADDITIONAL_OUT_DIR_DB` | Additional output directory on DB server |
| `ADDITIONAL_OUT_FNAME` | Additional output file name |
| `ORGANIZATION_ID` | Organization ID |

#### PLSQL API Returning a BLOB

You can create a blitz report from a PLSQL API. First create a new run_id, then set all parameter and runtime option values, and finally call the report creation API:

```sql
declare
  l_run_id number;
  l_blob blob;
  l_row_count pls_integer;
  l_filename varchar2(1000);
  l_return_status varchar2(1);
  l_msg_data varchar2(4000);
begin
  xxen_report.apps_initialize('ENGINATICS','RECEIVABLES_VISION_OPERATIONS','AR');
  --User Name, Responsibility Key, Resp. App. Short Code

  l_run_id:=xxen_api.create_run_id_(p_report_name=>'AP Suppliers');

  xxen_api.set_parameter_value_(
    p_parameter_name=>'Operating Unit',
    p_value=>'Vision Operations',
    p_run_id=>l_run_id
  );

  --xxen_api.set_runtime_option(p_runtime_option_name=>'TEMPLATE_NAME', p_value=>'Pivot template', p_run_id=>l_run_id); --hardcoded template
  xxen_api.set_runtime_option(
    p_runtime_option_name=>'TEMPLATE_NAME',
    p_value=>xxen_api.default_template_(p_report_name=>'AP Suppliers'),
    p_run_id=>l_run_id
  ); --user specific template

  xxen_api.run_report(
    p_run_id=>l_run_id,
    x_row_count=>l_row_count,
    x_output_file=>l_blob,
    x_filename=>l_filename,
    x_return_status=>l_return_status,
    x_msg_data=>l_msg_data
  );

  dbms_output.put_line('run_id: '||l_run_id||', row_count: '||l_row_count||', output size: '||length(l_blob)||', filename: '||l_filename||', l_return_status: '||l_return_status||', l_msg_data: '||l_msg_data);
end;
```

#### Opening Reports from Custom Forms

Using the above PLSQL API returning a BLOB, you can open a blitz report directly from a custom form, for example in a when-button-pressed trigger.

The file download is done through procedure `xxen_utils.download_file`, which is included in the `XXEN.pll` library, so you would need to attach this library to your custom form to use this functionality.

**Forms code example:**

```sql
declare
  l_run_id number:=xxen_api.create_run_id_('XXSP Bid Analysis');
begin
  if :rfq_header.rfq_number is not null then
    xxen_api.set_parameter_value_(
      p_parameter_name=>'RFQ Number',
      p_value=>:rfq_header.rfq_number,
      p_run_id=>l_run_id
    );
    if :rfq_line.rfq_line_number is not null then
      xxen_api.set_parameter_value_(
        p_parameter_name=>'RFQ Line Number',
        p_value=>:rfq_line.rfq_line_number,
        p_run_id=>l_run_id
      );
    end if;
    xxen_api.set_runtime_option(
      p_runtime_option_name=>'TEMPLATE_NAME',
      p_value=>xxen_api.default_template_(p_report_name=>'XXSP Bid Analysis'),
      p_run_id=>l_run_id
    );
    xxen_utils.download_file(xxen_api.report_file_id(l_run_id));
  end if;
end;
```

---

### 5.4 Useful DB Functions

There is a list of useful functions from custom package `XXEN_UTIL` that can be used during report creation.

#### XXEN_UTIL.CLIENT_TIME (p_date in date)

This function allows converting date from server's timezone to client local timezone.

- **Input parameter:** Date in server timezone
- **Result:** Date in client timezone

Server timezone derived from profile value 'Server Timezone'.
Client timezone derived from profile value 'Client Timezone'.

**Example:**

Assume:
- Server Timezone set to GMT+1
- Client Timezone set to GMT+2
- Server's current time is 2020.09.03 09:00:00

| SQL Statement | Result |
|--------------|--------|
| `select to_char(xxen_util.client_time(to_date('2020.09.03 09:00:00', 'YYYY.MM.DD HH24:Mi:Ss')), 'YYYY.MM.DD HH24:Mi:Ss') from dual` | 2020.09.03 10:00:00 |

#### XXEN_UTIL.SERVER_TIME (p_date in date)

This function works in the similar way as above. It checks if server timezone differs from client timezone and converts date if needed.

**Example:**

Assume:
- Server Timezone set to GMT+1
- Client Timezone set to GMT+2
- Client current time is 2020.09.03 09:00:00

| SQL Statement | Result |
|--------------|--------|
| `select to_char(xxen_util.server_time(to_date('2020.09.03 09:00:00', 'YYYY.MM.DD HH24:Mi:Ss')), 'YYYY.MM.DD HH24:Mi:Ss') from dual` | 2020.09.03 08:00:00 |

#### XXEN_UTIL.TIME (p_seconds in number)

This function converts number of seconds to the time in format "67d 14h 45.3s".

- **Input parameter:** Number of seconds
- **Result:** String showing days, hours, minutes and seconds

**Examples:**

| SQL Statement | Result |
|--------------|--------|
| `select xxen_util.time(3600) from dual` | 1h 0m 0s |
| `select xxen_util.time(361012) from dual` | 4d 4h 16m 52s |
| `select xxen_util.time(36101200) from dual` | 417d 20h 6m 40s |

#### XXEN_UTIL.USER_NAME (p_user_name in varchar2)

This function converts user name to the full description of the user.

- **Input parameter:** User Name
- **Result:** User Description

**Example:**

| SQL Statement | Result |
|--------------|--------|
| `select xxen_util.user_name('SYSADMIN') from dual` | SYSADMIN (System Administrator) |

#### XXEN_UTIL.USER_NAME (p_user_id in pls_integer)

Similar to the function above, returns user name and user description based on user id.

> **Note:** User description can be deactivated by profile option 'Blitz Report Show User Description'.

- **Input parameter:** User ID
- **Result:** User name and user description

**Example:**

| SQL Statement | Result |
|--------------|--------|
| `select xxen_util.user_name(0) from dual` | SYSADMIN (System Administrator) |

#### XXEN_UTIL.USER_ID (p_user_name in varchar2)

Function returns user id based on user name.

- **Input parameter:** User name
- **Result:** User ID

**Example:**

| SQL Statement | Result |
|--------------|--------|
| `select xxen_util.user_id('SYSADMIN') from dual` | 0 |

#### XXEN_UTIL.DFF_COLUMNS

Returns a SQL text for the descriptive flexfield columns of a specified table, to be used for dynamic `&lexical` replacement.

```sql
select
xxen_util.dff_columns(
  p_table_name=>'mtl_system_items_b',
  p_table_alias=>'msiv', --Table alias if different than the table name standard
  p_descr_flex_context_code=>null, --Restrict to a specific flexfield context
  p_column_name_prefix=>null, --Prefix for dff column name
  p_prefix=>null --Prefix for column name to replace table attribute columns
) dff_column_text
from dual
```

**Parameters:**

| Parameter | Description |
|-----------|-------------|
| `p_table_name` | Name of the table with DFF |
| `p_table_alias` | Table alias (e.g., 'msiv' generates: `msiv.attribute15 "Invoice UOM"`) |
| `p_descr_flex_context_code` | Restrict to specific flexfield context. Default shows all from all contexts |
| `p_column_name_prefix` | Prefix for dff column name (e.g., 'Item: ' creates: `msiv.attribute15 "Item: Invoice UOM"`) |
| `p_prefix` | Prefix for outer query (e.g., 'x.' generates: `x."Invoice UOM"`) |

---

*Source: [Enginatics Blitz Report Developer Guide](https://www.enginatics.com/blitz-report-developer-guide/)*
