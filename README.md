# Numerical Methods Labs

Four labs of a numerical methods course: series approximation, root finding, linear systems and numerical integration, each solved for several task variants in C++, Python, C and C# (file names use `AMO`, the course abbreviation).

Year: 2021 · Language: C++ (also Python, C, C#) · Context: university coursework (KPI)

## What's inside

Each lab has one folder per variant (`varN`, `varN_k` = an alternative solution of the same variant). Task texts are not in the repo; the descriptions below are read from the code.

| Lab | What it does | Variants (folders) |
|---|---|---|
| `lab1/` | Taylor-series evaluation of `ln x`, `ch x`, `sh x`, `sin x` with stopping by epsilon (1e-2 ... 1e-14); prints a table of n, absolute error and remainder term, then a second table over 11 points of the interval for the n found at eps = 1e-8 | `var1`, `var7`, `var12`, `var14`, `var14_1`, `var15` (C#), `var17`, `var21` |
| `lab2/` | Root of a nonlinear equation: simple iteration, Newton, chord methods; error estimate and iteration count per epsilon | `var1`, `var3_0` (Py), `var3_1`, `var6`, `var7`, `var12`, `var15_0`, `var15_1`/`var15_2` (C#), `var21`, `var27` (C) |
| `lab3/` | 4x4 linear system: Gauss-Jordan elimination vs. iterative methods (direct iteration, Seidel) | `var4`, `var7`, `var7_1`, `var12`, `var13`, `var20` |
| `lab4/` | Definite integrals: Simpson and trapezoid rules, step chosen from the error bound, comparison with the Newton-Leibniz value, Runge error estimate | `var3`, `var5`, `var7`, `var8`, `var12`, `var15`, `var20`, `var21`, `var26` |

## Interesting bits

- `lab1/var1/var1.cpp` computes `ln x` with range reduction: finds `m` with `2^m >= x`, sets `z = x / 2^m` and sums the series in `(1 - z) / (1 + z)`, then reassembles `m*ln2 - 2*sum`. `lab1/var15/TaskSolver.cs` does the same kind of reduction for `sin` by subtracting multiples of pi and tracking the sign.
- `lab2/var3_0/var3.py` implements simple iteration with `lambda = 1/M1` and contraction factor `q = 1 - m1/M1`, using the a-posteriori stop `q*|xn - x0| / (1 - q) < eps`; `lab2/var6/Chord.cpp` picks the fixed end by the sign of `f(a)*f''(a)` and stops on `|f(xn)| / m1 < eps`.
- `lab2/var27/var27.c` separates roots first: it scans the interval with step 0.01, collects sign-change intervals where the derivative keeps its sign into a hand-written linked list, and refines each with bisection or iteration.
- `lab4/var12/main.cpp` derives the trapezoid step directly from the error bound (`h = sqrt(12*eps / ((b-a)*M))`); `lab4/var21/Task.cpp` has a `runge()` function that doubles n until the Runge estimate `|I(n) - I(2n)| / (2^r - 1)` drops below the target.

## Run

Verified (build and run) with g++ 13.3 on Linux, one variant folder at a time:

```
g++ -std=c++14 lab1/var12/*.cpp -o var12 && ./var12     # lab1/var1, var7, var12, var14*, var17, var21; lab2/var1, var6, var7, var12, var15_0, var21, var3_1; lab3/var12
g++ -std=c++17 -fpermissive -include cmath lab4/var12/*.cpp -o var12 && ./var12    # lab3/var7, var7_1, lab4/*
gcc lab2/var27/var27.c -lm -o var27 && ./var27
python3 lab4/var3/main.py                                # all .py files run; lab3/var4 asks for epsilon on stdin
```

The `-include cmath` flag is needed because several files rely on a Visual Studio header that pulls in `<cmath>` implicitly; `lab3/var7*` use `std::size` (C++17). The C# files (`Program.cs`, `Task*.cs`, `TaskSolver.cs`) were not compiled (no .NET toolchain here).

## Note

Written in 2021, before AI coding assistants. Kept as-is for history.
