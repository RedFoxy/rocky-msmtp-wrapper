# Rocky Linux msmtp wrapper
A lightweight email wrapper script for Rocky Linux using msmtp to send authenticated emails with hostname-based subject tags and local alias resolution. Perfect for scripting, cron jobs, and system notifications via external SMTP.

<a href="https://paypal.me/redfoxydarrest" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-blue.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>

## Menu:
- [Requirements](#requirements)
- [Files and Configuration](#files-and-configuration)
- [Instructions to follow](#instructions-to-follow)

<hr>

## Requirements
- Rocky Linux (or any distribution compatible with msmtp)
- Access to an SMTP server
- msmtp and s-nail installed

## Files and Configuration
### 1. /opt/bin/msmtp-wrapper.sh
This script reads the input email, resolves the recipient's alias, modifies the subject by adding the hostname, and sends the email using msmtp.

### 2. /opt/etc/msmtp-aliases
File containing aliases for local users. Example:
```
root: user@example.com
postmaster: postmaster@example.com
```

### 3. /etc/msmtprc
Configuration file for `msmtp`. This example uses `SMTP authentication` and a `TLS connection on port 586`, adjust the settings according to your needs.
The `from field`, if your mail server allows sending emails from an alias, can be set to a specific server address such as server.notify@example.com.
```
defaults
auth           on
tls            on
tls_trust_file /etc/ssl/certs/ca-bundle.crt
logfile        /var/log/msmtp.log

account        default
host           smtp.example.com
port           587
from           user@example.com
user           user@example.com
password       yourpassword
domain         example.com
```

## Instructions to follow:
### 1. Prerequisites
   - Install the required packages: `msmtp` and `s-nail`.
      ```
      sudo dnf install msmtp s-nail
      ```

### 2. Create or use an existing msmtprc configuration file:
   - You can use the provided msmtprc file from this Git repository or create a new one at `/etc/msmtprc`.
   - Adjust the configuration as needed, for example using SMTP authentication and TLS on port 586.
   - Set the from field to a specific server address (e.g. `server.notify@example.com`) if your mail server allows sending from aliases.

### 3. Set correct permissions for `/etc/msmtprc`:
   ```
   sudo chmod 600 /etc/msmtprc
   ```

### 4. Download and set up the wrapper script:
   - Download `msmtp-wrapper.sh`.
   - Place it wherever you prefer (e.g. `/opt/bin`).
   - Make it executable:
      ```
      sudo chmod +x /opt/bin/msmtp-wrapper.sh
      ```

### 5. Create the aliases file msmtp-aliases:
   - Default location is `/opt/etc/msmtp-aliases`.
   - If you choose a different directory, remember to update the path inside the `msmtp-wrapper.sh` script.
   - Make sure the file is readable by all users:
      ```
      sudo chmod 644 /opt/etc/msmtp-aliases
      ```
### 6. Test it!
   - To test sending an email:​
      ```
      echo "This is the body of the test email." | mail -s "This is a test email sent via msmtp to a local alias." root
      ```
