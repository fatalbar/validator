## 1.0 Official Document Mundis
* [Discord](https://discord.gg/xYweyG7dzK)
* [Blog](https://docs.mundis.io/)
* [Twitter](https://twitter.com/MundisNetwork)
* [Github](https://github.com/mundisnetwork)

## 1.1 System Requirement
* CPU
8 cores / 16 threads, or more
2.6GHz, or faster
AVX2 instruction support (to use official release binaries, self-compile otherwise)
Support for AVX512f and/or SHA-NI instructions is helpful
The AMD Threadripper family is popular between validators
* RAM
16GB, or more
* Disks
Accounts & Ledger disk: 500GB stored on PCIe Gen3 x4 NVME SSD, or better
Operating System: 200GB
* The OS may be installed on the Accounts & Ledger disk, though testing has shown better performance with the ledger on its own disk
* GPU
Not necessary at this time

## _BEFORE YOU RUNNING YOUR NODE MAKE SURE YOU ALREADY REGISTER YOUR VALIDATOR ON JUNE OTHERWISE YOU DIDNT REWARDED, YOU CAN CHECK FROM OFFICIAL DOCUMENT FROM [MUNDIS](https://docs.mundis.io/rattle-shake/register)_

## RECOMMEND PROGRAM
Please Download MobaXterm to easy Managing your Directory DATA on VPS
You can Download MobaXterm on [Official Website](https://mobaxterm.mobatek.net/download.html)
if you using Android you can Download Application with support SFTP on [Playstore](https://play.google.com/store/apps/details?id=com.server.auditor.ssh.client&hl=in&gl=US) 

## 1.3 Download requirement file

Upgrade & install curl
```bash
sudo apt update && sudo apt install curl -y
```
Install Binary
```bash
curl -O https://release.mundis.io/v0.9.27/mundis_0.9.27-2_amd64.deb
sudo dpkg -i mundis_0.9.27-2_amd64.deb
```
Copy your `validator-keypair.json` identity keypair file generated during Registration to the `/var/lib/mundis/validator-identity.json` location.

If you already Backup your old SEED Pharse you can Restore by using this Command
```bash
mundis-keygen recover
```
Configure DEVNET endpoint and default identity:
```bash
mundis config set --url http://api.devnet.mundis.io
mundis config set -k /var/lib/mundis/validator-identity.json
```

Airdrop some coins and create your vote account:
```bash
mundis airdrop 5
mundis create-vote-account /var/lib/mundis/validator-vote-account.json /var/lib/mundis/validator-identity.json /var/lib/mundis/validator-identity.json --allow-unsafe-authorized-withdrawer
```
(Optional) If you have a dedicated disk for Accounts & Ledger files, you need to mount it under `/mnt`. The validator node uses the `/mnt/ledger` and `/mnt/accounts` folders to store data.

Start your node with:
```bash
sudo systemctl enable --now mundis-validator.service
```
Watch the log file for metrics submission:
```bash
sudo cat /var/log/mundis/validator.log | grep "mundis_metrics::metrics"
```
A normal output should start with something like this:
```bash
[2022-07-15T07:12:13.906368322Z INFO  mundis_metrics::metrics] metrics configuration: host=http://metrics.devnet.mundis.io:8086 db=devnet username=admin
[2022-07-15T07:12:13.910107077Z INFO  mundis_metrics::metrics] host id: AL49z2TdM7vnSR7yiJU1n9VbsLgJ8TRQuUqrNYqj4V4
[2022-07-15T07:12:23.932712091Z INFO  mundis_metrics::metrics] submitting 1198 points
[2022-07-15T07:12:33.936277831Z INFO  mundis_metrics::metrics] submitting 1553 points
[2022-07-15T07:12:43.958806809Z INFO  mundis_metrics::metrics] submitting 1536 points
[2022-07-15T07:12:53.977352171Z INFO  mundis_metrics::metrics] submitting 1563 points
[2022-07-15T07:13:03.994330118Z INFO  mundis_metrics::metrics] submitting 1456 points
[2022-07-15T07:13:14.007761702Z INFO  mundis_metrics::metrics] submitting 1441 points
```

Access the [DEVNET Metrics dashboard](http://metrics.devnet.mundis.io:3000/d/local/devnet-cluster-monitor?orgId=1&refresh=30s&var-datasource=default&var-testnet=devnet&var-hostid=All) and look for your public key as shown in the following image. If you don't find it, read the [Troubleshooting](https://docs.mundis.io/rattle-shake/validator.html#Troubleshooting) section.

![metrics-dashboard-c83c7a58ba94021d0666ef490fa0058d](https://user-images.githubusercontent.com/81378817/179369060-67a7014e-da82-474a-ad11-7fbcdf83eaba.png)

## 1.4 Troubleshooting
The `/var/log/mundis/validator.log` file is empty or does not exist.

Most probably your validator node failed to start. Look in the syslog to find the startup problem:

```bash
journalctl -e -n10000 | grep start-mundis-validator
```

Go to [Discord](https://discord.gg/xYweyG7dzK) under the RATTLE&SHAKE THE DEVNET / `#validator-chat` channel if you can't figure out the problem. Dev or friend will help.

Thank you