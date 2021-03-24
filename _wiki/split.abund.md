---
title: 'split.abund'
tags: 'commands'
redirect_from: '/wiki/Split.abund.html'
---
The **split.abund** command reads a fasta file, list file, name file or a count file and splits the sequences into rare and abundant groups.

## Default Settings

The **split.abund** command parameters are fasta, list, name, count, cutoff,
group, label, groups and accnos. The list, count or name parameter are
required, and you must provide a cutoff value.

Using a list file the splitting is done based on the size of the otus.

    mothur > split.abund(fasta=abrecovery.fasta, list=abrecovery.fn.list, cutoff=10)

Opening abrecovery.fn.rare.list you would see:

    unique 242 AY457723    AY457741    AY457686    AY457855   AY457698 ... 
    0.00   230 AY457723    AY457741    AY457686    AY457855   AY457698 ... 
    0.01   204 AY457723    AY457741    AY457686    AY457855   AY457698 ...             
    0.02   185 AY457723    AY457741    AY457686    AY457855   AY457698 ...         
    0.03   168 AY457723    AY457741    AY457686    AY457855   AY457698 ... 
    ...    

Opening abrecovery.fn.abund.list you would see:

    0.07   4   AY457701,AY457774,AY457838,AY457715,AY457859,AY457747,AY457809,AY457780,AY457763,AY457755,...   
    0.08   4   AY457701,AY457774,AY457838,AY457715,AY457859,AY457747,AY457809,AY457780,AY457763,AY457755,...   
    0.09   4   AY457864,AY457754,AY457910,AY457871,AY457701,AY457774,AY457838,AY457715,AY457859,AY457747,...
    ...

or

Using a name file the splitting is done based on the number of
sequences a sequence represents.

    mothur > split.abund(fasta=abrecovery.fasta, name=abrecovery.names, cutoff=10)

Since the name file does not contain any sequences that represent more
than 10 other sequences, you only get .rare files.

or

Using a [ count](/wiki/Count_File) file the splitting is done based
on the number of sequences a sequence represents.

    mothur > split.abund(fasta=abrecovery.fasta, count=abrecovery.count_table, cutoff=10)

Since the count file does not contain any sequences that represent more
than 10 other sequences, you only get .rare files.

## cutoff

The cutoff parameter is used to qualify what is abundant and rare. A
grouping is determined to be abundant if it contains more sequences than
the cutoff. The cutoff can be a integer or a percentage. For example:

    mothur > split.abund(count=final.count_table, cutoff=10)

will place all sequences with 10 or less abundance in the
\*.rare.count\_table file. You can also set the cutoff as a percentage
of the total number of sequences. For example let's set the cutoff to

0\.01, meaning reads that represent \<= 1% or the total reads in the
dataset are considered rare. final.count\_table includes 113964
sequences, so the cutoff is set to 1139:

    mothur > split.abund(count=final.count_table, cutoff=0.01)

## group

The group parameter allows you to parse a group file into rare and
abundant groups.

    mothur > split.abund(fasta=abrecovery.fasta, list=abrecovery.fn.list, cutoff=10, group=abrecovery.groups)

## groups

The groups parameter allows you to parse the files into rare and
abundant files by group. For example if you set groups=A-B-C, you will
get a .A.abund, .A.rare, .B.abund, .B.rare, .C.abund, .C.rare files. If
you want .abund and .rare files for all groups, set groups=all.

     mothur > split.abund(fasta=abrecovery.fasta, list=abrecovery.fn.list, cutoff=10, group=abrecovery.groups, groups=all)

## label

The label parameter is used to read specific labels in your listfile you
want to use.

    mothur > split.abund(fasta=abrecovery.fasta, list=abrecovery.fn.list, cutoff=10, label=0.10)

## accnos

The accnos parameter allows you to output a .rare.accnos and

\.abund.accnos files to use with the get.seqs and remove.seqs commands.

    mothur > split.abund(fasta=abrecovery.fasta, list=abrecovery.fn.list, cutoff=10, accnos=true)

## Revisions

-   1.28.0 Added count option
-   1.44.0 Cutoff parameter can now be a percentage. Removes fasta file
    requirement. [\#263](https://github.com/mothur/mothur/issues/263)
-   1.45.0 - Fixes bug with split.abund when run with names file. [\#715](https://github.com/mothur/mothur/issues/715)

