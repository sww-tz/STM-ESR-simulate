# STM-ESR Simulation Code

This directory contains the source code of a simulation program based on the hierarchical equations of motion (HEOM) for STM-ESR, together with two example cases: a steady-state case (`case_steady`) and a dynamics (time-evolution) case (`case_danmics`).

## Directory structure

| Path | Description |
| --- | --- |
| `software.tar.gz` | Program source code; unpacks to `src.sav92_1/` |
| `case_steady/` | Steady-state case: `input`, `output`, and the result `rho_spa.sav` |
| `case_dynamics/` | Dynamics (time-evolution) case: `input`, `output`, `curr.data` |

## 1. Compiling and installing the software

1. Unpack the source code:

   ```sh
   tar -xzf software.tar.gz
   ```

   This produces `src.sav92_1/`, where compilation is driven by the `Makefile`.

2. Edit `src.sav92_1/Makefile` so that the installation path points to your **local path**:

   - `NAME` is the path of the executable to be produced. In the distributed Makefile it is hard-coded to an absolute path on another machine (`/public/home/lc/heom_test/heom_test2`), and it **must be changed to your own local path**; make sure that directory already exists.
   - Also check the library and include paths: `-L/MKLPATH` and `-I/MKLINCLUDE` in `LIBDIR` should point to the Intel MKL library and include directories on your machine.
   - Make sure the compiler `F77` (default `ifort`) matches your local installation, and source the Intel compiler environment before compiling.

3. Compile:

   ```sh
   cd src.sav92_1
   make clean
   make
   ```

## 2. Running the program

**The steady-state calculation must be run first, followed by the dynamics calculation.** The steady-state run produces `rho_spa.sav`, which the dynamics run reads as its initial state, so the order cannot be reversed.

1. Run the steady-state case `case_steady` to generate `rho_spa.sav`.
2. Copy the generated `rho_spa.sav` into `case_dynamics/`.
3. Run the dynamics case `case_danmics` to obtain the time-evolution results (such as `curr.data`).

Submit the job from the corresponding case directory by redirecting the input and output:

```sh
./heom-92 < input > output &
```

(The executable name is determined by `NAME` in the Makefile; replace it with the actual name.)

## 3. Input parameters

- The settings and meaning of the input parameters are given in the comments of the `input` file in each case directory.
- The first line of `input` is the job type `init`: `2` solves the steady state (producing `rho_spa.sav`), whereas `1` reads `rho_spa.sav` and performs the time evolution (dynamics).
- The remaining namelist variables are documented in the source tree under `src.sav92_1/readme/` (for example `INSTRUCTION.txt` and `NAMELIST.txt`).
