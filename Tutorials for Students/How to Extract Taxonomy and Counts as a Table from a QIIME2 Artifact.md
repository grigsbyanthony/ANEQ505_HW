This is just a quick tutorial on how to extract a table from a `.qza` file for analyses in R or Python or whatever you want to use. Assuming you have been provided something along the lines of a `table.qza` file, here’s what you can do:

### Export to `.biom` format
```
qiime tools export \
  --input-path feature-table.qza \
  --output-path exported-table/
```

### Convert `.biom` to `.tsv`
```
biom convert \
  -i exported-table/feature_table.biom \
  -o feature_table.tsv \
  --to-tsv
```

### Export `taxonomy.qza`
```
qiime tools export \
  --input-path taxonomy.qza \
  --output-path exported-taxonomy/
```

Alternatively, since `.qza` are literally just `.zip` files, you can do the following without the need for QIIME2 itself:

```
unzip feature-table.qza -d extracted/ # Data is in extracted/*/data/
```

