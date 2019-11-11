---
category: Paper Reading
layout: post
section-type: post
tags:
- EECS591
title: Mencius Building Efficient Replicated State Machine for WANs

---
## Motivation

#### Provide high throughput under high client load and low latency under low client load in WAN

- Existing protocols (Paxos, Fast Paxos, etc) are not in general the best for WAN apps,.

  - Paxos relies on single leader to decide, which bottlenecks tput by bandwidth and CPU. 
  - Clients from other sites than leader suffers high latency.
- Fast Paxos has lower tput under high load due to msg complexity.

## System model

#### WAN

- Construct by multiple interconnected sites.
- Each site has a server, server pairwise connected through WAN with asymmetric and variable network.
- Async network, server fails by crashing, and perhaps recovering.

## Mencius design

#### Assumptions

- Crash failure
- Unreliable failure detector : It can make unbounded #mistakes. But eventually only failed processes are suspected
- Async FIFO communication: Based on TCP

#### Simple consensus

- For each instance (slot), there is only one coordinator who can propose any value. Others can only propose *noop* 
- Instance assign scheme is **known to every server**. (Like $cn+p$ for server *p*)

#### Coordinator Paxos

- Introduce specialized messages:
  - **SUGGEST**: Coordinator Proposes *v*
  - **SKIP**: Coordinator Proposes *noop*: On received Skip, any other server can **directly choose** because no other value can be proposed.
  - **REVOKE**:  Detecting leader failure and trying to be the new leader. Prepare like Multi-Paxos
- Main differences with Paxos
  - Starts from a different state
  - Learns *noop* upon receiving suggest

#### Simple state machine protocol

Introduce 4 rules to ensure liveness (requests eventually commit).

Safety is assumed to be true by other techniques

- Server *p* maintain next coordinated instance $I_p$, when receiving requests it SUGGEST requests and update $I_p$  

  - Only works if every server SUGGEST at the same rate
  - Otherwise it will leaves holes to prevent anyone from commit

- If server *p*  receive SUGGEST with instance $ i>I_p$ , it updates $I_p$ to **$I'_p =k: k > i \and p$  coordinates on $p$**. And propose *noop* for all instance between that it coordinates.

  - If server crashes, it can't broadcast its' SKIP

- If *p* suspects *q* has failed. Defines $C_q$  as the least un-learned instance for *p* that *q* coordinates. *q* REVOKES for all instances $[C_q, I_p]$ that *q* coordinates.

  - Solves faulty servers' coordinating instances. Now new leader can SUGGEST noop

  - If *q* is not crashed. It's coordinator is "replaced"

  - <p style="color: blue;"> What if it does crashed, does p take over the later instances? What if multiple 'q' wants to do the same thing? </p>  

- If server *p* SUGGEST *v* on instance *i*, but learns *noop* on *i*, it SUGGEST *v* again (on later instances).

  - <p style="color: blue;"> Which instances to propose? </p>  

#### Optimizations

Current messages complexity is high if there is unbalanced requests. (Consider there is only one server processing requests).

Consider a scenario that *q* SUGGEST on *i* > $I_p$ to *p*

- *p* doesn't need to send explicit SKIP to *q*.
  - By sending ACCEPT of *i*, *p* implies that it won't SUGGEST **any value < *i***

- For other servers *r*, *p* doesn't need to send SKIP to *r*. Instead it can send in the future SUGGEST
  - Because of FIFO. There is no way to get larger instance SUGGEST first
  - Could delay the SKIP passing if it doesn't SUGGEST
- *p* propose SKIP is there are too many outstanding SKIP.
- If *p* detects *q* fails, it REVOKE $[C_q, I_p + 2\beta]$ that *q* coordinates **if $C_q < I_p + \beta$*
  - Makes sure that when *p* learned $i > C_q$, any previous instances are committed.

## Evaluations

Omitted

