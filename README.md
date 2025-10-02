# AlienVault USM Sensor Deployment

## Table of Contents
1. Introduction 
2. Connecting the Sensor to AWS and The Running Assets
3. Sensor Deployment
4. Configuration of the Sensor
5. Log Collection

# Introduction
**Unified Security Management** (USM) is a cloud-native SaaS solution designed for rapid threat response. It goes beyond traditional SIEM by providing powerful threat and vulnerability detection across your environment. USM sensors deliver vital threat intelligence and integrate with multiple security tools. The platform automatically correlates all sensor data into actionable reports, alarms, and events, all of which are managed easily through your web browser.

In this project, I will demonstrate how to deploy a USM Sensor and connect cloud-based assets to the sensor for log ingestion.

# Connecting the Sensor to AWS and The Running Assets

In this example, my sensor is being hosted on AWS. Before starting I check to ensure that we are on the correct AWS server and that the asset as online. (Which we will be on us-east-2)

![alt text](<assets/Making sure to be on the correct AWS Server.png>)

Here I am able to can confirm that my assets are properly running.

![alt text](<assets/Checking all instances are live.png>)

# Sensor Deployment

Visiting the **AWS Console** and going over to the **EC2** page, I am able to view my **Sensor** asset. This is where I was able to grab the public IP of the sensor. 

In this example our sensor is being hosted on AWS; we will visit the sensor via the web browser.

![alt text](<assets/Connecting AWS logs by connecting my AWS instance with the IPv4 Public address.png>)

From here we will need to launch the USMA wizard, this can be done by logging into your USMA instance with the credentials you inputted or were provided.

![alt text](<assets/Connecting to USMA.png>)

We will click "**Get Started**" and here we will be provided with a **sensor code** that will connect your sensor to USM anywhere. (If you a code wasn't generated after clicking "**Get Started**", head over to the **Data Sources** section tab on the left hand side, and click "**New Sensor**").


![alt text](<assets/Configuring a new sensor by generating a sensor authentication code.png>)

Return to your deployed sensor's **webpage** and enter in the details, follow the naming scheme that best fits your environment, for this example I identify our sensor **"USMA-TestSensor"**. 

Once all the fields are filled in, click "**Start Setup**" and wait for the Sensor Setup to be completed.

![alt text](<assets/Connecting to AWS Public sensor IP to intiate sensor connection to USMA with generated code.png>)

# Configuration of the Sensor

Head back over to **Data Sources** section tab on the left hand side and click "**Sensors**". Within your USMA instance you should see your newly deployed sensor. To configure our sensor we will head over to the wrench icon and open up the configuration wizard.

![alt text](<assets/Configuring new sensor within USMA.png>)

Depending on your resources you may have more than one NIC, but for my example I have only 1 NIC, this may prompt a warning about Network Intrusion Deteciton interfaces, you can proceed by clicking OK.

The first section will be **AWS Log Collection**

For this example, We will be collecting logs from all sources besides Apache and IIS logs.

![alt text](<assets/AWS Log Collection Settings.png>)

Proceeding with the configuration, the next section was **Log Management**, we will have the system collect logs via syslog.

The next section is for the **Open Threat Exchange** section, this connects the sensor to LevelBlue's OTX platform, which provides users the ability to collaborate, research and receive alerts on emerging threats and indicators of compromise. 

In my setup, I setup the look-back to **unlimited**.

![alt text](<assets/Configuring sensor to collect OTX pulses.png>)

After this, you may complete the wizard by clicking **Start Using USM Anywhere**. 

![alt text](<assets/19. USMA Sensor is now configured.png>)

Heading back to the **Sensor** tab found within **Data Sources** I can now see that my sensor is properly configured, which is indicated by the green checkmark and is in the ready state.

![alt text](<assets/confirmation of aws sensor configuration.png>)

# Log Collection

Now heading back into our AWS environment, I will be configuring my asset to send it's logs to CloudWatch. During the sensor configuration I configured the sensor to retrieve logs from CloudWatch.

Remoting into my AWS asset, I install and launch the AWS CloudWatch agent via the CLI.

![alt text](<assets/13. installing cloudwatch agent.png>)

AFter the Agent is successfully installed; I return to the **AWS Console** and search for **CloudWatch**. 

Within **CloudWatch** I navigated to **Logs** and opened the Log Groups **Linux-Auth-Logs** and **OSQuery-Logs**, these logs are going to be sent from my asset, identified as **linux1**, and are being retrieved by the agent that was installed.

To confirm that the logs are properly being retrieved, I log back into **USM Anywhere** and head to the events section by going to **Activity** > **Events**.

Here we can see that the logs are being ingested within USMA.

![alt text](<assets/14. Receiving Linux logs from AWS Cloudwatch.png>)

