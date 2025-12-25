# RailOptimus: Real-Time Railway Traffic Optimization

A mixed-integer linear programming framework for real-time railway traffic optimization. Solves the rerouting, reordering, and rescheduling problem 27–78% faster than standard MILP approaches through multi-stage decomposition while maintaining near-optimal solution quality.

## SIH 2025

- **Ministry:** Ministry of Education's Innovation Cell (MIC)
- **Theme:** Transportation & Logistics
- **PS Code:** 25138
- **Problem we're tackling:** Student Innovation: Swadeshi for Atmanirbhar Bharat - Transportation & Logistics

## Team Details

- **Team Name:** RailOptimus
- **Team ID:** 102098
- **Institute Name:** Netaji Subhas University of Technology

**Team Leader:** [@poorva-tehlan](https://github.com/poorva-tehlan)

**Team Members:**

- **MEMBER_1**  - Poorva Tehlan - 2024UCA1891 - [@poorva-tehlan](https://github.com/poorva-tehlan)
- **MEMBER_2** - Bhavishya Maheshwari - 2024UCA1884 - [@BhavishyaMaheshwari](https://github.com/BhavishyaMaheshwari)
- **MEMBER_3** - Gauransh Gupta - 2024UCA1944 - [@GauranshGupta](https://github.com/GauranshGupta)
- **MEMBER_4** - Arnav Gupta - 2024UCA1885 - [@Arngithub407](https://github.com/Arngithub407)
- **MEMBER_5** - Joseph Jisso Aliyath - 2024UCA1957 - [@JosephJisso](https://github.com/JosephJisso)
- **MEMBER_6** - Srishti - 2024UCA1923 - [@OfEarthAndEther](https://github.com/OfEarthAndEther)

## Project Links

- **SIH Presentation:** [Presentation Link](https://drive.google.com/file/d/1Qq5v3u2YMuWnnH4N_ouCfDOluo1rIoEf/view)
- **Video Demonstration:** [Video Link](https://youtu.be/hF7HQPb_-J4)

## Overview

RailOptimus optimizes train scheduling under disturbances (delays, track blockages) by decomposing the complex optimization problem into two stages:

- **Stage 1**: Fast approximate solutions (routes and train precedence)
- **Stage 2**: Full rescheduling with safety constraints (overlaps, block sections)

This decomposition enables real-time decision support for railway dispatchers managing conflict resolution in seconds rather than minutes.

**Key Results**: 27–78% runtime reduction, max 17% solution quality overhead.

## Features

- Real-time rerouting, reordering, and rescheduling
- Safety-critical overlap modeling (braking distance beyond signals)
- Multi-objective support (delay, energy, passenger satisfaction)
- Multiple solver backends (Gurobi, CPLEX, CBC)
- Extensible architecture (integration with ATO, energy optimization)

## Requirements

- **MATLAB** R2020a+ or **Python 3.8+**
- MILP Solver: Gurobi (recommended) or CPLEX/CBC
- 4 GB RAM minimum; 8+ GB recommended for large networks

## Implementation Details

### File Structure

```
RailOptimus/
├── src/
│ ├── core/
│ │ ├── network_model.m # Graph representation
│ │ ├── train_model.m # Train dynamics
│ │ └── constraint_builder.m # Constraint generation (Alg. 1–5)
│ ├── stage1/
│ │ ├── suboptimal_model.m # Stage 1A implementation
│ │ └── global_optimum_model.m # Stage 1B implementation
│ ├── stage2/
│ │ ├── full_milp.m # Complete extended model
│ │ └── overlap_constraints.m # Alg. 3 implementation
│ ├── solvers/
│ │ ├── gurobi_interface.m # Gurobi MILP solver wrapper
│ │ ├── cplex_interface.m # CPLEX wrapper
│ │ └── cbc_interface.m # CBC wrapper
│ ├── analysis/
│ │ ├── solution_analyzer.m # Delay, energy, conflict analysis
│ │ ├── optimality_checker.m # Verify global optimum
│ │ └── benchmark.m # Performance evaluation
│ └── visualization/
│ ├── network_plotter.m # Network rendering
│ ├── timeline_plotter.m # Space-time diagram
│ └── solution_viewer.m # Interactive visualization
├── networks/
│ ├── example_network.json # Test network definitions
│ └── industrial_network.json # Large-scale example
├── trains/
│ ├── example_trains.json # Sample train sets
│ └── test_scenarios.json # Disturbance scenarios
├── tests/
│ ├── unit_tests/
│ │ ├── test_constraints.m
│ │ └── test_solvers.m
│ └── integration_tests/
│ ├── test_stage1.m
│ └── test_stage2.m
├── docs/
│ ├── ALGORITHM.md # Detailed algorithm description
│ ├── USER_GUIDE.md # End-user documentation
│ └── PAPER.pdf # Reference paper (Lindenmaier et al.)
└── README.md
```    

## Quick Start

### MATLAB
    
```
network = railoptimus_load_network('networks/example.json');
trains = railoptimus_load_trains('trains/example.json');

config = struct('stage1_model', 'global_optimum', 'overlap_safety', true);
optimizer = RTRTMPOptimizer(network, trains, config);
solution = optimizer.solve(initial_state);

fprintf('Total delay: %.1f min | Runtime: %.2f s\n', ...
solution.objective/60, solution.runtimes.stage1 + solution.runtimes.stage2);
railoptimus_visualize_solution(network, solution);
```

### Python

```
from railoptimus import RTRTMPOptimizer, Network

network = Network.from_json('networks/example.json')
trains = TrainSet.from_json('trains/example.json')

optimizer = RTRTMPOptimizer(network, trains)
solution = optimizer.solve(initial_state={...})

print(f"Total delay: {solution.objective:.0f} s | Runtime: {solution.runtimes.total:.2f} s")
solution.visualize()
```

## Installation
    
```
git clone https://github.com/OfEarthAndEther/RailOptimus.git
cd RailOptimus
```    

### MATLAB: Add paths and run setup    
```
addpath(genpath('./src'))
```

### Python    
```
pip install -e .
```


## Configuration

Edit `config/railoptimus_config.json`:

```
{
"stage1_model": "global_optimum",
"overlap_safety": true,
"solver": "gurobi",
"max_solver_time": 60,
"objective": "weighted_delay"
}
```


## Performance

| Scenario | Stage 1 Time | Stage 2 Time | Total Time | Speedup |
|----------|--------------|--------------|------------|----------|
| 4-train, 17-circuit | 3.8 s | 6.9 s | 10.7 s | 23x |
| Reference (no decomposition) | — | — | 244 s | — |

Max objective overhead: 17% (with overlaps); 0% (without overlaps).

## Documentation

- **Algorithm Details**: See [research paper](https://arxiv.org/pdf/2209.12689)
- **User Guide**: `docs/USER_GUIDE.md`
- **API Reference**: `docs/API.md`

## Safety

Explicit modeling of:
- Track circuit reservations
- Block section FIFO constraints
- Overlap zones (braking distance beyond signals)
- Signal constraints (formation time, release time)

Compliant with RTIG, RU-4000, EN 50128 railway standards.

## Contributing

### Code Contribution Process

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Implement changes; ensure code follows [MATLAB/Python style guide]
4. Add unit tests (minimum 80% coverage for new code)
5. Run existing test suite: `runtests` (MATLAB) or `pytest` (Python)
6. Submit pull request with detailed description

### Bug Reports

GitHub Issues: https://github.com/OfEarthAndEther/RailOptimus/issues

## Citation

```
@article{Lindenmaier2022,
title={Efficient Real-time Rail Traffic Optimization},
author={Lindenmaier, László and Lövétei, István and Aradi, Szilárd},
journal={arXiv preprint arXiv:2209.12689},
year={2022}
}
```

## License

MIT License. See LICENSE file.

## Support

- Issues: https://github.com/OfEarthAndEther/RailOptimus/issues
- Discussions: https://github.com/OfEarthAndEther/RailOptimus/discussions
