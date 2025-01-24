
# TCPDump Examples for Monitoring IPFS Behavior

Below are some useful `tcpdump` commands to analyze IPFS network traffic, including swarm communication, API requests, and HTTP gateway traffic.

---

## 1. Capture All Traffic on the Swarm Port (Default: 4001)
IPFS uses the **swarm port** to establish peer-to-peer connections.

```bash
sudo tcpdump -i eth1 port 4001
```

### Explanation:
- `-i eth1`: Listen on the `eth1` interface (replace with the correct interface, e.g., `ens33`).
- `port 4001`: Filter traffic on the default IPFS swarm port.

### What You'll See:
- Traffic exchanged with other IPFS nodes over the swarm protocol.

---

## 2. Capture Traffic to the IPFS API Port (Default: 5001)
The IPFS API runs on port 5001 by default. Use this to inspect API calls to the node.

```bash
sudo tcpdump -i eth1 port 5001
```

### What You'll See:
- Requests from applications (like `ipfs-cli` or other tools) interacting with the node.

---

## 3. Capture HTTP Gateway Traffic (Default: 8080)
The IPFS HTTP Gateway allows accessing files via HTTP.

```bash
sudo tcpdump -i eth1 port 8080
```

### What You'll See:
- HTTP requests and responses for files served via the IPFS gateway.

---

## 4. Capture All Traffic to a Specific Peer
If you know a peer's IP address (e.g., `192.168.33.20`), you can capture traffic specific to it.

```bash
sudo tcpdump -i eth1 host 192.168.33.20
```

### What You'll See:
- All traffic (incoming and outgoing) between your node and the specified peer.

---

## 5. Capture and Save IPFS Traffic to a File
Save the traffic to a file for later analysis with Wireshark or other tools.

```bash
sudo tcpdump -i eth1 port 4001 -w ipfs_swarm_traffic.pcap
```

### Explanation:
- `-w ipfs_swarm_traffic.pcap`: Writes the captured data to a file named `ipfs_swarm_traffic.pcap`.

You can later analyze the file in Wireshark:

```bash
wireshark ipfs_swarm_traffic.pcap
```

---

## 6. Filter Specific Protocols (e.g., TCP Only)
Capture only TCP traffic on the swarm port.

```bash
sudo tcpdump -i eth1 tcp port 4001
```

### What You'll See:
- Only TCP-based communications on the swarm port.

---

## 7. Inspect Packet Contents
Display the full packet contents (useful for debugging low-level issues).

```bash
sudo tcpdump -i eth1 port 4001 -X
```

### What You'll See:
- Detailed hex and ASCII representation of the packets.

---

## 8. Monitor Bandwidth Usage (Quick Overview)
To quickly see how much IPFS traffic is happening on the swarm port:

```bash
sudo tcpdump -i eth1 port 4001 | pv -br
```

### What You'll See:
- Real-time data rates of the captured traffic.

---

## 9. Capture Traffic from a Specific Protocol/Port Pair
If you want to monitor **DNS queries** used by IPFS during peer discovery:

```bash
sudo tcpdump -i eth1 udp port 53
```

---

## 10. Advanced: Filter by IPFS Data Blocks (CID-Related Traffic)
Capture packets with specific content hashes or CID-related data (though this typically requires additional tools like Wireshark filters or IPFS logs combined with `tcpdump`).

---

## Notes:
- Replace `eth1` with your VM’s active network interface. To find it, run:
  ```bash
  ip link show
  ```
- Run `tcpdump` with `sudo` since it requires elevated permissions.

By combining these `tcpdump` commands, you can gain valuable insights into IPFS network behavior.
