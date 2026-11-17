# Homework 3: Pipelines

## Summarize Genome Assembly

Script: [`code/scripts/hw3_genome_summary.sh`](code/scripts/hw3_genome_summary.sh)

I downloaded the *D. melanogaster* genome (FlyBase release r6.66, `dmel-all-chromosome-r6.66.fasta.gz`) and summarized it with `faSize`.

### File Integrity

I compared the file's MD5 checksum to the one published by FlyBase:

```bash
grep dmel-all-chromosome-r6.66.fasta.gz fasta_md5sum.txt | md5sum -c -
```

Result: `dmel-all-chromosome-r6.66.fasta.gz: OK`

### Genome Summary

```bash
faSize dmel-all-chromosome-r6.66.fasta.gz
```

Output summary line:

```
143726002 bases (1152978 N's 142573024 real 142573024 upper 0 lower) in 1870 sequences in 1 files
```

| Measure | Value |
|---|---|
| Total nucleotides | 143,726,002 |
| Total Ns | 1,152,978 |
| Total sequences | 1,870 |

## Summarize an Annotation File

Script: [`code/scripts/hw3_annotation_summary.sh`](code/scripts/hw3_annotation_summary.sh)

I downloaded the *D. melanogaster* GTF annotation (FlyBase release r6.66, `dmel-all-r6.66.gtf.gz`) and summarized it with `bioawk`.

### File Integrity

I compared the file's MD5 checksum to the one published by FlyBase:

```bash
grep dmel-all-r6.66.gtf.gz gtf_md5sum.txt | md5sum -c -
```

Result: `dmel-all-r6.66.gtf.gz: OK`

### Features of Each Type (most to least common)

```bash
bioawk -c gff '{ print $feature }' dmel-all-r6.66.gtf.gz | sort | uniq -c | sort -k1,1nr
```

```
 190176 exon
 163377 CDS
  46856 5UTR
  33778 3UTR
  30922 start_codon
  30862 stop_codon
  30836 mRNA
  17872 gene
   3059 ncRNA
    485 miRNA
    365 pseudogene
    312 tRNA
    270 snoRNA
    262 pre_miRNA
    115 rRNA
     32 snRNA
```

### Genes per Chromosome Arm

```bash
bioawk -c gff '$feature == "gene" && $seqname ~ /^(X|Y|2L|2R|3L|3R|4)$/ { print $seqname }' dmel-all-r6.66.gtf.gz | sort | uniq -c | sort -k1,1nr
```

| Chromosome arm | Genes |
|---|---|
| 3R | 4,226 |
| 2R | 3,649 |
| 2L | 3,508 |
| 3L | 3,481 |
| X | 2,704 |
| 4 | 114 |
| Y | 113 |

These seven arms contain 17,795 of the 17,872 annotated genes.
