# Creating Scenarios

This guide explains how to create benchmark scenarios for GeoBench using YAML files.

## Scenario Structure

A GeoBench scenario is defined in a YAML file with the following structure:

```yaml
name: Scenario Name
type: execution-type
command: command-to-execute
arguments:
  param1: value1
  param2:
    - value2a
    - value2b
inputs:
  input1: path/to/input1
  input2: path/to/input2
outputs:
  output1: path/to/output1
repeat: 2
run_wait: 2.0
run_monitor: 5.0
```

## Required Fields

Every scenario must include the following fields:

- `name`: A descriptive name for the scenario
- `type`: The execution type (python, shell, qgis-process, or qgis-python)
- `command`: The command to execute (can be a script file, QGIS algorithm, etc.)

## Parameter Sweeping with Arguments

The `arguments` field allows you to specify parameters for your benchmark. You can provide:

- Single values: `param: value`
- Multiple values: 
  ```yaml
  param:
    - value1
    - value2
    - value3
  ```

GeoBench will run benchmarks with all combinations of parameters. For example, if you specify:

```yaml
arguments:
  cores:
    - 4
    - 8
  num:
    - 1000000
    - 2000000
```

GeoBench will run benchmarks with:
1. cores=4, num=1000000
2. cores=4, num=2000000
3. cores=8, num=1000000
4. cores=8, num=2000000

## Inputs and Outputs

For benchmarks that process files:

- `inputs`: Specifies input files (can be a string, list, or dictionary)
- `outputs`: Specifies output files (can be a string, list, or dictionary)

Examples:

```yaml
# Simple input/output
inputs: input.shp
outputs: output.shp

# Multiple inputs/outputs as list
inputs:
  - input1.shp
  - input2.shp
outputs:
  - output1.shp
  - output2.shp

# Named inputs/outputs as dictionary
inputs:
  INPUT: input.shp
  OVERLAY: overlay.shp
outputs:
  OUTPUT: output.shp
```

## Repetition and Monitoring

To ensure reliable results, you can configure:

- `repeat`: Number of times to repeat each benchmark configuration
- `run_wait`: Wait time before and after each run (seconds)
- `run_monitor`: Monitoring time before and after each run (seconds)
- `system_wait`: Wait time before and after all runs (seconds)
- `system_monitor`: Monitoring time before and after all runs (seconds)

## Working Directories

You can specify directories for different aspects of the benchmark:

- `workdir`: Working directory for execution (defaults to current directory)
- `basedir`: Base directory for inputs/outputs (defaults to current directory)
- `outdir`: Output directory (defaults to sanitized scenario name)
- `venv`: Virtual environment path (optional)

## Example Scenarios

### Python Script Benchmark

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
idle_time: 2
```

### QGIS Process Benchmark

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
  area_units: m2
  ellipsoid: EPSG:7030
  SEGMENTS: 19
  END_CAP_STYLE: 0
  JOIN_STYLE: 0
  MITER_LIMIT: 2
  DISSOLVE: false
  SEPARATE_DISJOINT: false
  DISTANCE:
    - 0.001
    - 0.002
```

### Shell Script Benchmark

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

## Best Practices

1. **Use descriptive names**: Choose clear, descriptive scenario names
2. **Start small**: Begin with simple scenarios and add complexity as needed
3. **Allow warm-up time**: Use `run_wait` to ensure caches are cleared between runs
4. **Repeat benchmarks**: Use `repeat` to get statistically significant results
5. **Control parameters carefully**: Only vary parameters you're measuring
6. **Document your scenarios**: Add comments to explain complex benchmarks
