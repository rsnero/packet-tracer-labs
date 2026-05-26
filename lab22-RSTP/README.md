# RSTP

## Questions
1. Which switch is the root bridge?
    Use the CLI to examine the port role/state of each interface on the root.
    What appears different than what you have learned about the root bridge?
    What is the cause of this?

2. Without using the CLI, determine the port role/state of each remaining switch interface.
    Use the CLI to confirm.

3. Manually configure the appropriate RSTP link type on each interface.
    What do you think is the correct link type for SW1's F0/24?

## Answers

1. Root Bridge
RSTP chooses root bridge based on lowest Bridge ID
Bridge ID = priority + MAC address

- all have same priority, SW1 has lowest MAC
- SW1 is the root bridge

- the root bridge has a designated port in each collision domain its connected to. In modern networks, hubs aren't really used so we say that all interfaces are designated
- that's why F0/3 isn't designated because its in the same collision domain as F0/2

2. Port role/state of each remaining SW interface

SW2:
SW2 F0/1 <-> SW1 F0/1
Cost: 19

SW2 G0/1 -> SW3 G0/1 -> SW3 F0/2 -> HUB1 -> SW1
Cost: 4 + 19 = 23

Therefore, SW2 F0/1 is the root port/forwarding

SW2 F0/23 = Designated / Forwarding / Edge
SW2 F0/24 = Designated / Forwarding / Edge

SW3:
SW3 F0/2 -> HUB1 -> SW1
Cost: 19

SW3 F0/1 → SW4 F0/2 → SW2 F0/1 → SW1
19 + 19 + 19 = 57

Therefore, SW3 F0/2 is root port/forwarding

SW4:
SW4 F0/2 → SW2 F0/1 → SW1
19 + 19 = 38

SW4 F0/1 → SW3 F0/2 → Hub1 → SW1
19 + 19 = 38

Tie breaker: lowest neighbor bridge ID
Therefore, SW4 F0/1 is the root port/forwarding

Link: SW2 G0/1 ↔ SW3 G0/1
Both root cost = 19
Tie breaker: Lowest Bridge ID

Therefore, 
SW3 G0/1 = Designated / Forwarding
SW2 G0/1 = Alternate / Discarding

Link: SW2 F0/2 ↔ SW4 F0/2
SW3 root cost = 19
SW4 root cost = 38

Therefore, 
SW3 F0/1 = Designated / Forwarding
SW4 F0/1 = Root Port / Forwarding

Note: Since SW1 has two ports connected to the same shared hub segment, one root bridge port can become Backup / Discarding

3. 
Because it connects to a hub, the correct RSTP link type is not point-to-point. It is shared.
Edge means direct end-host port.
SW1 F0/24 is not directly connected to one end host.
It is connected to a hub/shared collision domain.

How to confirm in CLI:
show spanning-tree
show spanning-tree vlan 1

Root = Root Port
Desg = Designated Port
Altn = Alternate Port
Back = Backup Port

FWD = Forwarding
BLK / DISC = Blocking or Discarding

Notes:
1. Pick root bridge: lowest priority, then lowest MAC.
2. On root bridge: ports are usually Designated/Forwarding.
3. Exception: if two root ports connect to the same hub/shared segment, one can become Backup/Discarding.
4. Each non-root switch picks one Root Port: lowest cost path to root.
5. For every remaining link/segment, lower root cost side becomes Designated.
6. The other side becomes Alternate/Discarding if it would create a loop.
7. Direct switch-to-switch = point-to-point.
8. Direct host = edge.
9. Hub = shared.


