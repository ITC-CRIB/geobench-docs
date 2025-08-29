# Analyzing Results

After running benchmarks with GeoBench, you'll want to analyze the results to draw meaningful conclusions. This guide explains how to interpret and analyze GeoBench results.

## Result Directory Structure

When you run a benchmark, GeoBench creates an output directory with the following structure:

```
benchmark-name/
├── report.html           # HTML report with charts and tables
├── result.json           # Overall benchmark results
└── set_N/                # For each parameter set
    └── run_M/            # For each run repetition
        ├── result.json   # Individual run results
        ├── summary.json  # Run summary
        ├── [input files] # Copied input files (if applicable)
        └── [output files]# Generated output files
```

## Understanding Result Files

### result.json

The main `result.json` file contains:

- Benchmark configuration
- System information
- Baseline and endline system metrics
- Overall benchmark results

### set_N/run_M/result.json

Each run-specific `result.json` file contains:

- Run parameters
- Execution time
- System resource usage during execution
- Start and end timestamps
- Command executed
- Return code and any output

### set_N/run_M/summary.json

The `summary.json` files contain statistical summaries of the runs:

- Minimum, maximum, and average execution times
- Resource usage statistics
- Standard deviations

## HTML Report

The `report.html` file provides a visual representation of the benchmark results:

- **Overview Section**: Shows benchmark configuration and system information
- **Performance Charts**: Displays execution times across different parameter sets
- **Resource Usage Charts**: Shows CPU, memory, and disk usage
- **Comparison Tables**: Compares metrics across different parameter combinations
- **Statistical Summary**: Provides statistical analysis of the benchmark results

## Interpreting Benchmark Results

### Performance Analysis

1. **Execution Time**: The primary metric is execution time. Look at both average times and their variability.
2. **Scaling Behavior**: Analyze how performance scales with different parameters (e.g., input sizes, core counts).
3. **Resource Usage**: Examine CPU, memory, and disk I/O to identify potential bottlenecks.
4. **Outliers**: Pay attention to runs with unusually high or low execution times.

### Common Performance Patterns

- **Linear Scaling**: Execution time increases linearly with input size
- **Diminishing Returns**: Adding more cores provides less benefit after a certain point
- **Resource Saturation**: Performance plateaus when a resource (CPU, memory, disk) is saturated
- **Cache Effects**: Unusual performance jumps when data size exceeds cache sizes

## Comparing Different Scenarios

To compare different benchmark scenarios:

1. Run multiple benchmarks with consistent configuration settings
2. Compare the generated HTML reports
3. Look for differences in execution time and resource usage
4. Consider creating custom comparison charts using the data from `result.json` files

## Command-Line Analysis

You can use standard command-line tools to extract specific metrics from result files:

```bash
# Get execution times for all runs
jq '.duration' benchmark/*/run_*/result.json

# Get average execution time
jq '.duration' benchmark/*/run_*/result.json | awk '{ sum += $1; n++ } END { print sum / n }'

# Compare CPU usage across different parameter sets
jq '.cpu_percent' benchmark/*/run_*/result.json
```

## Custom Analysis

For more advanced analysis:

1. Use the JSON files as data sources for custom scripts
2. Import the data into tools like Python with pandas and matplotlib
3. Create custom visualizations to highlight specific aspects of performance
4. Combine results from multiple benchmark runs

## Example Analysis Script

Here's a simple Python script to analyze multiple benchmark results:

```python
import json
import glob
import pandas as pd
import matplotlib.pyplot as plt
import os

# Load all result files
result_files = glob.glob('benchmark/*/run_*/result.json')
results = []

for file in result_files:
    parts = file.split('/')
    set_id = int(parts[-3].split('_')[1])
    run_id = int(parts[-2].split('_')[1])
    
    with open(file, 'r') as f:
        data = json.load(f)
        
    # Extract parameters from the path
    data['set_id'] = set_id
    data['run_id'] = run_id
    
    results.append(data)

# Convert to DataFrame
df = pd.DataFrame(results)

# Create charts
plt.figure(figsize=(10, 6))
df.groupby('set_id')['duration'].mean().plot(kind='bar')
plt.title('Average Execution Time by Parameter Set')
plt.xlabel('Parameter Set')
plt.ylabel('Time (s)')
plt.savefig('execution_time.png')
```

## Best Practices for Result Analysis

1. **Run multiple repetitions** to get statistically significant results
2. **Control for external factors** (background processes, system load)
3. **Compare similar configurations** to isolate the impact of specific parameters
4. **Look for anomalies** that might indicate measurement errors or system issues
5. **Validate findings** by rerunning important benchmarks
6. **Document the context** of your benchmark (hardware, software versions)
7. **Share all result files** when comparing with others, not just summary statistics
