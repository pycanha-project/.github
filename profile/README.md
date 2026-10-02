# Pycanha Project

**pycanha** is an open-source thermal analysis tool for spacecraft. It models a system as a
lumped-parameter thermal network made of nodes with capacities and heat loads, joined by
conductive and radiative couplings. It also builds that network from geometry, with a
geometrical model, a conduction builder and GPU ray tracing for view factors and radiative
exchange factors.

All the numerical work runs in a C++23 core and is driven from Python. Sparse linear
algebra uses Eigen and Intel MKL, and radiation is computed on the GPU, so large models can
be built and solved from a Python script. The project follows current standards and
tracks the latest releases of its dependencies.

The method is not specific to spacecraft, so pycanha works for any problem that a lumped
thermal network describes well.

**Status:** pre-1.0, currently at version 0.22. The features below work and are tested,
but the API still changes between minor versions.

## What it does

**Thermal network**
- Nodes with temperature, capacity and heat loads, plus boundary nodes at a fixed
  temperature.
- Conductive and radiative couplings.
- Bulk construction from NumPy arrays, passed to C++ in a single call.
- Parameters and formulas on node and coupling attributes.
- Python callbacks during a solve, for control logic such as thermostats and heater
  switching.

**Solvers**
- Steady state, with sparse direct solvers from Eigen or Intel MKL (PARDISO).
- Transient, using Crank–Nicolson with linearised radiation.
- A transient variant that also outputs Jacobians with respect to model parameters, for
  sensitivity analysis.

**Geometry**
- STEP-TAS primitives: Triangle, rectangle, quadrilateral, disc, cylinder, cone, sphere and paraboloid; with transformations and an assembly tree.
- Cutting engine.
- Building the conductive network (nodes, capacities, in-plane and through-thickness
  conductances) from each primitive's own geometry.

**Radiation**
- Monte Carlo ray tracing for view factors, multi-reflection radiative exchange factors and
  absorbed solar flux.
- Hardware ray tracing through Vulkan on Windows and Linux, and through Metal on Apple M3
  or newer. No CUDA or specific GPU vendor is required.

**Interoperability**
- Reads and writes STEP-TAS and ESATAN-TMS models.

**Visualisation**
- An interactive 3D viewer (Qt and PyVista) with a geometry tree, picking, colouring by any
  property and playback of results over time.

## Coming soon

- Parametric geometry
- Orbits, attitude and pointing
- Planetary infrared and albedo fluxes
- Improve compatibility with standar tools

## Planned

- Mission module with load-case management
- Conductive interfaces and contact conductances between geometry items
- Solid geometries
- A standalone GUI

## Example

```python
import pycanha as pc
import pycanha.tmm as pm

tm = pc.ThermalModel("QuickStart")
tmm = tm.tmm

node1 = pm.Node(1)
node1.C = 100.0      # capacity [J/K]
node1.qi = 10.0      # internal heat load [W]

node2 = pm.Node(2)
node2.type = pm.NodeType.BOUNDARY
node2.T = 300.0      # [K]

tmm.add_node(node1)
tmm.add_node(node2)
tmm.conductive_couplings.add_coupling(1, 2, 0.5)   # conductance [W/K]

solver = tm.solvers.sslu
solver.initialize()
solver.solve()

print(tmm.nodes.get_T(1))   # 320.0
```

More examples, including geometry, cutting, ESATAN-TMS import and parametric analysis, are
in the [examples gallery](https://pycanha.readthedocs.io/en/latest/auto_examples/index.html).

## Installation

```bash
pip install pycanha
```

pycanha requires Python 3.13 or later, on Windows, Linux or macOS (Apple Silicon). The
Windows and Linux wheels include Intel MKL, and the macOS wheels do not.

## Repositories

The project has three repositories. Most users only need `pycanha`.

### [pycanha](https://github.com/pycanha-project/pycanha)

[![PyPI](https://img.shields.io/pypi/v/pycanha)](https://pypi.org/project/pycanha/)
[![Tests](https://github.com/pycanha-project/pycanha/actions/workflows/ci-test.yml/badge.svg)](https://github.com/pycanha-project/pycanha/actions/workflows/ci-test.yml)
[![Quality](https://github.com/pycanha-project/pycanha/actions/workflows/ci-quality.yml/badge.svg)](https://github.com/pycanha-project/pycanha/actions/workflows/ci-quality.yml)
[![docs](https://readthedocs.org/projects/pycanha/badge/?version=latest)](https://pycanha.readthedocs.io/en/latest/)

The Python package for users. It provides `ThermalModel`, the ESATAN-TMS and STEP-TAS
readers and writers, the viewer and the Python-side model API. It is pure Python.

### [pycanha-core](https://github.com/pycanha-project/pycanha-core)

[![CI](https://github.com/pycanha-project/pycanha-core/actions/workflows/ci.yml/badge.svg)](https://github.com/pycanha-project/pycanha-core/actions/workflows/ci.yml)
[![Code Checks](https://github.com/pycanha-project/pycanha-core/actions/workflows/code-checks.yml/badge.svg)](https://github.com/pycanha-project/pycanha-core/actions/workflows/code-checks.yml)
[![codecov](https://codecov.io/gh/pycanha-project/pycanha-core/graph/badge.svg?token=XZRHKH2G8I)](https://codecov.io/gh/pycanha-project/pycanha-core)
[![docs](https://img.shields.io/badge/doc-GitHub%20Pages-blue)](https://pycanha-project.github.io/pycanha-core/)

The C++23 library with the thermal network, the solvers, the geometry model, the conduction
builder and the ray-tracing engine (Slang kernels compiled to SPIR-V and Metal). It uses
Eigen, optionally Intel MKL, and Manifold, and is built with Conan and CMake. It has no
Python dependency and can be used on its own.

### [pycanha-core-python](https://github.com/pycanha-project/pycanha-core-python)

[![PyPI](https://img.shields.io/pypi/v/pycanha-core)](https://pypi.org/project/pycanha-core/)
[![Build and publish wheels](https://github.com/pycanha-project/pycanha-core-python/actions/workflows/build-publish-wheels.yml/badge.svg)](https://github.com/pycanha-project/pycanha-core-python/actions/workflows/build-publish-wheels.yml)
[![Documentation Status](https://readthedocs.org/projects/pycanha-core-python/badge/?version=latest)](https://pycanha-core-python.readthedocs.io/latest/?badge=latest)

The nanobind bindings, published on PyPI as `pycanha-core` and imported as `pycanha_core`.
They expose the C++ API one-to-one, and `pycanha` builds on them. A binding release always
has the same `major.minor` version as the core release it wraps.

## Documentation

- pycanha: <https://pycanha.readthedocs.io/>
- pycanha-core (C++ API): <https://pycanha-project.github.io/pycanha-core/>
- pycanha-core-python: <https://pycanha-core-python.readthedocs.io/>

## Contributing

Bug reports and feature requests are welcome as issues in the relevant repository. Pull
requests will be accepted once the API is stable at 1.0.

## License

MIT. See the `LICENSE` file in each repository.
