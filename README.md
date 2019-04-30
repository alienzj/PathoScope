PathoScope 2.0
==========

### Pathoscope: Species identification and strain attribution with unassembled sequencing data

[![install with bioconda](https://img.shields.io/badge/install%20with-bioconda-brightgreen.svg?style=flat)](http://bioconda.github.io/recipes/pathoscope/README.html)

#### Introduction
Pathoscope 2.0 consists of four core and two optional analysis modules for sequencing-based metagenomic profiling. The PathoLib module extracts genome reference libraries (target or host/filter) from all available sequences in the NCBI Nucleotide database that belong to a user-defined taxonomic clade. The PathoMap module aligns the reads to the target reference library and removes any reads that have sequence similarity with the host or filter genomes. PathoID resolves read ambiguity, identifies which of the target genomes are present in the sample and estimates the proportions of reads originating from each genome. PathoReport provides two report files: 1) a summary report (.tsv) that contains the numbers and proportions of reads aligned to each genome identified in the sample, and 2) detailed report (.xml) including read coverage, read assignments, and contiguous sequences generated by combining the reads. The PathoDB is an optional module that provides additional annotation (organism taxonomic lineage, gene loci, protein products) for all sequences identified in the sample. The PathoQC module can be used to preprocess the reads prior to alignment with PathoMap.


#### 1A. Installation (conda)

Recommended installation method is using [**Bioconda**](https://bioconda.github.io). See documentation to activate Bioconda channel.

To create a new conda environment including PathoScope:

```
conda create -n pathoscope python=2.7 pathoscope

# Activate this environment:
conda activate pathoscope
```

This will install dependencies including samtools (<1.0) and bowtie2. Otherwise, to install PathoScope into your current python 2.7 environment:

```
conda install pathoscope
```

#### 1B. Installation (python) 

1B.1 Prerequisite: Need to have python 2.7.3 or later version installed and add python to your PATH variable (Usually already done as part of python installation)
    
1B.2 Change directory to where you downloaded the code 

1B.3 Simply run `python setup.py install` if you want to install globally or  
simply run `python setup.py install --user` if you want to install for the local user.  
If you have never installed another python module before or you do not have python setuptools  
simply run `python install.py`, which will use ez_setup.py to bootstrap the right version of the package installer tooling for you.


#### 2. Testing installation

Simply run `pathoscope -h` after installation as above or  
`python pathoscope/pathoscope2.py -h` for top level usage information.

- Run `pathoscope LIB -h` for detailed usage information to run patholib.
- Run `pathoscope MAP -h` for detailed usage information to run pathomap.
- Run `pathoscope ID -h` for detailed usage information to run pathoid.
- Run `pathoscope REP -h` for detailed usage information to run pathoreport.  
If you have installed the PathoQC submodule:
- Run `pathoscope QC -h` for detailed usage information to run pathoqc.

There are also some unit tests for testing the validity of the functions. 

- Change directory to `pathoscope/pathomap/bowtie2wrapper/unittest` and simply run `python testBowtie2Wrap.py`.
- Change directory to `pathoscope/pathoid/unittest` and simply run `python testPathoID.py`.

Note: since NCBI is migrating from GI to TI taxon ids one should use the following script https://github.com/microgenomics/pasteTaxID to convert the ids prior to running the LIB command. 

#### 3. PathoQC

Optional: Download PathoQC (pathoqc_vXXX.tar.gz) from <http://sourceforge.net/projects/pathoscope/>
    Install "pathoqc" sub-directory under "pathoscope" directory.

####  4. Output TSV file format

At the top of the file in the first row, there are two fields called "Total Number of Aligned Reads" and "Total Number of Mapped Genomes". They represent the total number of reads that are aligned and the total number of genomes to which those reads align from the given alignment file.

Columns in the TSV file:

1. **Genome:**  
This is the name of the genome found in the alignment file.
2. **Final Guess:**  
This represent the percentage of reads that are mapped to the genome in Column 1 (reads aligning to multiple genomes are assigned proportionally) after pathoscope reassignment is performed.
3. **Final Best Hit:**  
This represent the percentage of reads that are mapped to the genome in Column 1 after assigning each read uniquely to the genome with the highest score and after pathoscope reassignment is performed.
4. **Final Best Hit Read Numbers:**  
This represent the number of best hit reads that are mapped to the genome in Column 1 (may include a fraction when a read is aligned to multiple top hit genomes with the same highest score) and after pathoscope reassignment is performed.
5. **Final high confidence hits:**  
This represent the percentage of reads that are mapped to the genome in Column 1 with an alignment hit score of 50%-100% to this genome and after pathoscope reassignment is performed.
6. **Final low confidence hits:**  
This represent the percentage of reads that are mapped to the genome in Column 1 with an alignment hit score of 1%-50% to this genome and after pathoscope reassignment is performed.
7. **Initial Guess:**  
This represent the percentage of reads that are mapped to the genome in Column 1 (reads aligning to multiple genomes are assigned proportionally) before pathoscope reassignment is performed.
8. **Initial Best Hit:**  
This represent the percentage of reads that are mapped to the genome in Column 1 after assigning each read uniquely to the genome with the highest score and before pathoscope reassignment is performed.
9. **Initial Best Hit Read Numbers:**  
This represent the number of best hit reads that are mapped to the genome in Column 1 (may include a fraction when a read is aligned to multiple top hit genomes with the same highest score) and before pathoscope reassignment is performed.
10. **Initial high confidence hits:**  
This represent the percentage of reads that are mapped to the genome in Column 1 with an alignment hit score of 50%-100% to this genome and before pathoscope reassignment is performed.
11. **Initial low confidence hits:**  
This represent the percentage of reads that are mapped to the genome in Column 1 with an alignment hit score of 1%-50% to this genome and before pathoscope reassignment is performed.

####  5. License: GNU-GPL

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more details.
    
You should have received a copy of the GNU General Public License along with this program.  If not, see <http://www.gnu.org/licenses/>.

####  6. Support and Contact

Pathoscope is developed at the Johnson Lab in Boston University and the Crandall Lab at George Washington University.  

W. Evan Johnson, Ph.D.  
Division of Computational Biomedicine  
Boston University School of Medicine  
72 E. Concord St., E-645  
Boston, MA 02118  

Keith A. Crandall, Ph.D.  
Computational Biology Institute  
George Washington University  
800 22nd Street NW, Suite 7000D  
Washington, DC 20052  



Developers:

Solaiappan Manimaran (Johnson)  
Changjin Hong (Johnson)
Eduardo Castro-Nallar (Crandall)
Matthew Bendall (Crandall)

####  7. References

1. Owen E. Francis, Matthew Bendall, Solaiappan Manimaran, Changjin Hong, Nathan L. Clement, Eduardo Castro-Nallar, Quinn Snell, G. Bruce Schaalje, Mark J. Clement, Keith A. Crandall and W. Evan Johnson "Pathoscope: Species identification and strain attribution with unassembled sequencing data." Genome research 23.10 (2013): 1721-1729. [PMID: 23843222](http://www.ncbi.nlm.nih.gov/pubmed/23843222)  
2. Changjin Hong, Solaiappan Manimaran, Ying Shen, Joseph F Perez-Rogers, Allyson L Byrd, Eduardo Castro-Nallar, Keith A Crandall and William Evan Johnson "PathoScope 2.0: a complete computational framework for strain identification in environmental or clinical sequencing samples." Microbiome 2.1 (2014): 1-15.[PMID: 25225611](http://www.ncbi.nlm.nih.gov/pubmed/25225611)  
3. Allyson L Byrd, Joseph F Perez-Rogers, Solaiappan Manimaran, Eduardo Castro-Nallar, Ian Toma, Tim McCaffrey, Marc Siegel, Gary Benson, Keith A Crandall and William Evan Johnson "Clinical PathoScope: rapid alignment and filtration for accurate pathogen identification in clinical samples using unassembled sequencing data." BMC bioinformatics 15.1 (2014): 262. [PMID: 25091138](http://www.ncbi.nlm.nih.gov/pubmed/25091138)  
4. Changjin Hong, Solaiappan Manimaran and William Evan Johnson "PathoQC: Computationally Efficient Read Preprocessing and Quality Control for High-Throughput Sequencing Data Sets", Cancer Informatics 2014:Suppl. 1 167-176. [PMID: 25983538](http://www.ncbi.nlm.nih.gov/pubmed/25983538)  
