

## Make the PRG

The VCF from which we built the whole genome PRG is 7G8_prg.vcf - it's simply a callset made with Cortex
from the 7G8-GB4 genetic cross of Plasmodium falciparum (see http://biorxiv.org/content/early/2015/12/23/024182 for
links to the raw data)

The reference is the standard 3D7 reference used for P. falciparum, included here:

This makes the PRG:
perl gramtools/utils/vcf_to_linear_prg.pl --vcf 7G8_prg.vcf  --ref Pf3D7_v3.fa --outfile wg

It makes a PRG file called wg, and it also makes a second identical file except it has
a fasta header line above, called wg.fa, which is needed in the next command


## Make some auxiliary files:

This generates a list of kmers in the PRG, which is needed as an input:

python gramtools/utils/variantKmers.py -f wg.fa -k 9 -n > wg.all.kmers



## Generate simulated haplotype

python gramtools/utils/variantKmers.py -r 10000 -k 9 -f wg.fa > deleteme.tmp.kmers

generates randomGenome.fa and randomReads.fa


## Generate index/precalculate SA intervals for vBWT

mkdir temp

gramtools/src/gramtools --prg wg  --csa temp/csa --input dummy.fastq --ps wg.mask_sites --pa wg.mask_alleles --co temp/covg --ro temp/read --po temp/bin --log temp/log --ksize 9 --kfile wg.sites.kmers > temp/out



## Do the actual mapping

gramtools/src/gramtools --prg wg --ps wg.mask_sites --pa wg.mask_alleles --ksize 9 --kfile wg.sites.kmers --input randomReads.fq --csa csa --co covg --ro readinfo --po binfile --log memlog 


