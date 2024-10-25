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
