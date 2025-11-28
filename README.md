# osimperf

> Simplified performance monitoring for opensim-core

`osimperf` provides scripts and infrastructure for:

- Building and installing historical commits of `opensim-core`, according to a regular sampling grid (i.e. annually,
  quarterly, monthly) into an installation directory (default `opensim-core_installs/`, see `./opynsim build-all`).
- Building a native performance measurement executable (`osimperf-runner`), so that measurements can
  skip initialization overhead and use specialized native clocks and profiler triggers (see `osimperf-runner.cpp`)
- Stage and run a single performance benchmark against a single commit with minimal terminal noise
  (see `./osimperf measure`).
- Stage and run a full analysis of many benchmarks against many commits, with the ability to
  continuously update the result CSV as new benchmarks/commits come in (`./osimperf full-analysis`).

# Setup & Install

On Ubuntu 24.04:

```bash
# Install OS dependencies
sudo apt-get install $(cat ubuntu24_apt-dependencies.txt)

# Get opensim-core sources
git clone https://github.com/opensim-org/opensim-core

# Create local virtual environment (IDEs prefer this)
python3 -m venv .venv/
source ./.venv/bin/activate
pip install -r requirements.txt
```

# Usage

```bash
# list all commits that the system should/will/has buil{t,d}
./osimperf all-commits

# builds + installs historical opensim-core distributions (and osimperf-runner)
#
# WARNING: this takes a very very long time.
./osimperf build-all

# build + installs a particular opensim-core commit (and osimperf-runner)
./osimperf build 009995b446fc50aad6768d88db9c0e6c8217f07e

# measures the performance of one commit + benchmark pair and prints the runtime
./osimperf measure 009995b446fc50aad6768d88db9c0e6c8217f07e RajagopalDrop  # 2>/dev/null  # silence log

# performs a "full analysis" (see analysis.toml - this is what generates plot data)
./osimperf full-analysis analysis.toml

# renders figure 1 to `figure1.png` (Profiler-Led Optimizations Doubled Simulation Performance...)
./osimperf plot-figure-1 full_analysis_results.csv figure1.png

# renders figure 1 to `figure1.png` (Profiler-Led Optimizations Doubled Simulation Performance...)
./osimperf plot-figure-2 full_analysis_results.csv figure2.png
```

# C++ Development Environment Setup

- Setup & Install the system (see Setup & Install)
- Build + install at least one opensim-core distribution (see Usage)
- Edit the `CMAKE_PREFIX_PATH`s in `CMakePresets.json` to point towards the opensim-core you
  want to develop the C++ against
- Open it in a C++ IDE that supports automatic configuration via `CMakePresets.json` (e.g. CLion,
  Visual Studio).

# Notes

- Does not include Thoracos/ specialization from https://github.com/ComputationalBiomechanicsLab/OpenSimPerfProjects
