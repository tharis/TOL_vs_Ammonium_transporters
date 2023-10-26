# TOL_vs_Ammonium_transporters

An attempt to compare a tree of ammonium transporting proteins with Banfields tree of life

https://doi.org/10.1038/nmicrobiol.2016.48

First extracted as many names as possible from the leaves of the tree from untitled1.txt using parse_tol.ipynb.

Then added ammonium to the end and searched uniprot to find possible ammonium transporters. From this search the uniprot IDs were downloaded.

These were then used to search uniprot again to retrieve proteins with 11 predicted TM regions. Rhesus proteins have 12 TM regions with the first being the xtra one. The last 11 TM regions of any protein that had 12 TM helices was downloaded in these instances.
Entry 'Q5ANJ4' has multiple records in teh result handle which crashes the TM region search, since it is not an ammonium trnasporter it was deleted from the list of ID to search.

Then 11 files were generated containing the TM regions for each protein.

The headers were adapted to reflect a uniprot header with the start and end point of each section.

Above steps using bioservices.ipynb

These protein sequences were then run through ncfp to convert the aa sequences to genomic DNA. 

    ncfp -v -s -l file.log file.fasta file.out email -d caches/new_ncfp -c ncfp_cache

This generates a cache of nucleotide sequences for each entry meaning they do not need to be downloaded seperately for every file. To reuse cache

    --file_stem cached --keepcache

The amino acid file of the ncfp output were then aligned using MAFFT

    mafft file.in > file.out

To backthread the alignment onto the nucleoide sequences the headers needed to match. To do this all information following the first space was deleted

    awk -F"/" '/^>/{print $1; next}1' in.fasta > out.fasta

was used to do this.

The backthread was done using tcoffee

    t_coffee -other_pg seq_reformat -in nt-file -in2 alignment-file -action +thread_dna_on_prot_aln -output fasta > outfile 


Then the nucleotide alignments were trimmed individually using trimal autotrim but before sequences with special characters were identified

>tr|A0A562BXM4|A0A562BXM4_9GAMM Has S and Y in helix 4
>sp|P69680|AMTB_ECO57 Has R in helix 11

which were manually deleted 
  
    trimal -in file.fasta -out file.fasta -automated1

Following this the regions were concatenated 

    seqkit concat file1 file2.. > outfile

duplicates were deleted 

    seqkit rmdup -s in.file > out.file

and an evolutionary model was generated for the whole sequence length using modestest-ng

     modeltest-ng -i helixconcat.fasta

The files was checked to see if raxml could read it

    raxml-ng --check --msa in.fasta --model 'model' --prefix 01_check
  
The file generated was then converted into binary to speed up raxml reading it 

    raxml-ng --parse --msa 01_check.raxml.reduced.phy --model 'model' --prefix 02_parse

A tree was then generated with 100 bootstraps 

    raxml-ng --msa 02_parse.raxml.rba --model 'model' --all --bs-trees 100 --seed 1659012405 --prefix 03_bootstrap

The model generated for the full length sequence was 'TMP1uf+G4'. This caused the tree to run very slowly with only one generated after 22hrs. For this reason the third model was chosen 'TIM2_G4'.





This was then used to generate a tree using Raxml-ng.
