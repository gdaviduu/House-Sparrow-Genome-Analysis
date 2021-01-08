# ***Genomic Landscape of Structural Variation in *Passer domesticus****

***Snakefile #1:***
BWA alignment and GATK pipeline

***Snakefile #2:***
Structural variant calling and genotyping with `smoove`[(more here)](https://github.com/brentp/smoove)

***Intermediate steps needed prior to visual curation:***

TO DO: JupyterNotebook for: 
Randomly selecting 3 individuals of each genotype (`HOM_REF, HET, HOM_ALT`) with `gen_samplot.py` for generating the Samplot images (.png) for PlotCritic

TO DO: ***Snakefile #3:***
Setting up a project in PlotCritic for visual curation, using SV-Plaudit on an Amazon Instance


## ***Data Exploration*** 
JupyterNotebooks for executing all steps, from extracting curated variants from raw PlotCritic reports to final plotting in Python3. These need to be run in the order presented below.


***Notebook #1, Part 1: Intersecting and plotting a PCA for all Deletions that were given "Yes" by all four curators.***

Click to open
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/gdaviduu/House-Sparrow-Genome-Analysis.git/main?filepath=Extract_SV_regions_PCA_Part1to4.ipynb)


***Notebook #1, Parts 2 to 4: Intersecting and plotting a PCA for all Deletions that were given either "Yes" or "Maybe" by all four curators; plotting all of the remaining SVs that were rejected by all four curators; and plotting all raw Uncurated Deletions.***

Click to open
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/gdaviduu/House-Sparrow-Genome-Analysis.git/main?filepath=Extract_SV_regions_PCA_Part1to4.ipynb)


***Notebook #2: Plotting Size Histograms for SVs***

Click to open
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/gdaviduu/House-Sparrow-Genome-Analysis.git/main?filepath=Plotting_Size_Histograms_for_SVs.ipynb)

***Notebook #3: Downsampling SNPs in PLINK and plotting PCAs to compare to similarly-sized SV-callsets***

Click to open
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/gdaviduu/House-Sparrow-Genome-Analysis.git/main?filepath=Plotting_PCA_for_Downsampled_SNPs.ipynb)

'<table border="1" class="dataframe">\n  <thead>\n    <tr style="text-align: right;">\n      <th></th>\n      <th></th>\n      <th>DEL</th>\n      <th>DUP</th>\n      <th>INV</th>\n    </tr>\n  </thead>\n  <tbody>\n    <tr>\n      <th>1</th>\n      <td>% True Positives</td>\n      <td>71%</td>\n      <td>22%</td>\n      <td>70%</td>\n    </tr>\n    <tr>\n      <th>2</th>\n      <td></td>\n      <td></td>\n      <td></td>\n      <td></td>\n    </tr>\n    <tr>\n      <th>3</th>\n      <td>2 Curators</td>\n      <td>1541</td>\n      <td>45</td>\n      <td>16</td>\n    </tr>\n    <tr>\n      <th>4</th>\n      <td>% True Positives</td>\n      <td>45%</td>\n      <td>18%</td>\n      <td>6%</td>\n    </tr>\n    <tr>\n      <th>5</th>\n      <td>% Agreement btwn Curators</td>\n      <td>99%</td>\n      <td>96%</td>\n      <td>89%</td>\n    </tr>\n    <tr>\n      <th>6</th>\n      <td></td>\n      <td></td>\n      <td></td>\n      <td></td>\n    </tr>\n    <tr>\n      <th>7</th>\n      <td>3 Curators</td>\n      <td>1532</td>\n      <td>42</td>\n      <td>15</td>\n    </tr>\n    <tr>\n      <th>8</th>\n      <td>% True Positives</td>\n      <td>44%</td>\n      <td>17%</td>\n      <td>6%</td>\n    </tr>\n    <tr>\n      <th>9</th>\n      <td>% Agreement btwn Curators</td>\n      <td>99%</td>\n      <td>89%</td>\n      <td>83%</td>\n    </tr>\n    <tr>\n      <th>10</th>\n      <td></td>\n      <td></td>\n      <td></td>\n      <td></td>\n    </tr>\n    <tr>\n      <th>11</th>\n      <td>4 Curators</td>\n      <td>1243</td>\n      <td>37</td>\n      <td>13</td>\n    </tr>\n    <tr>\n      <th>12</th>\n      <td>% True Positives</td>\n      <td>36%</td>\n      <td>15%</td>\n      <td>5%</td>\n    </tr>\n    <tr>\n      <th>13</th>\n      <td>% Agreement btwn Curators</td>\n      <td>80%</td>\n      <td>79%</td>\n      <td>72%</td>\n    </tr>\n  </tbody>\n</table>'
