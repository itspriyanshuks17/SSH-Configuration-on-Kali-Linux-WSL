# SSH Connection Troubleshooting Guide

This guide walks you through troubleshooting SSH connection issues, especially related to password authentication and connection refusals.

## Common SSH Login Error: Permission Denied

If you encounter the following error during SSH login:

```bash
ssh user@<server-ip>
user@<server-ip>'s password:
Permission denied, please try again.
```

### Solution 1: Enable Password Authentication in `sshd_config`

1. **Edit the SSH Configuration File**:
   - Open the `/etc/ssh/sshd_config` file in a text editor:
   
     ```bash
     sudo nano /etc/ssh/sshd_config
     ```
   
   - Find and uncomment the following line by removing the `#` at the beginning:

     ```bash
     #PasswordAuthentication yes
     ```

   - Change it to:

     ```bash
     PasswordAuthentication yes
     ```

   - **Optional**: If `PermitRootLogin` is set to `prohibit-password`, change it to `yes` (not recommended for security reasons):

     ```bash
     PermitRootLogin yes
     ```

2. **Restart the SSH Service**:
   After making these changes, restart the SSH service:

   ```bash
   sudo service ssh restart
   ```

3. **Retry SSH Login**:
   Now, try logging in again from your client machine:

   ```bash
   ssh user@<server-ip>
   ```

---

## Common SSH Error: Connection Refused

If you encounter the following error:

```bash
ssh: connect to host <server-ip> port 22: Connection refused
```

Follow these steps to resolve it:

### Solution 2: Ensure SSH Server is Running

1. **Check if the SSH Server is Installed**:
   On the server, verify if the OpenSSH server is installed:

   ```bash
   sudo apt update
   sudo apt install openssh-server
   ```

2. **Start the SSH Service**:
   Start the SSH service if it’s not running:

   ```bash
   sudo service ssh start
   ```

   To ensure it starts automatically on boot:

   ```bash
   sudo systemctl enable ssh
   ```

3. **Verify SSH is Running on Port 22**:
   Check if the SSH service is listening on port 22:

   ```bash
   sudo netstat -tuln | grep :22
   ```

   If it's not, ensure the `sshd_config` file is set to listen on port 22. After making changes, restart the SSH service:

   ```bash
   sudo service ssh restart
   ```

4. **Allow SSH Through the Firewall**:
   If a firewall is active, allow SSH traffic. For systems using `ufw` (Uncomplicated Firewall):

   ```bash
   sudo ufw allow 22/tcp
   sudo ufw reload
   ```

5. **Retry SSH Connection**:
   Try SSH again from your client machine:

   ```bash
   ssh user@<server-ip>
   ```

---

## Conclusion

By following these steps, you should be able to resolve common SSH issues such as permission denials and connection refusals. If problems persist, further investigation of the server logs (`/var/log/auth.log`) may be necessary to identify specific causes.
