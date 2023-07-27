# Use Health Rules to Monitor Entity Health with Cloud Native Application Observability(CNAO)
## Contents
        Use Cases
        Pre-requisites, Guidelines
        Python API Client - Health Rules, Actions, Trigger 
        Getting the API Token
        Exploring Health Rules API
        Exploring Actions API
        Exploring Trigger API
        Data Generation to trigger and clear events
        De-provisioning

### Use Cases
        * As a Cloud Admin, you have already provisioned cloud connections to AWS leveraging AppD 
        Cloud Connections API to pull metrics data from AWS Services (Load 
        Balancers, Storage, Hosts, Databases)

        * As Infra Ops, set up health rules for AWS EC2 instances to alert on thresholds exceeded for CPU Utilization

        * As DevOps/AppOps, be notified of thresholds exceeded for the underlying infrastructure


### Pre-requisites, Guidelines

1. Requires Cloud Native Application Observability Tenant, ClientID and Secret. For the purposes of this exercise, we will reserve a sandbox and get the required data from it.

https://dcloud2-rtp.cisco.com/content/instantdemo/appdynamics-observability-in-aws

Copy the valiues in the last column under DevNet. You will set the ENV variables for these parameters that will be used in the API Client:

![Copy Global Variables](https://github.com/prathjan/images/blob/main/reserve2.png?raw=true)

### Python API Client - Health Rules, Actions, Trigger

Check out a sample python client to exercise the Health Rules/Actions/Triggers API's here: 
https://github.com/CiscoDevNet/appdhr/blob/main/hractr.py 

Before running the python client, set up the following environment variables (sample values displayed):

        %env APPD_CLIENTID_AGT=agt_6DbMBU1d6zmQqBlEQCr7ir

        %env APPD_SECRET_AGT=k4hOZYQrZ5B5zHh3r0mn4ZrqwebVrQN18b-yTdDaO9Q

        %env APPD_CLIENTID_POST=srv_3yvuCrMr0FCpNuVigRsvqk

        %env APPD_SECRET_POST=Hjl5tOMqpTixfBo8LxsFVYk7m_OW5nPMTFTG29LdyvM

        %env APPD_CLIENTID_BASIC=srv_eAbU1aJOWGcjsxKUta9b8

        %env APPD_SECRET_BASIC=0oiakaHCkzSRmuRdHOJiAoCN2wN8xdFtzCFAOyCLdNs

        %env TENANT_NAME=cisco-devnet

        %env AWS_ACCESS_KEY=AKIAXQ7ZIQP4W7VTJIXC

        %env ID=AKIAXQ7ZIQP4W7VTJIXC

        %env AWS_SECRET_KEY=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

        %env AWS_CONNECTION_NAME=prathjan-a3ed1d35c8b1427db12033d921d747fc

        %env CL_ID=agt_6DbMBU1d6zmQqBlEQCr7ir

        %env CL_SEC=k4hOZYQrZ5B5zHh3r0mn4ZrqwebVrQN18b-yTdDaO9Q



 Try the Python API Client provided to execute the following:

### Getting the API Token

        * Generate Tenant ID - get_ten_id()

        * Generate token with POST authentication - get_token(ten_id)

        * Generate token with Basic authentication - get_token_basic(ten_id)


### Exploring Health Rules API

Please refer to the following devnet resource for a complete API definition: 

https://developer.cisco.com/docs/appdynamics/health-rules/#!introduction/appdynamics-cloud-health-rules-api

Some of the API's included in the sample python client are as follows and accounts for the health rule resource lifecycle. 

        * Get the list of health rules for a tenant - get_all_health_rules(appd_token, base_url)

        * Create health rules for a tenant - create_health_rule(appd_token, base_url, hr_name)

        * Get health rule ID by name - get_hrid_by_name(appd_token, base_url, conf_name)

        * Disable the health rules - disable_hr(appd_token, base_url, hrid)

        * Enable the health rules - enable_hr(appd_token, base_url, hrid)

        * Get the health roll up path of an entity - get_health_roll_up(appd_token, base_url, entity)

        * Modify a health rule - update_health_rule(appd_token, base_url, hrid, hr_name)

        * Delete a health rule - delete_health_rule(appd_token, hrid)

### Exploring Actions API

Please refer to the follwing devnet resources for a complete API definition: 

https://developer.cisco.com/docs/appdynamics/actions/#!introduction

Some of the API's included in the sample python client are as follows and accounts for the actions object lifecycle. 

        * Get All Actions - get_all_actions(appd_token, base_url)

        * Get an Action by Identifier - get_actionid_by_name(appd_token, base_url, conf_name)

        * Create an Action - create_action(appd_token, base_url, action_name)

        * Update an Action by Identifier - update_action(appd_token, base_url, action_id, action_name)

        * Delete an Action by Identifier - delete_action(appd_token, action_id)

### Exploring Trigger API

Please refer to the follwing devnet resources for a complete API definition: 

https://developer.cisco.com/docs/appdynamics/actions/#!introduction 

Some of the API's included in the sample python client are as follows and accounts for the trigger object lifecycle. 

        * Get All Triggers - get_all_triggers(appd_token, base_url)

        * Create a Trigger - create_trigger(appd_token, base_url, tr_name, hr_id, ac_id)

        * Get a Trigger by Identifier - get_id_by_name(appd_token, url, conf_name)

        * Update a Trigger by Identifier - update_trigger(appd_token, base_url, tr_name, tr_id, hr_id, ac_id)

        * Delete a Trigger by Identifier - delete_trigger(appd_token, base_url, tr_name, tr_id)

### Data Generation to trigger and clear events

## Create Health Rule, Action, Trigger

Use the above API's to do the following before you generate data. This is assuming that you do not have a HR, action,trigger provisioned.

* Create a health rule

* Create an Action 

* Create a Trigger 

## Prepare Data Generator

You will be running the datagenerator from your local computer. To run the data generator, you will need java installed on your local computer.

Open a Terminal and create a datagen directory: <your_local_datagen_dir>.

In this directory, you will git clone the following data generator git repo, prepare the data files and run the data generator from you local computer:

git clone https://github.com/CiscoDevNet/appdhr.git

You will see the following files in the datagen directory: <your_local_datagen_dir>:

        ati-vodka-local-all.jar	

        platformtarget.yml	

        trigger.yml

        clear.yml		

        resource.yml

## Data generation to trigger and clear events

Let's generate some utilization data that exceeds thresholds configured in the health rule in the previous section. We have two files that are provided:

trigger.yml - To generate data that exceeds thresholds

clear.yml - To generate data that is below threshods configured

With the above configuration, metrics in the range of 75-95% for CPU utilization to trigger events and metrics in the range of 25-55% to clear the event is configured.

Let's examine the two files:

cat trigger.yml 

        payloadFrequencySeconds: 30
        payloadCount: 300
        metrics:
        - name: infra:system.cpu.used.utilization
        unit: '%'
        otelType: summary
        valueFunction: 'randomSummary(75, 95, "", 5)'
        quantiles: [0, 0.5, 1]
        reportingEntities: [ec2]
        isDouble: true 

cat clear.yml 

        payloadFrequencySeconds: 30
        payloadCount: 300
        metrics:
        - name: infra:system.cpu.used.utilization
        unit: '%'
        otelType: summary
        valueFunction: 'randomSummary(25, 55, "", 5)'
        quantiles: [0, 0.5, 1]
        reportingEntities: [ec2]
        isDouble: true 

## Resource configuration

Let's simulate EC2 resources here. The health rules have been configured for EC2 resources and so thresholds configured will apply to these EC2 instances.

We will generate some unique EC2 instance ID's using the AWS account number configured for this lab. 

Execute the following code blocks to generate resource.yml which you will then copy to <your_local_datagen_dir>/resource.yml.

        cat resource.yml
        echo $ID
        echo "ID is" $ID
        sed "s/%accnum%/$ID/g" resource.yml > /tmp/resource.yml
        cat /tmp/resource.yml

## Update local resource.yml

Cut and paste contents of /tmp/resource.yml above to your local <your_local_datagen_dir>/resource.yml.

## Platform configuration

To run the data generator, we need to configure the agent client ID and secret. Execute the following code blocks to generate platformtarget.yml which you will then copy to /platformtarget.yml.

        cat platformtarget.yml
        echo $CL_ID
        echo "CL_ID is" $CL_ID
        echo $CL_SEC
        echo "CL_SEC is" $CL_SEC
        sed "s/%clientId%/$CL_ID/g" platformtarget.yml > /tmp/platformtarget1.yml
        sed "s/%clientsec%/$CL_SEC/g" /tmp/platformtarget1.yml > /tmp/platformtarget.yml
        cat /tmp/platformtarget.yml

## Update local platformtarget.yml

Cut and paste contents of /tmp/platformtarget.yml above to your local <your_local_datagen_dir>/platformtarget.yml.

## Trigger high CPU Utilization event

To generate high CPU utilization data that will trigger the threshold exceeded events in CNAO, let's run the following script in <your_local_datagen_dir>. 

Let it run contunuously to give it time to generate some valid data to populate the UI as well as send notifications to Slack.You can view data in UI and Slack before you stop this data generation.

        cd <your_local_datagen_dir>

        java -jar ati-vodka-local-all.jar -c platformtarget.yml -e resource.yml -m trigger.yml

## View Threshold Exceeded Metrics in UI

Click on Observe in your reservation window and you will land in the Observe page of Cloud Native Application Observability.

![dCloud page](https://github.com/prathjan/images/blob/main/reserve2.png?raw=true)

Click on the following link and pick the host that applies to your sandbox. It will be of the format `"Lab i-" <your AWS account#>`:
https://cisco-devnet.observe.appdynamics.com/ui/observe/infra/host?filter=isActive%20%3D%20true&since=now-1h

![alt text](https://github.com/prathjan/images/blob/main/hostlist.png?raw=true)

Click on the applicable host to view details:

![alt text](https://github.com/prathjan/images/blob/main/metuiex.png?raw=true)

## View Threshold Exceeded Slack Events

Open the Slack channel that you configured to view events for high CPU Utilization. You should see a warning alert as shown in this example:

![alt text](https://github.com/prathjan/images/blob/main/metslackex.png?raw=true)

## Clear high CPU Utilization event

To clear the high CPU utilization event, lets rerun the data generator but this time use the clear.yml which generates CPU utilization data in the clear zone. This should clear the violations raised in the last section.

Before you run this, make sure you have terminated the previous data generation that you started above.

        cd <your_local_datagen_dir>

        java -jar ati-vodka-local-all.jar -c platformtarget.yml -e resource.yml -m clear.yml

## View Threshold Cleared metrics in UI

Click on the following link and pick the host that applies to your sandbox. It will be of the format `"Lab i-" <your AWS account#>`:
https://cisco-devnet.observe.appdynamics.com/ui/observe/infra/host?filter=isActive%20%3D%20true&since=now-1h

![alt text](https://github.com/prathjan/images/blob/main/hostlist.png?raw=true)

Click on the applicable host to view details:

![alt text](https://github.com/prathjan/images/blob/main/metuicl.png?raw=true)

## View Threshold Cleared Slack Events

Open the Slack channel that you have configured to view events. You should see a Unknown Alert, which indicates that the previously raised alert has been cleared:

![alt text](https://github.com/prathjan/images/blob/main/metslackcl.png?raw=true)

### De-provisioning

The API client provides routines to do the following.

* Delete a health rule

* Delete an Action by Identifier

* Delete a Trigger by Identifier

