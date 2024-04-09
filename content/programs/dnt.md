---
title: "Digenic Network Test, DNT"
date: 2024-03-15
draft: true 
---

This website refers to a new method \[1\] of finding highly interactive DNA variants on the basis of long lists of genotype pairs in cases and controls. Such lists can be obtained for a given genetic trait like AMD and PD by the [Gpairs](https://lab.rockefeller.edu/ott/programs/GPM) program \[2\].

Program DNT
-----------

We implemented our new approach to interpreting lists of genotype patterns \[1\] in a computer program, [DNT](https://github.com/jurgott/gpm_dnt) (for Digenic Network Test). It is based on a test statistic that identifies variants highly connected with other variants, where a permutation procedure estimates their associated empirical significance levels. As described \[1\], the test statistic for a given variant is given by the number Nc of other variants it is connected with, weighted by the inverse of the rank at which the given variant first occurs in the list of sorted genotype pairs. In our experience, we find significant variants highly connected with other variants even when pairs of genotypes do not show significant frequency differences between cases and controls. Below, a short set of instructions allows you to use the DNT program in a command box, CMD (Windows) or a terminal (Linux). It is assumed that you have an output file created by the Gpairs program \[2\], sorted by decreasing chi-square (first item per output line). The Gpairs program will write two output files, one ending in "out" with all genotype pairs requested, and the other ending in "sorted" with up to 100,000 genotype pairs, sorted by decreasing chi-square. Below are the first five lines of a sorted output file (v1 and v2 refer to internal sequential variant numbers):

```
 chisq2df Y pNom   pBon supp conf a  b  c d   OR   v1  ch1 rs1       bp1      g1 v2    ch2 rs2        bp2     g2
 70.0631  2 3.58E-09 1  38   1    38 55 0 50 70.06 39510 5 rs1394608 155764189 3 48312   7 rs2350745  9199716  3
 61.9580  2 2.26E-08 1  36   1    36 59 0 50 61.96 8186  1 rs9287251 237995670 3 100460 20 rs7273825  38900703 3
 59.2645  2 2.74E-08 1  35   1    35 60 0 50 59.26 9157  2 rs721064  21492978  3 98253  18 rs10514028 67698010 2
 58.6522  2 4.60E-08 1  35   1    35 57 0 47 58.65 60483 8 rs1503761 134759848 3 61206   9 rs10511467 7363051  2
 58.3008  2 5.61E-08 1  35   1    35 61 0 50 58.30 64640 9 rs7863587 110085983 3 90795  15 rs10520583 83568670 3

```


The preferred way of using the [DNT](https://github.com/jurgott/gpm_dnt) program is to type DNT followed by the name of the "sorted" output file as previously obtained by Gpairs. If will then assume the program parameters outlined below in square brackets \[\]. If you simply type DNT, the program will prompt you to enter program parameters as follows:

1.  Input pattern file name \[name of sorted output file obtained by Gpairs\]?
2.  How many patterns do you want to use \[100,000\]?
3.  How many permutations should be applied for computing p-values \[100,000\]?
4.  Enter a p-value limit so that only those variants (significant variants) with p-value smaller than this limit are output \[0.05\].

The DNT program will use up to 100,000 of the genotype pairs in the sorted input file and will write a list of variants with _p_ < 0.05. It will write two output files, one ending in "out" containing significant unique SNPs occurring in the genotype pairs, and the other ending in "list" containing all SNPs connected with each of the significant SNPs in the "out" file. These output files are space-delimited and may be read into Excel or [LibreOffice](https://www.libreoffice.org/). The latter is my preferred spreadsheet because (1) it requires fewer clicks for what I generally want to do and (2) is available in Linux (Ubuntu).

Please be sure that you see file extensions, which is standard in Linux. In the Windows 11 File Explorer, click on View => Show => File name extensions.

References
----------

1.  Wang G, Ott J. Digenic analysis finds highly interactive genetic variants underlying polygenic traits. _Medical Research Archives_ 11 (2023) doi: [10.18103/mra.v11i10.4604](https://doi.org/10.18103/mra.v11i10.4604)
2.  Zhang Q, Bhatia M, Park T, Ott J. A multi-threaded approach to genotype pattern mining for detecting digenic disease genes. _Front Genet_ 14, 1222517 (2023) doi: [10.3389/fgene.2023.1222517](https://www.frontiersin.org/articles/10.3389/fgene.2023.1222517/full)
