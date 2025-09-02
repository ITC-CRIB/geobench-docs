# Jupyter Notebook Benchmarking

GeoBench supports benchmarking directly within Jupyter notebooks, allowing you to benchmark Python code interactively. This is particularly useful for data science workflows, geospatial analysis, and exploratory benchmarking where you want to iterate and test different approaches.

## Overview

Jupyter notebook benchmarking in GeoBench provides:

- **Interactive benchmarking** within Jupyter notebook cells
- **Real-time monitoring** of system resources during execution
- **Automatic report generation** with performance metrics
- **Flexible API** with multiple usage patterns
- **Integration** with existing notebook workflows

## Usage Methods

There are three ways to use Jupyter benchmarking functionality in GeoBench:

### 1. Using the JupyterBenchmark Class

The `JupyterBenchmark` class provides full control over the benchmarking process:

```python
from geobench.jupyter import JupyterBenchmark

# Create a benchmark instance
bench = JupyterBenchmark(
    name="my-benchmark",
    outdir="results",
    run_monitor=2.0,  # Monitor every 2 seconds
    clean=True        # Clean output directory before running
)

# Start benchmarking
bench.start("my-function")

# Run your code to benchmark
result = my_function()

# Finish benchmarking
bench.finish(True)  # Pass True for success, False for failure

# Generate HTML report
bench.generate_report()
```

#### Parameters

- `name`: Name of the benchmark (used for output directory and report)
- `outdir`: Output directory for results and reports
- `run_monitor`: Monitoring interval in seconds (optional)
- `clean`: Whether to clean the output directory before running

#### Methods

- `start(operation_name)`: Start monitoring and timing for an operation
- `finish(success)`: Stop monitoring and record the result
- `generate_report()`: Generate an HTML report with results

### 2. Using the Benchmark Decorator

The decorator provides a simpler way to benchmark individual functions:

```python
from geobench.jupyter import benchmark

@benchmark(name="my-function-benchmark", outdir="results", clean=True)
def my_function():
    # Your code here
    import time
    time.sleep(2)  # Simulate some work
    return "result"
    
# Call the decorated function
result = my_function()
```

The decorator automatically:
- Creates a benchmark instance
- Starts monitoring before function execution
- Stops monitoring after function completion
- Generates a report with the results

#### Decorator Parameters

- `name`: Name of the benchmark
- `outdir`: Output directory for results
- `clean`: Whether to clean output directory
- `run_monitor`: Monitoring interval in seconds

### 3. Using Context Manager (Alternative Pattern)

You can also use the JupyterBenchmark class as a context manager:

```python
from geobench.jupyter import JupyterBenchmark

with JupyterBenchmark(name="context-benchmark", outdir="results") as bench:
    bench.start("processing")
    
    # Your code here
    result = process_data()
    
    bench.finish(True)
```

## Example: Benchmarking Geospatial Operations

Here's a complete example of benchmarking a geospatial operation in a Jupyter notebook:

```python
# Cell 1: Import libraries
import geopandas as gpd
import pandas as pd
from shapely.geometry import Point
from geobench.jupyter import benchmark

# Cell 2: Create sample data
def create_sample_data(n_points=10000):
    """Create sample point data for benchmarking"""
    import numpy as np
    
    # Generate random points
    x = np.random.uniform(-180, 180, n_points)
    y = np.random.uniform(-90, 90, n_points)
    
    # Create GeoDataFrame
    geometry = [Point(xi, yi) for xi, yi in zip(x, y)]
    gdf = gpd.GeoDataFrame({'id': range(n_points)}, geometry=geometry)
    gdf.crs = "EPSG:4326"
    
    return gdf

# Cell 3: Benchmark buffer operation
@benchmark(name="buffer-operation", outdir="benchmark_results")
def benchmark_buffer_operation():
    # Create sample data
    gdf = create_sample_data(50000)
    
    # Perform buffer operation
    buffered = gdf.to_crs("EPSG:3857").buffer(1000)  # 1km buffer
    
    return len(buffered)

# Cell 4: Run the benchmark
result = benchmark_buffer_operation()
print(f"Processed {result} features")
```

## Advanced Example: Parameter Sweeping

You can manually implement parameter sweeping in Jupyter notebooks:

```python
from geobench.jupyter import JupyterBenchmark
import time

# Parameters to test
buffer_distances = [100, 500, 1000, 5000]
point_counts = [1000, 5000, 10000]

# Run benchmarks for different parameter combinations
for n_points in point_counts:
    for distance in buffer_distances:
        bench_name = f"buffer-{n_points}pts-{distance}m"
        
        bench = JupyterBenchmark(
            name=bench_name,
            outdir="parameter_sweep_results",
            clean=True
        )
        
        bench.start(f"buffer_{distance}m")
        
        # Create data and run operation
        gdf = create_sample_data(n_points)
        buffered = gdf.to_crs("EPSG:3857").buffer(distance)
        
        bench.finish(True)
        bench.generate_report()
        
        print(f"Completed: {n_points} points, {distance}m buffer")
```

## Understanding the Output

When running Jupyter benchmarks, GeoBench creates:

1. **Output directory**: Named after your benchmark
2. **Performance data**: JSON files with timing and resource usage metrics
3. **HTML reports**: Visual reports showing performance characteristics
4. **System monitoring data**: CPU, memory, and I/O usage during execution

### Report Contents

The generated HTML reports include:

- **Execution summary**: Total time, success/failure status
- **Resource usage graphs**: CPU, memory, disk I/O over time
- **Performance metrics**: Min, max, average resource usage
- **System information**: Hardware and software environment details

## Best Practices

### 1. Meaningful Benchmark Names
Use descriptive names that include key parameters:

```python
bench_name = f"ndvi-calculation-{tile_size}x{tile_size}-{band_count}bands"
```

### 2. Clean Output Directories
Set `clean=True` to ensure fresh results:

```python
@benchmark(name="my-test", clean=True)
def my_function():
    # ...
```

### 3. Handle Errors Gracefully
When using the class directly, handle potential errors:

```python
bench = JupyterBenchmark(name="error-handling-example")
bench.start("risky-operation")

try:
    result = risky_operation()
    bench.finish(True)
except Exception as e:
    print(f"Error: {e}")
    bench.finish(False)
```

### 4. Monitor Resource Usage
Use appropriate monitoring intervals based on operation duration:

```python
# For long operations (>1 minute)
bench = JupyterBenchmark(name="long-op", run_monitor=5.0)

# For short operations (<30 seconds)
bench = JupyterBenchmark(name="short-op", run_monitor=0.5)
```

## Integration with Existing Workflows

Jupyter benchmarking integrates seamlessly with existing data science and geospatial workflows:

- **Data exploration**: Benchmark different data loading strategies
- **Algorithm comparison**: Compare performance of different algorithms
- **Parameter optimization**: Find optimal parameters for processing
- **Scalability testing**: Test performance with different data sizes
- **Resource profiling**: Understand resource requirements for operations

This makes GeoBench's Jupyter support ideal for research, development, and optimization of geospatial processing workflows.
