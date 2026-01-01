# 2. Installation

An installation video is available on [YouTube](https://www.youtube.com/watch?v=YOUR_VIDEO_ID) to guide the deployment of Blitz Report, which installs the necessary components on the Oracle E-Business Suite server, including database objects, user interface elements, application setups, and all items prefixed with XXEN.

---

## 2.1 Installation Steps

**1.** Log in to the application tier node as the application owner user e.g. applmgr. If you continue from prerequisites section 1.2, ensure that you re-logged in to source the application environment context, including the newly created $XXEN_TOP application.

**2.** [Download](https://www.enginatics.com/download/) the installation file 'blitz_report_*.zip' and copy or ftp it to a location of your choice on the EBS application server node.

**3.** Extract the .zip file, navigate to the folder 'blitz_report', located under the directory from where it is extracted and perform the installation by running install.sh:

```bash
unzip blitz_report_*.zip
cd blitz_report_*/
./install.sh
```

During execution, the script will ask for the APPS database user password and, in case you did not create a new custom XXEN application, it will ask for the custom application short name to use for the installation.

Please note that in case the APPS user does not have the SELECT ANY DICTIONARY granted, the script will also ask for the SYSTEM password to add the missing grant. This grant is required, as explained in the [prerequisites section](part1_prerequisites.md#14-other-prerequisites).

> **Note:** To run the installation non-interactively, you also can set parameters before script execution, e.g. using the following commands (replace values as required):

```bash
export APPS_PWD=apps
export CUSTOM_APP=<your custom application short name, if different to XXEN>
```

> **Note:** Default values for the data and index tablespaces are APPS_TS_TX_DATA and APPS_TS_TX_IDX. To set the custom data and index tablespaces, you can set these variables before script execution with the following commands (replace values as required):

```bash
export CUSTOM_TABLESPACE=APPS_TS_TX_DATA
export CUSTOM_INDEXSPACE=APPS_TS_TX_IDX
```

**4.** Review the error summary and installation log file and in case of any doubt or errors, send us the created `*_blitz_report_install_logs_*.zip` file to [support@enginatics.com](mailto:support@enginatics.com) and our support team will analyze the problem.

**5.** If you have more than one application server and the APPL_TOP is not shared, repeat Steps 1 to 3 in this section on all the other nodes. To reduce the installation time on additional nodes, you can add the parameter skip_db like this:

```bash
./install.sh skip_db
```

**6.** If you have more than one oafm nodes, run the following script on each of the nodes:

```bash
etc/generate_webservice_artifacts.sh
```

**7.** In case you would like to use the Blitz Report OAF user interface, follow one of the below steps based on your Oracle E-Business Suite version:

### If your Oracle E-Business Suite version is R12.1.3

```bash
cd $ADMIN_SCRIPTS_HOME
adapcctl.sh stop
adapcctl.sh start
```

### If your Oracle E-Business Suite version is R12.2.*

Run the adcgnjar utility which exists in the $AD_TOP/bin directory. It will create the customall.jar file under $JAVA_TOP upon successful run.

> **Note:** Please take a backup of the customall.jar file if it already exists under $JAVA_TOP before running the adcgnjar utility.

```bash
cp $JAVA_TOP/customall.jar $JAVA_TOP/customall.jar_backup
adcgnjar
```

If you sign JAR files using a HSM code signing certificate, sign customall.jar.

Copy the generated and signed customall.jar to the patch filesystem:

```bash
JAVA_TOP_PATCH=`echo $JAVA_TOP|sed "s:${RUN_BASE}:${PATCH_BASE}:g"`
cp $JAVA_TOP/customall.jar $JAVA_TOP_PATCH/customall.jar
```

Bounce apache and managed servers OACORE_SERVER:

```bash
cd $ADMIN_SCRIPTS_HOME
adapcctl.sh stop
admanagedsrvctl.sh stop oacore_server1
admanagedsrvctl.sh start oacore_server1
admanagedsrvctl.sh stop oacore_server2
admanagedsrvctl.sh start oacore_server2
.....
adapcctl.sh start
```

---

*Previous: [Prerequisites](part1_prerequisites.md) | Next: [Application Setup](part3_application_setup.md)*
