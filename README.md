# manuka_NLR
**These are the steps involved in identifying the NBS-LRR (NLR) complement in the Manuka (*Leptospermum scoparium*) genome.**

1. hmmer/3.2 was used with the nhmmer function and DNA profile hidden markov models (EG_hmm) derived from *Eucalyptus grandis* alignments of the conserved nucleotide binding domain shared by Apaf-1, Resistance proteins and CED4 (NBARC) from coiled-coil (CC) NLR (here named nonTIR) and Toll interleukin-1 (TIR) NLR sequences. These EG_hmms were screened against the Ls_genome.fasta and the Ls_coding.fasta.

```
nhmmer EG_nonTIRhmm Ls_genome.fasta >Ls_nonTIRout
nhmmer EG_TIRhmm Ls_genome.fasta >Ls_TIRout
```

2. bed formatted files were created with the identified sequences (in the default hmmer inclusion list) in manuka for two sets: coding and non-coding regions.

3. bedtools/2.25.0 was used to extract sequences from genome and coding fasta files

```
bedtools getfasta -s -fi Ls_genome.fasta -bed TIR.bed -fo Ls_TIR_TIR.fasta
```

This process identified 710 total sequences (including 98 coding sequences) that have TIR-type NBARC domains matching the *E. grandis*. The process identified 379 sequences (none of them coding sequences) that match the nonTIR NBARC domains.

4. Species-specific profile hidden Markov models were made with the 98 TIR-type coding sequences and the 379 nonTIR-type (non-coding sequences) in accordance with the HMMER user manual. That is, multiple sequence alignments made and output in stockholm format. These were used as input files to build the profile HMM.

```
hmmbuild -nucleic Ls_nonTIRhmm clustalo-Ls_nonTIR.stockholm
hmmbuild -nucleic Ls_TIRhmm clustalo-Ls_TIR.stockholm
```

5. Step 1,2 and 3 from the species-specific HMM extracted stranded NBARC sequences from the genome (Ls_TIRall.fasta and Ls_nonTIRall.fasta). NB. Headers for the extracted fasta have (+) for forward strand and (-) for reverse for the location on the scaffold specified.  

6. All Ls_TIR and Ls_nonTIR sequences extracted with the species-specific hmm were combined into a single Ls_NBARC.fasta. Total sequences were: 1581.

7. Nucleotide NBARC sequences were 6-frame translated using and adapted Bioseq script and the longest ORF frame output to fasta for downstream analysis. Total sequences from this set, containing the Walker A "GKT" and/or Walker B "LDD" strings of amino acids were extracted 1240. Then any sequences with no "LDD" were removed (974 sequences retained). 

8. 246 CC NBARC sequences with the canonical "W" tryptophan within the "LDD*W" were identified (LDDVW = 122, LDDLW = 72, LDDIW = 18, LDDTW = 6, LDDAW = 16, LDDMW = 12). All 974 sequences were aligned with five E. grandis NBARC sequences, 3 nonTIR and 2 TIR using default parameters with MUSCLE in Mega X. A neighbour-joining tree (using defaults) was constructed in Mega X.

8. Branches were coloured blue for TIR_NBARC-type and red for nonTIR_NBARC-type. E. grandis TNL sequences denoted with blue dot and nonTNL with red dot.
