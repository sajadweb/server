# SSH

## Key generate

```sh
ssh-keygen -t rsa daram3118.gmail.com
cd ~/.ssh
```

To configure SSH to connect to a server without being prompted for a password, you need to set up SSH key-based authentication. Here’s how to do it:

1. **Generate an SSH key pair** (if you don’t already have one):

   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
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
