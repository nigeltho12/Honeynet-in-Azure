# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Overview

What happens when you leave the front door of your digital house wide open? You get a flood of uninvited guests! 🚪🌐  
In this project, I set up a **honeynet** in Microsoft Azure to study real-world attack traffic and evaluate security measures. Logs from various sources were funneled into a **Log Analytics workspace**, where **Microsoft Sentinel** did its thing—creating attack maps, triggering alerts, and managing incidents.

Here’s the game plan:
1. Measure attack metrics in an environment with all the doors and windows wide open (figuratively speaking).
2. Apply some serious security measures to lock it all down. 🛡️
3. Measure again to see if the “bad guys” got the memo.

Key metrics I analyzed include:
- **SecurityEvent** (Windows Event Logs)
- **Syslog** (Linux Event Logs)
- **SecurityAlert** (Triggered Alerts)
- **SecurityIncident** (Sentinel-Generated Incidents)
- **AzureNetworkAnalytics_CL** (Malicious Flows sneaking in before being evicted)

---

## Infrastructure Before Applying Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

Think of this setup as leaving your car in a sketchy parking lot with the doors unlocked and keys in the ignition. Sure, you’ll attract some attention—but not the good kind.

---

## Infrastructure After Applying Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

Now, imagine your car is in a garage with steel doors, an alarm system, and a guard dog named Sentinel. That’s the vibe here.

### Honeynet Architecture

The honeynet in Azure features:
- **Virtual Network (VNet)**
- **Network Security Group (NSG)**
- **Virtual Machines** (2 Windows, 1 Linux)
- **Log Analytics Workspace**
- **Azure Key Vault**
- **Azure Storage Account**
- **Microsoft Sentinel**

**Before hardening**:
- Everything was open to the internet. NSGs and firewalls were basically saying, "Come on in!"
- Public endpoints everywhere—imagine having a party with no guest list.

**After hardening**:
- NSGs became the bouncers, letting in only the admin workstation.
- Firewalls tightened up, and Private Endpoints said, "No trespassing."

---

## Attack Maps Before Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/dDyjCCj.png)  
![Linux Syslog Auth Failures](https://i.imgur.com/DNmVufd.png)  
![Windows RDP/SMB Auth Failures](https://i.imgur.com/bGaooRS.png)

As expected, there were plenty of uninvited guests trying to crash the party.

---

## Metrics: Pre-Hardening

Here’s what happened in the first 24 hours of running the environment wide open:

| Metric                   | Count  |
|---------------------------|--------|
| SecurityEvent            | 34,906 |
| Syslog                   | 14,646  |
| SecurityAlert            | 14,489     |
| SecurityIncident         | 3,490    |
| AzureNetworkAnalytics_CL | 0|

It’s like a Black Friday sale—too much traffic, not enough security.

---

## Attack Maps After Security Controls


---

## Metrics: Post-Hardening

After locking things down, here’s what the next 24 hours looked like:

| Metric                   | Count  |
|---------------------------|--------|
| SecurityEvent            | 443  |
| Syslog                   | 0     |
| SecurityAlert            | 0      |
| SecurityIncident         | 0      |
| AzureNetworkAnalytics_CL | 0      |

What a difference a little hardening makes!

---

## Conclusion

This project was a fun dive into building a honeynet in Azure and exploring the difference between an open environment and a secured one. With logs integrated into a Log Analytics workspace and monitored by Microsoft Sentinel, it was like turning chaos into order.

The metrics speak for themselves: fewer events, alerts, and incidents after security controls were applied. (Take that, cyber baddies! 💪)

And while this was a controlled setup, a production environment with active users might generate more events—but at least we’ve learned how to tame the noise.
