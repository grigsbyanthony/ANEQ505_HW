~={red}(1point)=~ for Alpha Rarefaction Plot
Run Core Metrics ~={red}(1 point; .25pts per line)=~
Make alpha diversity plots ~={red}(3points)=~
~={red}10 points=~ for the questions 

~={red}15 points total=~
------------------------------------------------------------------

Due: 

**For complete credit for this assignment, you must answer all questions and include all commands in your obsidian upload.**

------------------------------------------------------------------
**Learning Objectives**
1. Practice recording commands and editing code to match your analysis.
2. Perform alpha rarefaction and determine an appropriate sequencing depth.
3. Run core metrics, generate plots for alpha and beta diversity
--------------------------------------------------

**Cow Site Data Workflow**, part 3

Load qiime2 in a terminal session after you go into the **cow** folder

```
# Insert the two commands to activate qiime2

sinteractive --reservation=aneq505 --time=02:00:00 --partition=amilan --nodes=1 --ntasks=6 --qos=normal

# or just acompile
  
module purge  
module load qiime2/2024.10_amplicon
```

### Alpha Rarefaction Plot ~={red}(1 point)=~
- Chose the input sequencings depths (min and max) for generating the alpha rarefaction plot: 

```
#go to the cow directory
cd /scratch/alpine/$USER/cow

qiime diversity alpha-rarefaction \
--i-table dada2/cow_table_dada2_filtered300.qza \
--m-metadata-file metadata/cow_metadata.txt \
--o-visualization alpha_rarefaction_curves_16S.qzv \
--p-min-depth 100 \
--p-max-depth 10000
```


### Run Core Metrics ~={red}(1 point)=~

```
qiime diversity core-metrics-phylogenetic \
  --i-table dada2/cow_table_dada2_filtered300.qza \
  --i-phylogeny tree/tree_gg2.qza \
  --m-metadata-file metadata/cow_metadata.txt \
  --p-sampling-depth 8000 \
  --output-dir core_metrics_results
```


### Visualize alpha diversity plots
- generate a plot to visualize the observed features ~={red}(1 point)=~
```
qiime diversity alpha-group-significance \
  --i-alpha-diversity core_metrics_results/observed_features_vector.qza \
  --m-metadata-file metadata/cow_metadata.txt \
  --o-visualization core_metrics_results/observed-features-group-significance.qzv
```

- generate a plot to visualize faith's PD ~={red}(2 points)=~
```
qiime diversity alpha-group-significance \
  --i-alpha-diversity core_metrics_results/faith_pd_vector.qza \
  --m-metadata-file metadata/cow_metadata.txt \
  --o-visualization core_metrics_results/faith-pd-group-significance.qzv
```



## Homework questions ~={red}(10 points)=~

1. what is the name of the file you needed to use to figure out what min and max depths to use to generate the alpha rarefaction plot? (Hint: which file contains the sequencing depths for each sample) **The feature-table summary that reported “frequency per sample”,** `cow_table_dada2_filtered300.qzv`.
2. what did you choose for the rarefaction depth (the input for core metrics -p-sampling-depth flag)? why? **8k sits near the point where a lot of the curves started to plateau, so it seemed balanced enough when it came to within-sample coverage vs sample loss.**
3. Which cow body location had more observed features? Which has the lowest?  **Fecal and nasal respectively.**
4. What is the main difference between Faiths PD and Shannons alpha diversity metrics?  **Faith’s PD is a measure that communicates the phylogenetic breadth of a community whereas Shannon combines richness and evenness while ignoring phylogeny.**
5. Which diversity metrics produced by the core-metrics pipeline require phylogenetic information? **Faith’s PD and UniFrac.**
6. Which two body sites have the highest Faiths PD alpha diversity?  Are the groups significantly different? **Skin + Fecal, all groups differed significantly from eachother.**
7. Does it seem like there are any groupings in the beta diversity? What are the groupings? **Some grouping by bodysite in that fecal has a distinct cluster and udder + skin cluster together while nasal and oral samples are more variable.**
8. Why do you think these samples are grouping together? **Probably due to their similar local environment and the consistency of the microbiota in/around them.**
9. What test can you run to determine if the groups are significantly different? **PERMANOVA w/ the Bray-Curtis (or whatever distance matrix you want to use).**
10. What command would you use to run that test?

```
qiime diversity beta-group-significance \
  --i-distance-matrix core_metrics_results/weighted_unifrac_distance_matrix.qza \
  --m-metadata-file metadata/cow_metadata.txt \
  --m-metadata-column BodyLocation \
  --p-method permanova \
  --p-pairwise \
  --o-visualization core_metrics_results/weighted-unifrac-BodyLocation-permanova.qzv
```