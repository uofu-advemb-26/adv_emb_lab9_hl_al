Invariants: 

1. If a train is in thw intersection, the gate is down
2. Elapsed fires exactly 10 seconds after start_time is called
3. There is no latency when passing between states (we catch all signals)
4. Two signals cannot arrive at exactly the same time
5. northbound_approach/southbound_approach always arrive before northbound_depart/southbound_depart


Invalid Example FSM Sequence
 
* Start at idle
1. northbound_approach (-> ringing, arms up)
2. southbound_approach (stay in ringing, arms up)
3. elapsed (-> ringing, arms down)
4. nb_depart (-> ringing, arms up)
5. elapsed (-> idle)

! Southbound is still in the intersection, violating invariant 1


