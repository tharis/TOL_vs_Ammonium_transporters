# TOL_vs_Ammonium_transporters

An attempt to compare a tree of ammonium transporting proteins with Banfields tree of life

https://doi.org/10.1038/nmicrobiol.2016.48

First extracted as many names a possible from the leaves of the tree.

Then added ammonium to the end and searched uniprot to find possible ammonium transporters. From this search the uniprot IDs were downloaded.

These were then used to search uniprot again to retrieve proteins with 11 predicted TM regions. Rhesus proteins have 12 TM regions with the first being the xtra one. The last 11 TM regions of any protein that had 12 TM helices was downloaded in these instances.

Then 11 files were generated containing the TM regions for each protein.

The headers were adapted to reflect a uniprot header with the start and end point of each section.

These protein sequences were then run through ncfp to convert the aa sequences to genomic DNA. 
The amino acid file of the ncfp output were then aligned and the nucleotide files were back threaded onto this alignment.
Then the nucleotide alignments were trimmed using trimal autotrim


Following this the regions were concatenated duplicates were deleted and an evolutionary model was generated for the whole sequence length using modestest-ng.



This was then used to generate a tree using Raxml-ng.
