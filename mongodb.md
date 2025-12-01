## Ubuntu 22.04.1 LTS (GNU/Linux 5.15.0-125-generic x86_64)
# Install prerequisites
sudo apt update
sudo apt install -y wget gnupg

# Import MongoDB GPG key
wget -qO- https://www.mongodb.org/static/pgp/server-7.0.asc | sudo tee /etc/apt/trusted.gpg.d/mongodb.asc

# Add MongoDB 7.0 repository for Ubuntu 22.04 (Jammy)
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

# Install MongoDB
sudo apt update
sudo apt install -y mongodb-org

# Start MongoDB
sudo systemctl start mongod
sudo systemctl enable mongod

### Step 1: Check MongoDB's Service Status
First, verify whether the MongoDB service is installed and recognized by systemd:

```bash
sudo systemctl status mongod
```

If MongoDB is installed correctly, you should see output about its current status.

---

### Step 2: Enable MongoDB to Start on Boot
Run the following command to enable MongoDB to start automatically after a system reboot:

```bash
sudo systemctl enable mongod
```

This creates a symbolic link for the MongoDB service in the system's startup sequence.

---

### Step 3: Start MongoDB Manually (If Not Running)
If MongoDB is not currently running, you can start it with:

```bash
sudo systemctl start mongod
```

---

### Step 4: Verify MongoDB is Running
Check if MongoDB is running properly with:

```bash
sudo systemctl status mongod
```

You should see `active (running)` in the output.

---

### Step 5: Reboot and Test
Reboot your server to ensure MongoDB starts automatically:

```bash
sudo reboot
```

After the server restarts, check the MongoDB service status again:

```bash
sudo systemctl status mongod
```

---

### Troubleshooting (If MongoDB Still Does Not Start)

1. **Check Logs**  
   View the MongoDB logs to identify issues:

   ```bash
   sudo journalctl -u mongod
   ```

2. **Permissions Issue**  
   Ensure MongoDB has the correct permissions for its data directory:

   ```bash
   sudo chown -R mongodb:mongodb /var/lib/mongodb
   sudo chmod -R 700 /var/lib/mongodb
   ```

3. **Reinstall MongoDB**  
   If the service is not recognized, you may need to reinstall MongoDB.

## Backup
 ```bash
mongodump --host 127.0.0.1 --port 27017 \
          --db csodb \
          --username admin --password sajadweb1368 \
          --authenticationDatabase admin \
          --out /home/backup
 ```
 ## Store
 ```bash
          mongorestore --host 127.0.0.1 --port 20025 \
             --db csodb \
             --username admin --password sajadweb1368 \
             --authenticationDatabase admin \
             /home/backup/csodb
 ```
