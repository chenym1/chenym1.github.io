# 5 Core Workflow

The core workflow of genetribe, multiple-level homolog-identification pipeline, takes advantages of collinearity block scores and chromosome group information.
## command
```sh
#  genetribe core -h
#  Usage:
#  genetribe core -l <FirstName> -f <SecondName> [options]
#  	
#  Description:
#    The core workflow of genetribe
#    Author:Chen,Yongming; 2020-5-2; chen_yongming@126.com
#  	
#  Options:
#    -h         Show this message and exit
#    -l <str>   Prefix name of first file
#    -f <str>   Prefix name of second file
#    -d <dir>   Pre-computed BLAST file in <dir> [default ./]
#    -r         Calculate chromosome group score [default True]
#    -c         Calculate confidence score [default False]
#    -s <str>   The separator between gene name and number in the header of fasta [default .]
#    -e         E-value of BLASTP [default 1e-5]
#    -n <int>   Number of threads [default 1]
#    -b <float> Threshold based on BSR for filtering Match Score(0-100) [default 75]
```
## Parameters

<center>Table 3 General parameters in genetribe core</center>

Parameter | Description | Default | Required
-|-|-|-
-h | Print brief help message | |
-l | The prefix names of the files for genome 1, such as prefix_name.bed, prefix_name.fa, prefix_name.chrlist and prefix_name.confidence | |yes
-f | The prefix names of the files for genome 2, such as prefix_name.bed, prefix_name.fa, prefix_name.chrlist and prefix_name.confidence| |yes
-d | If you have any of the pre-computed files name1_name2.blast, name2_name1.blast, name1_name1.blast and name2_name2.blast, put it in directory "dir", add the parameter "-d dir" to genetribe core, the program will skip BLAST step | ./ |
-r | Whether to add penalty ratio to the match score according to different subgenome group number | True |
-c | Whether to add penalty ratio to the match score according to gene annotation confidence | False |
-s | The separator is used to distinguish gene ID from transcript ID (for more detail, see File formats) | . |
-e | The e-value in BLASTP. In general, the lower the threshold, the fewer results you get| 1e-5 |
-n | Number of threads. BLASTP will be calculated in parallel, so the number of threads will be divided equally | 1 |
-b | Filtering threshold. The final filtering threshold is 'BSR × Filtering threshold × α' | 75 |