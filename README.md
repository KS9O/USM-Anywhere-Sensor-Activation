# AlienVault USM Sensor Deployment

## Table of Contents
- [AlienVault USM Sensor Deployment](#alienvault-usm-sensor-deployment)
  - [Table of Contents](#table-of-contents)
- [1. Introduction](#1-introduction)
- [2. Connecting the Sensor to AWS and The Running Assets](#2-connecting-the-sensor-to-aws-and-the-running-assets)
- [3. Sensor Deployment](#3-sensor-deployment)
    - [Sensor Connection](#sensor-connection)
    - [Sensor Registration](#sensor-registration)
- [4. USM Sensor Configuration](#4-usm-sensor-configuration)
    - [AWS Log Collection](#aws-log-collection)
    - [Log Management](#log-management)
    - [Open Threat Exchange (OTX)](#open-threat-exchange-otx)
- [5. Cloud Asset Log Ingestion](#5-cloud-asset-log-ingestion)
    - [CloudWatch Agent Installation](#cloudwatch-agent-installation)
    - [Log Group Verification](#log-group-verification)
    - [Log Ingestion Confirmation](#log-ingestion-confirmation)

# 1. Introduction
**Unified Security Management** (USM) is a cloud-native SaaS solution designed for rapid threat response. It goes beyond traditional SIEM by providing powerful threat and vulnerability detection across your environment. USM sensors deliver vital threat intelligence and integrate with multiple security tools. The platform automatically correlates all sensor data into actionable reports, alarms, and events, all of which are managed easily through your web browser.

This project demonstrates the process for deploying a USM Sensor within an AWS environment and configuring it to ingest logs from cloud-hosted assets.

# 2. Connecting the Sensor to AWS and The Running Assets

The USM Sensor is deployed on an **Amazon Web Service**s (AWS) **EC2** instance, specifically in the `us-east-2 region`.

Before deployment, all associated assets must be verified as running and accessible.

![alt text](<assets/Making sure to be on the correct AWS Server.png>)

![alt text](<assets/Checking all instances are live.png>)

# 3. Sensor Deployment

### Sensor Connection
1. Navigate to the AWS Console and locate the EC2 instance hosting the USM Sensor.

2. Retrieve the sensor's Public IPv4 Address to access the web-based setup utility.

![alt text](<assets/Connecting AWS logs by connecting my AWS instance with the IPv4 Public address.png>)

1. Access the USM Sensor's web interface and log in with the provided credentials to launch the USMA wizard.

![alt text](<assets/Connecting to USMA.png>)

2. Click **"Get Started"** to generate a **unique sensor authentication code**. (If a code is not automatically generated, navigate to the **Data Sources** section in USM Anywhere and select "**New Sensor**").

### Sensor Registration
![alt text](<assets/Configuring a new sensor by generating a sensor authentication code.png>)

3. Return to the deployed sensor's webpage, enter the authentication code, and assign a descriptive name ( "**USMA-TestSensor**").

4. Click "**Start Setup**" and wait for the sensor registration process to complete.


![alt text](<assets/Connecting to AWS Public sensor IP to intiate sensor connection to USMA with generated code.png>)

# 4. USM Sensor Configuration

The sensor’s logging and threat intelligence capabilities are configured via the USM Anywhere web console.

1. In USM Anywhere, navigate to **Data Sources > Sensors**.

2. Select the newly deployed sensor ("**USMA-TestSensor**") and open the configuration wizard via the wrench icon.

3. Acknowledge any prompts regarding Network Intrusion Detection interfaces.

   - If you have multiple NICs (Network Interface Cards), you'll need to select the one to use; however, for this example, only one is present. This might trigger a warning about Network Intrusion Detection interfaces, which you can safely dismiss by clicking OK.

![alt text](<assets/Configuring new sensor within USMA.png>)

### AWS Log Collection

In the **AWS Log Collection** section, specify the log sources to be ingested. For this project, all sources were selected except Apache and IIS logs.

![alt text](<assets/AWS Log Collection Settings.png>)

### Log Management

In the **Log Management** section, the system was configured to collect logs via **syslog**.

### Open Threat Exchange (OTX)

The **Open Threat Exchange** section connects the sensor to **LevelBlue's OTX platform**. This provides continuous, community-driven threat intelligence and Indicators of Compromise (IOCs).

The look-back period for OTX pulses was configured to **unlimited**.

![alt text](<assets/Configuring sensor to collect OTX pulses.png>)

Complete the wizard by clicking "**Start Using USM Anywhere**".


![alt text](<assets/19. USMA Sensor is now configured.png>)

The sensor's successful configuration is verified by a green checkmark and a "**Ready**" status in the **Sensors** tab.

![alt text](<assets/confirmation of aws sensor configuration.png>)

# 5. Cloud Asset Log Ingestion

To finalize log collection, the target AWS asset must be configured to send its logs to an accessible collection point, which in this scenario is **CloudWatch**, since we just configured the USM Sensor to retrieve them.

### CloudWatch Agent Installation
1. Remote connect to the target AWS asset.

2. Install and launch the AWS CloudWatch agent using the Command Line Interface (**CLI**).

![alt text](<assets/13. installing cloudwatch agent.png>)


### Log Group Verification
1. Navigate to the **CloudWatch** service in the AWS Console.

2. Verify the existence and population of the relevant Log Groups (**Linux-Auth-Logs** and **OSQuery-Logs**), which are now receiving data from the asset (**linux1**).

### Log Ingestion Confirmation

To confirm successful log ingestion by USM Anywhere:

1. Log into the USM Anywhere instance.

2. Navigate to **Activity > Events**.

The presence of the ingested logs confirms that the logs are being ingested from the asset's **CloudWatch** agent to the USM Anywhere.

![alt text](<assets/14. Receiving Linux logs from AWS Cloudwatch.png>)

