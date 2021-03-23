---
title: 'make.shared'
tags: 'commands'
redirect_from: '/wiki/Make.shared.html'
---
The **make.shared** command reads a list and group file or biom file and
creates a .shared file as well as a rabund file for each group.

## Inputting list data for multiple samples

The Amazonian data set actually represents sequences from two samples -
one from soil collected at a pasture site and a rainforest site. Looking
at the file amazon.groups you can see which sequences belong to the
pasture and rainforest sites. There is no limit to the number of groups
you can have.

    mothur > make.shared(list=98_sq_phylip_amazon.an.list, group=amazon.groups)

or if you used a [ count file](/wiki/Count_File) instead of a name
file and group file:

    mothur > make.table(name=amazon.names, group=amazon.groups)
    mothur > cluster(phylip=98_sq_phylip_amazon.dist, count=amazon.count_table)
    mothur > make.shared(list=98_sq_phylip_amazon.an.unique_list, count=amazon.count_table)

or you can convert a [ count file](/wiki/Count_File) to a [shared
file](/wiki/shared_file) and [list file](/wiki/list_file)
where each representative sequence is placed in it's own OTU. This is the same thing as generating a shared file of ASVs

    mothur > make.shared(count=amazon.count_table, label=asv)

This command will generate a [shared file](/wiki/shared_file).

## Creating a shared file from a biom file

You can also convert a [biom file](https://github.com/biocore/biom-format) to a shared file to import your data into mothur.

    mothur > make.shared(biom=example.biom)

## Options

### label

There may only be a couple of lines in your OTU data that you are
interested in summarizing. There are two options. You could: (i)
manually delete the lines you aren't interested in from you rabund,
sabund, list, or shared file; (ii) or use the label option. If you only
want to read in the data for the lines labeled unique, 0.03, 0.05 and

0\.10 you would enter:

    mothur > make.shared(list=98_sq_phylip_amazon.fn.list, group=amazon.groups, label=unique-0.03-0.05-0.10)

### groups

You can use the groups parameter to specify which groups you want
included in your analysis:

    mothur > make.shared(list=98_sq_phylip_amazon.fn.list, group=amazon.groups, groups=forest-pasture)

## Revisions

-   1.22.0 Removed ordergroup parameter.
-   1.25.0 added biom parameter
-   1.28.0 added count parameter
-   1.36.0 mothur no longer checks for biom matrix type to allow for
    more flexibility.
-   1.36.0 rabund files are no longer outputted. mothur will create a
    rabund file with the [get.rabund](/wiki/get.rabund) command.
-   1.39.0 Eliminates zero abundance OTUs created by some floating point
    biom files converted to shared files.
-   1.41.0 Adds count file to shared file option in **make.shared** command.
    [\#519](https://github.com/mothur/mothur/issues/519)
-   1.42.0 Expands **make.shared** to allow for count files with no groups.
    [\#563](https://github.com/mothur/mothur/issues/563)
-   1.42.0 Adds map file to **make.shared** output from count file. This
    mapping file can then be used with the rename.seqs command to modify
    the associated files.
    [\#583](https://github.com/mothur/mothur/issues/583)
-   1.44.0 Adds list file output for **make.shared** with count table.
-   1.45.0 Adds shared file conversion to **make.shared** [\#716](https://github.com/mothur/mothur/issues/#716)
