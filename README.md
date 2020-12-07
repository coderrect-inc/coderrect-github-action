# Coderrect Github Action <img src="images/github.png" alt="Github" width="40" height="40">
![license](https://img.shields.io/github/license/coderrect-inc/coderrect-github-action)
![issue](https://img.shields.io/github/issues/coderrect-inc/coderrect-github-action)

Coderrect Scanner Github Action with HTML report output.

<img src="images/coderrect-logo.png" alt="Github" height="150">

- Coderrect is an efficient and accurate static analysis solution to identify potential concurrency bugs in your multi-threaded software.
- Coderrect supports C/C++/Fortran code and common parallel programming interfaces such as OpenMP.
- Coderrect Github Action supports generating better formatted HTML reports and hosting them on Coderrect cloud for you to review.

Please find more information in our official website: [Coderrect.com](coderrect.com) 

## Example Usage
```
- name: Coderrect Scan
  uses: coderrect-inc/coderrect-github-action@v1.0
  with:
      buildCommand: make all -j 6
```

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
