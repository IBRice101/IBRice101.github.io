# Censis IoT Hackathon notes

From the 2nd to the 28th of June 2021 I took part in an IoT device hackathon provided by CENSIS in collaboration with Abertay Uni. The following are my notes from the week or so that I had to do the project, I hope you enjoy reading this as much as I did doing it :).

## Checklist

- [x] Gain remote access to the system
  - cracked Wi-Fi password to access the network
- [x] Gain command line access
  - cracked SSH password for default user to gain root shell
- [x] Set clock to correct time
  - Turned off auto time synchronisation and set the time manually
- [x] Access services through web browser
  - once connected to the network, connected to the http web servers using the ports on which there was a server being hosted, found 3 sites, one of which is an admin page
- [ ] Crack password for site and login to LoRaWAN admin page
- [ ] Find out how the network server converts LoRaWAN messages to JSON
- [ ] Find out how the messages are being routed and perform a MITM attack
- [x] Obtain source code running on the sensors
  - found `developer` account with directory `monitoring`, which contained code for the site
- [ ] Infer info about the office outwith the scope of the project
- [x] Exploit default credentials
  - `pi` was the username for the raspberry pi, which is the default username

## Begin

- Discovered Wi-Fi signal
  - SSID: IoT-Demo-06
  - Security: WPA2-PSK
  - MAC Address (BSSID): B8:27:EB:B8:BB:F1

## Capturing Handshake

- ran `iwconfig` and found adapter name `wlp0s20f0u3` (using USB Wi-Fi adapter)
- `sudo airmon-ng check kill; sudo airmon-ng start wlp0s20f0u3` which killed processes, disconnected me, then started the interface `wlp0s20f0u3mon` in monitor mode
- ran `sudo airodump-sg wlp2s0mon` to capture BSSID for IoT-Demo-06 (B8:27:EB:B8:BB:F1)
- ran the command `sudo airodump-ng -c 7 --bssid B8:27:EB:B8:BB:F1 -w WPAcrack wlp0s20f0u3mon --ignore-negative-one`
  - `-c` determines the channel for the wireless network
  - `--bssid` is the MAC address for the access point
  - `-w` is the file name prefix for the file which will contain the authorisation handshake
  - `wlp0s20f0u3mon` is the wireless interface
  - `--ignore-negative-one` fixes an error message
- captured handshake `WPA2crack-01.cap`

## Cracking

- ran command `aircrack-ng -w words_small.txt -b B8:27:EB:B8:BB:F1 WPA2crack/WPA2crack-01.cap`
  - `aircrack-ng` cracks passwords
  - `-w` specifies wordlist, used `words_small.txt` which was provided to me
  - `-b` specifies BSSID as captured earlier
  - `WPA2crack/WPA2crack-01.cap` was the handshake capture fed into the cracking tool
- returned the following:

```txt
                               Aircrack-ng 1.6 

      [00:00:01] 3261/3311 keys tested (3062.98 k/s) 

      Time left: 0 seconds                                      98.49%

                           KEY FOUND! [ valegorov ]


      Master Key     : 4C 99 CF BF 5B 80 C9 B8 F1 8F 71 4B 43 98 AB B1 
                       78 6A 70 E2 A6 08 B6 99 E7 E8 A7 CF B5 AF DC 03 

      Transient Key  : 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 
                       00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 
                       00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 
                       00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 

      EAPOL HMAC     : F1 61 45 F2 AD 27 E4 2E 99 88 CF 25 F0 A5 97 57
```

- password: valegorov (tested w/ positive result)

## Scanning

### Wireshark

- Turned on Wireshark Monitoring for 5 minutes on the network to see what I could find
  - saved to `kit5min.pcapng`
- Found 2 devices at `192.168.66.1` and `192.168.66.146`
- packet 41 tells us that the sender (`192.168.66.1`) is the Raspberry Pi device, the other device is my laptop

### Nmap

- Basic scan of pi shows 3 open ports
  - 22, SSH
  - 53, domain
  - 8080, http-proxy
- a more in-depth scan of TCP ports exclusively (`nmap -sT -p- -vv -T5 192.168.66.1`) showed an additional 2 known ports in use
  - 1883, mqtt
  - 4369, epmd
  - Also showed 5 additional unknown ports open
- finally ran `nmap -sT -p- -vv -A 192.168.66.1` to gather OS information, versions, scripts, and traceroute for the aforementioned ports
  - OS: Raspbian
  - Port 22:
    - OpenSSH 7.9p1 Raspbian 10+deb10u2+rpt1 (protocol 2.0)
  - Port 53:
    - domain service
    - dnsmasq 2.0
  - Port 1883:
    - mosquitto version 1.5.7
  - Port 4369:
    - epmd
    - Erlang Port Mapper Daemon
    - LoRaWAN node 41199
  - Port 7245:
    - http
    - Werkzeug httpd 0.14.1 (Python 2.7.16)
    - Temperature web server
  - Port 8080
    - Cowboy httpd
    - LoRaWAN server
    - Cowboy server header
  - port 8245:
    - Werkzeug httpd 0.14.1 (Python 2.7.16)
    - PIR web server

## Cracking SSH

- Determined the IP address of the target
- Presumed default credentials
- ran `hydra -l pi -P words_small.txt ssh://192.168.66.1`
  - pi is the default username on Raspbian
  - presuming the password is in the aforementioned `words_small.txt` wordlist
  - valid credentials found
    - host: 192.168.66.1
    - login: pi
    - password: sourcetec
- found another user, `developer`, repeated process
  - not necessarily required because pi is a root user, makes GUI file browsing/exfiltration easier though
  - no valid password discovered

## Enumeration

### Opening Enumeration

- ran `curl "https://github.com/diego-treitos/linux-smart-enumeration/raw/master/lse.sh" -Lo lse.sh;chmod 700 lse.sh` to get an enumeration tool called lse
- Intent of using lse is to get a basic overview for further analysis
- ran `scp -r ~/Downloads/lse pi@192.168.66.1:~/Downloads/lse` to get the lse script to the remote device
- ran the script and outputted the contents to a .txt file which I SCP'd back to the host device for analysis

### Dirbuster

- Dirbuster is a GUI directory enumeration tool with dirb as the backend
- Threw it at he three ports where there was an obvious web server:
- Port 7245
  - only root directory
  - upon inspection, serves as a frontend for the temperature gauge
  - refreshes every 10 seconds or so
  - HTML below:

```html
<!DOCTYPE html>
<head>
   <title>Temperature Web Server</title>
   <!-- Latest compiled and minified CSS -->
   <link rel="stylesheet" href="/static/styles/bootstrap.min.css" crossorigin="anonymous">
   <!-- Optional theme -->
   <link rel="stylesheet" href="/static/styles/bootstrap-theme.min.css" crossorigin="anonymous">
</head>

<meta http-equiv="refresh" content="15">

<body>
    <h1>Temperature Web Server</h1>
    <br />
    
    <h2>Device (DEI)</h2>
    <form method="POST">
    <select id="SelectedDevice" name="SelectedDevice">
        
          <option value="70B3D5499C1AA8FB" selected="selected">70B3D5499C1AA8FB</option>
        
    </select>
    <input type="submit" value="Select Device">
    </form>
    <br />
    <h2>Heating currently: OFF</h2>
    <h2>Room Currently: 29.0 &#8451</h2>
    <h2>Pressure: 1018.8mb </h2>
    <h2>Humidity: 36.5% </h2>
    <h2>Battery: 4.802V</h2>
    <h2>DevEUI: 70B3D5499C1AA8FB</h2>
    <h2>Name: Test LoPy4</h2>
</body>

</html>
```

- Port 8080
  - Admin page, inaccessible on the surface web without credentials
  - Dirbuster shows the pages `/admin/` and `/admin/timeline/`
- Port 8245
  - Similar to 7245
  - Is also a temperature sensor, albeit with slightly less information
  - Also refreshes once every few seconds
  - HTML below:

```html
<!DOCTYPE html>
<head>
   <title>PIR Web Server</title>
   <!-- Latest compiled and minified CSS -->
   <link rel="stylesheet" href="/static/styles/bootstrap.min.css" crossorigin="anonymous">
   <!-- Optional theme -->
   <link rel="stylesheet" href="/static/styles/bootstrap-theme.min.css" crossorigin="anonymous">
</head>

<meta http-equiv="refresh" content="15">

<body>
    <h1>PIR Web Server</h1>
    <br />
    
    <h2>Device (DEI)</h2>
    <form method="POST">
    <select id="SelectedDevice" name="SelectedDevice">
        
          <option value="E24F43FFFE44CD2A">E24F43FFFE44CD2A</option>
        
    </select>
    <input type="submit" value="Select Device">
    </form>
    <br />
    <h2>PIR Count: 0 activations </h2>
    <h2>Room Currently:  &#8451</h2>
    <h2>Battery:  %</h2>
    <h2>DevEUI: </h2>
  <h2>Name: </h2>
    
</body>

</html>
```

### Nikto

- Nikto is a tool to scan web servers for known vulnerabilities
- Pointed it at port 8080 to check for possible methods of entry for the admin page
- Vulnerabilities found:
  - The anti-clickjacking X-Frame-Options header is not present.
  - The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
  - The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type

### Simple Search and Exfiltration

- ran `tree -a` command in the root directory to get an overview of all files on the device, outputted to text file for review.
  - Found LoRaWAN server in tree, hosted using cowboy (erlang OTP web server)
- collected interesting files into a directory called `Exfil`
  - cache and config files for chromium from `pi` user, as well as other misc files
  - directory called monitoring in `developer`
    - contains an interesting `readme` which links to `/etc/systemd/system`, files exfiltrated
    - Directory also contains source code for the site

## Misc

- **NOTE**, exploit stage was not practical to reach in the short time available, restructure report given the work done

## Thank you for reading!

If you learned something from this, why not send me over a little tip by way of thanks? No pressure but it would be much appreciated :)

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/A0A1D0FSN)