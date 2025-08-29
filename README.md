# GeoBench Documentation

This directory contains the documentation for GeoBench, a tool for benchmarking geospatial software and processing workflows.

## Building the Documentation

The documentation is built using [MkDocs](https://www.mkdocs.org/) with the ReadTheDocs theme.

### Prerequisites

To build the documentation locally, you'll need:

```bash
pip install mkdocs mkdocs-material pymdown-extensions
```

### Development Server

To start a development server and preview the documentation:

```bash
cd geobench-docs
mkdocs serve
```

This will start a local server at http://127.0.0.1:8000/ where you can preview the documentation as you edit it.

### Building Static Site

To build the static site:

```bash
cd geobench-docs
mkdocs build
```

This will create a `site` directory containing the HTML files.

## Documentation Structure

- `docs/index.md`: Home page
- `docs/installation.md`: Installation instructions
- `docs/getting-started.md`: Quick start guide
- `docs/creating-scenarios.md`: How to create benchmark scenarios
- `docs/execution-types.md`: Different modes of execution
- `docs/configuration-options.md`: Available settings and options
- `docs/analyzing-results.md`: Understanding benchmark outputs

## Contributing to the Documentation

1. Fork the repository
2. Make your changes to the documentation
3. Submit a pull request

Please ensure your documentation changes are clear, concise, and accurate.

## Deploying to ReadTheDocs

The documentation is automatically built and deployed to ReadTheDocs when changes are pushed to the repository.
