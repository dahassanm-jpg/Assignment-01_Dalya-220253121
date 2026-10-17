# Assignment 1 — Performance Analysis and Scaling Laws

**Course:** Selected Topics in Parallel and Distributed Computing  
**Submission:** PDF report, supporting Python code/notebook, and this README  
**Analysis environment:** Python 3 / Google Colab

## 1\. Overview

This repository contains the analysis and reproducible calculations for **Assignment 1: Performance Analysis and Scaling Laws**. The assignment covers five tasks:

1. **Amdahl’s Law:** Theoretical speedup for specified processor counts and serial fractions, including logarithmic-axis plots.
2. **Gustafson’s Law:** Scaled speedup and comparison with Amdahl’s Law.
3. **Karp–Flatt Metric:** Measured speedup verification, effective serial fraction, interpretation, and a model-based efficiency prediction at 32 processors.
4. **Strong vs. Weak Scaling:** Efficiency calculation and visual comparison based on provided timing measurements.
5. **Scaling Analysis and Discussion:** Statistics for five repeated timing measurements, speedup, efficiency, uncertainty/error bars, a 70% efficiency threshold, possible bottlenecks, and optimization suggestions.

The numerical inputs come from the instructor-provided **Assignment 1** PDF and the separate **Task 5 Representative Dataset** PDF. The mathematical definitions and interpretation follow the course’s **lecture** notes.

## 2\. Repository contents

A recommended layout is:

```text
Assignment-01\_Dalya-220253121/
├── README.md
├── report/
│   └── Parallel and Distributed Computing\_Assignment 01\_Dalya 220253121.pdf
├── code/
│   └── Parallel\_And\_Distribution\_Computing\_Assignment\_01\_Dalya\_220253121.ipynb
└── figures/
    ├── task1\_q3\_amdahl.png
    ├── task1\_q4\_amdahl\_comparison.png
    ├── task2\_amdahl\_vs\_gustafson.png
    ├── task4\_scaling\_efficiency.png
    ├── task5\_q21\_speedup.png
    └── task5\_q21\_efficiency.png


## 3\. Software requirements

* Python 3.10 or newer (a compatible Python 3 version also works)
* NumPy
* Matplotlib
* Google Colab (recommended), or Jupyter Notebook / local Python

### Google Colab (recommended)

1. Open https://colab.research.google.com/.
2. Open the submitted `Parallel\_And\_Distribution\_Computing\_Assignment\_01\_Dalya\_220253121.ipynb` notebook.
3. Select **Runtime → Run all** to reproduce computations and plots.
4. Download generated `.png` figures as needed. The cells contain `files.download(...)`, run it manually only when you want a browser download.

### Local Python / Jupyter

Install required packages:

```bash
python -m pip install numpy matplotlib pandas notebook
```

Open the notebook:

```bash
jupyter notebook code/Parallel\_And\_Distribution\_Computing\_Assignment\_01\_Dalya\_220253121.ipynb
```

Run cells in order from the top. If you use separate `.py` scripts, run each one using `python path/to/script.py`.

## 4\. Calculations and methods

### Task 1 — Amdahl’s Law

Use the formula

\[
S\_A(p)=\\frac{1}{s+(1-s)/p}.
]

* Processor counts: `1, 2, 4, 8, 16, 64, 1024`.
* Serial fractions: `s = 0.05, 0.10, 0.20`.
* The maximum theoretical speedup for `s > 0` is `1/s`.
* Produce the Question 3 plot for `s = 0.05` and the Question 4 plot with all three values of `s` using a logarithmic processor axis.

### Task 2 — Gustafson’s Law

Use

\[
S\_G(p)=s+p(1-s)
]

for `s = 0.05` and the same processor counts. Plot Gustafson’s and Amdahl’s theoretical speedups on one figure. Interpret their different fixed-workload versus scaled-workload assumptions.

### Task 3 — Karp–Flatt Metric

Given the assignment's observed times, calculate

\[
S(p)=\\frac{T(1)}{T(p)},\\qquad
e(p)=\\frac{1/S(p)-1/p}{1-1/p},\\quad p>1.
]

Use unrounded computed speedups in the Karp–Flatt calculations. Assess whether `e(p)` is constant. For the **32-processor efficiency prediction**, explicitly assume that the effective serial fraction observed at `p=16` also applies to `p=32`; the resulting number is a model-based estimate, not a new measurement:

\[
E(32)=\\frac{1}{1+(32-1)e(16)}.
]

### Task 4 — Strong and Weak Scaling

For the assignment's two timing series, calculate:

\[
E\_{strong}(p)=\\frac{T\_{strong}(1)}{pT\_{strong}(p)}, \\qquad
E\_{weak}(p)=\\frac{T\_{weak}(1)}{T\_{weak}(p)}.
]

Plot both percentages against processor count on the same figure. Discuss which form of scaling preserves efficiency better and plausible strong-scaling limitations.

### Task 5 — Repeated Measurements and Scaling

Use the Dr. Khalil-provided Task 5 dataset, containing five runtime measurements for each of `p = 1, 2, 4, 8, 16, 32, 64, 128`.

* Mean runtime: arithmetic mean of the five measurements.
* Sample standard deviation: `numpy.std(times, axis=1, ddof=1)` (`n−1` divisor).
* Speedup: `mean\_time\[0] / mean\_time`.
* Efficiency: `speedup / p`.
* Plot **speedup vs. p** and **efficiency vs. p** with error bars.

**Error-bar methodology:** The plots use approximate **±1 standard error** bars. Compute `SEM(p) = SD(p)/sqrt(5)` and apply first-order uncertainty propagation to the ratio of independently measured baseline and parallel mean times:

\[
\\sigma\_S(p)\\approx S(p)\\sqrt{\\left(\\frac{SEM(1)}{\\overline T(1)}\\right)^2+\\left(\\frac{SEM(p)}{\\overline T(p)}\\right)^2},
\\qquad \\sigma\_E(p)=\\frac{\\sigma\_S(p)}{p}.
]

At `p=1`, speedup equals `1` exactly by definition because the numerator and denominator reuse the **same** measured baseline mean; its propagated error is set to zero. The error bars are **not 95% confidence intervals**. This is a chosen methodology because the assignment does not specify how to derive the error bars.

Identify the **first measured processor count** where mean-based efficiency is below `70%`. Discuss at least three plausible bottlenecks and two possible scalability improvements. Runtime results alone do not establish which bottleneck caused the decrease.

## 5\. Reproducibility notes

* Times are reported in **seconds**.
* Speedup and efficiency are dimensionless; multiply efficiency by `100` for a percentage.
* Calculate with full precision; round only when displaying tables and discussing results.
* Input values are taken from the assignment documents, not newly generated benchmark measurements.
* Theoretical predictions and uncertainty estimates are labeled as such.
* Graphs are saved with axis labels, legends, and descriptive filenames.

## 6\. Assignment deliverables and submission

1. Include the completed **PDF report** (derivations, tables, plots, interpretation, and references).
2. Include the **Python notebook or scripts** used to generate the results and plots.
3. Include **README.md** with instructions for reproducing the computations.
4. Upload the files to a **Git repository** (for example, on GitHub) and submit the repository URL in the course LMS.

Repository URL:

```text

https://github.com/dahassanm-jpg/Assignment-01\_Dalya-220253121
```

## 7\. References

* Course handout: *Assignment 1 — Performance Analysis and Scaling Laws*.
* Course handout: *Task 5: Scaling Analysis — Representative Dataset*.
* Course notes: *Lectures 1–2* and *Lectures 3–4*

