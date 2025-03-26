### Use CloudGuard to search for MITRE ATT&CK Techniques detections

On my previous [blog](https://learnoci.cloud/use-auditd-logs-in-oci-with-logging-service-5caa13719315) entry, I have shown how to leverage AuditD data in OCI Logging service. Now I will use the OCI Logging Service search option to create some MITRE ATT&CK Searches that will be used by Cloud Guard Insight Rules.

From this rule mapping and the Attack Rules, I am able to create the searches that I need:

[**linux-audit/DS-to-audit.MD at main · izysec/linux-audit**  
_Some resources to facilitate my blog on auditd for security monitoring - linux-audit/DS-to-audit.MD at main ·…_github.com](https://github.com/izysec/linux-audit/blob/main/DS-to-audit.MD "https://github.com/izysec/linux-audit/blob/main/DS-to-audit.MD")[](https://github.com/izysec/linux-audit/blob/main/DS-to-audit.MD)

[**auditd-attack/auditd-attack.rules at master · bfuzzy1/auditd-attack**  
_A Linux Auditd rule set mapped to MITRE's Attack Framework - auditd-attack/auditd-attack.rules at master ·…_github.com](https://github.com/bfuzzy1/auditd-attack/blob/master/auditd-attack/auditd-attack.rules "https://github.com/bfuzzy1/auditd-attack/blob/master/auditd-attack/auditd-attack.rules")[](https://github.com/bfuzzy1/auditd-attack/blob/master/auditd-attack/auditd-attack.rules)

As seen in this picture you can go and do your mapping manually:

![](https://cdn-images-1.medium.com/max/1200/0*xTHj3bCHqQTx4aSe.png)

In my auditd custom rules, I only have enabled this rules:

T1107_File_Deletion T1070_Indicator_Removal_on_Host T1055_Process_Injection T1072_third_party_software T1219_Remote_Access_Tools T1005_Data_from_Local_System T1057_Process_Discovery T1081_Credentials_In_Files T1049_System_Network_Connections_discovery T1082_System_Information_Discovery T1082_System_Information_Discovery T1016_System_Network_Configuration_Discovery T1082_System_Information_Discovery T1033_System_Owner_User_Discovery T1087_Account_Discovery T1068_Exploitation_for_Privilege_Escalation T1166_Seuid_and_Setgid T1169_Sudo T1043_Commonly_Used_Port T1021_Remote_Services T1201_Password_Policy_Discovery T1071_Standard_Application_Layer_Protocol T1021_Remote_Services T1108_Redundant_Access T1052_Exfiltration_Over_Physical_Medium T1078_Valid_Accounts T1168_Local_Job_Scheduling T1079_Multilayer_Encryption T1099_Timestomp T1215_Kernel_Modules_and_Extensions locklvm

as this rules are mapped as keys, I will use them in the OCI Logging Search.

Now if we go to OCI Logging, and look at the Auditd generated logs, we can go and drill down on more specific searches by clicking Filter matching:

![](https://cdn-images-1.medium.com/max/1200/1*slTHtcCRtKcwUCao9ICY_A.png)

If we select Show Advanced Mode, we will see a simular query, and in there we will see that the selected query will not return anything.

![](https://cdn-images-1.medium.com/max/1200/1*RZJt0HXFwwvVKBImDAPimQ.png)

![](https://cdn-images-1.medium.com/max/1200/1*kqrIyGUnZUFXAcsUMtJfIw.png)

![](https://cdn-images-1.medium.com/max/1200/1*oMd3jm3Q4GzsxS4rjssudw.png)

Now, in the Advanced Mode, we can change the format from:

![](https://cdn-images-1.medium.com/max/1200/1*hJ7YTzrCgjYpjiSgIH3cqw.png)

to

data.”body.key”=’ ”T1043_Commonly_Used_Port”’

![](https://cdn-images-1.medium.com/max/1200/1*RfYzTyRwLOp1oZL2tl3tEg.png)

We need to do this, as the proper log field is **“body.key”** not key.

With this format you will be able to see the proper logs and save the searches:

![](https://cdn-images-1.medium.com/max/1200/1*xAO2OKirgrxK_0rZxZhguQ.png)

![](https://cdn-images-1.medium.com/max/1200/1*5SB-BaXDGWlPaXJNuFAgcw.png)

> search “ocid1.compartment.oc1..aaaaaaaayouzltprojlao2a2f7xlg5kzhtpcs64ncnpvlwqgsl4aoaw57yta” “ocid1.compartment.oc1..aaaaaaaarsk54oz6cd4kd5pgnrgqcxiyd2ldo2n5zx6psna2hyln6vtzkgka/ocid1.loggroup.oc1.eu-frankfurt-1.amaaaaaagysxq7aaz52drpqvc45q6aukcg6pxgldm4r52pzbs25uja3i374a” **|** data.”body.key”=’”T1078_Valid_Accounts”’ **|** sort by datetime desc

In this query I have removed the exclusion of VCN Flow Logs, as this was used by me to filter only certain logs when I created the search.

Using this search, and changing the filed value I have created multiple searches based on MITRE ATT&CK Techiques:

![](https://cdn-images-1.medium.com/max/1200/1*78Nl_YtNs9pc1aUd7TGn-w.png)

Next step will be to set up Cloud Guard to use Insight logging as described in my first blog entry from this series and will use the saved Searches for detection.

[**Use Cloud Guard Insight Recipes to monitor Windows Instances against Interesting Windows Event IDs…**  
_Recently Oracle lunched a new Recipe called Insight. With this service you will be able to leverage the OCI logging…_learnoci.cloud](https://learnoci.cloud/use-cloud-guard-insight-recipes-to-monitor-windows-instances-against-interesting-windows-event-ids-7ef796174d37 "https://learnoci.cloud/use-cloud-guard-insight-recipes-to-monitor-windows-instances-against-interesting-windows-event-ids-7ef796174d37")[](https://learnoci.cloud/use-cloud-guard-insight-recipes-to-monitor-windows-instances-against-interesting-windows-event-ids-7ef796174d37)

Go to Data Sourrces and create a new Query:

![](https://cdn-images-1.medium.com/max/1200/1*Xb7YS8749mcr4qcGb7z_jA.png)

Give the Query the proper name and Press Import Saved Search Query:

![](https://cdn-images-1.medium.com/max/1200/1*MD2DUxH8iqPi4ywwLLSq0A.png)

![](https://cdn-images-1.medium.com/max/1200/1*nz8Lu-1JqkeZ02YWbU7-sg.png)

Select the Saved Search and press Import:

![](https://cdn-images-1.medium.com/max/1200/1*SrTFZ5cHnsdClYU1Jis5eg.png)

After the import map the body.jey field with the Cloud Guard key

![](https://cdn-images-1.medium.com/max/1200/1*EijyAcphGtg-CbOjzOXW3Q.png)

> search “ocid1.compartment.oc1..aaaaaaaayouzltprojlao2a2f7xlg5kzhtpcs64ncnpvlwqgsl4aoaw57yta” “ocid1.compartment.oc1..aaaaaaaarsk54oz6cd4kd5pgnrgqcxiyd2ldo2n5zx6psna2hyln6vtzkgka/ocid1.loggroup.oc1.eu-frankfurt-1.amaaaaaagysxq7aaz52drpqvc45q6aukcg6pxgldm4r52pzbs25uja3i374a” **|** data.”body.key”=’”T1215_Kernel_Modules_and_Extensions”’ **|** select data.”body.key” as cgkey01

and select the trigger time:

![](https://cdn-images-1.medium.com/max/1200/1*gZfT673WeoX_t_iYYbCiPA.png)

If you want to decrease the number of detected events, you can remove additional fields in the OCI Logging search, like removing the body.euid for Oracle-cloud-agent:

![](https://cdn-images-1.medium.com/max/1200/1*H9Xza17N8BsiFJLaA-1SyA.png)

search “ocid1.compartment.oc1..aaaaaaaayouzltprojlao2a2f7xlg5kzhtpcs64ncnpvlwqgsl4aoaw57yta” “ocid1.compartment.oc1..aaaaaaaarsk54oz6cd4kd5pgnrgqcxiyd2ldo2n5zx6psna2hyln6vtzkgka/ocid1.loggroup.oc1.eu-frankfurt-1.amaaaaaagysxq7aaz52drpqvc45q6aukcg6pxgldm4r52pzbs25uja3i374a” **|** data.”body.key”=’”T1166_Seuid_and_Setgid”’ and data.”body.EUID”!= ‘“oracle-cloud-agent”’ **|** sort by datetime desc

After the Query is created, you need to attach it to a new Detector or you can attach it to an existing one.

![](https://cdn-images-1.medium.com/max/1200/1*GXNpF6yC1qjBIeyNENhmMg.png)

![](https://cdn-images-1.medium.com/max/1200/1*nDIbvy-rfG8uhzsVutsvPQ.png)

Now Press Create Rule and select the created techniques:

![](https://cdn-images-1.medium.com/max/1200/1*HOlpWyPpWvSgwgn-LNQq7Q.png)

![](https://cdn-images-1.medium.com/max/1200/1*X7jLwER_rC_9OtPS27-Qtw.png)

![](https://cdn-images-1.medium.com/max/1200/1*fSKEjscUDifuj_HlrUYcTg.png)

Please note that some of the Techniques had their number changes, so it’s better to use the Navigator [https://attack.mitre.org/techniques/enterprise/](https://attack.mitre.org/techniques/enterprise/) .

![](https://cdn-images-1.medium.com/max/1200/1*mf63ivYOvwGIOBLC2dcikA.png)

Last step is to create a new target and attach the detector recipes and enable the queries. You can also reuse existing ones.

![](https://cdn-images-1.medium.com/max/1200/1*PJ_3J1cWbtJwgy2s_IfHRA.png)

If you select any of the Queries, and you see that you have green checks on all steps, Congratulations, you have finished configuring the Cloud Guard detection.

![](https://cdn-images-1.medium.com/max/1200/1*0KjLiPVSpFsHrmNYDgz23g.png)

![](https://cdn-images-1.medium.com/max/1200/1*MtahfN7tNPfdCe6Ngz3Tug.png)
