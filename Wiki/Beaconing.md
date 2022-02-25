## BEACONING
## Info
- Network node that advertise its presence
- Can be legitimate (WAP, NTP, Auto update)
- Can be malicious (C2-communication, Botnet)

### Process
1. Malicious beacon uses ping/heartbeat to verfiy it is active
2. Attackers use DGA (Domain Generation Algorithm), Fast Flux DNS or Jitter
3. After successful Beaconing, C2 server send commands

### Command Channels for Beaconing
1. IRC: Declining trend because many organizations block it
2. HTTP/HTTPS: Mitigation via intercepting Proxy (MITM)
3. DNS
    - Effective C2 channel because no outside network needed
    - Instead it uses local DNS resolver
    - IOC1: Same query is repeated when bot checks into C2 server
    - IOC2: Commands within response query become longer
4. Social Media
    - Can allow "live off the land" (!)
    - Commands through Twitter / LinkedIn etc. (!)
5. Cloud: Botnet can use Google's App Engine to send C2 messages
6. Media/Document Files: Metadata within media files can contain C2 messages