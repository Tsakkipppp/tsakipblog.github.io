---
layout: post
title: "Why Distributed Consensus Is Hard: Rebuilding Raft"
date: 2026-10-16
categories: [distributed-systems, go]
---

Distributed systems fail in subtle, messy ways. When a network splits, how do you prevent two leaders from accepting conflicting writes?

Here is what I learned rebuilding the Raft consensus algorithm from scratch...

## 1. The Leader Election Problem

When a follower node misses two heartbeats, it transitions to a candidate state:

```go
func (rf *Raft) startElection() {
    rf.currentTerm++
    rf.votedFor = rf.me
    rf.resetElectionTimer()
    // broadcast RequestVote RPCs
}
