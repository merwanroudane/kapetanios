# Publishing to PyPI - Step-by-Step Guide

This guide walks you through publishing the `kapetanios-test` package to PyPI (Python Package Index).

## Prerequisites

1. **Python 3.8+** installed
2. **PyPI Account**: Register at https://pypi.org/account/register/
3. **TestPyPI Account** (optional but recommended): Register at https://test.pypi.org/account/register/
4. **API Tokens**: Create API tokens for both PyPI and TestPyPI

## Installation of Build Tools

```bash
# Install/upgrade build tools
python -m pip install --upgrade pip
python -m pip install --upgrade build twine
```

## Step 1: Test the Package Locally

Before publishing, ensure everything works:

```bash
# Navigate to package directory
cd kapetanios_test

# Install in development mode
pip install -e .

# Run tests
pytest tests/

# Check if examples work
python examples/basic_examples.py
```

## Step 2: Build the Distribution

```bash
# Clean previous builds (if any)
rm -rf build/ dist/ *.egg-info

# Build the package
python -m build
```

This creates two files in the `dist/` directory:
- `kapetanios_test-1.0.0.tar.gz` (source distribution)
- `kapetanios_test-1.0.0-py3-none-any.whl` (wheel distribution)

## Step 3: Check the Distribution

```bash
# Check the distributions
twine check dist/*
```

This should show no errors or warnings.

## Step 4: Test on TestPyPI (Recommended)

TestPyPI is a separate instance of PyPI for testing:

```bash
# Upload to TestPyPI
python -m twine upload --repository testpypi dist/*

# You'll be prompted for credentials:
# Username: __token__
# Password: your-testpypi-api-token
```

### Test Installation from TestPyPI

```bash
# Create a new virtual environment
python -m venv test_env
source test_env/bin/activate  # On Windows: test_env\Scripts\activate

# Install from TestPyPI
pip install --index-url https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple/ kapetanios-test

# Test it
python -c "from kapetanios_test import kapetanios_test; print('Success!')"

# Deactivate and remove test environment
deactivate
rm -rf test_env
```

## Step 5: Upload to PyPI

Once you've verified everything works on TestPyPI:

```bash
# Upload to PyPI
python -m twine upload dist/*

# You'll be prompted for credentials:
# Username: __token__
# Password: your-pypi-api-token
```

## Step 6: Verify Installation

```bash
# Install from PyPI
pip install kapetanios-test

# Test it
python -c "from kapetanios_test import kapetanios_test; print('Installed successfully!')"
```

## Using API Tokens (Recommended)

Instead of entering credentials each time, create a `.pypirc` file:

### For Windows

Create file at: `C:\Users\YourUsername\.pypirc`

### For Linux/Mac

Create file at: `~/.pypirc`

### Content:

```ini
[distutils]
index-servers =
    pypi
    testpypi

[pypi]
username = __token__
password = pypi-YOUR-API-TOKEN-HERE

[testpypi]
username = __token__
password = pypi-YOUR-TESTPYPI-API-TOKEN-HERE
```

**Important**: Keep this file secure! Add it to `.gitignore`.

## Uploading Updates

When you release a new version:

1. Update version number in:
   - `setup.py`
   - `pyproject.toml`
   - `kapetanios_test/__init__.py`

2. Update `CHANGELOG.md`

3. Clean and rebuild:
```bash
rm -rf build/ dist/ *.egg-info
python -m build
```

4. Upload:
```bash
twine upload dist/*
```

## Troubleshooting

### Error: "File already exists"

You can't upload the same version twice. Increment the version number.

### Error: "Invalid distribution"

Run `twine check dist/*` to see what's wrong.

### Error: "Authentication failed"

- Check your API token is correct
- Ensure you're using `__token__` as username
- Token should start with `pypi-`

### Windows-Specific Issues

If you encounter path issues on Windows:
```cmd
# Use forward slashes or escape backslashes
python -m twine upload dist/*
```

## Best Practices

1. **Always test on TestPyPI first**
2. **Use API tokens instead of passwords**
3. **Keep `.pypirc` secure and out of version control**
4. **Follow semantic versioning** (MAJOR.MINOR.PATCH)
5. **Update CHANGELOG.md** for each release
6. **Tag releases in Git**:
   ```bash
   git tag -a v1.0.0 -m "Release version 1.0.0"
   git push origin v1.0.0
   ```

## Automation with GitHub Actions (Optional)

Create `.github/workflows/publish.yml`:

```yaml
name: Publish to PyPI

on:
  release:
    types: [published]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.9'
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install build twine
    - name: Build package
      run: python -m build
    - name: Publish to PyPI
      env:
        TWINE_USERNAME: __token__
        TWINE_PASSWORD: ${{ secrets.PYPI_API_TOKEN }}
      run: twine upload dist/*
```

Add your PyPI API token as a GitHub secret named `PYPI_API_TOKEN`.

## Post-Publication Checklist

- [ ] Package appears on https://pypi.org/project/kapetanios-test/
- [ ] `pip install kapetanios-test` works
- [ ] Documentation is correct
- [ ] Links in README work
- [ ] Create GitHub release with the same version tag
- [ ] Announce on relevant forums/mailing lists
- [ ] Update personal website/CV

## Support

For issues:
- Check PyPI packaging guide: https://packaging.python.org/
- PyPI help: https://pypi.org/help/
- Create issue on GitHub: https://github.com/merwanroudane/kapetanios/issues

## Contact

**Dr. Merwan Roudane**
- Email: merwanroudane920@gmail.com
- GitHub: https://github.com/merwanroudane
