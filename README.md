 # Nexus Scheduler

> Hardware-inspired agent orchestration for Python. 2.16M scheduling steps/second on a single CPU node.

## What is this?

Nexus Scheduler is a commercial Python library that brings CPU scheduler architecture to agent orchestration.

Most agent stacks use a single-priority FIFO queue. Nexus Scheduler uses 64 hierarchical queues, priority aging, and per-agent TD-error credit assignment — the same principles that make hardware schedulers fast and fair.

## Benchmark

Two phases are reported separately:

| Phase | Detail | Result |
|---|---|---|
| Creation phase | 50,000 agents instantiated + enqueued | 2,853 agents/sec (17.526s) |
| **Scheduling phase** | 250,000 steps dispatched | **2,166,522 steps/sec (0.115s)** |

**Headline number:** Nexus Scheduler processes **2.16M scheduling steps/second** on a single Windows CPU node (50,000 agents, 250,000 steps, 0.115s total scheduling time) — versus Ray RLlib's ~1.2ms dispatch latency per step.

## Architecture

- **64 hierarchical queues** — 8 priority levels × 8 queues per level with round-robin scheduling.
- **Priority aging** — agents that wait too long are automatically promoted to avoid starvation.
- **TD-error credit assignment** — per-agent 256-entry ring buffers, γ=0.99, λ=0.95, precomputed decay LUT.
- **1M+ agent contexts** — in-memory or Redis-backed, thread-safe.
- **Chiplet affinity routing** — CPU / GPU / LPU / ANY per agent.
- **Signed license delivery** — pip-installable package + HMAC-signed license JSON.

## Quick install

```bash
pip install nexus_scheduler-1.0.0.tar.gz

# Redis support
pip install "nexus-scheduler[redis]"

# Verify
python -c "from nexus_scheduler import AgentOrchestrator; print('OK')"
```

## Quick example

```python
from nexus_scheduler import AgentOrchestrator, AgentContext, Chips

orch = AgentOrchestrator()

for i in range(10_000):
    ctx = AgentContext(agent_id=i)
    ctx.priority = i % 8
    ctx.chiplet_affinity = Chips.GPU
    orch.create_agent(ctx)

orch.start(interval_us=1.0)
orch.set_completion_callback(lambda msg: print(f"Agent {msg.agent_id} done"))
```

## Modules

| Module | Description |
|---|---|
| `scheduler.py` | Core AgentOrchestrator + HierarchicalQueue |
| `fsm.py` | Scheduler FSM — exact hardware algorithm |
| `credit.py` | TD-error credit assignment engine |
| `context.py` | Agent context store (memory / Redis) |
| `queues.py` | Adaptive priority queue + MLFQ |

## Commercial licensing

This repository is a **public teaser**. Full source is available under commercial license.

| Tier | Price | Details |
|---|---|---|
| Startup | $50,000 one-time | 90-day license, 10K agents |
| Enterprise | $250,000 one-time | 1-year license, 1M+ agents |
| Royalty / OEM | 0.5% of revenue | Embed in your product |
| **Full IP transfer** | **$5,000,000 one-time** | Exclusive ownership, all rights |

## Contact

cvetovet@gmail.com
