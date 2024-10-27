### What is SSH?

SSH (Secure Shell) is a protocol used to securely connect to a remote server and execute commands. It provides encrypted communication between a client and a server, allowing users to manage remote systems, transfer files, and tunnel traffic over secure connections.

#### **Key Points:**
- SSH creates an encrypted channel between two computers.
- It uses a client-server model: the **SSH client** is installed on your local machine and the **SSH server** on the remote machine.

---

### Setting Up SSH Server and Client

1. **Installing SSH Server**:
   To set up an SSH server, you need to install the `openssh-server` package.

   On Debian-based systems:
   ```bash
   sudo apt install openssh-server
   ```

   On RedHat-based systems:
   ```bash
   sudo yum install openssh-server
   ```

   **Starting the SSH service**:
   ```bash
   sudo systemctl enable ssh
   sudo systemctl start ssh
   sudo systemctl status ssh
   ```

2. **Installing SSH Client**:
   The SSH client allows you to connect to a remote server. In most Linux systems, the SSH client is pre-installed. If not, install it using:
   ```bash
   sudo apt install openssh-client
   ```

3. **Firewall Configuration**:
   If a firewall is running, you need to allow SSH traffic:
   ```bash
   sudo ufw allow OpenSSH
   ```

---

### Connecting to a Remote Server via SSH

Once both client and server are set up, you can connect to a remote machine using:
```bash
ssh username@remote_host
```
For example:
```bash
ssh user@192.168.1.100
```

If this is your first time connecting, SSH will ask if you want to trust the server's fingerprint. Type `yes`, and the host's fingerprint will be stored in `~/.ssh/known_hosts`.

To exit the remote shell, type `exit`.

---

### Using SSH Keys for Passwordless Authentication

**SSH keys** are more secure than password-based authentication. Here's how to generate and use SSH keys.

1. **Generate SSH Keys**:
   On your local machine, run:
   ```bash
   ssh-keygen
   ```
   Follow the prompts, and it will create two files:
   - `~/.ssh/id_rsa` (your private key)
   - `~/.ssh/id_rsa.pub` (your public key)

2. **Copy the Public Key to the Server**:
   You can copy your public key to the remote server using `ssh-copy-id`:
   ```bash
   ssh-copy-id user@remote_host
   ```

3. **Login with SSH Key**:
   Now, you can log in without a password:
   ```bash
   ssh user@remote_host
   ```

---

### Copying Files via SSH (Using SCP)

The **scp** (secure copy) command lets you transfer files between your local machine and the remote server.

- **Copy a file from the server to your local machine**:
   ```bash
   scp user@remote_host:/path/to/remote/file /path/to/local/directory
   ```

- **Copy a file from your local machine to the server**:
   ```bash
   scp /path/to/local/file user@remote_host:/path/to/remote/directory
   ```

---

### Advanced SSH Configurations

#### **Using an SSH Config File**

You can simplify SSH connections by using an SSH configuration file (`~/.ssh/config`). Here’s an example configuration:
```bash
Host myserver
    HostName 192.168.1.100
    User user
    Port 22
```
Now, you can connect with just:
```bash
ssh myserver
```

#### **Change SSH Port for Security**

To reduce unauthorized access attempts, you can change the default SSH port from 22 to another port (e.g., 4444). Edit `/etc/ssh/sshd_config`:
```bash
sudo nano /etc/ssh/sshd_config
```
Change the port line:
```bash
Port 4444
```
Restart the SSH service:
```bash
sudo systemctl restart ssh
```
Then, connect with:
```bash
ssh -p 4444 user@remote_host
```

#### **Disable Password Authentication**

Once you’ve set up SSH keys, you can disable password authentication for increased security. Edit `/etc/ssh/sshd_config`:
```bash
PasswordAuthentication no
```
Then restart SSH:
```bash
sudo systemctl restart ssh
```

---

### Port Forwarding with SSH

SSH allows port forwarding, which is useful for tunneling traffic.

- **Local Port Forwarding**: Forward traffic from your local machine to a remote server.
  ```bash
  ssh -L local_port:remote_host:remote_port user@remote_host
  ```
  Example:
  ```bash
  ssh -L 8080:localhost:80 user@remote_host
  ```
  This forwards port 80 on the remote host to port 8080 on your local machine.

- **Remote Port Forwarding**: Forward a port from the remote machine to your local machine.
  ```bash
  ssh -R remote_port:localhost:local_port user@remote_host
  ```
  Example:
  ```bash
  ssh -R 8080:localhost:80 user@remote_host
  ```

---

### Using SSH Tunnels for Secure Traffic

**Dynamic Port Forwarding** enables the creation of a SOCKS proxy, allowing you to securely route traffic through an SSH connection.

- Establish a dynamic tunnel:
  ```bash
  ssh -D local_port user@remote_host
  ```
  Example:
  ```bash
  ssh -D 8080 user@remote_host
  ```

Now, you can configure your web browser to use `localhost:8080` as a SOCKS proxy, encrypting all traffic through the SSH tunnel.

---

### Multiplexing SSH Sessions

To reduce the overhead of establishing new SSH sessions, use SSH multiplexing, which allows multiple SSH sessions to share a single connection.

Add this to your SSH config file (`~/.ssh/config`):
```bash
Host *
    ControlMaster auto
    ControlPath ~/.ssh/control_%r@%h:%p
    ControlPersist 10m
```

This creates a control socket for SSH connections, speeding up subsequent connections.

---

### SSH Escape Codes and Controlling Connections

SSH escape codes allow control over your connection:

- **Disconnect** from a frozen session: Press `ENTER`, then type `~.`.
- **Put an SSH session into the background**: Press `ENTER`, then type `~[CTRL-z]`.

---
