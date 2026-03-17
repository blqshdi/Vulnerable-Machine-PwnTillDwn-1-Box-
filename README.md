# PwnTillDawn Lab Write-up

## Target
- **IP:** 10.150.150.11
- **OS:** Windows (XAMPP)
- **Difficulty:** Beginner/Intermediate

## Tools Used
- **Nmap** – Port scanning, service enumeration
- **Gobuster** – Directory enumeration
- **Browser / Dashboard** – Manual exploration
- **PHP Shell** – Remote Code Execution (RCE)
- **Terminal / Nano** – File creation

---

## Steps Taken

### 1. Recon
- Ran Nmap:
  '''nmap -sV -sC 10.150.150.11'''
