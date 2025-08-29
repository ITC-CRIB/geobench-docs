# Execution Types

GeoBench supports multiple execution types to benchmark different kinds of geospatial processing workflows. This page describes each execution type and how to use them.

## Overview

GeoBench supports the following execution types:

1. `python` - Executes Python scripts
2. `shell` - Executes shell commands or scripts
3. `qgis-process` - Executes QGIS processing algorithms via the command line
4. `qgis-python` - Executes Python scripts in a QGIS Python environment

## Python Execution (`python`)

The `python` execution type runs Python scripts directly.

### Configuration

```yaml
name: Python Benchmark
type: python
command: script.py
arguments:
  arg1: value1
  arg2: 
    - value2a
    - value2b
```

### How It Works

- `command`: Path to a Python script (relative to `workdir` or absolute)
- `arguments`: Command-line arguments passed to the script
- Arguments are passed as: `python script.py --arg1=value1 --arg2=value2a`

### Example

```yaml
name: Count Primes Python
type: python
command: count_primes.py
arguments:
  cores:
    - 4
    - 8
  num:
    - 1000000
    - 2000000
repeat: 2
```

This will execute the `count_primes.py` script with different combinations of `--cores` and `--num` arguments.

### Sample Python Script

```python
import argparse

def main():
    parser = argparse.ArgumentParser()
    parser.add_argument('-c', '--cores', type=int, default=4)
    parser.add_argument('--n', '--num', type=int, default=1000000)
    args = parser.parse_args()
    
    # Benchmark code here
    print(f"Using {args.cores} cores, checking numbers up to {args.n}.")
    
if __name__ == "__main__":
    main()
```

## Shell Execution (`shell`)

The `shell` execution type runs commands or scripts in the system shell.

### Configuration

```yaml
name: Shell Benchmark
type: shell
command: script.sh
arguments:
  arg1: value1
  arg2:
    - value2a
    - value2b
```

### How It Works

- `command`: Shell command or script to execute (relative to `workdir` or absolute)
- `arguments`: Command-line arguments passed to the command
- Arguments are passed as: `./script.sh --arg1=value1 --arg2=value2a`

### Example

```yaml
name: Count Primes Shell
type: shell
command: count_primes.bat
arguments:
  cores:
    - 4
    - 8
  num:
    - 1000000
    - 2000000
repeat: 2
```

## QGIS Process Execution (`qgis-process`)

The `qgis-process` execution type runs QGIS processing algorithms via the command line interface.

### Configuration

```yaml
name: QGIS Process Benchmark
type: qgis-process
command: algorithm_name
inputs:
  INPUT_NAME: path/to/input
outputs:
  OUTPUT_NAME: path/to/output
parameters:
  param1: value1
  param2:
    - value2a
    - value2b
```

### How It Works

- `command`: Name of the QGIS processing algorithm (e.g., `native:buffer`)
- `inputs`: Input files for the algorithm
- `outputs`: Output files to be generated
- `parameters`: Algorithm-specific parameters

### Example

```yaml
name: Buffer QGIS Process
type: qgis-process
command: native:buffer
repeat: 1
inputs:
  INPUT: data/enschede/point.shp
outputs:
  OUTPUT: output.shp
parameters:
  distance_units: meters
  DISTANCE:
    - 0.001
    - 0.002
  SEGMENTS: 19
  END_CAP_STYLE: 0
  JOIN_STYLE: 0
  MITER_LIMIT: 2
  DISSOLVE: false
```

## QGIS Python Execution (`qgis-python`)

The `qgis-python` execution type executes Python scripts in a QGIS Python environment, providing access to PyQGIS libraries and functionality.

### Configuration

```yaml
name: QGIS Python Benchmark
type: qgis-python
command: script.py
arguments:
  arg1: value1
  arg2:
    - value2a
    - value2b
```

### How It Works

- `command`: Path to a Python script (relative to `workdir` or absolute)
- `arguments`: Command-line arguments passed to the script
- The script is executed in a QGIS Python environment with access to PyQGIS libraries
- Arguments are passed as: `qgis_process run python script.py --arg1=value1 --arg2=value2a`

### Example

```yaml
name: Buffer QGIS Python
type: qgis-python
command: buffer.py
arguments:
  input: data/enschede/point.shp
  output: output.shp
  distance:
    - 0.001
    - 0.002
repeat: 2
```

### Sample QGIS Python Script

```python
import argparse
from qgis.core import QgsApplication, QgsProcessingFeedback, QgsProcessing
from qgis.analysis import QgsNativeAlgorithms

def main():
    parser = argparse.ArgumentParser()
    parser.add_argument('--input', required=True)
    parser.add_argument('--output', required=True)
    parser.add_argument('--distance', type=float, required=True)
    args = parser.parse_args()
    
    # Initialize QGIS Application
    app = QgsApplication([], False)
    app.initQgis()
    
    # Process the buffer operation
    feedback = QgsProcessingFeedback()
    processing.runAlgorithm(
        "native:buffer",
        {
            'INPUT': args.input,
            'DISTANCE': args.distance,
            'SEGMENTS': 19,
            'END_CAP_STYLE': 0,
            'JOIN_STYLE': 0,
            'MITER_LIMIT': 2,
            'DISSOLVE': False,
            'OUTPUT': args.output
        },
        feedback=feedback
    )
    
    app.exitQgis()

if __name__ == "__main__":
    main()
```

## Choosing the Right Execution Type

- **Python**: Use for general Python scripts, especially when benchmarking Python-based geospatial libraries
- **Shell**: Use for shell scripts, command-line tools, or cross-language benchmarks
- **QGIS Process**: Use for benchmarking specific QGIS processing algorithms with different parameters
- **QGIS Python**: Use when you need PyQGIS functionality in a Python script
