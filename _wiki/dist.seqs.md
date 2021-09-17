---
title: 'dist.seqs'
tags: 'commands'
redirect_from: '/wiki/Dist.seqs.html'
---
The **dist.seqs** command will calculate uncorrected
pairwise distances between aligned DNA sequences. This approach is
better than the commonly used DNADIST because the distances are not
stored in RAM, rather they are printed directly to a file. Furthermore,
it is possible to ignore "large" distances that one might not be
interested in. The command will generate a [column-formatted distance
matrix](/wiki/column-formatted_distance_matrix) that is compatible
with the column
option in the various [cluster](/wiki/cluster) commands. The command is also able to
generate a phylip-formatted distance matrix. There are several options
for how to handle gap comparisons and terminal gaps. This tutorial uses
the data files in [ amazondata.zip](https://mothur.s3.us-east-2.amazonaws.com/wiki/amazondata.zip).


## Default settings

To run **dist.seqs** an alignment file must be provided in fasta format. By
default an gap is only penalized once, terminal gaps are penalized, all
distances are calculated, and only one processor is used. To save time,
filter out the leading and trailing periods with the
[filter.seqs](/wiki/filter.seqs) command. Running the following
command will generate amazon.unique.dist

    mothur > filter.seqs(fasta=amazon.unique.fasta, trump=.)
    mothur > dist.seqs(fasta=amazon.unique.filter.fasta)

By default, **dist.seqs** will only count a string of gaps as a single gap
and will penalize for terminal gaps

## Options

### calc

mothur has several ways of calculating a distance based on how gaps are
treated. First, the default option - onegap - only counts a string of
gaps as a single gap. For example:

    SequenceA  ATGCATGCATGC
    SequenceB  ACGC---CATCC

Would have two mismatches and one gap. The length of the shorter
sequence is 10 nt, since the gap is considered as a single position.
Therefore the distance would be 3/10 or 0.30. This is the distance
calculating method employed by Sogin et al. (1995). The logic behind
this type of penalty is that a gap represents an insertion and it is
likely that a gap of any length represents a single insertion. This can
be called within mothur by the command:

    mothur > dist.seqs(fasta=amazon.unique.filter.fasta, calc=onegap)

DNADIST actually ignores gaps. For example the two sequences above would
have a distance of 2/9 or 0.2222. This type of distance calculation does
not make much sense for sequences that are known to have a significant
number of insertions. This can be used in mothur using the nogaps
option. mothur can use this distance calculating method with the
command:

    mothur > dist.seqs(fasta=amazon.unique.filter.fasta, calc=nogaps)

A another option is to penalize each gap. For the two sequences above, the
distance would be 5/12 or 0.4167. This can be used in mothur using the
eachgap option. mothur can use this distance calculating method with the
command:

    mothur > dist.seqs(fasta=amazon.unique.filter.fasta, calc=eachgap)
    
#### Mothur also has several methods to calculate the distances between protein sequences. 

jtt (Jones-Taylor-Thornton matrix): 

    mothur > dist.seqs(fasta=protein.fasta, calc=jtt)

pmb (Henikoff/Tillier PMB matrix)

    mothur > dist.seqs(fasta=protein.fasta, calc=pmb)
    
pam (Dayhoff PAM matrix)

    mothur > dist.seqs(fasta=protein.fasta, calc=pam)
    
kimura (Kimura formula) 

    mothur > dist.seqs(fasta=protein.fasta, calc=kimura)
    
Protein calculators are based on work by https://evolution.genetics.washington.edu/phylip/credits.html. 

### countends

There is some discussion over whether to penalize gaps that occur at the
end of sequences. For example, consider the following sequences:

    Sequence1 ATGCATGCATGC
    Sequence2 ---CAAGTA---

If terminal gaps are penalized, as is the default (and we are using the
calc=onegap option), then the distance would be 4/8 or 0.5000. If they
are not penalized, then the distance would be 2/6 or 0.3333. Ideally,
all sequences would be aligned over the same region; however, if this is
not possible or desired for some reason the ends option can be employed
to tell mothur to ignore the penalization:

    mothur > dist.seqs(fasta= amazon.unique.filter.fasta, countends=F)

The default is for countends to equal T.

### cutoff

If you know that you are not going to form OTUs with distances larger
than 0.10, you can tell mothur to not save any distances larger than

0\.10. This will significantly cut down on the amount of hard drive space
required to store the matrix. This can be done as follows:

    mothur > dist.seqs(fasta=amazon.unique.filter.fasta, cutoff=0.10)

Without setting cutoff to 0.10 this command would have generated 4560
distances (i.e. 96x95/2 = 4560). With the cutoff only 56 distances are
saved. The savings can be substantial when there are a large number of
distances. The actual cutoff used by the command is 0.005 higher than
the value that is set to allow for rounding in the [ clustering
steps](/wiki/cluster#precision).

### processors

The processors option will allow you to multiple processors to calculate
a distance matrix. Default processors=Autodetect number of available
processors and use all available. Multiple processors can be called as
follows:

    mothur > dist.seqs(fasta=amazon.unique.filter.fasta, cutoff=0.10, processors=2)

### output

The output option allows you specify the form of the matrix generated by
dist.seqs. By default, **dist.seqs** will generate a column-formatted
matrix. You can set the output to "lt", for a phylip formatted lower
triangle matrix, or to "square" for a phylip formatted square matrix.
If output is set to lt or square the cutoff option is ignored.

    mothur > dist.seqs(fasta=amazon.unique.filter.fasta, output=lt)

### oldfasta & column

These parameters are used to append to a column-formatted distance
matrix. Given a column matrix, a new fasta file and a old fasta file we
add distances to the original distance matrix calculated with the old
fasta file.

    mothur > dist.seqs(oldfasta=amazon.unique.fasta, column=amazon.unique.dist, fasta=esophagus.unique.fasta) 

## Revisions

-   1.22.0 - Added processors option for Windows users.
-   1.39.0 - Removes cutoff adjust.
-   1.40.0 - Rewrite of threaded code. Default processors=Autodetect
    number of available processors and use all available.
-   1.43.0 - Improves speed of **dist.seqs** and
    pairwise.seqs.[\#653](https://github.com/mothur/mothur/issues/653)
-   1.46.0 - Adds jtt, pmb, pam and kimura calculators to dist.seqs to find distances between protein sequences. [\#776](https://github.com/mothur/mothur/issues/776)
