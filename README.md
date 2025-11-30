# Setup RSCLI Action

A GitHub Action to install and set up [RSCLI](https://github.com/reysys-technology/rscli), a command-line tool for interacting with Reysys services.

## Features

- Installs RSCLI with a specified version or latest
- Optionally sets up Go environment
- Outputs the installed RSCLI version
- Cross-platform support (Linux, macOS, Windows)

## Usage

### Basic Usage

```yaml
- name: Setup RSCLI
  uses: reysys-technology/setup-rscli-action@v1
```

This will install the latest version of RSCLI with Go 1.24.

### Specify RSCLI Version

```yaml
- name: Setup RSCLI
  uses: reysys-technology/setup-rscli-action@v1
  with:
    version: '1.0.3'
```

### Custom Go Version

```yaml
- name: Setup RSCLI
  uses: reysys-technology/setup-rscli-action@v1
  with:
    version: 'latest'
    go-version: '1.24'
```

### Skip Go Installation

If Go is already set up in your workflow:

```yaml
- name: Setup Go
  uses: actions/setup-go@v6
  with:
    go-version: '1.24'

- name: Setup RSCLI
  uses: reysys-technology/setup-rscli-action@v1
  with:
    skip-go-install: 'true'
```

### Using the Version Output

```yaml
- name: Setup RSCLI
  id: setup-rscli
  uses: reysys-technology/setup-rscli-action@v1

- name: Display RSCLI Version
  run: echo "Installed RSCLI version: ${{ steps.setup-rscli.outputs.rscli-version }}"
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `version` | Version of RSCLI to install | No | `latest` |
| `skip-go-install` | Skip Go installation (set to `true` if Go is already set up) | No | `false` |
| `go-version` | Go version to install (ignored if `skip-go-install` is `true`) | No | `1.24` |

## Outputs

| Output | Description |
|--------|-------------|
| `rscli-version` | The installed version of RSCLI |

## Examples

### Complete Workflow Example

```yaml
name: Deploy with RSCLI

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup RSCLI
        id: setup-rscli
        uses: reysys-technology/setup-rscli-action@v1
        with:
          version: 'latest'

      - name: Verify Installation
        run: rscli --version

      - name: Deploy Application
        run: rscli deploy
        env:
          RS_SECRET_ID: ${{ secrets.RS_SECRET_ID }}
          RS_SECRET: ${{ secrets.RS_SECRET }}
          RS_BASE_URL: ${{ secrets.RS_BASE_URL }}
```

## Requirements

- GitHub Actions runner (Linux, macOS, or Windows)
- Go 1.24 or higher (installed automatically unless `skip-go-install` is `true`)

## Support

For issues and questions:
- RSCLI Issues: [github.com/reysys-technology/rscli/issues](https://github.com/reysys-technology/rscli/issues)
- Action Issues: [github.com/reysys-technology/setup-rscli-action/issues](https://github.com/reysys-technology/setup-rscli-action/issues)

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
