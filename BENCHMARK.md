# Benchmark Sheet

## Benchmark Context
These results should be presented in two distinct phases:
- Creation phase: agent object instantiation and enqueue setup.
- Scheduling phase: dispatch and processing throughput.

## Creation Phase
| Metric | Value |
|---|---:|
| Agents created | 50,000 |
| Creation time | 17.526 s |
| Creation rate | 2,853 agents/sec |

## Scheduling Phase
| Metric | Value |
|---|---:|
| Total steps processed | 250,000 |
| Scheduling time | 0.115 s |
| Throughput | 2,166,522 steps/sec |

## Clean Sales Statement
Nexus Scheduler processes 2.16M scheduling steps/second on a single Windows CPU node (50,000 agents, 250,000 steps, 0.115s total scheduling time) — versus Ray RLlib’s ~1.2ms dispatch latency per step.

## Interpretation Notes
- Creation time is setup overhead and should not be mixed with runtime scheduling throughput.
- Scheduling throughput is the headline production metric.
- This benchmark is strongest when the buyer sees both numbers separately.

## Comparison Table
| System | Metric | Value |
|---|---|---:|
| Nexus Scheduler | Creation rate | 2,853 agents/sec |
| Nexus Scheduler | Scheduling throughput | 2,166,522 steps/sec |
| Ray RLlib | Dispatch latency | ~1.2 ms per step |

## Sales Note
Use this benchmark to support the startup, enterprise, and full IP transfer offers.
