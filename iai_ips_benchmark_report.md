name: IAI-IPS Consciousness Benchmark CI with Report

on: push: branches: - main - dev pull_request:

jobs: test-and-report: runs-on: ubuntu-latest

steps:
- name: Checkout repository
  uses: actions/checkout@v4

- name: Set up Python
  uses: actions/setup-python@v5
  with:
    python-version: '3.11'

- name: Install dependencies
  run: |
    python -m pip install --upgrade pip
    pip install pytest

- name: Run IAI-IPS Consciousness Benchmark Tests
  run: |
    pytest -v tests/iai_ips_pytest_harness.py > benchmark_test_report.log

- name: Generate Markdown Report
  run: |
    python tests/iai_ips_report_generator.py

- name: Upload Benchmark Logs and Report
  uses: actions/upload-artifact@v4
  with:
    name: iai-ips-benchmark-artifacts
    path: |
      benchmark_test_report.log
      iai_ips_benchmark_report.md

