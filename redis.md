Here are several methods to install Redis on Ubuntu 24.04 (Noble):

## **Method 1: Install from Official Ubuntu Repository (Easiest)**

```bash
# Update package list and install Redis
sudo apt update
sudo apt install -y redis-server

# Check Redis version
redis-server --version

# Start and enable Redis service
sudo systemctl start redis-server
sudo systemctl enable redis-server
sudo systemctl status redis-server
```

## **Method 2: Install Latest Redis from Official Repository (Recommended)**

### **Step 1: Add Redis Official Repository**
```bash
# Install prerequisites
sudo apt update
sudo apt install -y curl

# Add Redis repository
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg

# Add repository for Ubuntu 24.04 (if not available, use 22.04/jammy)
echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list

# If the above fails (no noble repo yet), use jammy instead:
echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb jammy main" | sudo tee /etc/apt/sources.list.d/redis.list
```

### **Step 2: Install Redis**
```bash
# Update package list and install Redis
sudo apt update
sudo apt install -y redis

# Verify installation
redis-server --version
```

## **Method 3: Install Specific Redis Version**

### **Install Redis 7.x**
```bash
# For Redis 7.2
sudo apt install -y redis-server=7.2*

# For Redis 7.0
sudo apt install -y redis-server=7.0*
```

## **Method 4: Install via Snap**

```bash
# Install Redis via Snap (latest stable)
sudo snap install redis

# Check status
sudo snap services redis

# Connect to Redis
redis-cli
```

## **Method 5: Build from Source (Latest Version)**

```bash
# Install build dependencies
sudo apt update
sudo apt install -y build-essential tcl

# Download latest Redis source
cd /tmp
curl -O https://download.redis.io/redis-stable.tar.gz
tar xzf redis-stable.tar.gz
cd redis-stable

# Build and install
make
make test
sudo make install

# Create Redis user and directories
sudo mkdir -p /etc/redis /var/lib/redis
sudo useradd -r -s /bin/false redis
sudo chown redis:redis /var/lib/redis

# Copy configuration
sudo cp redis.conf /etc/redis/

# Create systemd service
sudo nano /etc/systemd/system/redis.service
```

Add this to `/etc/systemd/system/redis.service`:
```ini
[Unit]
Description=Redis In-Memory Data Store
After=network.target

[Service]
User=redis
Group=redis
ExecStart=/usr/local/bin/redis-server /etc/redis/redis.conf
ExecStop=/usr/local/bin/redis-cli shutdown
Restart=always

[Install]
WantedBy=multi-user.target
```

```bash
# Reload systemd and start Redis
sudo systemctl daemon-reload
sudo systemctl start redis
sudo systemctl enable redis
```

## **Configure Redis**

### **Basic Configuration**
```bash
# Edit Redis configuration
sudo nano /etc/redis/redis.conf

# Common changes:
# - bind 127.0.0.1 ::1  (change to 0.0.0.0 for remote access)
# - requirepass yourpassword  (set password)
# - maxmemory 256mb  (set memory limit)
# - maxmemory-policy allkeys-lru  (eviction policy)

# After changes, restart Redis
sudo systemctl restart redis-server
```

### **Test Redis Connection**
```bash
# Connect to Redis CLI
redis-cli

# Test with ping
redis-cli ping

# Set and get a key
redis-cli set test "Hello Redis"
redis-cli get test

# Monitor Redis in real-time
redis-cli monitor

# Get Redis info
redis-cli info
```

## **Useful Commands**

### **Service Management**
```bash
# Start Redis
sudo systemctl start redis-server

# Stop Redis
sudo systemctl stop redis-server

# Restart Redis
sudo systemctl restart redis-server

# Check Redis status
sudo systemctl status redis-server

# View Redis logs
sudo journalctl -u redis-server -f
```

### **Redis CLI Operations**
```bash
# Connect with authentication
redis-cli -a yourpassword

# Flush all data
redis-cli flushall

# Check Redis statistics
redis-cli info stats
redis-cli info memory

# List all keys
redis-cli keys "*"

# Check Redis performance
redis-cli --latency
redis-cli --latency-history
```

## **Security Configuration**

```bash
# Set Redis password
sudo nano /etc/redis/redis.conf
# Find and uncomment/add: requirepass your_strong_password

# Bind to localhost only (default for security)
# Change to specific IP if needed

# Rename dangerous commands
sudo nano /etc/redis/redis.conf
# Add at bottom:
rename-command FLUSHDB ""
rename-command FLUSHALL ""
rename-command CONFIG ""

# Restart to apply changes
sudo systemctl restart redis-server
```

## **Enable Persistence**

```bash
sudo nano /etc/redis/redis.conf

# Enable RDB persistence
save 900 1      # save after 900 sec if at least 1 key changed
save 300 10     # save after 300 sec if at least 10 keys changed
save 60 10000   # save after 60 sec if at least 10000 keys changed

# Enable AOF persistence
appendonly yes
appendfilename "appendonly.aof"

# Restart Redis
sudo systemctl restart redis-server
```

## **Uninstall Redis**

```bash
# Stop and remove Redis
sudo systemctl stop redis-server
sudo apt remove --purge redis-server

# Remove configuration files
sudo rm -rf /etc/redis
sudo rm -rf /var/lib/redis
sudo rm -rf /var/log/redis

# Remove from Snap
sudo snap remove redis

# Remove repository if added
sudo rm /etc/apt/sources.list.d/redis.list
sudo rm /usr/share/keyrings/redis-archive-keyring.gpg
```

## **Quick Installation (Recommended)**

For most users on Ubuntu 24.04, this is the simplest approach:

```bash
sudo apt update
sudo apt install -y redis-server
sudo systemctl enable --now redis-server
redis-cli ping
```

## **Troubleshooting**

### **If Redis won't start:**
```bash
# Check logs
sudo journalctl -u redis-server -f

# Test configuration
sudo redis-server /etc/redis/redis.conf --test

# Check if port 6379 is in use
sudo netstat -tlnp | grep 6379
```

### **If you get connection errors:**
```bash
# Check if Redis is listening
sudo ss -tlnp | grep redis

# Check bind address in config
sudo grep -E "^(bind|protected-mode)" /etc/redis/redis.conf

# Check firewall
sudo ufw status
sudo ufw allow 6379
```
