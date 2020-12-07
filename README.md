# Coderrect Github Action <img src="images/github.png" alt="Github" width="40" height="40">

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
      host: xxx.com
      config: xxx.json

```

## Options
