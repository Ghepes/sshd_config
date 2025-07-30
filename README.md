# sshd_config
sshd_config vm login ssh only with certificat


### Setting


### **Set root pasword If only no password to SSH:**
```bash
sudo passwd root
```

### **Steps to resolve the issue root:**
#### 1. **Check root's SSH keys**
### Go to root's home directory to see what keys are configured:
```bash
sudo -i
cd /root/.ssh
ls -l
```

### If the `authorized_keys` file does not exist or if it does not contain the appropriate public key, add the public key used when installing Ubuntu. You can do this like this:
### Replace `<your_public_key>` with the exact contents of the `.pub` file you are using for `user ubuntu`.
```bash
echo "<your_public_key.pub>" >> /root/.ssh/authorized_keys
```bash


### Make sure the permissions are set correctly:
```bash
chmod 700 /root/.ssh
chmod 600 /root/.ssh/authorized_keys
```


### 2. **Enable root authentication **
### Edit the `/etc/ssh/sshd_config` file:
```bash
sudo nano /etc/ssh/sshd_config
```


### Find and modify the following lines:
### Change the value:

PermitRootLogin yes

PubkeyAuthentication yes

AuthorizedKeysFile	.ssh/authorized_keys

PasswordAuthentication no



### After saving the changes, restart the SSH service:
```
sudo systemctl restart sshd
```

######################### Add file.pub Key to VM  /root file  ###########################

#### Add Only file.pub to /root/.ssh/authorized_keys
#### 3. **Ensure that the key .pub from vm end .ppk from local is correct**
Check which SSH key is used for the connection. If you are using an SSH client like `PuTTY` or `FileZilla`, make sure that the `.ppk` or `.pub` file matches the key configured in `/root/.ssh/authorized_keys`.
---
#### 4. **Test Authentication**
To verify authentication directly as root:
```bash
ssh root@<IP_server>
```
Replace `<IP_server>` with the IP address of the node. If you encounter errors, you can enable more error details by running SSH in verbose mode:
```bash
ssh -v root@<IP_server>
```

---

#### 5. **Optional: Password authentication for root**
If you prefer to use password authentication, also adjust this setting in the `/etc/ssh/sshd_config` file:
```plaintext
PasswordAuthentication yes
```
After changing, restart SSH:
```bash
sudo systemctl restart sshd
```

---

