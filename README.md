# Workshop 8: Greybox Fuzzing With AFL++

## Description

Use AFL++ to perform a greybox fuzzing run against the provided C program, `w8.c`, and record the results.

## Background

Greybox fuzzing uses feedback from program execution, such as code coverage, to guide the generation of test cases. AFL++ is a widely used fuzzing tool. It includes the `afl-fuzz` fuzzer, instrumentation options, and utilities for minimizing test cases and corpora.

Learn more: [AFL++](https://aflplus.plus/).

## Activities

### 1. Prepare the target

The target program is `w8.c`. Compile it with AFL++ instrumentation:

```bash
afl-clang-fast -g -O0 w8.c -o w8
```

Create a directory for the initial input corpus and add a seed file:

```bash
mkdir -p fuzzing-inputs
printf 'hello\n' > fuzzing-inputs/seed
```

### 2. Run AFL++

Run the fuzzer from the project directory:

```bash
AFL_I_DONT_CARE_ABOUT_MISSING_CRASHES=1 afl-fuzz -i fuzzing-inputs -o afl-output -V 60 -- ./w8
```

The environment variable was needed in the Codespaces environment because its configured core-dump handler can interfere with AFL++ crash handling. AFL++ may display a warning about this limitation.

While the fuzzer is running, observe the execution screen and save a screenshot. AFL++ stores its results in `afl-output/`, including statistics in `afl-output/default/fuzzer_stats`.

## Tasks For You To Do

- Run AFL++ against `w8.c`.
- Save a screenshot of the AFL++ execution screen.
- Save the AFL++ output folder.
- Record the number of crashes and hangs reported by the run in a text file.

## Results From This Run

The fuzzer ran for 60 seconds. The recorded results were:

| Metric | Result |
| --- | ---: |
| Total executions | 6,757 |
| Unique crashes saved | 0 |
| Unique hangs saved | 0 |
| Timeouts reported | 0 |
| Coverage stability | 100% |
| Bitmap coverage | 33.33% |

The results are also recorded in [`fuzzing-report.txt`](fuzzing-report.txt).

## Important Note

The provided `w8.c` program generates random numbers but does not read the fuzzing input. Therefore, this run does not meaningfully exercise input-dependent behavior. These results describe this 60-second run only and do not prove that the program is free of defects.

## Deliverables

- Screenshot of the AFL++ execution screen.
- AFL++ output folder (`afl-output/`).
- Text report listing the crashes and hangs observed (`fuzzing-report.txt`).
