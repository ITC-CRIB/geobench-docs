# Installation

This guide will help you install GeoBench on your system.

## Prerequisites

Before installing GeoBench, ensure you have the following:

- **Operating System**: Linux, Windows (with WSL2 or PowerShell), or macOS
- **Python**: Python 3.10 or later with pip package manager
- **QGIS**: Required only if you plan to use QGIS processing algorithms or Python scripts

## Installation Methods

### Method 1: Using pip

You can install GeoBench directly from GitHub using pip:

```bash
# Install directly from GitHub (recommended)
pip install --upgrade git+https://github.com/ITC-CRIB/geobench.git

# Or install from a local clone
git clone https://github.com/ITC-CRIB/geobench
cd geobench
pip install . --upgrade
```

### Method 2: Using uv (Recommended)

[uv](https://github.com/astral-sh/uv) is a faster and more efficient Python package installer. To install GeoBench using uv:

1. Install the uv tool:

   **For macOS and Linux:**
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

   **For Windows (from PowerShell/Terminal):**
   ```powershell
   powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
   ```

2. Install GeoBench:

   ```bash
   # Clone the repository (if not already done)
   git clone https://github.com/ITC-CRIB/geobench
   cd geobench
   
   # Install using uv
   uv tool install . --upgrade
   ```

## Development Installation

If you want to contribute to GeoBench or modify the code, you can set up a development environment:

### Using venv

```bash
# Clone the repository (if not already done)
git clone https://github.com/ITC-CRIB/geobench
cd geobench

# Create and activate a virtual environment
python3 -m venv .venv
source .venv/bin/activate  # On Windows, use `.venv\Scripts\activate`

# Install dependencies
pip install -r requirements.txt
```

### Using uv (Recommended for Development)

```bash
# Clone the repository (if not already done)
git clone https://github.com/ITC-CRIB/geobench
cd geobench

# Sync dependencies using uv
uv sync
```

## Verifying the Installation

After installation, you can verify that GeoBench is correctly installed by running:

```bash
geobench --help
```

You should see the help message describing the available command-line options.

## Running GeoBench

If you've installed GeoBench as a tool, you can run it using:

```bash
geobench scenario.yaml
```

If you're using a development installation, you can run it using:

```bash
python -m geobench.cli scenario.yaml
```
