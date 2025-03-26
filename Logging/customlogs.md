# Custom Logs Enabling Steps in OCI Instances

This document outlines the step-by-step process to enable custom log collection from Oracle Cloud Infrastructure (OCI) compute instances using the Oracle Cloud Agent Custom Logs plugin.

---

## Custom Logs Enabling Steps

| Steps | Description | Notes |
|-------|-------------|-------|
| 1 | Ensure the Oracle Cloud Agent is installed on the compute instance. | The Oracle Cloud Agent is required for all monitoring and management features. It is installed by default on most OCI images, but custom images may require manual installation. You can verify its presence via the instance details page in the Console. |
| 2 | Enable the *Custom Logs* plugin for the Oracle Cloud Agent. | Go to the instance details page → Oracle Cloud Agent → Plugins. Make sure the "Custom Logs" plugin is enabled and running. This plugin collects logs from the specified files on the instance and sends them to OCI Logging. |
| 3 | Create a Dynamic Group to include your target compute instances. | Go to Identity & Security → Dynamic Groups → Create Dynamic Group. Example rule: `ALL {instance.compartment.id = '<compartment-ocid>'}`. This group allows you to assign permissions to instances without using instance principals. |
| 4 | Create IAM Policies to allow logging operations for the Dynamic Group. | Example policy statements:<br>`Allow dynamic-group <group-name> to use log-content in compartment <compartment-name>`<br>`Allow dynamic-group <group-name> to read log-groups in compartment <compartment-name>`<br>These permissions enable instances in the dynamic group to push logs to OCI Logging. |
| 5 | Create a Logging Log Group (if not already created). | Go to Observability & Management → Logging → Log Groups → Create Log Group. Log groups are containers for organizing log data from various sources. |
| 6 | Create an Agent Configuration for Custom Logs. | Go to Logging → Agent Configurations → Create Agent Configuration. Choose type: "Custom Logs", Scope: Dynamic Group, then define the path to the log file you want to collect (e.g., `/var/log/myapp.log`). You can also set optional parsing or filtering rules. |
| 7 | Confirm logs are being collected. | Go to Logging → Logs → select your log group → View logs. You should see entries from your specified log file(s). It may take a few minutes for logs to appear after the configuration is applied. |

---

## Example: Custom Logs Configuration

Here is an example `Source` section in the Agent Configuration UI:

```json
{
  "sourceType": "LOG_PATH",
  "path": "/var/log/myapp.log",
  "logGroupId": "ocid1.loggroup.oc1..exampleuniqueID",
  "logId": "ocid1.log.oc1..exampleuniqueID"
}
