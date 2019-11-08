---
category: Paper Reading
layout: post
section-type: post
tags:
- SOSP-OSDI
- EECS591
title: "Aegean: Replication beyond the client-server model"
---

## Problem

- Large scale environments where services are no longer standalone

  Multiple components work together to provide a high-level service, like Google photon and HBase

  They are also mission-critical and requires to be highly-available. Like 2PC's C in Spanner

- Not only incorrect, but also inefficient bound y sequential execution.

  - Incorrect

    - Primary-Backup

      Client --> Primary --> Backend. At the time Primary requests to Backend, it crashes and backup now have no idea.

    - Paxos

      Client --> Replica --> Backend. When replica makes 10 requests, crashed. Now the other replicas should reach the same state, But they are unable to, because the replica has crashed.

  - Inefficient

    Consider Paxos sending nested requests. To guarantee consistency, each request must finish executing before another rest can start.

  

## Solution

### Correctness

Aegean proposes three methods together to ensure correctness. 

- Server Shim

  - Used for preventing active replication mid-service sending multiple identical requests.
  - Deal with authentication. Only forward to backend when it received a **quorum** of matching requests.
  - When response, responds to all mid=service. One thread for each response to insure delivery.

- Durability of nested requests

  Now there is another input to the replication other than client requests, **nest response**. The replicas should be able durablize any msg from outside.

  - Forward response to all replicas
  - Only consider response durable when received $u+1$ acknowledgements.

- Spec-Tame

  Speculate first and rely on rollback is not applicable. Because one could cause **visible** changes to backend.

  - Nested requests are output commit from clients, which causes inconsistency. Speculation can still be applied if there is no requests sending out.
  - Set parallel barrier when proceeding the output send
  
  ![](https://raw.githubusercontent.com/JingyZhu/image-bed/master/posts/Screen%20Shot%202019-11-08%20at%201.17.51%20AM.png)

<br>
### Efficiency: Improve sequential execution

- Request Pipelining
  
  - Executing afterwards jobs when while waiting for the previous response.
  
    ![](https://raw.githubusercontent.com/JingyZhu/image-bed/master/posts/Screen%20Shot%202019-11-08%20at%201.23.18%20AM.png)
  
  - Executing all requests in a batch ($k$) until all of them reach the nested request send. This is to make sure that all the replicas **execute different job pieces** in the same order.
  
    - Consider if $r_1$ response arrived before $r_k$ doing execution for replica A. But $r_1$ response didn't arrive for replica B so it executes $r_k$. Now A: $r_{1resp}\rightarrow r_k$ vs. B: $r_k\rightarrow r_{1resp}$.
- Not necessarily linearizable