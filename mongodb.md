## Ubuntu 22.04.1 LTS (GNU/Linux 5.15.0-125-generic x86_64)

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
