This is just a brief explanation of how to use the R package `qiime2R` to simplify your `R` analysis workflow with `.qza` (QIIME) artifacts.

To install `qiime2R`, run the following code:

```
if (!requireNamespace("devtools", quietly = TRUE)){install.packages("devtools")}
devtools::install_github("jbisanz/qiime2R")
```

Once you’ve done this, you can import your QIIME2 artifacts as a `phyloseq` object. You’ll obviously need this library as well, so install it with this command:

```
if (!require("BiocManager", quietly = TRUE)) install.packages("BiocManager") BiocManager::install("phyloseq")
```

Then load both packages (though I think `qiime2R` might load `phyloseq` by default because of the dependency):

```
library(phyloseq)
library(qiime2r)
```

Now you can make a `phyloseq` object and do the requisite analyses with it. Here’s a simple way to do that, provided you have all of the files necessary:

```
ps <- qiime2R::qza_to_phyloseq(
  features = "table.qza",
  tree = "tree.qza",
  taxonomy = "taxonomy.qza",
  metadata = "metadata.tsv"
)
```

The file names will vary depending on what you’ve been provided/generated. Once you have it, you can do things like this to generate basic taxonomy information, alpha diversity metrics, etc.