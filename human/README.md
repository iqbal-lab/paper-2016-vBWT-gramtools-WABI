
gramtools current implementation will not scale to mapping to human in reasonable time.
So we simply measured time to create the vBWT


The command for creating the human PRG from the 1000G Phase3 VCF is:

perl gramtools/utils/vcf_to_linear_prg.pl 
--vcf  ALL.wgs.phase3_shapeit2_mvncall_integrated_v5a.20130502.sites.vcf 
--ref  Homo_sapiens.GRCh37.60.dna.WHOLE_GENOME.fa --min_freq 0.05

The VCF can be obtained from the 1000 Genomes ftp site

http://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release/20130502/ALL.wgs.phase3_shapeit2_mvncall_integrated_v5b.20130502.sites.vcf.gz



The commandline used to load the human PRG was

gramtools/src/gramtools 
--prg  human_prg_min_freq_0.05 
--csa csa --input test.fastq 
--ps dummy1 --pa dummy2 --co covg --ro reads --po binfile --log memlog 
--ksize 9 --kfile kmers

All of the arguments except the first are effectively dummies.

We used syrupy (https://github.com/jeetsukumaran/Syrupy ) to monitor RAM usage during construction, plus
the automated memory monitor that comes with the SDSL library