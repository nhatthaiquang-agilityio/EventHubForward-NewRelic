# EventHub Forward Logs to NewRelic
    - The azure function receives messages from EventHub and then sends messages to New Relic.
        `The Azure Function --> Even Hub --> The EventHubForwardNewRelic Azure Function --> New Relic site`
### Steps
+ Zip the javascript function
    ```
    powershell Compress-Archive ./EventHubForwarder/* EventHubForward.zip
    ```

+ Create an Event Hub
    + 1.1 Settings > Networking > Select All networks in Public Access tab
    + 1.2 OR Select Selected networks and add range IPs(Make sure that the Azure function is the same virtual network with the Event Hub)

+ Create an Azure Function
    + Add `FUNCTIONS_WORKER_RUNTIME = Node, FUNCTIONS_EXTENSION_VERSION=~4` values into `Settings > Environment Variables`
    + Add NewRelic Variables
    ![NewRelic Variables](./Images/newrelic-variables.png)

    + Add Event Logs of the Azure Functions to Event Hub
        + Go to Monitoring > Diagnostics settings
        + Add Event Hub Setting in the Azure Function
        ![Event Hub Setting](./Images/event-hub-settings.png)

+ Upload the EventHubForward.zip file to the storage account container.
    + Create `Generate SAS` (expired time to access the zip file)
    + Copy the URL and paste into the WEBSITE_RUN_FROM_PACKAGE variable in `Settings > Environment Variables` of the Azure Function
