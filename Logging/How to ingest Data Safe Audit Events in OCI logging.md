### How to ingest Data Safe Audit Events in OCI logging

One of the main advantages of running Oracle Databases in OCI, besides the automation, performance and advance functions, is also that you get Data Safe free of charge. As a short resume, Oracle Data Safe is a unified control center for your Oracle databases which helps you understand the sensitivity of your data, evaluate risks to data, mask sensitive data, implement and monitor security controls, assess user security, monitor user activity, and address data security compliance requirements.

As stated in OCI Documentation,

> Oracle Data Safe provides the following set of features for protecting sensitive and regulated data in Oracle databases, all in a single, easy-to-use management console:

> **Security Assessment** helps you assess the security of your database configurations. It analyzes database configurations, user accounts, and security controls, and then reports the findings with recommendations for remediation activities that follow best practices to reduce or mitigate risk.

> **User Assessment** helps you assess the security of your database users and identify high risk users. It reviews information about your users in the data dictionary on your target databases, and calculates a risk score for each user. For example, it evaluates the user types, how users are authenticated, the password policies assigned to each user, and how long it has been since each user has changed their password. It also provides a direct link to audit records related to each user. With this information, you can then deploy appropriate security controls and policies.

> **Data Discovery** helps you find sensitive data in your databases. You tell Data Discovery what kind of sensitive data to search for, and it inspects the actual data in your database and its data dictionary, and then returns to you a list of sensitive columns. By default, Data Discovery can search for a wide variety of sensitive data pertaining to identification, biographic, IT, financial, healthcare, employment, and academic information.

> **Data Masking** provides a way for you to mask sensitive data so that the data is safe for non-production purposes. For example, organizations often need to create copies of their production data to support development and test activities. Simply copying the production data exposes sensitive data to new users. To avoid a security risk, you can use Data Masking to replace the sensitive data with realistic, but fictitious data.

> **Activity Auditing** lets you audit user activity on your databases so you can monitor database usage.

> **Alerts** keep you informed of unusual database activities as they happen.

[Oracle Data Safe Overview](https://docs.oracle.com/en-us/iaas/data-safe/doc/oracle-data-safe-overview.html#:~:text=Oracle%20Data%20Safe%20is%20a,address%20data%20security%20compliance%20requirements.)

[https://www.youtube.com/watch?v=UUc26bpdFnc](https://www.youtube.com/watch?v=UUc26bpdFnc)

![](https://cdn-images-1.medium.com/max/1200/1*HC3qUe9KUZ172LTkvHfV4A.png)

As shown in this picture, the main feature I want to leverage today is the Activity Auditing.

> Activity Auditing lets you collect audit data from your target databases so that you can monitor database activities.

All the audited activities can be checked [here](https://docs.oracle.com/en-us/iaas/data-safe/doc/activity-auditing-overview.html#GUID-741E8CFE-041E-46C4-9C04-D849573A4DB7).

Now, if a customer want to get all the Database activities and use them in their own Analytics tools, or just to store the full Audit_Trail of the DB, without effort, adding the DB to Data Safe and creating Audit Policies is the fastest way.

This is blog is not about how to crate Audit Policies, so I will consider that all the policies are properly configured, and at least the Pre-defined policies are there. We need this so we can get this events.

![](https://cdn-images-1.medium.com/max/1200/1*Dmo_89svLMTS2_G29EWI1A.png)

Now that we have the cofirmation that we have the DB audit events we will get them outside of OCI.

My first interaction with the Data Safe export option was using OCI CLI.

oci data-safe audit-event-summary list-audit-events — compartment-id ocid1.compartment.oc1..PutYourCompartment OCIDHere >> datasafe.log

This command will list all the Audit Events generated for the DB’s that are running and Audited by Data Safe from that compartment and the >> exports the output of the command to the datasafe.log file in JSON format.

This file can be uploaded now manually in other tools.

![](https://cdn-images-1.medium.com/max/1200/1*OuuOgMyKpOSZDZ6gN8ryQA.png)

The JSON File format is this:

      {  
        "action-taken": null,  
        "audit-event-time": "2023-01-16T18:40:26.087000+00:00",  
        "audit-location": "AUDIT_TABLE",  
        "audit-policies": "ORA_DV_AUDPOL",  
        "audit-trail-id": null,  
        "audit-type": "STANDARD",  
        "client-hostname": "e2pod-imwp78.subdb1.vcnadwcfra1.oraclevcn.com",  
        "client-id": null,  
        "client-ip": "172.17.0.1",  
        "client-program": "oracle@e2pod-imwp78 (J000)",   
        "command-param": " #1(21):ORA_TEMP_2_DS_1721605 #2(3):SYS",  
        "command-text": "DELETE FROM LBACSYS.OLS$POLT WHERE TBL_NAME = :B2 AND OWNER = :B1 ",  
        "compartment-id": "ocid1.compartment.oc1..aaaaaaaarsk54oz6cd4kd5pgnrgqcxiyd2ldo2n5zx6psna2hyln6vtzkgka",  
        "database-type": "AUTONOMOUS_DATABASE",  
        "db-user-name": "SYS",  
        "defined-tags": null,  
        "error-code": null,  
        "error-message": null,  
        "event-name": "DELETE",        "extended-event-attributes": "PROXY_SESSIONID=0::INSTANCE_ID=2::SESSIONID=2136013151::AUTHENTICATION_TYPE=(TYPE=(DATABASE));(CLIENT ADDRESS=((PROTOCOL=unknown)(HOST=172.17.0.1)));::DBID=3236716633::DBPROXY_USERNAME=null::EXTERNAL_USERID=null::GLOBAL_USERID=null::DBLINK_INFO=null::STATEMENT_ID=3::OS_PROCESS=201551::TRANSACTION_ID=0000000000000000::SCN=39215587987241::EXECUTION_ID=null::OBJECT_SCHEMA=LBACSYS::APPLICATION_CONTEXTS=(USERENV,AUTHENTICATED_IDENTITY=)::NEW_SCHEMA=null::NEW_NAME=null::OBJECT_EDITION=null::SYSTEM_PRIVILEGE_USED=null::SYSTEM_PRIVILEGE=null::AUDIT_OPTION=null::OBJECT_PRIVILEGES=null::ROLE=null::TARGET_USER=null::EXCLUDED_USER=null::EXCLUDED_SCHEMA=null::EXCLUDED_OBJECT=null::ADDITIONAL_INFO=AutoTask++ OperationDB_UNIQUE_NAME=\"ep71pod\";::DIRECT_PATH_NUM_COLUMNS_LOADED=null::CURRENT_USER=LBACSYS::KSACL_USER_NAME=null::KSACL_SERVICE_NAME=null::KSACL_SOURCE_LOCATION=null",  
        "freeform-tags": null,  
        "id": "ocid1.auditevent.oc1..aaaaaaaa3gdlbwwoteivc2lkjvkmmr4ktwjltyfhsbrsuargurrubozgbq6a8679526",  
        "is-alerted": false,  
        "object-name": "OLS$POLT",  
        "object-owner": "LBACSYS",  
        "object-type": null,  
        "operation": "DELETE",  
        "operation-status": "SUCCESS",  
        "os-terminal": "UNKNOWN",  
        "client-program": "oracle@e2pod-imwp78 (J000)",  
        "command-param": null,  
        "command-text": "truncate table sys.ora_temp_2_ds_1721605",  
        "compartment-id": "ocid1.compartment.oc1..aaaaaaaarsk54oz6cd4kd5pgnrgqcxiyd2ldo2n5zx6psna2hyln6vtzkgka",  
        "database-type": "AUTONOMOUS_DATABASE",  
      } "db-user-name": "SYS",

As I really love the OCI Logging Capabilities, and the PutLogAPI function, i decided to get the Data Safe Audit logs and Copy it to Logging.

I have created a new Custom log and named it DataSafe:

![](https://cdn-images-1.medium.com/max/1200/1*FMqAKTeVJ2kswj01_IXU6Q.png)

I have copied the OCID of the Log and moved to the collection part through API. After a few tryouts to use Data Safe, one of my friends ,convinced me that it will be much simpler to do this using Python. Thank you Silviu!!!

![](https://cdn-images-1.medium.com/max/1200/1*_i_kPCKvePfJuyVdcDVJBg.png)

I have configured Python on my Workstation, configured the OCI credentials, Installed [OCI SDK](https://docs.oracle.com/en-us/iaas/tools/python/2.90.0/installation.html) and created my first OCI Python API script.

Now I needed the OCI Sample code for this, and I have got it from the Rest API Page:

[**Oracle Cloud Infrastructure API Reference and Endpoints**  
_Edit description_docs.oracle.com](https://docs.oracle.com/en-us/iaas/api/#/en/data-safe/20181201/AuditEventSummary/ListAuditEvents "https://docs.oracle.com/en-us/iaas/api/#/en/data-safe/20181201/AuditEventSummary/ListAuditEvents")[](https://docs.oracle.com/en-us/iaas/api/#/en/data-safe/20181201/AuditEventSummary/ListAuditEvents)

  
```
import oci  
config = oci.config.from_file()  
data_safe_client = oci.data_safe.DataSafeClient(config)  
list_audit_events_response = data_safe_client.list_audit_events(  
    compartment_id="ocid1.test.oc1..<unique_ID>EXAMPLE-compartmentId-Value",  
    opc_request_id="IK7SZJKOJGVMXX7ELYWO<unique_ID>",  
    compartment_id_in_subtree=False,  
    access_level="ACCESSIBLE",  
    limit=2773,  
    page="EXAMPLE-page-Value",  
    scim_query="EXAMPLE-scimQuery-Value",  
    sort_order="ASC",  
    sort_by="auditLocation")  
print(list_audit_events_response.data)
```

As my scope is to do the most simple thing and get the logs, i have removed the ListAuditEvents there are Optional and I have added only compartmentId:

![](https://cdn-images-1.medium.com/max/1200/1*sz1sIxoPWlCEHTawavCIIQ.png)

  
  
`import oci`  
  
`config = oci.config.from_file("~/.oci/config","DEFAULT")`  
`identity = oci.identity.IdentityClient(config)`  
  
`data_safe_client = oci.data_safe.DataSafeClient(config)`  
  
`list_audit_events_response = data_safe_client.list_audit_events(`  
    `compartment_id="ocid1.compartment.oc1..ReplaceHereYourCompartmentOCID",`  
    `limit=2,`  
    `sort_order="ASC",`  
    `sort_by="auditLocation")`  
  
`print(list_audit_events_response)`

I have selected a limit of 2 as this is just for test. The result was this:

![](https://cdn-images-1.medium.com/max/1200/1*NEfSXAgYk0T9SgAJDCApbw.png)

At this point I am able to get the Data Safe logs, and next I have to send the logs to OCI Logging.

The API that I used is the Custom Logs PutLogs Request:

[PutLogs | Oracle Cloud Infrastructure API Reference and Endpoints](https://docs.oracle.com/en-us/iaas/api/#/en/logging-dataplane/20200831/LogEntry/PutLogs)

I already have the LogId as this is required by the API:

![](https://cdn-images-1.medium.com/max/1200/1*a1mfYazSP2HdPlmTBENTig.png)

I have also checked the LogEntryBatch mandatory Parameters:

![](https://cdn-images-1.medium.com/max/1200/1*nhc2k83DLTefisfc5mbmUA.png)

Now I got the Python SDK sample and did the changes to make it as simple as possible. As this was my first API call to Custom logs, I have only sent a test log.

  
`from datetime import datetime`  
`import oci`  
  
  
`config = oci.config.from_file("~/.oci/config","DEFAULT")`  
`identity = oci.identity.IdentityClient(config)`  
`user = identity.get_user(config["user"]).data`  
`data_safe_client = oci.data_safe.DataSafeClient(config)`  
`loggingingestion_client = oci.loggingingestion.LoggingClient(config)`  
  
  
`list_audit_events_response = data_safe_client.list_audit_events(`  
    `compartment_id="ocid1.compartment.oc1..aaaaaaaarsk54oz6cd4kd5pgnrgqcxiyd2ldo2n5zx6psna2hyln6vtzkgka",`  
    `limit=1,`  
    `sort_order="DESC",`  
    `sort_by="auditLocation")`  
`audit_event_time=list_audit_events_response.data.items[0].audit_event_time`  
  
  
  
  
`format_data = "%Y-%m-%d %H:%M:%S.%f+00:00"`  
  
`put_logs_response = loggingingestion_client.put_logs(`  
    `log_id="ocid1.log.oc1.eu-frankfurt-1.amaaaaaagysxq7aakcwce4w3pkrsthj72p3wtm4kqphz6rwekio2a2o3ut2a",`  
    `put_logs_details=oci.loggingingestion.models.PutLogsDetails(`  
        `specversion="1.0",`  
        `log_entry_batches=[`  
            `oci.loggingingestion.models.LogEntryBatch(`  
                `entries=[`  
                    `oci.loggingingestion.models.LogEntry(`  
                        `data=str(list_audit_events_response.data),`  
                        `id="test",`  
                             `time=datetime.strptime(`  
                           `str(audit_event_time),`  
                    `str(format_data))`  
                        `)],`  
                `source="DataSafe",`  
                `type="DataSafeLogs",`  
                `defaultlogentrytime=datetime.strptime(`  
                    `str(audit_event_time),`  
                    `str(format_data)),`  
                `subject="EXAMPLE-subject-Value")]),`  
    `timestamp_opc_agent_processing=datetime.strptime(`  
        `str(audit_event_time),`  
        `str(format_data)),`  
    `opc_agent_version="",`  
    `opc_request_id="")`  
  
  

![](https://cdn-images-1.medium.com/max/1200/1*bS39ezROtCgJBFl6ZaXusA.png)

The Python code that worked and helped me identify the rights steps:

`from datetime import datetime`  
`import oci`  
  
`config = oci.config.from_file("~/.oci/config","DEFAULT")`  
`identity = oci.identity.IdentityClient(config)`  
  
`loggingingestion_client = oci.loggingingestion.LoggingClient(config)`  
  
  
`put_logs_response = loggingingestion_client.put_logs(`  
    `log_id="ocid1.log.oc1.eu-frankfurt-1.YourOCID",`  
    `put_logs_details=oci.loggingingestion.models.PutLogsDetails(`  
        `specversion="1.0",`  
        `log_entry_batches=[`  
            `oci.loggingingestion.models.LogEntryBatch(`  
                `entries=[`  
                    `oci.loggingingestion.models.LogEntry(`  
                        `data="test",`  
                        `id="1",`  
                        `time=datetime.strptime(`  
                            `"2023-01-16T12:21:35.523Z",`  
                            `"%Y-%m-%dT%H:%M:%S.%fZ"))],`  
                `source="DataSafe",`  
                `type="DataSafeLogs",`  
                `defaultlogentrytime=datetime.strptime(`  
                    `"2023-01-16T12:21:35.523Z",`  
                    `"%Y-%m-%dT%H:%M:%S.%fZ"),`  
                `subject="EXAMPLE-subject-Value")]),`  
    `timestamp_opc_agent_processing=datetime.strptime(`  
        `"2023-01-16T11:12:19.676Z",`  
        `"%Y-%m-%dT%H:%M:%S.%fZ"),`  
    `opc_agent_version="",`  
    `opc_request_id="")`  
  
# Get the data from response  
`print(put_logs_response.data)`

As the Log Time Stamps will not help me very much in my quest and later searches, I have decided to replace the example times with the audit_event_time and I have also used this page to understand how Python time works:

[Python DateTime — strptime() Function — GeeksforGeeks](https://www.geeksforgeeks.org/python-datetime-strptime-function/)

Now, at the end, the final code that works is this:

  
`from datetime import datetime`  
`import oci`  
  
  
`config = oci.config.from_file("~/.oci/config","DEFAULT")`  
`identity = oci.identity.IdentityClient(config)`  
`user = identity.get_user(config["user"]).data`  
  
`data_safe_client = oci.data_safe.DataSafeClient(config)`  
`loggingingestion_client = oci.loggingingestion.LoggingClient(config)`  
  
  
  
`list_audit_events_response = data_safe_client.list_audit_events(`  
    `compartment_id="ocid1.compartment.oc1..aaaaaaaarsk54oz6cd4kd5pgnrgqcxiyd2ldo2n5zx6psna2hyln6vtzkgka",`  
    `limit=1,`  
    `sort_order="DESC",`  
    `sort_by="auditLocation")`  
`audit_event_time=list_audit_events_response.data.items[0].audit_event_time`  
  
`format_data = "%Y-%m-%d %H:%M:%S.%f+00:00"`  
  
`put_logs_response = loggingingestion_client.put_logs(`  
    `log_id="ocid1.log.oc1.eu-frankfurt-1.amaaaaaagysxq7aakcwce4w3pkrsthj72p3wtm4kqphz6rwekio2a2o3ut2a",`  
    `put_logs_details=oci.loggingingestion.models.PutLogsDetails(`  
        `specversion="1.0",`  
        `log_entry_batches=[`  
            `oci.loggingingestion.models.LogEntryBatch(`  
                `entries=[`  
                    `oci.loggingingestion.models.LogEntry(`  
                        `data=str(list_audit_events_response.data),`  
                        `id="test",`  
                             `time=datetime.strptime(`  
                           `str(audit_event_time),`  
                    `str(format_data))`  
                        `)],`  
                `source="DataSafe",`  
                `type="DataSafeLogs",`  
                `defaultlogentrytime=datetime.strptime(`  
                    `str(audit_event_time),`  
                    `str(format_data)),`  
                `subject="TestDataSafeLogs")]),`  
    `timestamp_opc_agent_processing=datetime.strptime(`  
        `str(audit_event_time),`  
        `str(format_data)),`  
    `opc_agent_version="",`  
    `opc_request_id="")`  
  

I have also sorted the Event order from last event, and i have set the limit to 1, so I will only have only 1 Item created at each run.

If I select >1 you will the the log content this way:

![](https://cdn-images-1.medium.com/max/1200/1*0lVFHU7eItL9A2iwovihWA.png)

![](https://cdn-images-1.medium.com/max/1200/1*OPjrBbtem_sbm1yT6AtTAg.png)

In customer case, this can be updated based on the script execution time, and other usecases.

Congratulations! You have added the Data Safe logs to Logging Service.

From here you can send them to OCI Logging Analytics, OCI Streams, 3rd party SIEM that can’t read directly the Data Safe Rest API, and so on.
