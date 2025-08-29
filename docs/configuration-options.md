# Configuration Options

GeoBench offers a wide range of configuration options to control how benchmarks are executed and monitored. This page details all available configuration options and their meanings.

## Basic Configuration

These are the fundamental options required for any benchmark scenario:

| Option | Description | Required | Default |
|--------|-------------|----------|---------|
| `name` | Descriptive name for the scenario | Yes | - |
| `type` | Execution type (`python`, `shell`, `qgis-process`, `qgis-python`) | Yes | - |
| `command` | Command or script to execute | Yes | - |
| `repeat` | Number of times to repeat each benchmark configuration | No | 1 |

## Input and Output Configuration

Options for specifying input and output files:

| Option | Description | Required | Default |
|--------|-------------|----------|---------|
| `inputs` | Input file(s) for the benchmark (string, list, or dictionary) | No | {} |
| `outputs` | Output file(s) for the benchmark (string, list, or dictionary) | No | {} |
| `arguments` | Command-line arguments (dictionary or list) | No | {} |

## Directory Configuration

Options for specifying directories:

| Option | Description | Required | Default |
|--------|-------------|----------|---------|
| `workdir` | Working directory for execution | No | Current directory |
| `basedir` | Base directory for relative paths | No | Current directory |
| `outdir` | Output directory for benchmark results | No | Sanitized scenario name |
| `venv` | Path to virtual environment | No | None |

## Timing and Monitoring Configuration

Options for controlling timing and monitoring:

| Option | Description | Required | Default |
|--------|-------------|----------|---------|
| `run_wait` | Wait time before and after each run (seconds) | No | 0.0 |
| `run_monitor` | Monitoring time before and after each run (seconds) | No | 0.0 |
| `system_wait` | Wait time before and after all runs (seconds) | No | 0.0 |
| `system_monitor` | Monitoring time before and after all runs (seconds) | No | 0.0 |

## Command Line Overrides

GeoBench allows certain options to be overridden via command-line arguments:

```bash
geobench scenario.yaml --name "New Name" --repeat 3 --outdir custom-output
```

| CLI Option | Description |
|------------|-------------|
| `-n, --name` | Override the scenario name |
| `-r, --repeat` | Override the number of repetitions |
| `-o, --outdir` | Specify a custom output directory |
| `-c, --clean` | Clean the output directory before running |
| `-d, --debug` | Enable debug logging |

## Configuration Examples

### Minimal Configuration

```yaml
name: Simple Benchmark
type: python
command: script.py
```

### Comprehensive Configuration

```yaml
name: Comprehensive Benchmark
type: python
command: script.py
arguments:
  cores:
    - 4
    - 8
  num:
    - 1000000
    - 2000000
inputs:
  INPUT: data/input.shp
outputs:
  OUTPUT: output.shp
repeat: 3
run_wait: 5.0
run_monitor: 3.0
system_wait: 10.0
system_monitor: 5.0
workdir: /path/to/working/directory
basedir: /path/to/base/directory
outdir: custom-output-directory
venv: /path/to/virtual/environment
```

## Configuration Best Practices

1. **Use descriptive names** that clearly identify what's being benchmarked
2. **Set appropriate wait times** to allow system caches to settle between runs
3. **Use monitoring times** proportional to the expected runtime of your benchmark
4. **Choose repetition counts** based on the variability of your benchmark
5. **Organize input and output files** consistently
6. **Use absolute paths** when possible to avoid path resolution issues
7. **Document any special requirements** in the scenario file using comments

## Advanced Configuration Techniques

### Parameter Sweeping

Use lists of parameter values to automatically run benchmarks with all combinations:

```yaml
arguments:
  param1:
    - value1
    - value2
  param2:
    - valueA
    - valueB
```

This will generate 4 benchmark configurations (value1+valueA, value1+valueB, value2+valueA, value2+valueB).

### Environment Configuration

Use a virtual environment for Python benchmarks to ensure consistent dependencies:

```yaml
venv: /path/to/venv
```

### Output Organization

Use a custom output directory to organize results from related benchmarks:

```yaml
outdir: benchmark-results/algorithm-comparison
```
