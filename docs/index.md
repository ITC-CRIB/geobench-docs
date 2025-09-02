# GeoBench

GeoBench is a tool designed to benchmark geospatial software and processing workflows. It allows users to run benchmarks based on scenarios specified in YAML files and provides detailed performance metrics and reports.

## Overview

GeoBench helps users:

- Benchmark geospatial processing operations across different tools and configurations
- Measure and compare performance across various input parameters
- Analyze system resource usage during execution
- Generate detailed reports with visualizations for better understanding performance characteristics
- Run interactive benchmarks directly in Jupyter notebooks

The tool supports benchmarking workflows implemented in:

- Python scripts
- Shell scripts/commands
- QGIS processing algorithms (via qgis-process)
- QGIS Python scripts
- Jupyter notebook environments

## Key Features

- **Flexible scenario configuration** via YAML files
- **Multiple execution modes** for different geospatial tools and languages
- **Jupyter notebook integration** for interactive benchmarking
- **Parameter sweeping** to test performance across different input values
- **System monitoring** to capture resource usage (CPU, memory, disk I/O)
- **HTML report generation** for visualization and analysis
- **Caching control** to ensure fair benchmarking

## Documentation Contents

- [Installation](installation.md) - How to install GeoBench
- [Getting Started](getting-started.md) - Quick start guide
- [Creating Scenarios](creating-scenarios.md) - How to define benchmark scenarios
- [Execution Types](execution-types.md) - Different modes of execution
- [Jupyter Benchmarking](jupyter-benchmarking.md) - Interactive benchmarking in Jupyter notebooks
- [Configuration Options](configuration-options.md) - Available settings and options
- [Analyzing Results](analyzing-results.md) - Understanding benchmark outputs
