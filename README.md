# Invariants: 

1. If a train is in thw intersection, the gate is down
2. Elapsed fires exactly 10 seconds after start_time is called
3. There is no latency when passing between states (we catch all signals)
4. Two signals cannot arrive at exactly the same time
5. northbound_approach/southbound_approach always arrive before northbound_depart/southbound_depart


# Invalid Example FSM Sequence
 
*Start at idle*
1. northbound_approach (-> ringing, arms up)
2. southbound_approach (stay in ringing, arms up)
3. elapsed (-> ringing, arms down)
4. nb_depart (-> ringing, arms up)
5. elapsed (-> idle)

*! Southbound is still in the intersection, violating invariant 1*


**State table**

| number | arms_down | alarm_on | northbound_present | southbound_present | north_approach | south_approach | north_depart | south_depart | ringing | safety_hazard |
|---|---|---|---|---|---|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 | 6 | 5 | 21 | 21 | 0 | |
| 1 | 0 | 0 | 0 | 1 | 7 | 20 | 21 | 0 | 1 | 16 |
| 2 | 0 | 0 | 1 | 0 | 20 | 7 | 0 | 21 | 2 | 16 |
| 3 | 0 | 0 | 1 | 1 | 20 | 20 | 1 | 2 | 3 | 16 |
| 4 | 0 | 1 | 0 | 0 | 6 | 5 | 21 | 21 | 0 | |
| 5 | 0 | 1 | 0 | 1 | 7 | 20 | 21 | 4 | 13 | |
| 6 | 0 | 1 | 1 | 0 | 20 | 7 | 4 | 21 | 14 | |
| 7 | 0 | 1 | 1 | 1 | 20 | 20 | 5 | 6 | 15 | |
| 8 | 1 | 0 | 0 | 0 | 14 | 13 | 21 | 21 | 8 | |
| 9 | 1 | 0 | 0 | 1 | 15 | 20 | 21 | 0 | 9 | 17 |
| 10 | 1 | 0 | 1 | 0 | 20 | 15 | 0 | 21 | 10 | 17 |
| 11 | 1 | 0 | 1 | 1 | 20 | 20 | 9 | 10 | 11 | 17 |
| 12 | 1 | 1 | 0 | 0 | 14 | 13 | 21 | 21 | 4 | 19 |
| 13 | 1 | 1 | 0 | 1 | 15 | 20 | 21 | 4 | 13 | |
| 14 | 1 | 1 | 1 | 0 | 20 | 15 | 4 | 21 | 14 | |
| 15 | 1 | 1 | 1 | 1 | 20 | 20 | 13 | 14 | 15 | |

**Error / constraint codes**

| number | rule |
|---|---|
| 16 | If a train is present, the alarm must be on |
| 17 | The arms should only be down if the alarm is on |
| 18 | 10 s after a train is present, the arms must go down |
| 19 | The arms should only be down if a train is present |
| 20 | A second approach on the same track cannot occur until the current train on that track has departed |
| 21 | A departure cannot occur on a given track unless an approach has occurred on that track |


*See State Machine.pdf for completed table and updated fsm*



