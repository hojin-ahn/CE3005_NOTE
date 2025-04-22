# Computer Networks Core Concepts and Problem Explanations

## 1. OSI 7-Layer Model

### Layer Characteristics
1. **Physical Layer**
   - Converts bit streams into electrical signals
   - Equipment includes cables, repeaters, hubs
   - Responsible for bandwidth, voltage levels, physical connections

2. **Data Link Layer**
   - MAC address-based communication
   - Processes data in frame units
   - Error detection and flow control (흐름 제어)
   - Devices like switches and bridges operate at this layer
   - Formula: Maximum window size = 2^(sequence bit count)-1

3. **Network Layer**
   - IP address-based routing
   - Processes data in packet units
   - Routers operate at this layer
   - Handles subnetting and routing protocols

4. **Transport Layer**
   - Ensures end-to-end (종단 간) communication
   - TCP: Reliable connection-oriented protocol
   - UDP: Connectionless protocol
   - Uses port numbers to distinguish applications
   - Manages flow control, error recovery, and congestion control (혼잡 제어)

5. **Session Layer**
   - Establishes, maintains, and terminates communication sessions
   - Manages full-duplex and half-duplex (반이중) communication
   - Controls checkpoints (동기화 지점)

6. **Presentation Layer**
   - Transforms data formats
   - Handles encryption/decryption and data compression
   - Converts character encodings (ASCII, EBCDIC, etc.)

7. **Application Layer**
   - Provides user interfaces
   - Protocols include HTTP, FTP, SMTP, DNS
   - Processes user data

### Comparison with TCP/IP Model
- **Application Layer**: Combines OSI layers 5-7
- **Transport Layer**: Same as OSI layer 4
- **Internet Layer**: Same as OSI layer 3
- **Network Interface Layer**: Combines OSI layers 1-2

## 2. Data Transmission and Window Size

### Key Concepts
- **Transmission time (Tx)**: Data size / Bandwidth
  - Formula: Tx = Packet size (bits) / Bandwidth (bps)

- **Propagation delay (Tp)**: Distance / Propagation speed
  - Formula: Tp = Distance (m) / Propagation speed (m/s)
  - Typically 2 × 10^8 m/s in fiber optics (2/3 of light speed)
  - Rule of thumb: 1μs/100m

- **Normalized propagation delay (a)**: 
  - Formula: a = Propagation delay / Transmission time = Tp / Tx

- **Link utilization (U)**: 
  - Formula: U = W / (1 + 2a), where W is window size

- **Minimum window size calculation**:
  - Formula: W ≥ 1 + 2a
  - For specific utilization (U): W = U(1 + 2a)
  - Sequence number field size: 2^k ≥ W + 1 (k is bit count)

### Frame Sequencing and Sliding Window
- **Sequence Number**: Tracks the order of data packets
- **Acknowledgement Number**: Next sequence number expected by the receiver

#### Go-Back-N Protocol
- Sender window size > 1, Receiver window size = 1
- When errors occur, retransmits all frames from the error point
- Discards out-of-order frames

#### Selective Repeat/Reject Protocol
- Sender window size > 1, Receiver window size > 1
- Only retransmits frames with errors
- Buffers out-of-order frames

#### Sliding Window Operation
1. **Sender behavior**:
   - Can transmit all frames within the window
   - Moves window forward upon receiving ACK
   - Begins retransmission when timeout occurs

2. **Receiver behavior**:
   - Sends ACK when receiving correct sequence number
   - Go-Back-N: Only accepts in-order frames
   - Selective Repeat: Buffers out-of-order frames

3. **Frame sequence number determination**:
   - Required bit count = log₂(Window size × 2)
   - Maximum sequence number = 2^(bit count) - 1

### Problem Solving Method
1. Calculate transmission time (Tx): Packet size / Bandwidth
2. Calculate propagation delay (Tp): Distance × Propagation delay constant
3. Calculate a = Tp / Tx
4. Calculate minimum window size W = 1 + 2a
5. Determine required sequence bit count k: 2^(k-1) ≥ W

## 3. Reliability and Network Resilience

### Reliability Calculation
- **Reliability (r)**: Network availability percentage (e.g., 99.9999%)
- **Failure probability (b)**: 1 - r (e.g., 1 - 0.999999 = 10^-6)
- **Annual downtime**:
  - Formula: T = 365 days × 24 hours/day × 60 minutes/hour × 60 seconds/minute × failure probability
  - 99.9999% reliability (6-9's): 31.5 seconds/year downtime

### Network Resilience Calculation
- **Serial connection**: All links must be operational for communication
  - Formula: r_total = r1 × r2 × ... × rn
  - Formula: b_total = 1 - r_total

- **Parallel connection**: Only one link needs to be operational
  - Formula: b_total = b1 × b2 × ... × bn
  - Formula: r_total = 1 - b_total

- **Complex connection**: 
  1. Calculate reliability of each path
  2. Calculate total failure probability for parallel connections

## 4. ARQ (Automatic Repeat reQuest) Protocols

### Selective-Reject ARQ
- Only retransmits frames with errors
- Utilization (U) calculation:
  - Formula: U = W(1-P) / (1 + 2a)
  - W: Window size
  - P: Frame error probability
  - a: Normalized propagation delay

### Stop-and-Wait ARQ
- Waits for acknowledgment (ACK) after transmission
- Utilization (U) calculation:
  - Formula: U = (1-P) / (1 + 2a)

### Go-Back-N ARQ
- Retransmits all frames from the error point
- Utilization (U) calculation:
  - Formula: U = (1-P) / (1 + 2aP)

## 5. CSMA/CD and Binary Exponential Backoff

### CSMA/CD (Carrier Sense Multiple Access with Collision Detection) Mechanism
- Stops transmission when collision is detected, retries using backoff algorithm
- Maximum collision attempts: 16 (typical Ethernet)

### CSMA/CD Minimum Frame Size
- CSMA/CD requires frame transmission time to exceed round-trip propagation delay
- Minimum frame size (time) = 2 × end-to-end signal propagation time
- End-to-end signal propagation time = (Segment delay × Segment count) + (Repeater delay × Repeater count)
- Minimum frame size (bits) = Minimum frame size (time) × Bandwidth (bps)
- Ethernet standard minimum frame size: 64 bytes (512 bits)

### Binary Exponential Backoff (BEB)
- Waits random time slots after collision
- Slot range: 0 ~ (2^k - 1) (k is collision count, maximum 10)
- Collision probability calculation:
  - Probability all n stations select the same slot: (1/2^k)^(n-1)
  - Probability at least one station selects a different slot: 1 - (1/2^k)^(n-1)

### Aloha Protocols
- Simple MAC protocol for channel access
- **Pure Aloha**:
  - Transmits immediately when data is available
  - Retransmits after random time when collision occurs
  - Throughput (S) = G * e^(-2G), where G is offered load
  - Maximum Throughput = 1/(2e) ≈ 0.184 (at G=0.5)
- **Slotted Aloha**:
  - Divides time into slots, transmits only at slot beginnings
  - Reduces collision possibility
  - Throughput (S) = G * e^(-G)
  - Maximum Throughput = 1/e ≈ 0.368 (at G=1.0)

### Problem Solving Method
1. Calculate collision probability
2. Calculate collision probability for next event
3. Success probability = 1 - collision probability

## 6. TCP Connection Management and Congestion Control

### TCP Connection Establishment (3-way Handshake)
1. Client → Server: SYN(SEQ=x)
2. Server → Client: SYN-ACK(SEQ=y, ACK=x+1)
3. Client → Server: ACK(SEQ=x+1, ACK=y+1)

### TCP Connection Termination (4-way Handshake)
1. Client → Server: FIN(SEQ=x)
2. Server → Client: ACK(SEQ=y, ACK=x+1)
3. Server → Client: FIN(SEQ=y)
4. Client → Server: ACK(SEQ=x+1, ACK=y+1)

### TCP Header Structure
- Header size: 20-60 bytes
- Key fields: source/destination ports, sequence number, acknowledgment number, window size
- Flags: SYN, ACK, FIN, RST, PSH, URG

### TCP Flow Control and Congestion Control
1. **Slow Start**
   - Initial cwnd = 1 MSS (Maximum Segment Size)
   - cwnd doubles every RTT (Round Trip Time)
   - Exponential increase until reaching threshold (ssthresh)

2. **Congestion Avoidance**
   - Activates when cwnd ≥ ssthresh
   - Increases cwnd by 1 MSS per RTT (linear increase)

3. **Fast Retransmit**
   - Immediate retransmission upon receiving 3 duplicate ACKs
   - Detects loss without timeout waiting

4. **Fast Recovery**
   - Sets cwnd = ssthresh + 3 after duplicate ACKs
   - Increases cwnd by 1 for each additional duplicate ACK
   - Sets cwnd = ssthresh when receiving new ACK

### TCP Throughput Calculation
- **RTT-limited throughput**: 
  - Formula: Throughput = cwnd / RTT
  - cwnd = min(receiver window, congestion window)
  
- **Bandwidth-limited throughput**:
  - Formula: Throughput = Bandwidth × (1 - Packet loss rate)

## 7. IP Addressing and Subnetting

### IPv4 Address Structure
- 32-bit address (4 octets of 8 bits each, separated by dots)
- Classes:
  - Class A: 0.0.0.0 - 127.255.255.255 (/8)
  - Class B: 128.0.0.0 - 191.255.255.255 (/16)
  - Class C: 192.0.0.0 - 223.255.255.255 (/24)

### IPv6 Address Structure
- 128-bit address, typically shown as 8 groups of hexadecimal digits
- Provides much larger address space than IPv4

### Subnet Mask
- Separates network portion from host portion
- CIDR notation: /n (n is network bit count)
- Formula: Available host count = 2^(32-n) - 2

### Subnet Calculation
1. Subnet count: 2^(additional network bits)
2. Hosts per subnet: 2^(host bits) - 2
3. Network address: All host bits set to 0
4. Broadcast address: All host bits set to 1

### Variable Length Subnet Masking (VLSM)
- Uses different subnet mask sizes based on network requirements
- Reduces IP address waste

### Problem Solving Method
1. Analyze requirements (needed subnet count, host count)
2. Determine subnet bit count
3. Calculate network address, range, and broadcast address for each subnet

## 8. Packet Switching and Network Delay

### Packet Switching Types
- **Datagram**: Each packet is routed independently
- **Virtual Circuit**: All packets use the same path

### Delay Types
1. **Processing Delay**: Time to process packet headers
2. **Queuing Delay**: Waiting time in output link queue
3. **Transmission Delay**: Time to push packet onto link (L/R)
4. **Propagation Delay**: Time for signal to travel through the link (d/s)

### Total Delay Calculation
- **Store-and-Forward Delay**:
  - Formula: Total delay = Hop count × (Tx + Tp) + (Packet count - 1) × Tx
  - Additional packets increase delay only by transmission time (pipelining effect)

### Packet Switched Network Transmission Time Calculation
- **For message size M, packet count N, packet size L (= M/N), header size h, bandwidth B, hop count H**:
  
1. **Total transmitted data**: (L+h) × N bits
2. **Transmission time per packet**: (L+h)/B seconds
3. **Store-and-Forward delay**:
   - End-to-end delay for first packet: H × (L+h)/B
   - Additional delay for subsequent packets: (L+h)/B (pipelining effect)
4. **Total end-to-end transmission time**: H × (L+h)/B + (N-1) × (L+h)/B = (H+N-1) × (L+h)/B

### Optimal Packet Size
- When sending a message in multiple packets, there's an optimal packet size that minimizes total transmission delay
- In store-and-forward networks, Total Delay = (Hop count + N - 1) * Tx (assuming zero propagation delay)
- Tx = (P + H) / R, N = File size / P
- Delay = (Hop count + (File size / P) - 1) * (P + H) / R
- Optimal payload size can be found through differentiation (generally in the form P = sqrt(File size * Header size / (Hop count - 1)))

- **Optimal packet size in error-free environment**:
  - Larger packet sizes are more efficient (reduced header overhead)
  
- **Optimal packet size in error-prone environment**:
  - Formula: Optimal packet size = √((2 × Message size × Header size) / (Error rate × Hop count))
  - Using differential equations to minimize total delay:
    ```
    Total delay = (N+H-1) × Packet transmission time
               = (N+H-1) × (L+h)/B
               = (M/L + H - 1) × (L+h)/B
    ```
  
- **Optimization process**:
  1. Differentiate the total delay equation with respect to L and set to zero
  2. Calculate optimal payload size (L): L = √(h × M/(H-1))
  3. Verify L is between minimum (≥ header size) and maximum (≤ MTU)

### Circuit Switching vs Packet Switching
- **Circuit Switching**: 
  - Dedicated path setup
  - Delay = Setup time + Transmission time + Propagation delay
  
- **Packet Switching**: 
  - Resource sharing, statistical multiplexing
  - Delay = Processing + Queuing + Transmission + Propagation delays

### When Packet Switching is Advantageous
- Formula: (k-1) × p/b < s
  - k: Hop count
  - p: Packet size
  - b: Bandwidth
  - s: Circuit setup time

## 9. Routing Protocols and Algorithms

### Dijkstra's Algorithm (Link State Routing)
1. Set starting node, initialize distance table
2. Calculate distances to all adjacent nodes
3. Select unvisited node with minimum distance
4. Update distances to other nodes through selected node
5. Repeat until all nodes are visited

### Bellman-Ford Algorithm (Distance Vector Routing)
- Each node receives distance information from neighboring nodes
- Updates routing table when discovering new paths
- Has long convergence time and count-to-infinity problem

### Major Routing Protocols

#### BGP (Border Gateway Protocol)
- **Characteristics**: Path vector protocol for inter-AS routing
- **Operating layer**: Application layer (TCP port 179)
- **Message types**: OPEN, UPDATE, KEEPALIVE, NOTIFICATION
- **Routing criteria**: Path attributes (AS path, origin, next hop) and policies
- **Usage**: Internet backbone router connections (e.g., between ISPs)

#### OSPF (Open Shortest Path First)
- **Characteristics**: Link state routing protocol using Dijkstra's algorithm
- **Operating layer**: Network layer (IP protocol 89)
- **Area concept**: Uses areas for hierarchical routing
- **Metric**: Bandwidth-based cost
- **Usage**: Internal routing in medium to large enterprise networks
- **Hello packets**: Sent periodically to maintain neighbor relationships (default 10 seconds)

#### RIP (Routing Information Protocol)
- **Characteristics**: Distance vector protocol using Bellman-Ford algorithm
- **Operating layer**: Application layer (UDP port 520)
- **Metric**: Hop count only, maximum 15 hops (16 considered infinity)
- **Timers**: Update (30s), Invalidation (180s), Flush (240s)
- **Usage**: Internal routing in small networks
- **Disadvantages**: Slow convergence in large networks, count-to-infinity problem

### Routing Table Interpretation
- Destination network/host
- Next hop router
- Interface
- Metric

## 10. ARP, DNS, DHCP Protocols

### ARP (Address Resolution Protocol)
- Converts IP addresses to MAC addresses
- Operation:
  1. ARP request broadcast (Target IP, Sender MAC)
  2. Only IP owner sends ARP reply unicast (including its MAC)
  3. Mapping stored in ARP cache

### DNS (Domain Name System)
- Converts domain names to IP addresses
- Hierarchical distributed database
- Operation:
  1. Client request
  2. Local DNS server lookup
  3. Sequential queries: Root → TLD → Authoritative server

### DHCP (Dynamic Host Configuration Protocol)
- Automatically assigns IP addresses
- Operation:
  1. DISCOVER (broadcast)
  2. OFFER (server response)
  3. REQUEST (client selection)
  4. ACK (server confirmation)

## 11. IP Fragmentation

### IP Fragmentation Process
- Divides packets larger than MTU (Maximum Transmission Unit)
- Formula: Fragment count = ⌈(Original size - Header size) / (MTU - Header size)⌉

### Fragmentation Fields
- **Identification**: Identifies original packet
- **DF (Don't Fragment)**: Prohibits fragmentation
- **MF (More Fragments)**: Indicates additional fragments
- **Fragment Offset**: Position in original data (8-byte units)

### Fragmentation Process Steps
1. Compare original packet size with MTU
2. Calculate required fragment count
3. Calculate offset for each fragment
4. Set MF flag for all fragments except the last
5. Assign same ID to all fragments

## 12. TCP/IP Network Communication Steps

### Web Page Loading Process
1. **ARP**: Determine gateway MAC address
2. **DNS**: Look up web server IP address
3. **TCP**: Establish connection via 3-way handshake
4. **HTTP**: Send request and receive response
5. **TCP**: Terminate connection

### NAT (Network Address Translation)
- Converts private IPs to public IPs
- Operation:
  1. Internal→External: Transform source IP/port and create mapping table
  2. External→Internal: Reference mapping table to transform destination IP/port 
