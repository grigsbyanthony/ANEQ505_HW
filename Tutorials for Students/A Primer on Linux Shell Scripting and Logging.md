### How to create a script in the Linux terminal:
This is pretty simple: if you want to make an all purpose script file that you can run over and over without the tedium of re-writing all of the code, you can do something like this:

```
nano hello.sh
```

This command creates a new `.sh` file for you to copy and paste/write your script into. Here’s an example:

```
#!/bin/bash
echo "Enter your name: "
read name
echo "Hello $name! Welcome to Bash scripting."
```

When you’re done, just do `ctrl+x`, then `enter`, and finally `ctrl+o`.

To make your new file a recognizable script, use the following command.

```
chmod +x hello.sh
```

Only after doing this (`chmod+x YOURSCRIPTHERE)` will your script be useable in the terminal. If you do not do this, you will not have permission to run it. When you want to run it, just run this:

```
./hello.sh
```

**This will run your script!**

If you want to do things a little bit quicker, this is an alternative (less self-explanatory) workflow you can try:

```
cat << 'EOF' > quick.sh
#!/bin/bash
echo "Quick hello from a script!"
EOF

chmod +x quick.sh

./quick.sh
```

#### Here’s an example of one of your scripts simplified as a useable linux script:

Start by running `nano qiime_script.sh`. When the editor is open, copy-paste the following:

```
#!/bin/bash

module purge​ module 
load qiime2/2024.10_amplicon​ 

Mkdir Biom 

qiime taxa collapse  
    --i-table table_johnson_filtered.qza  
    --i-taxonomy taxonomy_gg2.qza  
    --p-level 7  
    --o-collapsed-table table_johnson_filtered_l7.qza 

qiime feature-table relative-frequency  
    --i-table table_johnson_filtered_l7.qza  
    --o-relative-frequency-table ra_table_johnson_filtered_l7.qza 

qiime tools export 
    --input-path ra_table_johnson_filtered_l7.qza 
    --output-path /scratch/alpine/c837498656@colostate.edu/MICROBIOMECALVES/Biom  

cd Biom 

biom convert -i ra_table_johnson_filtered_l7.biom -o ra_table_johnson_filtered_l7.biom.tsv --to-tsv 
biom convert -i feature-table.biom -o feature-table.biom.tsv --to-tsv
```

Once you’ve copy-pasted this code in, do `ctrl+x`, then `enter`, and finally `ctrl+o`. Then run `chmod +x qiime_script.sh`. When you’re done, navigate to the folder with your data and simple run the command `./qiime_script.sh`. Let it run and check the folder to see if the outputs are there.

## To save log as a file:
If you want to save the contents of your terminal for future reference or to make a script with your work there later, here’s something simple you can do. **If you simply want to copy everything you see in the terminal, just double click on it with your cursor.**

Run the following code at the **beginning of your session, before you start to do anything:**

```
script session.log
```

Once you’ve run this, you can work as usual. When you’re done working, run the command `exit`. To view your `session.log`, just run the following command:

```
cat session.log
```

If you just want to save the output of a specific script/command, you can do the following:

```
my_command > output.txt        # overwrite
my_command >> output.txt       # append
```

Replace `my_command` with the actual command you’re using and it will save the result/output of that command. Use `>` to overwrite/make a new file, and use `>>` to simply add the new command output to an existing file.