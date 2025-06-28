# SSH

## Key generate

```sh
ssh-keygen -t rsa daram3118.gmail.com
cd ~/.ssh
```

To configure SSH to connect to a server without being prompted for a password, you need to set up SSH key-based authentication. Here’s how to do it:

1. **Generate an SSH key pair** (if you don’t already have one):

   ```bash
   //github
   ssh-keygen -t ed25519 -C daram3118.gmail.com

   ssh-keygen -t rsa -b 4096 -C daram3118.gmail.com
   ```

   - This command creates a new SSH key using the RSA algorithm with a 4096-bit key size.
   - When prompted, you can accept the default file location (`~/.ssh/id_rsa`) and optionally set a passphrase (you can skip it by pressing Enter for no passphrase).

2. **Copy the public key to the server**:

   ```bash
   ssh-copy-id root@server_ip
   ```

   - Replace `server_ip` with the IP address of your server.
   - This command will copy your public key (`~/.ssh/id_rsa.pub`) to the server’s `~/.ssh/authorized_keys` file for the root user.

3. **Manually copying the public key** (if `ssh-copy-id` is not available):
   - Open your public key file (`~/.ssh/id_rsa.pub`) and copy its content.
   - Connect to your server manually:

     ```bash
     ssh root@server_ip
     ```

   - Once logged in, open the `authorized_keys` file:

     ```bash
     mkdir -p ~/.ssh
     echo "your-public-key-content" >> ~/.ssh/authorized_keys
     chmod 600 ~/.ssh/authorized_keys
     chmod 700 ~/.ssh
     ```

   - Make sure to replace `"your-public-key-content"` with the content of your `id_rsa.pub` file.

4. **Disable password authentication (optional, for increased security)**:
   - Edit the SSH configuration file on the server:

     ```bash
     sudo nano /etc/ssh/sshd_config
     ```

   - Find and modify the following settings:

     ```
     PasswordAuthentication no
     PermitRootLogin prohibit-password
     ```

   - Restart the SSH service:

     ```bash
     sudo systemctl restart sshd
     ```

Now, you should be able to connect using `ssh root@server_ip` without being prompted for a password.

If `ssh-copy-id` is not available on your system (common on Windows), you can manually copy the public key to the server. Here’s how to do it:

1. **Generate an SSH key pair** (if you haven’t already):

   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```

   - You can do this in a Git Bash terminal, WSL (Windows Subsystem for Linux), or any terminal that supports SSH commands.
   - When prompted, accept the default location for the key file (`~/.ssh/id_rsa`) and set a passphrase if desired.

2. **Manually copy the public key to the server:**
   - Open the public key file. On Windows, it’s usually located at:

     ```
     C:\Users\your_username\.ssh\id_rsa.pub
     ```

   - Open the `id_rsa.pub` file with a text editor and copy its content.

3. **Log in to the server:**
   - Use an SSH client like PuTTY, Git Bash, or WSL to log in:

     ```bash
     ssh root@server_ip
     ```

   - Replace `server_ip` with your actual server IP address.

4. **Add the public key to the `authorized_keys` file on the server:**
   - Once logged in, create the `.ssh` directory and set the correct permissions (if it doesn’t already exist):

     ```bash
     mkdir -p ~/.ssh
     chmod 700 ~/.ssh
     ```

   - Add your public key to the `authorized_keys` file:

     ```bash
     echo "your-public-key-content" >> ~/.ssh/authorized_keys
     chmod 600 ~/.ssh/authorized_keys
     ```

   - Replace `"your-public-key-content"` with the content of your `id_rsa.pub` file that you copied earlier.

5. **Test the SSH connection:**
   - Try connecting again:

     ```bash
     ssh root@server_ip
     ```

   - You should now be able to log in without being prompted for a password.

This manual approach achieves the same result as `ssh-copy-id`, setting up passwordless SSH login.

## Multissh
```ssh
ssh-keygen -t ed25519 -C "email@account2.com" -f ~/.ssh/id_ed25519_account2
```

از لیست فایل‌های شما مشخص است که دو کلید SSH دارید:

1. کلید اصلی (احتمالاً برای اکانت اول):
   - `id_ed25519` (کلید خصوصی)
   - `id_ed25519.pub` (کلید عمومی)

2. کلید جدید برای اکانت دوم:
   - `id_ed25519_account2` (کلید خصوصی)
   - `id_ed25519_account2.pub` (کلید عمومی)

### 🔧 راه‌حل گام‌به‌گام:

---

### 1. **ساخت فایل `config` در پوشه `.ssh`**
از آنجا که این فایل را ندارید، آن را ایجاد کنید:

```bash
nano ~/.ssh/config
```

سپس این محتوا را وارد کنید (با توجه به کلیدهای شما):

```config
# اکانت اصلی (پیش‌فرض)
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519
  IdentitiesOnly yes

# اکانت دوم
Host github.com-account2
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_account2
  IdentitiesOnly yes
```

- **ذخیره و خروج**:  
  در `nano` کلیدهای `Ctrl+O` (ذخیره)، سپس `Enter` و بعد `Ctrl+X` (خروج).

---

### 2. **تنظیم مجوزهای فایل `config`**
فایل کانفیگ باید مجوزهای امنیتی داشته باشد:

```bash
chmod 600 ~/.ssh/config
```

---

### 3. **اضافه کردن کلید جدید به SSH-Agent**
برای اطمینان از شناسایی کلیدها:

```bash
eval "$(ssh-agent -s)"
ssh-add --apple-use-keychain ~/.ssh/id_ed25519_account2
```

> 📝 **نکته**: اگر خطایی دیدید، `--apple-use-keychain` را حذف کنید (مخصوص macOS جدید).

---

### 4. **تست اتصال برای اکانت دوم**
بررسی کنید که کلید جدید کار می‌کند:

```bash
ssh -T git@github.com-account2
```

اگر موفق بود، پیامی شبیه این می‌بینید:  
```
Hi username2! You've successfully authenticated...
```

---

### 5. **استفاده از اکانت دوم در Git**
حالا می‌توانید برای ریپوهای اکانت دوم از آدرس ویژه استفاده کنید:

```bash
git clone git@github.com-account2:username2/repo.git
```

یا اگر ریپویی از قبل دارید، آدرس ریموت را تغییر دهید:

```bash
git remote set-url origin git@github.com-account2:username2/repo.git
```

---

### 6. **تنظیم ایمیل/نام Git برای اکانت دوم (اختیاری)**
در هر ریپو می‌توانید مشخصات اکانت دوم را تنظیم کنید:

```bash
git config user.name "username2"
git config user.email "email@account2.com"
```

---

### 🔍 عیب‌یابی:
- **اگر خطای `Permission denied` داد**:  
  - مطمئن شوید کلید عمومی (`id_ed25519_account2.pub`) در تنظیمات GitHub اکانت دوم اضافه شده است.  
  - دستور `ssh-add -l` را اجرا کنید تا ببینید کلیدها اضافه شده‌اند یا خیر.  

- **اگر فایل `config` تأثیر نداشت**:  
  - دستور `ssh -vT git@github.com-account2` را اجرا کنید تا خطاهای دقیق را ببینید.  

