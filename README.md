name: documentation
on:
  push:
    tags:
      - 'v*'

jobs:
  build-docs:
    if: github.event_name == 'push' && github.ref == 'refs/heads/master'
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Verify tag is on master
        run: |
          # Get the branch containing this tag
          BRANCH=$(git branch -r --contains ${{ github.ref }} | grep 'master' || true)

          # Check if the tag is on master
          if [ -z "$BRANCH" ]; then
            echo "Error: Tag must be created on master branch"
            exit 1
          fi

      - name: Install uv
        run: curl -LsSf https://astral.sh/uv/install.sh | sh

      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
          cache: 'uv'

      - name: Install dependencies with dev group
        run: uv sync --group dev

      # Deploy docs
      - name: Deploy documentation
        run: |
          uv run mkdocs gh-deploy --force


<div align="center">

  <a href="">[![GitHub release](https://img.shields.io/github/v/release/rkdan/cancer-prediction?include_prereleases)](https://GitHub.com/rkdan/cancer-prediction/releases)</a>
  <a href="">![Test status](https://github.com/rkdan/cancer-prediction/actions/workflows/tests.yml/badge.svg?branch=dev)</a>

</div>

<div align="center">

  <a href="">[![Imports: isort](https://img.shields.io/badge/%20imports-isort-%231674b1?style=flat&labelColor=ef8336)](https://pycqa.github.io/isort/)</a>
  <a href="">[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)</a>
  <a href="">[![Checked with mypy](https://www.mypy-lang.org/static/mypy_badge.svg)](https://mypy-lang.org/)</a>
</div>