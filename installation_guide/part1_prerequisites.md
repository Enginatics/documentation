# 1. Prerequisites

## Quick Installation Summary

1. Ensure [prerequisites](#11-required-tools-and-access-credentials) are met.
2. Optionally create a new [custom application](#12-creating-a-custom-application-in-oracle-e-business-suite) to host Blitz Report objects.
3. [Configure ISG](#13-configure-oracle-ebs-integrated-soa-gateway) to enable Blitz Upload and FSG connectivity.
4. Run the [installation script](part2_installation.md) on the application node using the application owner user, e.g. applmgr, while the application services are up.
5. Update the CUSTOM.pll with the required Blitz Report changes, if you see a CUSTOM.pll related warning message in the installation log.
6. In case you want to use the Blitz Report OAF (HTML) user interface then, based on your Oracle E-Business Suite version, bounce Apache and managed servers OACORE_SERVER.
7. Setup the [access profile option](part3_application_setup.md#31-access-profile-option) for developers.
8. Run the [Blitz Report Update Menu Entries](part3_application_setup.md#33-update-menu-entries-concurrent-program) concurrent program to assign the Blitz Report function and program to menus and request groups.
9. Perform additional [application setup](part3_application_setup.md) steps.


## 1.1 Required tools and access credentials

The installation of Blitz Report is done in a similar way to Oracle standard patches by connecting to the application server as the application owner. Different to an Oracle patch deployment, the installation or upgrade should be done while the application server is up.

### Required tools

- Kitty/Putty ssh client
- WinSCP
- Edge or Chrome browser and a java client installed to access the Oracle EBS application front end
- SQL Developer or Toad for investigation tasks, in case of any issues encountered

### Required access

- Database access credentials for the APPS and the SYSTEM user
- Application server access as the application owner (e.g. applmgr)
- Oracle application access to the System Administrator responsibility

### Space requirements

- At least 1 GB of free space in APPS_TS_TX_DATA tablespace and 500 MB in APPS_TS_TX_IDX tablespace.
- At least 100 MB of free space under $APPL_TOP for EBS 12 and 500 MB for EBS 11i.
- More space may be required in case additional languages are enabled in the instance.


## 1.2 Creating a custom application in Oracle E-Business Suite

In order to keep Blitz Report separate from Oracle EBS standard code, we recommend installing it into a custom application database schema.

You can either install Blitz Report into an already existing custom application schema, or alternatively create a new application to keep Blitz Report separate from your existing custom code. If you decide to use an existing application, you can continue directly with the installation described in [section 2](part2_installation.md), otherwise follow below process to create a new application.

If you are unfamiliar with the process of creating a new custom application, please follow our instructions further below, which summarizes Oracle's detailed note 1577707.1. We suggest using application short code XXEN and application description 'Enginatics Custom Application' and have prepared customized files to run adsplice for this application for you to download.

> **Note for installation on EBS R12.2:** The procedure described in this document should be performed on the run file system while there is no patch cycle active.

> **Note for installation on EBS 11i:** AD Splice is also available for 11i through patch 3636980 as explained in Oracle note 167000.1, but not all environments might have it installed. To check if adsplice exists, execute `ls $AD_TOP/bin/adsplice` from the application owner user. If the file exists, you can continue running adsplice as explained in steps 1-4 further down. If you do not have adsplice installed yet, and you do not have access to Oracle support to download and apply patch 3636980, please follow the steps explained in [section 5.12](part5_optional_configurations.md#512-manually-creating-a-custom-application) to create a custom application manually, and then continue with step 6 below.

> **Note:** Creation of a new custom application requires an application server bounce (after completion of autoconfig) to have concurrent requests for the new application picked up by the concurrent manager. Thus, if you decide for a new application, you need to plan downtime as well.

If you decide for an installation into an existing application however, all Blitz Report installation steps can be done safely while the application is in use.

We have prepared the files required for running adsplice, which are already customized per instructions from Oracle note 1577707.1 for the new XXEN application. These files are located under adsplice directory of the extracted Blitz Report installation archive. They should suit most environments, but feel free to inspect and adjust them. For example, target tablespace names may differ on your system, or you might have application_id 57890 already in use for a different custom application.

### Steps to create custom application

**1.** Log in to the application tier node as the application owner user e.g. applmgr and ensure that the application environment context is sourced.

**2.** [Download](https://www.enginatics.com/download/) the installation file 'blitz_report_*.zip' and copy or ftp it to a location of your choice on the EBS application server node.

**3.** Extract the .zip file, navigate to the folder 'blitz_report_*', located under the directory from where it is extracted.

```bash
unzip blitz_report_*.zip
cd blitz_report_*
```

**4.** Copy the contents of adsplice directory to the $APPL_TOP/admin directory, change to that folder and run adsplice

```bash
cp $APPL_TOP/admin/topfile.txt $APPL_TOP/admin/topfile.txt_`date +%F`
cp $APPL_TOP/admin/newprods.txt $APPL_TOP/admin/newprods.txt_`date +%F`
cd adsplice/
cp *.txt $APPL_TOP/admin
cd $APPL_TOP/admin
adsplice
```

When prompted for the following, you can press Enter to accept the default location:

```
Enter the directory where your AD Splicer control file is located.
The default directory is [/oracle/VIS/apps/apps_st/appl/admin] :
```

When prompted for the following, you can press Enter to accept the default filename:

```
Please enter the name of your AD Splicer control file [newprods.txt] :
```

When prompted for the following, you can press Enter to accept the default value and regenerate the environment file:

```
Do you wish to regenerate your environment file [Yes] ?
```

AutoConfig will be run automatically as part of this procedure.

**5.** Review the AD Splice log file to ensure the procedure completed successfully.

**6.** Review the AutoConfig log file to ensure the procedure completed successfully.

**7.** Run the following query to identify if you use the shared APPL_TOP.

```sql
sqlplus apps/*****

set linesize 200
col "Node name" format a30;
col "APPL_TOP name" format a20;
col "Directory path" format a70;
select
fn.node_name "Node Name",
fat.name "APPL_TOP Name",
fat.path "Directory path"
from
fnd_appl_tops fat,
fnd_nodes fn
where
fat.node_id=fn.node_id;
```

**7.1** If APPL_TOP Name is the same for all the Node Names then it means that the APPL_TOP is shared. Simply run AutoConfig (there is no need to rerun AD Splice) on the additional (internal and external) nodes.

**7.2** If APPL_TOP is shared between the internal nodes, but is not shared for an external (e.g. iSupplier) node, repeat Steps 1 to 5 in this section on the external node. It is required to avoid adop issues due to the missing XXEN product top.

**7.3** If the APPL_TOP is not shared between the internal nodes, repeat Steps 1 to 5 in this section on all the other internal nodes.

**8.** Run AutoConfig on the admin node in case it has been run on other nodes. It is required to make sure that the Oracle EBS login works correctly.

**9.** In case you already have other custom products created, verify that they are present in the following files after autoconfig:

```bash
grep xx $APPL_TOP/admin/topfile.txt
grep s_xx $CONTEXT_FILE
grep c_xx $CONTEXT_FILE
```

In case the custom products are missing, add them manually to $APPL_TOP/admin/topfile.txt and rerun autoconfig.

**10.** Apply the new applications environment file which was generated by autoconfig.

Or just logout and login again to the applmgr session in case the environment file is applied automatically upon login.

To ensure the new environment file is picked up, run the following command to confirm the $XXEN_TOP variable is correctly set:

```bash
env | grep XXEN
```

**11.** Change the XXEN schema password.

```bash
FNDCPASS apps/***** 0 Y system/****** ORACLE XXEN *******
```

**12.** Stop and restart the application tier services.



## 1.3 Configure Oracle EBS Integrated SOA Gateway

The Blitz Upload and Financial Statement features require the Oracle EBS Integrated SOA Gateway (ISG) to be configured. Please refer to the following blog posts for [EBS R12.1.3](https://www.enginatics.com/blog/configuring-oracle-ebs-integrated-soa-gateway-isg-in-ebs-r12-1-3/) and [EBS R12.2](https://www.enginatics.com/blog/configuring-oracle-ebs-integrated-soa-gateway-isg/) to configure the ISG in your instances.



## 1.4 Other prerequisites

Blitz Report runs on Oracle E-Business Suite versions R12 and 11i and requires database version 11g or above.

Both the database and EBS application services should be running.

Please note that the target database schema for the Blitz Report installation requires sufficient privileges to create Oracle text (ctxsys.context) indexes e.g. by having the database role RESOURCE granted.

To make use of the DBA performance tuning reports, and to enable view expansion when importing Discoverer folders, the APPS database user requires the SELECT ANY DICTIONARY privilege granted, which is not included by default in all EBS installations.

```sql
grant select any dictionary to apps
```

In case this grant is missing, the installation script will ask for the SYSTEM password and add this grant automatically.

Please note that granting the SELECT_CATALOG_ROLE role is not sufficient. To use dictionary tables in PLSQL code, the user must have the direct grant on the base object, not through a role, which can be validated by the following query (should return one row if the grant exists):

```sql
select dsp.* from dba_sys_privs dsp where dsp.grantee='APPS' and dsp.privilege='SELECT ANY DICTIONARY'
```



*Next: [Installation](part2_installation.md)*
