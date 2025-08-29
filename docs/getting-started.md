# Getting Started

This guide will help you get started with GeoBench by running your first benchmark scenario.

## Basic Usage

GeoBench runs benchmarks based on scenario files written in YAML format. The basic command to run a benchmark is:

```bash
geobench path/to/scenario.yaml
```

## Running Example Scenarios

GeoBench comes with several example scenarios that you can use to get familiar with the tool. These examples are located in the `examples` directory of the repository.

### Example 1: Python Script Benchmark

To run a benchmark for a Python script that counts prime numbers:

```bash
geobench examples/count_primes_python.yaml
```

This example will:
1. Execute a Python script that counts prime numbers
2. Run with different parameters (core counts and number ranges)
3. Repeat each configuration multiple times
4. Generate a report with performance metrics

### Example 2: QGIS Process Benchmark

To run a benchmark for a QGIS processing algorithm (buffer):

```bash
geobench examples/buffer_qgis_process.yaml
```

This example will:
1. Execute the QGIS native buffer algorithm on a shapefile
2. Run with different buffer distances
3. Generate output shapefiles
4. Create a report with execution times and metrics

## Understanding the Output

When you run a benchmark, GeoBench:

1. Creates an output directory based on the scenario name
2. Executes the benchmark with the specified parameters
3. Monitors system resources during execution
4. Collects performance data and metrics
5. Generates result files (JSON) and a report (HTML)

The output directory structure typically looks like:

```
scenario-name/
├── report.html           # HTML report with charts and tables
├── result.json           # Overall benchmark results
└── set_1/               
    ├── run_1/
    │   ├── result.json   # Individual run results
    │   ├── summary.json  # Run summary
    │   └── output.shp    # (If applicable) Output files
    └── run_2/
        ├── result.json
        ├── summary.json
        └── output.shp
```

## Viewing the Report

After running a benchmark, you can view the HTML report in any web browser. The report includes:

- System information
- Performance metrics and statistics
- Charts comparing different parameter sets
- Resource usage graphs

## Command-Line Options

GeoBench provides several command-line options to customize benchmark execution:

```
geobench [options] scenario.yaml
```

Options:
- `-n, --name`: Override the scenario name
- `-r, --repeat`: Override the number of repetitions
- `-o, --outdir`: Specify an output directory
- `-c, --clean`: Clean the output directory before running
- `-d, --debug`: Enable debug logging

## Next Steps

Now that you're familiar with the basics of GeoBench, you can:

1. Create your own [benchmark scenarios](creating-scenarios.md)
2. Explore different [execution types](execution-types.md)
3. Learn about advanced [configuration options](configuration-options.md)
4. Understand how to [analyze results](analyzing-results.md)
