# 2 Installation

## 2.1 Dependencies

**GeneTribe** is implemented in `python3`, and three external command-line tools, which are executable on your `PATH`,  are needed:
- [blast](https://blast.ncbi.nlm.nih.gov/Blast.cgi?)     
the best installing way: `conda install blast -c bioconda`
- [MCscan](https://github.com/tanghaibao/jcvi/)    
the best installing way: `pip install jcvi`
- [bedtools](https://bedtools.readthedocs.io/en/latest/)  
the best installing way: `conda install bedtools -c bioconda`

## 2.2 Installation

Before installation, make sure the above three tools have been installed.
The easy way to obtain **genetribe**:

1. Create a directory to work in.
```sh
mkdir genetribe_dir
cd genetribe_dir
```

2. Download the latest version of **genetribe** from [github](https://github.com/chenym1/genetribe)
```sh
git clone git@github.com:chenym1/genetribe.git
```

3. Run script to install **genetribe**
```sh
cd genetribe
./install.sh
```

4. add **genetribe** to your system PATH if necessary
```sh
vi ~/.bash_profile
# add the following lines to the end of ~/.bash_profile
export PATH=/path/to/genetribe/:$PATH
source ~/.bash_profile
```

5. Print help file of **genetribe**
```
genetribe -h
```