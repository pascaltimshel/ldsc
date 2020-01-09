# LDSC (LD SCore) `v1.0.0` - modified for CELLECT

This repo is forked from [bulik/ldsc](https://github.com/bulik/ldsc) to better suit the needs for [CELLECT](https://github.com/perslab/CELLECT). `ldsc` is a command line tool for estimating heritability and genetic correlation from GWAS summary statistics. We have made the following modifications.

**Edits**

1. ldsc.py: will not compute the 'Annotation Correlation Matrix' to the log file. This can take a long time if you have many annotations. 

2. ldsc.py: will not compute the 'correlation matrix including all LD Scores and sample MAF' and condition number. Again, this may take a long time.

3. sumstats.py: modified cell_type_specific() function:
    - 'result caching': write a ".cell_type_results.tmp.txt" file after each regression, so we don't loose all computations if ldsc fails during one of the regressions (or the server terminates during the regressions). This is especially important to when running ldsc with many CTS annotations.
    - display/log progress of the CTS regressions ("running regression no. ...")
    - wrapped 'CTS mode loop' inside try/except for better monitoring of errors.
    - added sys.stdout.flush() to enable 'online monitoring' of jobs - even without running in unbuffered mode (python -u).

**New scripts**

1. `quantile_M_fixed_non_zero_quantiles.pl`: modified version of `quantile_M.pl` that support h2 calculations for fixed intervals.
2. `mtag_munge.py`: an improved version of `munge_sumstats.py` created by [mtag](https://github.com/omeed-maghzian/mtag) developers. We have made a few small convenient adjustments to `mtag_munge.py` (see git history) .

**Environments**
1. Added environment_munge.yml with `numpy` and `pandas` versions that works with `munge_sumstats.py` (and `mtag_munge.py`). All of LDSC (including `munge_sumstats.py` and `mtag_munge.py`) runs only on **python 2.7**.
