# Lab 20 - Analyzing STP

## Goal

Q: Which switch is the root bridge?
A: SW3

1. Compare bridge priority
2. Tie Breaker: Go with lowest MAC address

Therefore, all 4 interfaces on SW3 are root ports (Designated).

Q: Identify the role (root/designated/non-designated) of each switch port:
A:

Speed				STP Cost
10 Mbps (Reg. Ethernet)		100
100 Mbps (Fast Ethernet)	19
1 Gbps (Gigabit Ethernet)	4
10 Gbps (10 Gigabit Ethernet)	2

* Each other SW in topology MUST have a single root port
1. The port with the lowest STP root cost = SW's root port
2. Tie breaker: Neighbour Bridge ID (lower wins)
3. Tie breaker: Neighbour SW Port ID (lower wins)

* All ports connected to a root port = D
* Ports connected directly to root bridge that are not R = N
* Interfaces on the SW with the lower root cost will be D, the other side N

SW3: (all Designated since Root Bridge)
F0/1: D   
F0/2: D   
F0/3: D   
G0/1: D

SW1:
F0/1: N, since higher root cost than SW2     
F0/2: N, since higher root cost than SW2 
F0/3: N, since connected to SW3    
F0/4: R

root cost:
f01/2 = 19 (itself) + 4 (SW2 g0/1) + 4 (SW4 g02) = 27
f03/4 = 19 (tie)
1. Same root cost
2. Same neighbour bridge ID
3. f01 = lower, so SW1's f0/4 = root port

SW2:
F0/1: D, since lower root cost than SW1     
F0/2: D, since lower root cost than SW1 
F0/3: N    
G0/1: R

root cost:
f03 = 19 + 0, since directly connected to root bridge
g01 = 4 + 4, therefore g01 = root port

SW4:
G0/1: D, since it's connected to root port    
G0/2: R, since g0/1 must be D, and it has low cost (4)

Q: Confirm your answers in the CLI

>en
`#show spanning-tree`
`#show spanning-tree detail`

## Screenshots


### Topology


### Router Configuration


