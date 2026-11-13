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

## The part that took the longest

Picking a region and a size. Azure for Students applies a policy called "Allowed resource deployment regions" that limits each subscription to a small list of regions, and the list is different for every student. Mine is Canada Central, Denmark East, Belgium Central, Germany West Central, and Sweden Central. There is no US region on it.

The smallest size, Standard_B1s, was not available in East US, Canada Central, or Germany West Central for this subscription. It worked in Denmark East, so that is where the VM lives.

If you are on a student subscription and a size shows as not available, check your own allowed list first. In the Azure portal, search for Policy, open Assignments, open "Allowed resource deployment regions", and read the allowed locations under Parameters. Then try the sizes in one of those regions.

## Stopping the VM

When I am done using it, I stop the VM in the Azure portal. Stopping it deallocates it and stops the compute charges. The VM can be started again the same way next time.
