# Publishing to PyPI

## Prerequisites

1. **Create PyPI accounts** (if you don't have them):
   - Test PyPI: https://test.pypi.org/account/register/
   - Production PyPI: https://pypi.org/account/register/

2. **Install build tools**:
   ```bash
   # Using uv (recommended)
   uv pip install build twine

   # Or using pip
   pip install build twine
   ```

3. **Create API tokens**:
   - Go to https://pypi.org/manage/account/token/ (for production)
   - Go to https://test.pypi.org/manage/account/token/ (for test)
   - Create a new API token with "Upload packages" scope
   - Save the token securely (format: `pypi-...`)

## Publishing Steps

### Step 1: Update Version (if needed)

Edit `pyproject.toml` and increment the version:
```toml
version = "0.1.0"  # Change to 0.1.1, 0.2.0, etc.
```

### Step 2: Build the Package

```bash
# Build both wheel and source distribution
python -m build

# This creates:
# - dist/skills_sync-0.1.0-py3-none-any.whl
# - dist/skills_sync-0.1.0.tar.gz
```

### Step 3: Test on Test PyPI (Recommended)

```bash
# Upload to Test PyPI
python -m twine upload --repository testpypi dist/*

# When prompted:
# Username: __token__
# Password: <your-test-pypi-api-token>
```

**Test the installation**:
```bash
# Install from Test PyPI
pip install --index-url https://test.pypi.org/simple/ skills-sync

# Or using uvx
uvx --index-url https://test.pypi.org/simple/ skills-sync config
```

### Step 4: Publish to Production PyPI

```bash
# Upload to production PyPI
python -m twine upload dist/*

# When prompted:
# Username: __token__
# Password: <your-production-pypi-api-token>
```

### Step 5: Verify Installation

```bash
# Install from PyPI
pip install skills-sync

# Or using uvx
uvx skills-sync config

# Test the command
skills --help
```

## Using Environment Variables (Optional)

Instead of entering credentials each time, you can set environment variables:

```bash
# For Test PyPI
export TWINE_USERNAME=__token__
export TWINE_PASSWORD=<your-test-pypi-api-token>

# For Production PyPI
export TWINE_USERNAME=__token__
export TWINE_PASSWORD=<your-production-pypi-api-token>
```

Then upload without prompts:
```bash
python -m twine upload dist/*
```

## Using .pypirc (Alternative)

Create `~/.pypirc`:
```ini
[distutils]
index-servers =
    pypi
    testpypi

[pypi]
username = __token__
password = <your-production-pypi-api-token>

[testpypi]
repository = https://test.pypi.org/legacy/
username = __token__
password = <your-test-pypi-api-token>
```

Then upload:
```bash
python -m twine upload --repository testpypi dist/*  # Test
python -m twine upload --repository pypi dist/*      # Production
```

## Quick Commands Summary

```bash
# 1. Update version in pyproject.toml
# 2. Build
python -m build

# 3. Test on Test PyPI
python -m twine upload --repository testpypi dist/*

# 4. Publish to Production PyPI
python -m twine upload dist/*

# 5. Clean up build artifacts (optional)
rm -rf dist/ *.egg-info/
```

## Troubleshooting

- **"Package already exists"**: Increment the version number
- **"Invalid credentials"**: Check your API token is correct
- **"File already exists"**: Delete old files in `dist/` or use a new version
- **"Missing metadata"**: Ensure `pyproject.toml` has all required fields

## Additional Metadata (Optional)

Consider adding to `pyproject.toml`:
```toml
[project]
authors = [
    {name = "Your Name", email = "your.email@example.com"},
]
license = {text = "MIT"}  # or other license
keywords = ["skills", "sync", "claude", "cursor", "agent"]
classifiers = [
    "Development Status :: 4 - Beta",
    "Intended Audience :: Developers",
    "License :: OSI Approved :: MIT License",
    "Programming Language :: Python :: 3",
    "Programming Language :: Python :: 3.8",
    "Programming Language :: Python :: 3.9",
    "Programming Language :: Python :: 3.10",
    "Programming Language :: Python :: 3.11",
    "Programming Language :: Python :: 3.12",
]
```
