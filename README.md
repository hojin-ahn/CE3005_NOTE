# 컴퓨터 네트워크 핵심 개념 및 문제 해설

## 1. OSI 7-계층 모델 (OSI 7-Layer Model)

### 계층별 특징
1. **물리 계층 (Physical Layer)**
   - 비트 스트림(bit stream)을 전기적 신호로 변환
   - 케이블(cable), 리피터(repeater), 허브(hub) 등의 장비
   - 대역폭(bandwidth), 전압 레벨(voltage level), 물리적 연결 등을 담당

2. **데이터 링크 계층 (Data Link Layer)**
   - MAC 주소 기반 통신
   - 프레임(frame) 단위의 데이터 처리
   - 오류 감지(error detection) 및 흐름 제어(flow control)
   - 스위치(switch), 브릿지(bridge)가 이 계층에서 작동
   - 계산식: 최대 윈도우 크기(maximum window size) = 2^(시퀀스 비트 수)-1

3. **네트워크 계층 (Network Layer)**
   - IP 주소 기반 라우팅(routing)
   - 패킷(packet) 단위의 데이터 처리
   - 라우터(router)가 이 계층에서 작동
   - 서브넷팅(subnetting), 라우팅 프로토콜(routing protocol) 처리

4. **전송 계층 (Transport Layer)**
   - 종단 간(end-to-end) 통신 보장
   - TCP(Transmission Control Protocol): 신뢰성 있는 연결 지향적 프로토콜
   - UDP(User Datagram Protocol): 비연결성 프로토콜
   - 포트 번호(port number)로 애플리케이션 구분
   - 흐름 제어(flow control), 오류 복구(error recovery), 혼잡 제어(congestion control)

5. **세션 계층 (Session Layer)**
   - 통신 세션(session) 설정, 유지, 종료
   - 전이중(full-duplex), 반이중(half-duplex) 통신 관리
   - 동기화 지점(checkpoint) 관리

6. **표현 계층 (Presentation Layer)**
   - 데이터 형식 변환
   - 암호화/복호화(encryption/decryption), 데이터 압축(data compression)
   - 문자 인코딩 변환(ASCII, EBCDIC 등)

7. **응용 계층 (Application Layer)**
   - 사용자 인터페이스 제공
   - HTTP, FTP, SMTP, DNS 등의 프로토콜
   - 사용자 데이터 처리

### TCP/IP 모델과 비교
- **응용 계층(Application Layer)**: OSI의 5-7계층 통합 (Application, Session, Presentation)
- **전송 계층(Transport Layer)**: OSI의 4계층과 동일 (Transport)
- **인터넷 계층(Internet Layer)**: OSI의 3계층과 동일 (Network)
- **네트워크 인터페이스 계층(Network Interface Layer)**: OSI의 1-2계층 통합 (Physical, Data Link)

## 2. 데이터 전송 및 윈도우 크기 (Data Transmission & Window Size)

### 핵심 개념
- **전송 시간(Transmission time, Tx)**: 데이터 크기 / 대역폭
  - 계산식: Tx = 패킷 크기(bits) / 대역폭(bps)

- **전파 지연(Propagation delay, Tp)**: 거리 / 전파 속도
  - 계산식: Tp = 거리(m) / 전파 속도(m/s)
  - 일반적으로 광섬유에서는 2 × 10^8 m/s (빛의 속도의 2/3)
  - 계산식: 1μs/100m의 비율로 계산

- **정규화된 전파 지연(Normalized propagation delay, a)**: 
  - 계산식: a = 전파 지연 시간 / 전송 시간 = Tp / Tx

- **링크 활용도(Link utilization, U)**: 
  - 계산식: U = W / (1 + 2a), W는 윈도우 크기(window size)

- **최소 윈도우 크기 계산(Minimum window size)**:
  - 계산식: W ≥ 1 + 2a
  - 특정 활용도(U)를 위한 윈도우 크기: W = U(1 + 2a)
  - 시퀀스 번호 필드 크기(sequence number field size): 2^k ≥ W + 1 (k는 비트 수)

### 문제 해결 방법
1. 전송 시간(Tx) 계산: 패킷 크기 / 대역폭
2. 전파 지연(Tp) 계산: 거리 × 전파 지연 상수(보통 3ms/km 또는 1μs/100m)
3. a = Tp / Tx 계산
4. 최소 윈도우 크기 W = 1 + 2a 계산
5. 필요한 시퀀스 비트 수 k: 2^(k-1) ≥ W

## 3. 신뢰성 및 네트워크 복원력 (Reliability & Network Resilience)

### 신뢰성 계산
- **신뢰성(Reliability, r)**: 네트워크 가용 시간 비율 (예: 99.9999%)
- **고장 확률(Failure probability, b)**: 1 - r (예: 1 - 0.999999 = 10^-6)
- **연간 허용 다운타임(Annual downtime)**:
  - 계산식: T = 365일 × 24시간/일 × 60분/시간 × 60초/분 × 고장확률
  - 99.9999% 신뢰성(6-9's): 31.5초/년 다운타임

### 네트워크 복원력 계산
- **직렬 연결(Serial connection)**: 모든 링크가 작동해야 통신 가능
  - 계산식: r_total = r1 × r2 × ... × rn
  - 계산식: b_total = 1 - r_total

- **병렬 연결(Parallel connection)**: 하나의 링크만 작동해도 통신 가능
  - 계산식: b_total = b1 × b2 × ... × bn
  - 계산식: r_total = 1 - b_total

- **복합 연결(Complex connection)**: 
  1. 각 경로의 신뢰성 계산
  2. 병렬 연결에 대한 전체 고장 확률 계산

## 4. ARQ (Automatic Repeat reQuest) 프로토콜

### Selective-Reject ARQ
- 오류가 발생한 프레임만 재전송
- 활용도(Utilization, U) 계산:
  - 계산식: U = W(1-P) / (1 + 2a)
  - W: 윈도우 크기(window size)
  - P: 프레임 오류 확률(frame error probability)
  - a: 정규화된 전파 지연(normalized propagation delay)

### Stop-and-Wait ARQ
- 전송 후 확인(ACK) 수신까지 대기
- 활용도(Utilization, U) 계산:
  - 계산식: U = (1-P) / (1 + 2a)

### Go-Back-N ARQ
- 오류 발생 시 해당 프레임 이후 모든 프레임 재전송
- 활용도(Utilization, U) 계산:
  - 계산식: U = (1-P) / (1 + 2aP)

## 5. CSMA/CD와 이진 지수 백오프 (CSMA/CD & Binary Exponential Backoff)

### CSMA/CD (Carrier Sense Multiple Access with Collision Detection) 메커니즘
- 충돌 감지 시 전송 중단, 백오프 알고리즘으로 재시도
- 최대 충돌 시도 횟수: 16회 (일반적인 Ethernet)

### 이진 지수 백오프 (Binary Exponential Backoff, BEB)
- 충돌 후 랜덤 시간(slot) 대기
- 대기 슬롯 범위: 0 ~ (2^k - 1) (k는 충돌 횟수, 최대 10)
- 충돌 확률 계산:
  - n개 스테이션이 모두 같은 슬롯 선택 확률: (1/2^k)^(n-1)
  - 적어도 하나의 스테이션이 다른 슬롯 선택 확률: 1 - (1/2^k)^(n-1)

### Aloha 프로토콜 (Aloha Protocols)
- 채널 접근을 위한 간단한 MAC 프로토콜.
- **Pure Aloha**:
  - 전송할 데이터가 있으면 즉시 전송.
  - 충돌 발생 시 랜덤 시간 후 재전송.
  - 처리율(Throughput, S) = G * e^(-2G), G는 시도 횟수(offered load).
  - 최대 처리율(Max Throughput) = 1/(2e) ≈ 0.184 (G=0.5 일 때).
- **Slotted Aloha**:
  - 시간을 슬롯(slot)으로 나누고, 슬롯 시작 시점에만 전송 가능.
  - 충돌 가능성 감소.
  - 처리율(Throughput, S) = G * e^(-G).
  - 최대 처리율(Max Throughput) = 1/e ≈ 0.368 (G=1.0 일 때).

### 문제 해결 방법
1. 충돌 발생 확률 계산
2. 다음 이벤트에서 충돌 확률 계산 (모든 가능한 슬롯 선택 조합 고려)
3. 성공 확률 = 1 - 충돌 확률

## 6. TCP 연결 관리 및 혼잡 제어 (TCP Connection Management & Congestion Control)

### TCP 연결 설정 (TCP Connection Establishment, 3-way Handshake)
1. 클라이언트 → 서버: SYN(SEQ=x)
2. 서버 → 클라이언트: SYN-ACK(SEQ=y, ACK=x+1)
3. 클라이언트 → 서버: ACK(SEQ=x+1, ACK=y+1)

### TCP 연결 종료 (TCP Connection Termination, 4-way Handshake)
1. 클라이언트 → 서버: FIN(SEQ=x)
2. 서버 → 클라이언트: ACK(SEQ=y, ACK=x+1)
3. 서버 → 클라이언트: FIN(SEQ=y)
4. 클라이언트 → 서버: ACK(SEQ=x+1, ACK=y+1)

### TCP 헤더 구조 (TCP Header Structure)
- 헤더 크기: 20-60바이트
- 주요 필드: 소스/목적지 포트(source/destination port), 시퀀스 번호(sequence number), 확인 번호(acknowledgment number), 윈도우 크기(window size) 등
- 플래그: SYN, ACK, FIN, RST, PSH, URG

### TCP 흐름 제어 및 혼잡 제어 (TCP Flow Control & Congestion Control)
1. **슬로우 스타트(Slow Start)**
   - 초기 cwnd = 1 MSS (Maximum Segment Size)
   - 매 RTT(Round Trip Time)마다 cwnd 2배 증가
   - 임계값(ssthresh, slow start threshold)에 도달할 때까지 지수적 증가

2. **혼잡 회피(Congestion Avoidance)**
   - cwnd ≥ ssthresh일 때 작동
   - 매 RTT마다 cwnd를 1 MSS씩 증가 (선형 증가)

3. **빠른 재전송(Fast Retransmit)**
   - 3개의 중복 ACK 수신 시 즉시 재전송
   - 타임아웃 대기 없이 손실 감지

4. **빠른 회복(Fast Recovery)**
   - 중복 ACK 수신 후 cwnd = ssthresh + 3
   - 추가 중복 ACK마다 cwnd 1 증가
   - 새로운 ACK 수신 시 cwnd = ssthresh

### TCP 처리율(Throughput) 계산
- **RTT 제한 처리율(RTT-limited throughput)**: 
  - 계산식: Throughput = cwnd / RTT
  - cwnd = min(받는 측 윈도우, 혼잡 윈도우)
  
- **대역폭 제한 처리율(Bandwidth-limited throughput)**:
  - 계산식: Throughput = 대역폭 × (1 - 패킷 손실률)

## 7. IP 주소 지정 및 서브넷팅 (IP Addressing & Subnetting)

### IPv4 주소 체계 (IPv4 Address Structure)
- 32비트 주소 (4개의 8비트 옥텟, 점으로 구분)
- 클래스(Class):
  - A 클래스(Class A): 0.0.0.0 - 127.255.255.255 (/8)
  - B 클래스(Class B): 128.0.0.0 - 191.255.255.255 (/16)
  - C 클래스(Class C): 192.0.0.0 - 223.255.255.255 (/24)

### IPv6 주소 체계 (IPv6 Address Structure)
- 128비트 주소, 일반적으로 16진수 8개 그룹으로 표시.
- IPv4보다 훨씬 큰 주소 공간 제공.

### 서브넷 마스크 (Subnet Mask)
- 네트워크 부분과 호스트 부분 구분
- CIDR(Classless Inter-Domain Routing) 표기법: /n (n은 네트워크 비트 수)
- 계산식: 사용 가능한 호스트 수 = 2^(32-n) - 2

### 서브넷 계산 (Subnet Calculation)
1. 서브넷 수: 2^(추가 네트워크 비트 수)
2. 각 서브넷의 호스트 수: 2^(호스트 비트 수) - 2
3. 네트워크 주소(Network address): 호스트 비트가 모두 0
4. 브로드캐스트 주소(Broadcast address): 호스트 비트가 모두 1

### 가변 길이 서브넷 마스크 (Variable Length Subnet Masking, VLSM)
- 네트워크 요구사항에 따라 서로 다른 크기의 서브넷 마스크를 사용하여 IP 주소를 효율적으로 할당하는 기법.
- IP 주소 낭비를 줄일 수 있음.

### 문제 해결 방법
1. 요구 사항 분석 (필요한 서브넷 수, 호스트 수)
2. 서브넷팅 비트 수 결정
3. 각 서브넷의 네트워크 주소, 범위, 브로드캐스트 주소 계산

## 8. 패킷 스위칭 및 네트워크 지연 (Packet Switching & Network Delay)

### 패킷 스위칭 유형 (Packet Switching Types)
- **다트그램(Datagram)**: 각 패킷이 독립적으로 라우팅
- **가상 회선(Virtual Circuit)**: 모든 패킷이 동일한 경로 사용

### 지연 유형 (Delay Types)
1. **처리 지연(Processing Delay)**: 패킷 헤더 처리 시간
2. **큐잉 지연(Queuing Delay)**: 출력 링크에서 대기 시간
3. **전송 지연(Transmission Delay)**: 패킷을 링크에 푸시하는 시간 (L/R)
4. **전파 지연(Propagation Delay)**: 링크를 통해 이동하는 시간 (d/s)

### 총 지연 계산 (Total Delay Calculation)
- **스토어-앤-포워드 지연(Store-and-Forward Delay)**:
  - 계산식: 총 지연 = 홉 수 × (Tx + Tp) + (패킷 수 - 1) × Tx
  - 파이프라인 효과(pipelining effect)로 인해 추가 패킷의 지연은 전송 시간(Tx)만큼만 증가

### 최적 패킷 크기 (Optimal Packet Size)
- 메시지를 여러 패킷으로 나누어 전송할 때, 전체 전송 지연을 최소화하는 패킷 크기가 존재.
- 스토어-앤-포워드 네트워크에서 총 지연(Total Delay) = (홉 수 + N - 1) * Tx (전파 지연 0 가정).
- Tx = (P + H) / R, N = 파일 크기 / P.
- Delay = (홉 수 + (파일 크기 / P) - 1) * (P + H) / R
- 미분을 통해 최적 페이로드 크기(P)를 찾을 수 있음 (일반적으로 P = sqrt(파일 크기 * 헤더 크기 / (홉 수 - 1)) 형태).

### 회선 스위칭 vs 패킷 스위칭 (Circuit Switching vs Packet Switching)
- **회선 스위칭(Circuit Switching)**: 
  - 전용 경로 설정
  - 지연 = 설정 시간 + 전송 시간 + 전파 지연
  
- **패킷 스위칭(Packet Switching)**: 
  - 자원 공유, 통계적 다중화(statistical multiplexing)
  - 지연 = 처리 + 큐잉 + 전송 + 전파 지연

### 패킷 스위칭이 유리한 조건
- 계산식: (k-1) × p/b < s
  - k: 홉 수(hop count)
  - p: 패킷 크기(packet size)
  - b: 대역폭(bandwidth)
  - s: 회선 설정 시간(circuit setup time)

## 9. 라우팅 프로토콜 및 알고리즘 (Routing Protocols & Algorithms)

### 다익스트라 알고리즘 (Dijkstra's Algorithm, Link State Routing)
1. 출발 노드 설정, 거리 테이블 초기화
2. 모든 인접 노드까지의 거리 계산
3. 방문하지 않은 노드 중 최소 거리 노드 선택
4. 선택한 노드를 통한 다른 노드까지의 거리 갱신
5. 모든 노드 방문할 때까지 반복

### 벨만-포드 알고리즘 (Bellman-Ford Algorithm, Distance Vector Routing)
- 각 노드가 이웃 노드로부터 거리 정보 수신
- 새로운 경로 발견 시 라우팅 테이블 갱신
- 수렴 시간이 길고 카운트-투-인피니티(count-to-infinity) 문제 존재

### 라우팅 테이블 해석 (Routing Table Interpretation)
- 목적지 네트워크/호스트(destination network/host)
- 넥스트 홉 라우터(next hop router)
- 인터페이스(interface)
- 메트릭(metric)

## 10. ARP, DNS, DHCP 프로토콜 (ARP, DNS, DHCP Protocols)

### ARP (Address Resolution Protocol)
- IP 주소를 MAC 주소로 변환
- 작동 방식:
  1. ARP 요청 브로드캐스트(ARP request broadcast) (대상 IP, 송신자 MAC)
  2. 해당 IP 소유자만 ARP 응답 유니캐스트(ARP reply unicast) (자신의 MAC 포함)
  3. ARP 캐시(ARP cache)에 매핑 저장

### DNS (Domain Name System)
- 도메인 이름을 IP 주소로 변환
- 계층적 분산 데이터베이스(hierarchical distributed database)
- 작동 방식:
  1. 클라이언트 요청(client request)
  2. 로컬 DNS 서버 검색(local DNS server lookup)
  3. 루트(root) → TLD(Top-Level Domain) → 권한 있는 서버(authoritative server) 순차 쿼리

### DHCP (Dynamic Host Configuration Protocol)
- 자동으로 IP 주소 할당
- 작동 방식:
  1. DISCOVER (브로드캐스트)
  2. OFFER (서버 응답)
  3. REQUEST (클라이언트 선택)
  4. ACK (서버 확인)

## 11. 패킷 단편화(IP Fragmentation)

### IP 단편화 (IP Fragmentation)
- MTU(Maximum Transmission Unit)보다 큰 패킷을 분할
- 계산식: 단편 수 = ⌈(원본 크기 - 헤더 크기) / (MTU - 헤더 크기)⌉

### 단편화 필드 (Fragmentation Fields)
- **Identification**: 원본 패킷 식별
- **DF(Don't Fragment)**: 단편화 금지
- **MF(More Fragments)**: 추가 단편 여부
- **Fragment Offset**: 원본 데이터에서의 위치 (8바이트 단위)

### 단편화 처리 (Fragmentation Process)
1. 원본 패킷 크기와 MTU 비교
2. 필요 단편 수 계산
3. 각 단편의 오프셋 계산
4. 마지막 단편을 제외한 모든 단편의 MF 플래그 설정
5. 모든 단편에 동일한 ID 할당

## 12. TCP/IP 네트워크 통신 단계 (TCP/IP Network Communication Steps)

### 웹 페이지 로딩 과정 (Web Page Loading Process)
1. **ARP**: 게이트웨이 MAC 주소 확인
2. **DNS**: 웹 서버 IP 주소 조회
3. **TCP**: 3-way 핸드셰이크로 연결 설정
4. **HTTP**: 요청 전송 및 응답 수신
5. **TCP**: 연결 종료

### NAT (Network Address Translation)
- 사설 IP를 공인 IP로 변환
- 작동 방식:
  1. 내부→외부: 소스 IP, 포트 변환 및 매핑 테이블 생성
  2. 외부→내부: 매핑 테이블 참조하여 목적지 IP, 포트 변환 
