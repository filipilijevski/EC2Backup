# EC2 Backup Script

A Bash script to securely back up a local directory to an AWS EC2 instance using `rsync` over SSH, with optional email notifications.

## Features

- **Secure File Transfer:** Uses `rsync` over SSH to transfer files.
- **Backup Directory Management:** Optionally creates time-stamped backup directories on the remote host.
- **Logging:** Saves logs to a specified directory with restricted permissions.
- **Email Notifications:** Sends success/failure notifications via Postfix (or any MTA providing a `sendmail` interface).

---

## Requirements

- **Linux/Unix environment** with Bash shell (Debian-based distributions recommended).
- **Installed Commands:**  
  - `rsync`
  - `ssh` (for the `-e "ssh -i KEY_FILE"` parameter)
  - `sendmail` (provided by Postfix or another MTA)
  - `mkdir`
  - `chmod`
  - `tee`
  - `date`
  - `echo`
- **SSH Key** allowing passwordless login to the remote EC2 instance.
- **Postfix (or equivalent)** configured if you want email notifications.

---

## Installation

1. **Clone or copy** this repository to your local machine.
2. **Make the script executable:**
   ```bash
   chmod +x ec2backup.sh

3. **(Optional) Install the script system-wide:**
      ```bash
      sudo mv ec2backup.sh /usr/local/bin/ec2backup
      sudo chmod +x /usr/local/bin/ec2backup

---

## Configuration

Environment Variables Setup

Before running the script, you need to export several environment variables that configure its behavior. 
These variables define connection details, paths, and notification settings.
You can set them in your current shell session by running:
```bash
export REMOTE_USER=admin                     # SSH username on EC2 (e.g., ec2-user, ubuntu, admin)
export REMOTE_HOST=1.2.3.4                   # Public DNS or IP of your EC2 instance
export KEY_FILE=~/.ssh/my-ec2-key.pem        # Path to your private SSH key
export REMOTE_BASE_DIR=/home/admin/backups   # Base directory on EC2 instance for backups
export DRY_RUN=false                         # Optional: Set to true to perform a dry run
export LOG_DIR=logs                          # Optional: Directory for log files
export EMAIL_RECIPIENT="you@example.com"     # Email address for notifications
```
---

## Troubleshooting

- Command Not Found:
    Ensure all required commands (rsync, ssh, sendmail, etc.) are installed on both local and remote machines.

- Email Issues:
    Confirm EMAIL_RECIPIENT is set to the desired address.
    Check Postfix configuration and logs (typically /var/log/mail.log).
    Verify your SMTP relay settings if using Gmail, Outlook, etc.

- SSH/rsync Errors:
    Validate that your SSH key (KEY_FILE) is correct and has proper permissions.
    Ensure your remote host details (REMOTE_HOST, REMOTE_USER) are correct.
    Check network connectivity and security group settings on AWS.

---
