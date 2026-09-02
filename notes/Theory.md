## Nmap
### Scan techniques

```bash
wehtikal@htb[/htb]$ nmap --help 
<SNIP> 
SCAN TECHNIQUES: 
-sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans 
-sU: UDP Scan 
-sN/sF/sX: TCP Null, FIN, and Xmas scans 
--scanflags <flags>: Customize TCP scan flags 
-sI <zombie host[:probeport]>: Idle scan 
-sY/sZ: SCTP INIT/COOKIE-ECHO scans 
-sO: IP protocol scan -b <FTP relay host>: FTP bounce scan <SNIP>
```

Default scan is TCP-SYN scan `-sS`
The scan send one packet with SYN flag:
- If our target sends a `SYN-ACK` flagged packet back to us, Nmap detects that the port is `open`.
- If the target responds with an `RST` flagged packet, it is an indicator that the port is `closed`.
- If Nmap does not receive a packet back, it will display it as `filtered`. Depending on the firewall configuration, certain packets may be dropped or ignored by the firewall.

### Host discovery
To check systems that are online, using **ICMP echo requests**

#### Range

```bash
wehtikal@htb[/htb]$ sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5

10.129.2.4
10.129.2.10
10.129.2.11
10.129.2.18
10.129.2.19
10.129.2.20
10.129.2.28
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.0/24`|Target network range.|
|`-sn`|Disables port scanning.|
|`-oA tnet`|Stores the results in all formats starting with the name 'tnet'.|

ONLY IF the firewalls of the hosts allow it.

#### List
```bash
wehtikal@htb[/htb]$ cat hosts.lst

10.129.2.4
10.129.2.10
10.129.2.11
10.129.2.18
10.129.2.19
10.129.2.20
10.129.2.28
```

If we use the same scanning technique on the predefined list, the command will look like this:

```bash
wehtikal@htb[/htb]$ sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5  10.129.2.18 10.129.2.19 10.129.2.20
```

|**Scanning Options**|**Description**|
|---|---|
|`-sn`|Disables port scanning.|
|`-oA tnet`|Stores the results in all formats starting with the name 'tnet'.|
|`-iL`|Performs defined scans against targets in provided 'hosts.lst' list.|

#### Specified IP addresses.
```bash
wehtikal@htb[/htb]$ sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20| grep for | cut -d" " -f5  10.129.2.18 10.129.2.19 10.129.2.20
```

If these IP addresses are next to each other, we can also define the range in the respective octet.

```bash
wehtikal@htb[/htb]$ sudo nmap -sn -oA tnet 10.129.2.18-20| grep for | cut -d" " -f5  10.129.2.18 10.129.2.19 10.129.2.20
```

| **Scanning Options** | **Description**                                                                   |
| -------------------- | --------------------------------------------------------------------------------- |
| `10.129.2.18`        | Performs defined scans against the target.                                        |
| `-sn`                | Disables port scanning.                                                           |
| `-oA host`           | Stores the results in all formats starting with the name 'host'.                  |
| `-PE`                | Performs the ping scan by using 'ICMP Echo requests' against the target.          |
| `--reason`           | Displays the reason for specific result.                                          |
| `--disable-arp-ping` | To disable ARP requests and scan our target with the desired `ICMP echo requests` |
