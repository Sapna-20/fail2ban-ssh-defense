
# 🛡️ SSH Brute-Force Defense using Fail2Ban

This project demonstrates how to protect an Ubuntu server from SSH brute-force attacks (e.g. using Hydra) by using **Fail2Ban**.

---

## 🚀 Setup Summary

- **Target VM** IP: `192.168.92.11`
- **Attacker VM** IP: `192.168.92.10` (Kali)
- **SSH Port**: `22`
- **Tool used for attack**: Hydra
- **Tool used for defense**: Fail2Ban

---

## ⚙️ Fail2Ban Installation

```bash
sudo apt update && sudo apt install fail2ban -y
```

---

## 🛠️ Configuration

Edit the configuration:

```bash
sudo nano /etc/fail2ban/jail.local
```

Make sure the `[sshd]` section looks like this:

```ini
[sshd]
enabled = true
port    = ssh
filter  = sshd
logpath = /var/log/auth.log
maxretry = 3
findtime = 300
bantime = 600
```

Save and exit the file.

---

## ▶️ Start and Check Fail2Ban

```bash
sudo systemctl restart fail2ban
sudo fail2ban-client status sshd
```

---

## 🧪 Test the Defense

From Kali Linux, run:

```bash
hydra -l sapna -P passwords.txt ssh://192.168.92.11
```

After 3 failed attempts, Fail2Ban will block the attacker’s IP:
```bash
sudo fail2ban-client status sshd
```

If a legitimate user or yourself gets blocked by mistake, you can remove the ban using:

```bash
sudo fail2ban-client set sshd unbanip <IP_ADDRESS>
```
To unban:

```bash
sudo fail2ban-client set sshd unbanip 192.168.92.10
```
To check the current status:

```bash
sudo fail2ban-client status sshd
```

Look at the "Banned IP list" section to confirm the IP has been removed.

---

## ✅ Result

The attacker (Hydra) gets blocked after repeated SSH login failures.

---

## 📁 Files

- `jail.local` – example config
- `README.md` – step-by-step guide
