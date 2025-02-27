---
title: FastQC
displayed_sidebar: multiqcSidebar
description: >
  Quality control tool for high throughput sequencing data
---

<!--
~~~~~ DO NOT EDIT ~~~~~
This file is autogenerated from the MultiQC module python docstring.
Do not edit the markdown, it will be overwritten.

File path for the source of this content: multiqc/modules/fastqc/fastqc.py
~~~~~~~~~~~~~~~~~~~~~~~
-->

:::note
Quality control tool for high throughput sequencing data

[http://www.bioinformatics.babraham.ac.uk/projects/fastqc/](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
:::

FastQC generates an HTML report which is what most people use when
they run the program. However, it also helpfully generates a file
called `fastqc_data.txt` which is relatively easy to parse.

A typical run will produce the following files:

```
mysample_fastqc.html
mysample_fastqc/
  Icons/
  Images/
  fastqc.fo
  fastqc_data.txt
  fastqc_report.html
  summary.txt
```

Sometimes the directory is zipped, with just `mysample_fastqc.zip`.

The FastQC MultiQC module looks for files called `fastqc_data.txt`
or ending in `_fastqc.zip`. If the zip files are found, they are
read in memory and `fastqc_data.txt` parsed.

:::note
The directory and zip file are often both present. To speed
up MultiQC execution, zip files will be skipped if the file name suggests
that they will share a sample name with data that has already been parsed.
:::

You can customise the patterns used for finding these files in your
MultiQC config (see [Module search patterns](#module-search-patterns)).
The below code shows the default file patterns:

```yaml
sp:
  fastqc/data:
    fn: "fastqc_data.txt"
  fastqc/zip:
    fn: "*_fastqc.zip"
```

:::note
Sample names are discovered by parsing the line beginning
`Filename` in `fastqc_data.txt`, _not_ based on the FastQC report names.
:::

#### Theoretical GC Content

It is possible to plot a dashed line showing the theoretical GC content for a
reference genome. MultiQC comes with genome and transcriptome guides for Human
and Mouse. You can use these in your reports by adding the following MultiQC
config keys (see [Configuring MultiQC](https://docs.seqera.io/multiqc/getting_started/config)):

```yaml
fastqc_config:
  fastqc_theoretical_gc: "hg38_genome"
```

Only one theoretical distribution can be plotted.
The following guides are available: _(txome = transcriptome)_

- `hg38_genome`
- `hg38_txome`
- `mm10_genome`
- `mm10_txome`

Alternatively, a custom theoretical guide can be used in reports. To do this,
create a file with `fastqc_theoretical_gc` in the filename and place it with your
analysis files. It should be tab delimited with the following format (column 1 = %GC,
column 2 = % of genome):

```bash
# FastQC theoretical GC content curve: YOUR REFERENCE NAME
0	0.005311768
1	0.004108502
2	0.004060371
3	0.005066476
[...]
```

You can generate these files using an R package called
[fastqcTheoreticalGC](https://github.com/mikelove/fastqcTheoreticalGC)
written by [Mike Love](https://github.com/mikelove).
Please see the [package readme](https://github.com/mikelove/fastqcTheoreticalGC)
for more details.

Result files from this package are searched for with the following search pattern
(can be customised as described above):

```yaml
sp:
  fastqc/theoretical_gc:
    fn: "*fastqc_theoretical_gc*"
```

If you want to always use a specific custom file for MultiQC reports without having to
add it to the analysis directory, add the full file path to the same MultiQC config
variable described above:

```yaml
fastqc_config:
  fastqc_theoretical_gc: "/path/to/your/custom_fastqc_theoretical_gc.txt"
```

#### Overrepresented sequences

The overrepresented sequences table shows the most common sequences found,
measured by the number of samples they occur as overrepresented. By default, the
table shows top 20 sequences. This can be customised in the config:

```yaml
fastqc_config:
  top_overrepresented_sequences: 50
```

You can also choose to rank the top sequences by the total number of reads
rather than by number of samples:

```yaml
fastqc_config:
  top_overrepresented_sequences_by: "total"
```

#### Changing the order of sections

Remember that it is possible to customise the order in which the different module sections appear
in the report if you wish.
See [the docs](https://docs.seqera.io/multiqc/#order-of-module-and-module-subsection-output) for more information.

For example, to show the _Status Checks_ section at the top, use the following config:

```yaml
report_section_order:
  fastqc_status_checks:
    order: -1000
```

#### Showing FastQC status checks

FastQC uses thresholds to mark samples as "pass", "warn" or "fail" for various checks.
If you prefer the MultiQC module to ignore those thresholds, and use standard MultiQC
colors for samples instead, use the following config:

```yaml
fastqc_config:
  status_checks: false
```

### File search patterns

```yaml
fastqc/data:
  fn: fastqc_data.txt
fastqc/theoretical_gc:
  fn: "*fastqc_theoretical_gc*"
fastqc/zip:
  fn: "*_fastqc.zip"
```
