| Steps | Description | Notes |
|--------|-------------|--------|
| **1** | **Go to Menu → Observability&Management → Agent Configurations**  |
|        | ![Agent Configurations](https://cdn-images-1.medium.com/max/1200/1*mdZdbXRp_9dFyV28toyVww.png)  |  |
|     **2**    | **Click "Create agent config"**  | |
|        | ![Create Agent Config](https://cdn-images-1.medium.com/max/1200/1*Th8QF3jrmtr3IuzAmeRwOg.png)  | |
|        | ![Config Details](https://cdn-images-1.medium.com/max/1200/1*MebXcJ8Dix6qN1y5aEdC3Q.png)  | |
| **3** | **Create the Dynamic Group**  | |
|      | Navigate to: Menu → Identity & Security → Dynamic Groups** and press **Create Dynamic Group.  | |
|        | ![Create Dynamic Group](https://cdn-images-1.medium.com/max/1200/1*k-8D0bHps-APdUYcQKie0A.png)  |  |
|        | Add the matching rule following the Service documentation  |      [Documentation](https://docs.oracle.com/en-us/iaas/Content/Identity/dynamicgroups/Writing_Matching_Rules_to_Define_Dynamic_Groups.htm)  
|        | Example: ![Matching Rule](https://cdn-images-1.medium.com/max/1200/1*KWtewCOrWDEefq-NY_FvfQ.png)  ![Matching Rule Example](https://cdn-images-1.medium.com/max/1200/1*elJFqGnZFeLEXtvDZQweWA.png) | Ensure that the matching rule covers the required instances.|
| **3** | **Specify what you want to collect with the Agent**  | |
|        | ![Log Collection](https://cdn-images-1.medium.com/max/1200/1*gOpnGb0ja1XuwnrVOa4sTg.png)  | Define proper collection filters to ensure relevant log data is captured.  |
|        | If you want to monitor logs from files, specify the log location: ![Log Location](https://cdn-images-1.medium.com/max/1200/1*7RWKGclmZo4GKHUAdiCMRQ.png) ![Log Path](https://cdn-images-1.medium.com/max/1200/1*n_i8A0VAKwdSUkUkQCCUkA.png)|   
|        | Select the **Log Group destination** and press **Create**.  | |
| **4** | **Ensure that the Logging & Monitoring Agent is enabled on the monitored hosts** ![Enable Logging](https://cdn-images-1.medium.com/max/1200/1*hQ4Jr5sa3dSWuPABHCT8dA.png) | Verify that the agent is properly installed and configured on all required instances. After a few minutes, check if the logs are available on an instance where you have enabled Custom Logging. ||
| **5** | **Check logs in OCI Logging Service**  |  |
|        | ![Check Logs](https://cdn-images-1.medium.com/max/1200/1*5JtxZl_TbtecH2eLzSx3Xg.png)  | Logs should appear within a few minutes if all configurations are correct. |
|        | You can also check the logs directly in the OCI Logging Service:  | |
|        | ![OCI Logging](https://cdn-images-1.medium.com/max/1200/1*Y3oEZ7mhO0lRhqXA9svx5w.png)  | |
|        | ![OCI Logs View](https://cdn-images-1.medium.com/max/1200/1*gWErPH7efAgqOZZLs3JMjA.png)  | |
