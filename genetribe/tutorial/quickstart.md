# 3 Quick Start

We provide example files, stored in ```test```, to help users quickly run **genetribe**.

## 3.1 Core workflow using .fasta, .bed, and .chrlist
```sh
cd test/
../genetribe core -l aet -f rice
2020-05-04 21:22:52 ...prepared the required original files...
2020-05-04 21:39:19 ...calculating BSR, CBS and Penalty...
2020-05-04 21:40:07 ...evaluating the optimal alpha for weighted collinearity score...
2020-05-04 21:41:05 ...aerfa = 90...
2020-05-04 21:41:05 ...weighted the original score...
2020-05-04 21:41:07 ...processed each pair of chromosomes...
2020-05-04 21:41:07 ...number of rounds: 1 ...
2020-05-04 21:41:11 ...merged the results...
2020-05-04 21:41:12 ...done!
```
**genetribe** will save the output files in the current working directory
```sh
ls
aet_rice.one2many   rice_aet.one2many     aet_rice.one2one
rice_aet.one2one    aet_rice.RBH          aet_rice.SBH
rice_aet.SBH        aet_rice.singleton    rice_aet.singleton
```


## 3.2 Homolog inference for same assembly using .bed .genelength
```sh
cd test/
../genetribe sameassembly -l IWGSCv1p1 -f IWGSCv1
2020-05-04 21:45:42 ...prepared the required original files...
2020-05-04 21:45:42 ...Obtain the overlapping length between genes...
2020-05-04 21:45:42 ...calculate match score...
2020-05-04 21:45:42 ...Obtain RBH and SBH gene pair...
2020-05-04 21:45:43 ...Done!
```
**genetribe** will save the output files in the current working directory
```sh
ls
IWGSCv1_IWGSCv1p1.one2many  IWGSCv1_IWGSCv1p1.one2one
IWGSCv1p1_IWGSCv1.one2many  IWGSCv1p1_IWGSCv1.one2one
```
