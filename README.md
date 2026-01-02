# OpenTYNDP Custom Conda Recipes

Custom conda package recipes for the OpenTYNDP project, built and published to prefix.dev.

## Repository Structure

```
.
├── .github/
│   └── workflows/
│       └── build-recipes.yml    # CI/CD workflow for building and uploading
├── recipes/
│   └── snakemake/               # Custom snakemake build
│       └── meta.yaml
├── pixi.toml                    # Development environment configuration
└── README.md
```

## Using Packages

Install packages from our custom channel:

```bash
# With pixi
pixi add --channel https://prefix.dev/open-tyndp snakemake-minimal

# With conda/mamba
conda install -c https://prefix.dev/open-tyndp snakemake-minimal
```

## Adding New Recipes

1. Create a new directory under `recipes/` with your package name
2. Add a `meta.yaml` file following conda-build conventions
3. Create a PR - the workflow will automatically build and test
4. Once merged to `main`, packages are automatically uploaded to prefix.dev

## Local Development

Build recipes locally using pixi:

```bash
# Install dependencies
pixi install

# Build a specific recipe
pixi run build recipes/snakemake

# Upload to channel (requires PREFIX_API_KEY)
pixi run upload
```

## CI/CD Workflow

The repository uses GitHub Actions to:

- **On Pull Requests**: Build changed recipes and upload as artifacts for testing
- **On Merge to Main**: Build and upload packages to prefix.dev channel

### Setup

Add the following secret to your GitHub repository:
- `PREFIX_API_KEY`: Your prefix.dev API token

Update the channel name in `.github/workflows/build-recipes.yml`:
```yaml
env:
  PREFIX_CHANNEL: your-username/your-channel
```

## Recipe Guidelines

- Use `noarch: python` or `noarch: generic` when possible for platform-independent packages
- Always include tests in `meta.yaml`
- Pin dependencies appropriately
- For git-based sources, add `setuptools-scm` to build dependencies if the package uses it

## Example: Snakemake Custom Build

The `recipes/snakemake` directory contains a custom build of Snakemake from a specific git branch:

```yaml
source:
  git_url: https://github.com/coroa/snakemake
  git_rev: fix/unstuck-windows
```

This allows us to test unreleased fixes before they're merged upstream.

## License

Individual recipes may have different licenses. Check each recipe's `meta.yaml` for license information.
