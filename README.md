
# Nginx Monitoring and VirusTotal Integration with Wazuh

## Installation

### Step 1: Configure the Wazuh Agent for Nginx Monitoring

Edit the shared configuration file for the Wazuh agent:

sudo nano /var/ossec/etc/shared/default/agent.conf

Add the following configuration to monitor Nginx logs and check file integrity:

<agent_config>
    <!-- Shared agent configuration here -->
    <localfile>
        <log_format>apache</log_format>
        <location>/var/log/nginx/access.log</location>
    </localfile>
    <localfile>
        <log_format>apache</log_format>
        <location>/var/log/nginx/error.log</location>
    </localfile>
    <syscheck>
        <directories check_all="yes" report_changes="yes" realtime="yes">/var/www/html</directories>
        <directories realtime="yes">/var/www/html</directories>
    </syscheck>
</agent_config>

### Step 2: Install jq

Install jq, a utility for processing JSON input:

sudo apt update && sudo apt upgrade -y
sudo apt install jq -y

### Step 3: Create Active Response Script

Create a bash script for threat removal:

sudo nano /var/ossec/active-response/bin/remove-threat.sh

Add the provided script content.

### Step 4: Set Permissions and Ownership

Change the permissions and ownership for the script:

sudo chmod 750 /var/ossec/active-response/bin/remove-threat.sh
sudo chown root:wazuh /var/ossec/active-response/bin/remove-threat.sh

### Step 5: Restart the Wazuh Agent

Restart the Wazuh agent to apply the configuration changes:

sudo systemctl restart wazuh-agent

### Step 6: Add Rules for File Monitoring

Add the provided rules to monitor changes in the /var/www/html directory:

sudo nano /var/ossec/etc/rules/local_rules.xml

### Step 7: Configure VirusTotal Integration

Edit the Wazuh server configuration file to enable VirusTotal integration:

sudo nano /var/ossec/etc/ossec.conf

Add the provided configuration, replacing <YOUR_VIRUS_TOTAL_API_KEY> with your VirusTotal API key.

### Step 8: Enable Active Response

Append the provided configuration to enable active response and trigger the remove-threat.sh script.

sudo nano /var/ossec/etc/ossec.conf

### Step 9: Add Alert Rules

Add the provided rules to the Wazuh server to alert about the active response results.

sudo nano /var/ossec/etc/rules/local_rules.xml

### Step 10: Restart the Wazuh Manager

Restart the Wazuh manager to apply the configuration changes:

sudo systemctl restart wazuh-manager

## Additional Notes

- Ensure the VirusTotal API key is valid and has sufficient quota.
- Verify the Nginx log file paths and adjust if necessary based on your server configuration.
- Adjust the directory paths in the syscheck configuration to match the locations you want to monitor.

This guide provides a basic setup for integrating VirusTotal with Wazuh and monitoring Nginx logs. Adjustments may be needed based on your specific environment and requirements.
