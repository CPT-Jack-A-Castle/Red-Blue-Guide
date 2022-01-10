**>>** [https://nmap.org/ncat/guide/index.html](https://nmap.org/ncat/guide/index.html)

## Advantages/Disadvantages
- (+) TCP/IP Swiss Army Knife
- (+) Receives packages until connection gets terminated (no EOF-Input)
- (+) Client and server Functionality
- (-) unencrypted (use alternativly Cryptcat or SSH-tunnel)
- (-) deprecated ( >20 years)

## Fundamentals
- `nc [options] [host] [port]`: Execute a port scan
- `nc -l [host] [port]`: Initiate a listener on the given port
- `nc -4`:  Use IPv4 only
- `nc -6`: Use IPv6
- `nc -u`: Use UDP instead of TCP
- `nc -k -l`: Continue listening after disconnection
- `nc -n`: Skip DNS lookups / numerical
- `nc -v`: verbose output

## How To

**Create Channel**
- Kali: `nc -lvnp [Port]` (Check your listening status in another terminal by using command "netstat")
- Windows: `nc [IPv4] [Port]` ; `get`

**Transfer text** (*while connection is already established*)
- Windows: `nc -lvnp [port] > example.txt`
- Kali: enter your text

**Transfer files** (*while connection is already established*)
- Windows: `nc -lnvp [port] > wget.exe`
- Kali: `nc [IPv4] [port] < /usr/share/windows-binaries/wget.exe`

**Portscan**
- `nc -v -w 2 -z [IPv4] [port range]`
  - Exp: `nc -v -w 2 -z 3.45.21.19 10-1000`
  - w = set timeout in [s]
  - z = no transmission, only check listening services 

**Communicate with Web Server**
- `nc [host] [port]`
  - `GET / HTTP/1.0`
  - Press 2x Enter 