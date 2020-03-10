# Server Management

(Work in progress) Setting up and managing a VPS or bare metal server.

**TODO**:
- SSH authentication
- Node.js, Docker setup, etc

## Getting Started

For this guide, we'll be using a Debian 10 server as a starting point.

### Step 1 – SSHing in to your server

First, we need to know our server's **public IP address** (in the form of `n`.`n`.`n`.`n`). Once we know this, we need to login to the **root** user.

**root** is a dangerous user, as it has super privileges by default, and should only be used once for provisioning a machine. After this, we will **disable root** login.

```bash
ssh root@server_ip_address
# e.g. ssh root@192.0.2.255
```

You can now set some environment variables to use in the next steps: (needs to be done fresh on each Terminal window/tab/session).

```bash
MY_USERNAME=john;

```

### Step 2 – Creating Our User Account

```bash
adduser $MY_USERNAME
# e.g. adduser john
```

You will be prompted to enter a secure password. Please use a password manager for this and use at least 20-30 characters+, as this can be bruteforced. The other steps can be skipped by pressing `enter`.

### Step 3 – Setting up `sudo` (Administrator Privileges)

```bash
usermod -aG sudo $MY_USERNAME
# e.g. usermod -aG sudo john
```

### Step 3 – Setting up a Firewall

A firewall should be setup to secure our server and whitelist which ports we want to expose to the internet.

This is especially important for services such as SQL servers or Redis, where weak authentication can result in the server being rooted by attackers.

```sh
apt update
apt install ufw
```

Allow SSH (to make sure we can reconnect). If we miss this step, you'll have to recover access to the server through the server management interface (boot into a recovery OS).

```sh
ufw allow OpenSSH
```

Enable `ufw` firewall

```sh
ufw enable
```

Check the status of the firewall with `ufw status`
