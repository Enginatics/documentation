# 6. Upgrade



To perform the Supply Chain Hub upgrade, download and deploy the latest installation file in the same way as for a new installation by following steps 1-3 in the [Installation](part2_installation.md) section.

## Upgrade Steps

1. Log in to the application tier node as the application owner user e.g. `applmgr`.

2. Download the latest installation file `supply_chain_hub_*.zip` and copy or ftp it to a location of your choice on the EBS application server node.

3. Extract the .zip file and navigate to the folder `supply_chain_hub`, located under the directory from where it is extracted. Add execution permissions for shell scripts and perform the installation by running `install.sh`:

```bash
unzip supply_chain_hub_*.zip
cd supply_chain_hub_*/
./install.sh
```

> **Note:** Ignore any `ORA-00955: name is already used by an existing object` error messages. These are expected during upgrade as the objects already exist from the previous installation.



*Previous: [Profile Options](part4_profile_options.md) | Next: [Troubleshooting](part6_troubleshooting.md)*
