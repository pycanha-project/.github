# 🔥 Pycanha Project

<div align="center">

**High-Performance Thermal Analysis Tool**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![C++23](https://img.shields.io/badge/C%2B%2B-23-blue.svg)](https://en.cppreference.com/w/cpp/23)
[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/)

*Fast and flexible thermal mathematical modeling focused on spacecraft thermal analysis*

</div>

---

## 🌟 Overview

**Pycanha** is a modern, high-performance thermal analysis toolkit designed mainly for engineers and researchers working on the spacecraft thermal control field. 
Built with a powerful C++ core and intuitive Python interfaces, Pycanha enables efficient simulation of thermal systems with both conductive and radiative heat transfer.

Although is Pycanha is intended for spacecraft thermal analysis, in the end is a thermal lumped network analysis tool so it can be used in a wide range of thermal problems where this technique is suitable.

> **⚠️ Development Status**: Pycanha is currently in active pre-release development (not even beta phase yet). The core functionality is operational, but the API will change and some of the capabilities listed below are still being implemented.

### Key Features

- ⚡ **High-Performance Computing**: Optimized core with modern C++ features
- 🔬 **Advanced Solvers**: Multiple solvers with optional with optional support for Intel MKL for maximum perfomance
  - Steady-state
  - Transient
  - Jacobian propagation methods for sensitivity analysis
- 🌡️ **Comprehensive Thermal Modeling**: Conductive and radiative heat transfer based on lumped models
- 🐍 **Python-First Interface**: Easy-to-use Python API while maintaining C++ performance
- 🔧 **Flexible Architecture**: Extensible design with Python
- 📊 **Industry Compatibility**: Support for ESATAN files and STEP-TAS models
- 🚀 **GPU Ray-Tracing Capabilities**: GPU-accelerated calculation of View Factors (VF) and Radiative Exchange Factors (REFs) *(coming soon)*

---

## 📦 Project Structure

The Pycanha project consists of three interconnected repositories:

### [pycanha-core](https://github.com/pycanha-project/pycanha-core)
The high-performance C++ core library containing:
- Thermal network data structures
- Sparse matrix operations
- Advanced numerical solvers
- Core algorithms for thermal analysis

[![CI](https://github.com/pycanha-project/pycanha-core/actions/workflows/ci.yml/badge.svg)](https://github.com/pycanha-project/pycanha-core/actions/workflows/ci.yml)
[![codecov](https://codecov.io/gh/pycanha-project/pycanha-core/graph/badge.svg?token=XZRHKH2G8I)](https://codecov.io/gh/pycanha-project/pycanha-core)
[![docs](https://img.shields.io/badge/doc-GitHub%20Pages-blue)](https://pycanha-project.github.io/pycanha-core/)

### [pycanha-core-python](https://github.com/pycanha-project/pycanha-core-python)
Python bindings for pycanha-core using modern binding techniques:
- Direct C++ integration via pybind11
- Type-safe Python interfaces
- Efficient data exchange between Python and C++

[![Build and publish wheels](https://github.com/pycanha-project/pycanha-core-python/actions/workflows/build-publish-wheels.yml/badge.svg)](https://github.com/pycanha-project/pycanha-core-python/actions/workflows/build-publish-wheels.yml)
[![Documentation Status](https://readthedocs.org/projects/pycanha-core-python/badge/?version=latest)](https://pycanha-core-python.readthedocs.io/latest/?badge=latest)

### [pycanha](https://github.com/pycanha-project/pycanha) *(Coming Soon)*
Pure Python layer extending pycanha-core-python with:
- High-level thermal modeling abstractions
- I/O utilities and file format support
- Visualization and post-processing tools
- User-friendly workflows for common analysis tasks

---

## 🚀 Quick Start - Steaty-state analysis

```python
import pycanha

# Create a thermal mathematical model
tmm = pycanha.ThermalMathematicalModel("MyModel")

# Define thermal nodes
tmm.add_node(pycanha.Node(1, nodetype="B"))  # Boundary node
tmm.add_node(pycanha.Node(2, nodetype="D"))  # Diffusive node

# Add conductive couplings between nodes
tmm.conductive_couplings.add_coupling(1, 2, coupling_value=1.0)

# Set boundary temperature
tmm.nodes.set_T(1, 300.0)

# Set dissipation
tmm.nodes.set_qi(2, 10)

# Solve steady-state
solver = tmm.steady_state_solvers.sslu
solver.initialize()
solver.solve()

# Access results
for node in tmm.nodes:
    print(f"Node {node.node_number}: {node.T} K")
```

---

## 🎯 Use Cases

- **Spacecraft Thermal Design**
- **VF and REFs calculation**
- **Parametric analysis**
- **Post-processing**
- **Research & Education**

---

## 🛠️ Technical Highlights

- **Modern C++23** implementation for maximum performance
- **Eigen** library for efficient sparse matrix operations
- **Intel MKL** integration (optional) for optimized linear algebra
- **pybind11** for seamless Python-C++ integration
- **CMake** build system with Conan package management
- Comprehensive **unit testing** with high code coverage
- **CI/CD** pipelines ensuring code quality
- Extensive **documentation** with API references and examples

---

## 📖 Documentation

- **pycanha-core**: [GitHub Pages](https://pycanha-project.github.io/pycanha-core/)
- **pycanha-core-python**: [Read the Docs](https://pycanha-core-python.readthedocs.io/)

---

## 🤝 Contributing

Pycanha is still in a development phase. After releasing the version 1.0, contributions will be welcomed!

---

## 📄 License

All Pycanha project repositories are released under the [MIT License](LICENSE), making them free for both academic and commercial use.

---

## 🌐 Community & Support

- **Issues**: Report bugs or request features in the respective repository
- **Discussions**: Share ideas and ask questions in GitHub Discussions
- **Email**: Contact the maintainers through the repository

---


<div align="center">


[Getting Started](https://github.com/pycanha-project/pycanha-core-python) • [Documentation](https://pycanha-core-python.readthedocs.io/) • [Examples](https://github.com/pycanha-project/pycanha-core-python/tree/main/examples)

</div>
