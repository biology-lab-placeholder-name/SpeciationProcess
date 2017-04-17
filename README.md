# Bacteria Speciation Process
some kind words of introduction

## Process

### Preparing the data
probably something about downloading the NCBI cataloge that lists what all they have.
- `species.py` creates species.txt with all species that will be applicable. (all the species with more than 15 complete genomes)
- `folders.py` Prepare one folder for each species. Within that folder, creates folders for other portions of the process, including `genes`, `genomes`, `align` and `BBH`.
- `download.py` Download genomes from NCBI into respective folders
- `unzip.py` unzips the genomes
- Parse GFF
    - `parse_gff_build.py` makes a list in `todo/parse_gff.txt` with all parsing work that needs to be done
    - `parse_gff_multi.py`	creates multiple parallel processes to parse the gff files listed in 'todo/parse_gff.txt' through fasta files into the species' folder. We just utilize .fa


### Resampling analysis

- USEACH
    - `usearch_build.py` makes a list in `todo/usearch.txt` with all usearch comparisons needing to be performed.
    - `usearch_multi.py` creates parallell processes to do the work listed by comparing all the genome pairs with eachother using USEARCH.
- `parse_multiple_usearch.py` Finds the pair of orthologs genes
    - creates input for MCL in  `PATH_TO_OUTPUT/(sp)/input.txt`

***
transfer work to the supercomputer TACC
***
- `launch_mcl.py` Cluster orthologs into families
	- creates input for get_core in `PATH_TO_OUTPUT/(sp)/out.input_(sp).txt.I12`

- `get_core.py` Define the core genome at 85% and extract core proteins into the folder `PATH_TO_OUTPUT/(sp)/align/`
    - generates output of core genomes and puts it in: `PATH_TO_OUTPUT/(sp)/orthologs.txt`
	- also puts all orthologs genes and corresponding ids into `PATH_TO_OUTPUT/(sp)/align/ortho(#)`
	- also generates our list of `selectedSpecies.txt` which will be used for the rest of the process.

- `launch_mafft.py` Align the core proteins with MAFFT.
    - produces output in `PATH_TO_OUTPUT/(sp)/align/(gene).align`
    
- `concat85.py` Merges the core genes into a single alignment
- `raxml_distance.csh` Compute the distances with RAxML
- `sample.py` Remove nearly identical strains and generate random combinations of strains
- `calcHM.py` # Compute the h/m ratios across all the combinations of strains
### Graphing the data
- `graph.py` Generate the input files to generate the graphs
- `big_graph.py` Generate the script for R
- `big_graph.R` Generate the graphs

## Exclusion Criterion
- `distrib.py`
- `kmeans.py`
- `split_kmaens.py`
- `criterion.py`
