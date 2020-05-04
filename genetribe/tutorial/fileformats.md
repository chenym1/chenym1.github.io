# 4 File formats
Here we have described in detail the acquisition method and formats of the input files and the output files.
## 4.1 Files of core
The core workflow of genetribe
## Input files
### File1 Protein sequences in fasta format
1. One of input files of **genetribe** is [fasta](https://en.wikipedia.org/wiki/FASTA_format) format file that stores protein sequences.
You can obtain the fasta file from [Emselbl](https://www.ensembl.org/) or [NCBI](https://www.ncbi.nlm.nih.gov/).
Their file name is generally ```name.pep.all.fa.gz```.
2. We decompress them all using the command ```gunzip name.pep.all.fa.gz```, and rename it as ```name.fa```, **genetribe**  extracts longest transcript per gene as the representative sequences.
3. Repeat the above steps for another genome.
<br>**example:**
```sh
>AT2G27490.4 pep chromosome:TAIR10:2:11748087:11749153:-1 gene:AT2G27490 transcript:AT2G27490.4 gene_biotype:protein_coding
MRIVGLTGGIASGKSTVSNLFKASGIPVVDADVVARDVLKKGSGGWKRVVAAFGEEILLP
SGEVDRPKLGQIVFSSDSKRQLLNKLMAPYISSGIFWEILKQWASGAKVIVVDIPLLFEV
KMDKWTKPIVVVWVSQETQLKRLMERDGLSEEDARNRVMAQMPLDSKRSKADVVIDNNGS
LDDLHQQFEKVLIEIRRPLTWIEFWRSRQGAFSVLGSVILGLSVCKQLKIGS
```
Note:  
In this example. **genetribe** uses lines where the greater-than ">" is to distinguish sequences and extract longest transcript per gene. genetribe uses the ID that starts with ```gene:``` to get ID "AT2G27490". If this ID does not exist, genetribe uses the ID splited by separator ```.```. The separator is used to distinguish gene ID from transcript ID and specified by parameters ```-s``` in ```genetribe core```.


### File2 Annotation file in bed format
1. One of input files of **genetribe** is [bed](https://en.wikipedia.org/wiki/BED_(file_format)) format file that stores annotation information. First, you can obtain the [gff](https://en.wikipedia.org/wiki/General_feature_format) file from [Emselbl](https://www.ensembl.org/) or [NCBI](https://www.ncbi.nlm.nih.gov/). Their file name is generally ```name.gff3.gz```.
2. We decompress them all using the command ```gunzip name.gff3.gz```, and rename it as ```name.gff3```.
3. We convert ```gff``` file to ```bed``` file that only contains gene line. there are two ways to choose(of course, there are more ways):<br>
  MCscan + sed:
  ```sh
  python -m jcvi.formats.gff bed --type=gene --key=ID name.gff3 -o name.bed
  sed -i 's/gene://g' name.bed
  ```
  gff2bed + gawk + sed:
  ```sh
  gff2bed < name.gff3 | gawk -vOFS="\t" '{if($8=="gene")print $1,$2,$3,$4,$5,$6}' | sed 's/gene://g' > name.bed
  ```
**example:**
```sh
1       3630    5899    AT1G01010       0       +
1       6787    9130    AT1G01020       0       -
1       11648   13714   AT1G01030       0       -
1       23120   31227   AT1G01040       0       +
1       31169   33171   AT1G01050       0       -
1       33364   37871   AT1G01060       0       -
1       38443   41017   AT1G01070       0       -
1       44969   47059   AT1G01080       0       -
1       47233   49304   AT1G01090       0       -
```
Each column is chromosome, chromosome start, chromosome end, name of gene, score and strand.  
Note:  
For some annotations, the fourth column of ```bed``` contains some strings, such as ```gene:```, ```ID:```, etc., we should delete.
### File3 Chromosome Group information
In this part, we should specify the chromosome name characteristics of each subgenome of the species genome, and name it ```name.chrlist```.
<br>**example:**<br>
Triticum aestivum IWGSC RefSeqv1:
```sh
cat Triticum_aestivum.chrlist
chrNA,chrNB,chrND
```
Triticum urartu Tu2.0:
```sh
cat Triticum_urartu.chrlist
TuN
```
Oryza sativa Japonica IRGSP v1
```sh
cat Oryza_sativa_Japonica.chrlist
N
```
Note:<br>
The chromosome name in ```.chrlist``` must be the same as in chromosome name in the ```bed```.

### File4 Gene annotation confidence (Optional)
This file is only needed when we specified parameters ```-c``` in ```genetribe core```.
1. The confidence ( HC / high confidence, LC / low confidence ) can be found in ```gff``` or name ```fasta``` ( ```name.HC.fasta```,```name.LC.fasta```) downloaded from the database.
2. Extract two columns of information, gene ID and confidence, and name it ```name.confidence```.
<br>**example:**<br>
```sh
TraesCS1A02G000100      HC
TraesCS1A02G000200      HC
TraesCS1A02G000300      HC
TraesCS1A02G000100LC    LC
TraesCS1A02G000200LC    LC
TraesCS1A02G000300LC    LC
```

## Output files
<center>Table 1 Output files of the core workflow</center>

Output files | Contents  
-|-
.one2many | It can also be called the ```.many2many``` file, which contains all homologous gene pairs, there are three columns of information, namely the gene in genome 1, the gene in genome 2 and the match score
.one2one | The RBH+SBH, which contains four columns of information, namely the gene in genome 1, the gene in genome2, the homologous type (RBH/SBH) and the chromosome group of gene in genome 2
.RBH | The gene pairs belonging to the Reciprocal Best Hits
.SBH | The genes pairs belonging to the Single-side Best Hits, where RBH is not found but the best matching gene is found
.singleton | The genes with no homologous genes

## 4.2 Files of sameassembly
The Homolog inference for same assembly
## Input files
### File1 Annotation file in bed format
The file is the same as bed in ```core```.
### File2 Length of genes 
We can obtain it from ```bed``` using the following command (of course, there are more ways):
```sh
cat name.bed | gawk -vOFS="\t" '{print $4,$3-$2}' > name.genelength
```
## Output files

<center>Table 2 Output files of the genereibe sameassembly</center>

Output files | Contents  
-|-
.one2many | It can also be called the ```.many2many``` file, which contains all gene pairs with overlapping positions, there are three columns of information, namely the gene in genome 1, the gene in genome 2 and the match score
.one2one | The RBH+SBH, which contains three columns of information, namely the gene in genome 1, the gene in genome2 and the homologous type (RBH/SBH)