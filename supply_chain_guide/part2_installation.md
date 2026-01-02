# 3. Installation

---

Follow these steps to install the Supply Chain Hub:

1. Log in to the application tier node as the application owner user e.g. `applmgr`.

2. Download the installation file `supply_chain_hub_*.zip` and copy or ftp it to a location of your choice on the EBS application server node.

3. Extract the .zip file and navigate to the folder `supply_chain_hub`, located under the directory from where it is extracted. Add execution permissions for shell scripts and perform the installation by running `install.sh`:

```bash
unzip supply_chain_hub_*.zip
cd supply_chain_hub_*/
./install.sh
```

During execution, the script will ask for the APPS database user password.

> **Note:** To run the installation non-interactively, you also can set the apps password before script execution, for example:

```bash
export APPS_PWD=apps
```

Review the error summary and installation log file and in case of any errors, please contact our support and send us the created `supply_chain_hub_install_logs_*.zip` file to analyze the problem.

> **Note:** If you are running decentralized ASCP and wish to use the Action Release/Reschedule/Cancel recommendations functionality from the Supply Chain Hub running in the EBS Instance; then you must also install Blitz Report on the ASCP instance.

---

*Previous: [Prerequisites](part1_prerequisites.md) | Next: [Application Setup](part3_application_setup.md)*
