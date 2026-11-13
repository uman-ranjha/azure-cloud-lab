# Azure Cloud Lab

I built this lab to get hands-on cloud experience. I wanted a real virtual machine in the cloud, not just a local one, and I wanted to run one of my own security tools on it.

## What I built

One Ubuntu virtual machine in Microsoft Azure, on an Azure for Students subscription.

- VM name: loglab-vm
- Size: Standard_B1s, 1 vCPU, 1 GiB RAM
- Image: Ubuntu Server 24.04 LTS
- Region: Denmark East
- Resource group: cloud-lab-rg
- Login: SSH public key only, username azureuser
- Open port: SSH (22) only
- OS disk: Standard SSD, 30 GiB

## What I ran on it

My SSH log analyzer, from the cyber-log-analyzer repo. I cloned it onto the VM and ran it against the sample SSH auth log that ships with it.

```bash
sudo apt update && sudo apt install -y git
git clone https://github.com/uman-ranjha/cyber-log-analyzer.git
cd cyber-log-analyzer
python3 log_analyzer.py --log samples/sample_auth.log
```

## What it found

The run on the cloud VM gave the same result as the run on my own PC.

```
=== Log analysis summary ===
Events parsed : 36 (5 accepted, 31 failed/invalid)
Unique IPs    : 9
Findings      : 6

[CRITICAL] Successful login from 203.0.113.7 after 12 failures, possible compromise  (T1078)
[HIGH]     Possible brute-force attack from 203.0.113.7  (T1110)
[HIGH]     Possible brute-force attack from 203.0.113.44  (T1110)
[HIGH]     Possible password spraying against user 'jdoe'  (T1110.003)
[MEDIUM]   Username enumeration from 203.0.113.44  (T1590)
[MEDIUM]   Off-hours login: 'uman' from 198.51.100.99 at 03:12  (T1078)
```
