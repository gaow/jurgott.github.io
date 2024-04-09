---
title: "Pseudomarker: Running one marker at a time"
date: 2024-03-15
draft: true 
---

The _Pseudomarker_ program \[1-3\] can estimate parameters for linkage and/or linkage disequilibrium (association) in family data and case-control data. It is based on the _Ilink_ program, which is part of the [Linkage](https://www.ncbi.nlm.nih.gov/research/staff/schaffer/fastlink/) package. This page is based on the official _Pseudomarker_ [program manual](https://www.mv.helsinki.fi/home/tsjuntun/pseudomarker/), modified with updated information. As outlined below, this webpage refers to a shell program, [RunPM](https://github.com/jurgott/RunPM), that repeatedly calls the _Pseudomarker_ program, which has to be downloaded separately for _RunPM_ to work.

Current version of _RunPM_ program: 15 March 2024

RunPM program features
----------------------

Pseudomarker is available for 64 bit Linux PCs. One of its main attractions is that it accommodates both family and case-control data and draws information from genetic linkage as well as genetic association (between marker and putative trait locus). Also, it estimates marker allele frequencies both under linkage and no linkage (treats them as nuisance parameters) so that the analysis becomes virtually independent of marker allele frequencies. You are requested to [register](https://elomake.helsinki.fi/lomakkeet/43718/lomake.html) and then [download](https://www.mv.helsinki.fi/home/tsjuntun/pseudomarker/download.html) the _pseudomarker_ executable code.

The Pseudomarker program requires error-free data and can check for mendelian inconsistencies. However, in larger families it tends to initially miss inconsistencies and then crashes when it encounters them in the course of calclulations. To avoid this problem I wrote a shell program, _RunPM_, that runs _Pseudomarker_ for one marker at a time and prepares an output file containing all marker results. It can run _Pseudomarker_ under dominant (_D_ \> +) or recessive (+ > _D_) modes of inheritance (_D_ \= disease allele, + = wild type allele), and under (1) affected-only or (2) incomplete penetrance or (3) full penetrance models. Parameters for these models are shown below and will automatically be implemented by the program depending on the parameter values chosen by the user.

While the pseudomarker program can handle autosomal or X-linked data, the implementation here focuses on autosomal analysis.


|Dominant  |        |         |   |            |Model 1|Model 2|Model 3  |
|----------|--------|---------|---|------------|-------|-------|---------|
|Genotype  |++      |+D       |DD |p =         |-      |0.005  |1-√(1-K) |
|Frequency |(1 - p)2|2p(1 - p)|p2 |f1 =        |0      |0.01   |0        |
|Penetrance|f1      |f2       |f2 |f2 =        |0.00001|0.96   |1        |
|          |        |         |   |prevalence =|-      |0.02   |K (input)|
|Recessive |        |         |   |            |Model 1|Model 2|Model 3  |
|Genotype  |++      |+D       |DD |p =         |-      |0.11   |√K       |
|Frequency |(1 - p)2|2p(1 - p)|p2 |f1 =        |0      |0.01   |0        |
|Penetrance|f1      |f1       |f2 |f2 =        |0.00001|0.83   |1        |
|          |        |         |   |prevalence =|-      |0.02   |K (input)|


Output messages from _Pseudomarker_ can be somewhat cryptic. In particular, “this pedigree is not whole” requires some explanation – it refers to the fact that for a given family, relationships are incorrectly specified. For example, a child may have only one parent, or an individual has no relation with any other individual with the same family ID.

Executing RunPM
---------------

Written in [Pascal](https://freepascal.org/), this program is available as a [Linux executable](https://github.com/jurgott/RunPM). It has been tested in 64-bit Ubuntu. Here are brief instructions on its use.

Start with your family data in [plink format](http://pngu.mgh.harvard.edu/%7Epurcell/plink/plink2.shtml) so you will have files like _data.map_ and _data.ped_. Make certain that family members are contiguous although, within a family, individuals can occur in any order. Each individual is characterized by a family ID (FID) and individual ID (IID). The FID-IID combination must be unique, but the same IIDs may occur in different families. Initially, try to find mendel errors with the plink option, `--mendel`, and make corrections as needed. Below, substitute _data_ with whatever file names you are using. Then run _plink_ with the following command line options:

`--file data --recode 12 transpose --output-missing-phenotype 0 --out data2`

where you may choose any name for _data_ and _data2_. This operation will result in two new files, _data2.tped_ and _data2.tfam_, which are required as input files to the _RunPM_ program; the latter will ask for an input file name to which you should reply _data2_ (or whatever name you have been using). Then simply follow instructions or modify the enclosed _runPM.param_ parameter file. It is highly recommended to initially run 5-10 markers in the foreground to verify that the program runs fine and the output file generated looks ok. The output file name will be, for example, _resPMdata2D1.out_, where _D1_ refers to dominant inheritance and analysis model 1 (affecteds only). Each new marker analyzed will write a new line to the output. Markers with mendel errors and monomorphic markers are skipped, where the latter are defined as having fewer than 2 minor alleles in all families combined, that is, the data must contain at least 2 minor alleles. Also, the minimum number of known alleles (alleles numbered 1 or 2) must be 4. If everythig looks good, submit the data for analysis as shown below.

In _plink_, the `--output-missing-phenotype 0` option is important; without it, unknown phenotypes will be coded as -9. In addition to the options listed above you may want to use the `--geno` option to eliminate variants with call rates less than 0.90, for example, which is the _plink_ default, `--geno 0.1`. Use of the `--maf` option is discouraged as linkage analysis has no problem with low marker allele frequencies. Another useful option is to run only one set of chromosomes, for example, chromosomes 1-3, which would be specified with `--chrom 1-3`.

The _RunPM_ program repeatedly calls the _Pseudomarker_ program and expects relevant program files to reside in a specific folder (directory). The following executables must reside in a folder named in the parameter file: _pseudomarker_, _unknownpseudo_, _makepedpseudo_, _ilinkpseudo_.

These files are included in the _pseudomarker_ download package. The _RunPM_ program file should be in the `/usr/local/bin` or the `~/bin` folder if the latter folder is in the path accessed by programs. After copying files (with _sudo_ privileges) to `/usr/local/bin`, make sure they are executable; otherwise, type `cd /usr/local/bin` followed by `sudo chmod +x *`.

In some situations, every marker leads to an error, in which case the output file will be empty save for some header lines. It may then be useful to capture screen output, for example, by invoking the program as `RunPM < runPM.param > output.txt`, where _runPM.param_ is a small text file holding input parameter values. Please be prepared to expect long running times with _pseudomarker_. In a recent analysis of 1,100 individuals in small to medium-sized families, _RunPM_ took 2.5 seconds to analyze one marker. Thus, analyzing 700,000 markers takes just about 4 weeks of uninterrupted runnning.

Parameter file
--------------

Below is an example of a parameter file. The program is preferably run as, for example, `RunPM < runPM.param`, where the latter file contains the following lines, and _joe_ is your login:

```
Tun
d        dominant (d) or recessive (r) analysis
2        model 1 (affecte only) or 2 (incomplete penetrance) or 3 (full penetrance)
1 0      start and end line numbers of markers to process
/home/joe/
/usr/local/bin/

```


EXPLANATIONS  
Each line in the above sample parameter file has the following meaning:

1.  Name of input file, without ".tfam" and ".tped"
2.  Dominant (d) or recessive (r)
3.  Model 1 (affected only) or 2 (incomplete penetrance) or 3 (full penetrance). With model 3, a second number after "3" is expected, which is the disease prevalence, for example, 0.001. Occasionally, full penetrance leads to mendel errors for the trait, in which case penetrances may be specified as 0.0001 and 0.9999 rather than 0 and 1. If this is desired, then the prevalence should be furnished as a negative number, for example, -0.001. The program will read this as a prevalence of 0.001 with penetrances of 0.0001 and 0.9999.
4.  Start and end line numbers of markers to process. For example, 200 3000 means that markers #200 - #3000 will be processed. To run all markers, set these numbers equal to 1 0. You may want to initially specify a small number of markers just to see whether the program works correctly.
5.  A directory to which the program will print a progress report for every 10 markers processed so that you will see how many markers total have been analyzed at any given time. Here, this is your home directory, for example, `/home/joe`
6.  A directory containing the executable programs: pseudomarker, unknownpseudo, makepedpseudo, ilinkpseudo. In some installations, the programs must be in the path, for example, in /usr/local/bin/.
7.  The normal situation is that this line is blank. If it contains the number 1 then a file with statistical details on program execution will be saved for each marker. The file names will be PPPn.out, where n is the sequential number of each marker. Each file will occupy 4 KB, so this feature should not be used for too many markers.

Sample data
-----------

Included in the [RunPM program package](https://github.com/jurgott/RunPM) is a small test dataset, _Tun.tfam_ and _Tun.tped_. It may be run by typing `RunPM < runPM.param`. The generally recommended approach is to run it unattended by typing `(runPM < runPM.param > /dev/null)&`, after which the program will keep running even after you log out. Interrupting/killing the program once it started running in the background is a little tricky because _RunPM_ keeps invoking the various _pseudo_ routines in succession, so it is best to know that the program runs fine before you submit a long run in the backgrund. After completion, the program will leave a file, _ppp.out_, in your work directory. This file refers to the marker last run and may be deleted if you wish. It provides detailed information on the calculations performed.

The two relevant output files are _resPMTunD2.out_ and _resPMTunD2.log_. The _out_ file is preferably read into a spreadsheet. It contains 11 columns described below (a few columns have been omitted as not being relevant). Traditionally, the likelihood L1, maximized over θ, is expressed as a lod score, Z = log10(L1/L0), where L0 is the likelihood under the null hypothesis of no linkage. In statistics, that likelihood ratio is usually transformed into chi-square, _c_ \= 2 loge(L1/L0). In the table below, to make results for all hypotheses tested comparable, the _p_\-value formulation is used.



* num: chr
  * Sequential number of markers in a given run. This number refers to all markers in the input file, whether or not calculations were carried out.: Chromosome number for given marker
* num: Marker
  * Sequential number of markers in a given run. This number refers to all markers in the input file, whether or not calculations were carried out.: Marker ID, rs number
* num: bp
  * Sequential number of markers in a given run. This number refers to all markers in the input file, whether or not calculations were carried out.: Basepair location of marker. The next 3 columns (cM, Phenotype, Model) may safely be deleted as they are not needed.
* num: LOD
  * Sequential number of markers in a given run. This number refers to all markers in the input file, whether or not calculations were carried out.: Maximum lod score attained by given marker over recombination fractions, log10(L ratio) of H0 vs H1
* num: LD|Linkage
  * Sequential number of markers in a given run. This number refers to all markers in the input file, whether or not calculations were carried out.: p-value for log10(L ratio) of H1 vs H3
* num: LD|NoLinkage
  * Sequential number of markers in a given run. This number refers to all markers in the input file, whether or not calculations were carried out.: p-value for log10(L ratio) of H0 vs H2
* num: Linkage|LD
  * Sequential number of markers in a given run. This number refers to all markers in the input file, whether or not calculations were carried out.: p-value for log10(L ratio) of H2 vs H3, that is, testing for linkage while allowing for LD
* num: Linkage+LD
  * Sequential number of markers in a given run. This number refers to all markers in the input file, whether or not calculations were carried out.: p-value for log10(L ratio) of H0 vs H3, combined effect of linkage and LD
* num: all1
  * Sequential number of markers in a given run. This number refers to all markers in the input file, whether or not calculations were carried out.: Number of "1" (minor) alleles seen in all individuals. Not used in calculations, for information only.
* num: all2
  * Sequential number of markers in a given run. This number refers to all markers in the input file, whether or not calculations were carried out.: Number of "2" (major) alleles seen in all individuals. Not used in calculations, for information only.


The _p_\-values are computed from the corresponding chi-square distribution (1 df except for Linkage+LD). Some of the likelihood ratios can be very large and lead to very small _p_\-values, which will underflow and will be shown as “0”. Such _p_\-values should be interpreted as _p_ < 1.5 x 10\-45, corresponding to LOD > 43.

The _pseudomarker_ program outputs detailed information about maximum likelihoods and estimated allele frequencies in a file called _ppp.out_ but this file is overwritten by _RunPM_ for each new marker, except it will be saved for the last marker in the output file. To view detailed output files for a given set of markers, it is best to do a separate run just for these markers and set the last line of the parameter file equal to 1 (see above).

Note that, considering the multiple testing involved in genome-wide linkage analysis, _p_ < 0.05 corresponds to a maximum lod score of at least 3.3 \[4\], which corresponds to chi-square = 15.2 and an empirical one-sided significance level of _p_ \= 4.8 x 10\-5.

1\. Gertz EM, Hiekkalinna T, Digabel SL, Audet C, Terwilliger JD, et al. (2014) PSEUDOMARKER 2.0: efficient computation of likelihoods using NOMAD. BMC Bioinformatics 15: 47.

2\. Goring HH, Terwilliger JD (2000) Linkage analysis in the presence of errors IV: joint pseudomarker analysis of linkage and/or linkage disequilibrium on a mixture of pedigrees and singletons when the mode of inheritance cannot be accurately specified. Am J Hum Genet 66: 1310-1327.

3\. Hiekkalinna T, Schaffer AA, Lambert B, Norrgrann P, Goring HH, et al. (2011) PSEUDOMARKER: a powerful program for joint linkage and/or linkage disequilibrium analysis on mixtures of singletons and related individuals. Hum Hered 71: 256-266.

4\. Lander E, Kruglyak L (1995) Genetic dissection of complex traits: guidelines for interpreting and reporting linkage results. Nat Genet 11: 241-7.
