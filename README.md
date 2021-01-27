<p align="center">
<a href="https://coderrect.com"><img src="images/coderrect-logo.png" height="100px"/></a>
 </p>
<h1 align="center">Coderrect Github Action </h1>
<div align="center">
 <strong>
    GitHub Action for the Coderrect static race detection scanner
 </strong>
</div>
<br/>

![license](https://img.shields.io/github/license/coderrect-inc/coderrect-github-action)
![issue](https://img.shields.io/github/issues/coderrect-inc/coderrect-github-action)
![release](https://img.shields.io/github/v/release/coderrect-inc/coderrect-github-action?include_prereleases)

## About
The `coderrect-github-action` runs static analysis in a GitHub workflow using [Coderrect](https://coderrect.com) to automatically detect concurrency bugs in your code:
- Coderrect is an efficient and accurate static analysis solution to identify potential concurrency bugs in your multi-threaded software.
- Coderrect supports C/C++/Fortran code and common parallel programming interfaces including OpenMP, pthread, and C++ std::thread.
- Coderrect Github Action supports generating formatted HTML reports and hosting them on Coderrect cloud for users to review.

Documentation and examples are available at: [Coderrect Action Documentation](https://coderrect.com/docs/github-actions/)

More information about Coderrect Scanner is available at: [Coderrect.com](https://coderrect.com)

**Table of Contents**

- [Demo](#demo)
- [Example Usage](#example-usage)
  - [Versioning](#versioning)
  - [For CMake projects](#for-cmake-projects)
  - [For Fortran projects](#for-fortran-projects)
  - [For customized analysis](#for-customized-analysis)
- [Inputs](#inputs)
- [License](#license)

## Demo

<img src="images/demo-small.gif" alt="Demo" height="600">

## Example Usage
Take [memcached](https://github.com/coderrect-inc/memcached) as an example for integrating Coderrect.
Check [here](https://github.com/coderrect-inc/memcached/blob/master/.github/workflows/ci.yml) for the final script.

In general, integrating Coderrect requires two steps:

1. Install all dependencies required to build your project:
```yaml
- name: Install deps
  run: |
    sudo apt-get update -y
    sudo apt-get install -y libevent-dev libseccomp-dev git libsasl2-dev
```

2. Apply Coderrect Github Action
```yaml
- name: Coderrect Scan
  uses: coderrect-inc/coderrect-github-action@v1.2
```

### For CMake projects
You will need to install and setup `cmake` first.
```yaml
- name: download cmake
  run: |
    wget https://cmake.org/files/v3.18/cmake-3.18.2-Linux-x86_64.tar.gz
    tar xf cmake-3.18.2-Linux-x86_64.tar.gz
    mkdir build && cd build
    ../cmake-3.18.2-Linux-x86_64/bin/cmake ..
```
Since we are building the project under the `build` directory instead of the root path.
We will also need to specify the build directory for Coderrect.
```yaml
- name: Coderrect Scan
  uses: coderrect-inc/coderrect-github-action@v1.2
  with:
    buildPath: "build"
```

### For Fortran projects
You will need to install the fortran compiler first. For example:
```yaml
- name: Install fortran
  run: |
    sudo apt-get update -y
    sudo apt-get install -y gfortran
```
Then it is likely that you need to specify the fortran compiler when you use `make`. If so, you should also pass the full compilation command to Coderrect. (`gcc` is pre-installed in the Github Action environment.)
```yaml
- name: coderrect scan
  uses: coderrect-inc/coderrect-github-action@v1.2
  with:
    buildCommand: "make COMPILER=GNU MPI_COMPILER=gfortran C_MPI_COMPILER=gcc"
```

For more details, take a look at this [cmake project](https://github.com/coderrect-inc/covid-sim) and [fortran project](https://github.com/coderrect-inc/CloverLeaf_OpenMP) to learn how to integrate Coderrect into more complex projects

### For customized analysis
Coderrect allows you to provide a configuration file to fully customize your analysis.

In order to do so, check our [documentation](https://coderrect.com/documentation/reference/) to see available configurations. Once you write a configuration file (say `coderrect.json`). You can pass it to the scanner as below:
```yaml
- name: coderrect scan
  uses: coderrect-inc/coderrect-github-action@v1.2
  with:
    options: "-analyzeAllBinaries -conf=/path/to/coderrect.json"
```

The path should be **a relative path from your build directory** (e.g., if your build directory is `./build/` and your config file is under the root path, then you should specify the config file as `"-conf=../coderrect.json"`).

## Inputs
- `buildCommand`
  - Default: `"make -j"`
  - Description: The command to build your project. For example, the command to build your whole project might be `make all` instead of `make`.
- `cleanCommand`
  - Default: `"make clean"`
  - Description: The command to clean your previous build. Coderrect needs to capture the building process for analysis, therefore if you have done a test build before applying Coderrect, we need to clean your test build first.
- `buildPath`
  - Default: `"."`
  - Description: The relative path for your cmake project's build directory.
- `options`
  - Default: `"-analyzeAllBinaries"`
  - Description: The command line options for Coderrect Scanner. Check the [documentation](https://coderrect.com/documentation/reference/) for all supported options.
- `exit0`
  - Default: false
  - Description: By default, Coderrect will exit with 1 if any race is detected, thus your following CI/CD workflow will be blocked. Set this to true if you want CI/CD to proceed after Coderrect no matter what.

## License
   Copyright 2020 Coderrect Inc.

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
