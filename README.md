# Deploy SoftEther and Wazuh using CloudFormation on AWS

[cite_start]This project focuses on deploying SoftEther VPN and Wazuh, an open-source security monitoring platform, on Amazon Web Services (AWS) using CloudFormation (CF) to monitor and block malicious IPs[cite: 2]. [cite_start]This is achieved through an Infrastructure as Code (IaC) approach, leveraging AWS CloudFormation stacks to provision all necessary resources[cite: 3].

## How It Works

The deployment requires a VPC with a minimum of three availability zones. The CloudFormation templates for this setup are available in this repository.

The CloudFormation stack will install and configure the Wazuh client on the SoftEther instance. This client will then collect application logs and send them to the central Wazuh server. Upon receiving the logs, Wazuh will display the information on a pre-configured dashboard imported from this repository.

## Infrastructure Deployment with AWS CloudFormation

[cite_start]To begin, the core infrastructure needs to be implemented[cite: 5]. [cite_start]AWS CloudFormation is utilized for this purpose, transforming templates into essential AWS resources[cite: 6]. These resources include:

* [cite_start]VPC (Virtual Private Cloud) [cite: 7]
* [cite_start]Private and Public Subnets [cite: 8]
* [cite_start]EC2 Instances [cite: 9]
* [cite_start]Security Groups [cite: 10]
* [cite_start]IAM Roles [cite: 11]
* [cite_start]Network Interfaces [cite: 12]
* [cite_start]Elastic IPs [cite: 13]

[cite_start]The repository link for the CloudFormation templates is: [https://github.com/cloudkeynet/softether_wazuh.git](https://github.com/cloudkeynet/softether_wazuh.git) [cite: 17, 18]

### Step 1: Deploying the VPC Template

[cite_start]The initial step involves uploading the `VPC.yml` template[cite: 14, 15]. [cite_start]This template defines your VPC and the subnets necessary for subsequent connections[cite: 15]. [cite_start]It includes two public and two private subnets, from which one of each type will be used for the deployment[cite: 16].

### Step 2: Deploying the SoftEther and Wazuh Template

[cite_start]Next, deploy the `SoftEther_internal.yml` template[cite: 19, 20]. [cite_start]This template contains the resources required to create the SoftEther VPN instance, which acts as a secure gateway to the Wazuh instance[cite: 20].

[cite_start]This template requires specific configurations[cite: 21]:

* [cite_start]Wazuh and SoftEther Passwords [cite: 22]
* [cite_start]Private and Public Subnet IDs [cite: 23]
* [cite_start]VPC ID [cite: 24]
* [cite_start]Environment Name [cite: 25]
* [cite_start]Encrypted Message [cite: 26]

[cite_start]The resources created by this template include a security group, a network interface, an elastic IP, and the Wazuh instance hosting the security platform[cite: 27]. [cite_start]The IP address to access Wazuh will be displayed in the "Outputs" section of the CloudFormation Stack[cite: 28].

### Step 3: Connect to SoftEther VPN Server

[cite_start]Open the SoftEther VPN Server Manager program[cite: 29]. [cite_start]Connect to the VPN using the Elastic IP found in the "Outputs" section of the "SoftEther_internal" CloudFormation stack[cite: 30].

### Step 4: Create VPN Users and Connect via Client

[cite_start]After connecting to the server, create the necessary user accounts within the SoftEther VPN Server Manager[cite: 31, 32]. [cite_start]Subsequently, use the SoftEther VPN Client Manager to connect using the new user credentials and the same Elastic IP[cite: 33]. [cite_start]Once connected, you will be able to access the graphical interface of Wazuh[cite: 34].

### Step 5: Log in to Wazuh

[cite_start]Access the Wazuh interface and log in using the default user "admin" and the password you assigned during the CloudFormation stack creation[cite: 35, 36].

### Step 6: Active Response Configuration on Wazuh EC2

[cite_start]To configure active response on the Wazuh instance, follow these steps[cite: 37, 38]:

1.  [cite_start]**Connect to the AWS EC2 Instance**: [cite: 39]
    * [cite_start]Open the AWS console and navigate to the EC2 service[cite: 40].
    * [cite_start]Locate your Wazuh instance[cite: 41].
    * [cite_start]Select the instance and click “Connect”[cite: 42].
    * [cite_start]Choose the “Session Manager” option and connect to the instance[cite: 43].
2.  [cite_start]**Gain Superuser Privileges**: [cite: 44]
    * [cite_start]Once connected, execute `sudo su` to obtain superuser permissions[cite: 45, 46].
3.  [cite_start]**Modify the Wazuh Configuration File**: [cite: 47]
    * [cite_start]Open the `ossec.conf` configuration file with a text editor (e.g., `vi` or `nano`) at `/var/ossec/etc/ossec.conf`[cite: 48, 49].
    * [cite_start]Scroll to the `<active-response>` section and add the following code block[cite: 50]:
        ```xml
        <ossec_config>
          <active-response>
            <disabled>no</disabled>
            <command>netsh</command>
            <location>local</location>
            <rules_id>100100</rules_id>
            <timeout>60</timeout>
          </active-response>
        </ossec_config>
        [cite_start]```[cite: 51, 52, 53, 54, 55, 56, 57, 58, 59]
    * [cite_start]Save changes and close the file[cite: 60].
4.  [cite_start]**Restart the Wazuh Manager Service**: [cite: 61]
    * [cite_start]Apply changes by restarting the Wazuh Manager service with `sudo systemctl restart wazuh-manager`[cite: 62, 63].

### Step 7: Import Dashboard Visualizations

[cite_start]From the top-right corner menu, navigate to "Dashboards Management" and select "Dashboards Management" again from the dropdown[cite: 64, 65]. [cite_start]Within this section, click on "Saved Objects" and then import the `Visualizations.ndjson` file[cite: 66]. [cite_start]This file contains all the pre-configured graphs and visualizations for the "Explore-Dashboard" section[cite: 67].

## Conclusion

[cite_start]This project provides a detailed guide for deploying Wazuh on AWS using an Infrastructure as Code approach with CloudFormation, ensuring scalable and reproducible security monitoring[cite: 69]. [cite_start]The integration of SoftEther VPN secures access to the Wazuh instance[cite: 70]. [cite_start]Key benefits of this approach include automated and reproducible deployments, an enhanced security posture, scalability, operational efficiency, and cost-effectiveness[cite: 70]. [cite_start]This blueprint enables organizations to establish a resilient and proactive security framework on AWS, protecting cloud assets and maintaining business continuity[cite: 71].