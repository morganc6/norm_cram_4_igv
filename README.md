
# norm_cram_4_igv
normalisation of cram files for visualations in IGV
this process was put together with the goal of comparing normalized alignments between individauls so structural variants  could be visually validated on IGV
Using igv version(X>) allows you to normalize files using the tdf input


## Programs needed for execution
bedtools,samtools, igvtools (version X>) , igv (version X>)


## CRAM to BAM with extraction of

sample=$1
chr=$2
reference_fa=genome.fam
mygenome=hg19.chrom.sizes

# convert bam to cram using samtools.
## to do this for a specific chromosome or set of co-ordinates add this as the last arugment. 

'''
samtools view -b -T $reference_fa $sample.cram $chr > $sample.bam
'''

# convert bam file to bed file with with bedtools
'''
bamToBed -i $sample.bam > $sample.bed
'''

# convert bed file to wiggle file with bedtools
'''
genomeCoverageBed –i $sample.bed -bg –g $mygenome > $sample.wig
'''

#convert wiggle file to tdf file using igvtools
'''
igvtools toTDF $sample.wig $sample.tdf hg19
'''
