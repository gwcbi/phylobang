## Continuous Phylogeography Tutorial

Welcome to the Continuous phylogeography tutorial for the [International Conference on Bioinformatics and Biostatistics for Agriculture, Health, and Environment](http://dept.ru.ac.bd/stat/bio-conference/) at the [University of Rajshahi](http://www.ru.ac.bd), Bangladesh. 

In this tutorial, we will learn how to use phylogenetics and statistics to infer the most probable origin and geographic spread of a pathogen outbreak. Phylogenetics and coalescent models can be used to track disease outbreaks over space and time, and also to infer epidemiological parameters such R0, and population size changes. Microbes, especially viruses, are well-suited for these models due to their high population sizes and high substitution rates, which leave marks on their genomes that we can use to learn about epidemiology.  
In your computers, you should have the following software installed:  

* **BEAST** - this package contains the BEAST program, BEAUti, TreeAnnotator and other utility programs. This tutorial is written for BEAST v2.3. It is available for download from [http://beast2.org/](http://beast2.org/).
* **Tracer** - this program is used to explore the output of BEAST (and other Bayesian MCMC programs). It graphically and quantitively summarizes the distributions of continuous parameters and provides diagnostic information. It is available for download from [http://tree.bio.ed.ac.uk/software/tracer/](http://tree.bio.ed.ac.uk/software/tracer/).
* **FigTree** - this program is used to visualize and create figures of phylogenetic trees. It is available for download from http://tree.bio.ed.ac.uk/software/figtree/
* **Spread** for summaryzing the geographic spread in a KML (available from [http://www.kuleuven.ac.be/aidslab/phylogeography/SPREAD.html](http://www.kuleuven.ac.be/aidslab/phylogeography/SPREAD.html).
* **GoogleEarth** for displaying the KML file (just google for it, if you have not already have it installed).

The objective of the tutorial is to walk you through the necessary steps to get you from sequence alignment to GoogleEarth animation to understand the spread of infectious agents. In this case, we will use Hepatitis B virus  (HBV) sequences isolated throughout Eurasia and Africa. The data is described in an article [here](https://peerj.com/articles/2406/). However, we will use a subset of sequences for the sake of time and demonstration that consists of 17 sequences of 3221 characters. Please note that this tutorial relies on one provided by Remco Bouckart at [http://beast2.org/tutorials/](http://beast2.org/tutorials/).  

The outline for this activity is:  

* Install the GEO_SPHERE package through BEAUti
* Use BEAUti to set up the analysis and create an input file
* Run BEAST
* Check the results for convergence
* Postprocess the output and generate a visualization

#### Install the GEO_SPHERE package  

In your computer, open the program BEAUti. Go to File --> Manage Packages --> GEO_SPHERE. You should see a window like:

![geosphere](https://github.com/gwcbi/phylobang/raw/master/img/geosphere.png)

Check install/uninstall all dependencies and click on Install/Upgrade. That was easy, wasn't it?

#### Setting up the analysis  

##### Setting up dates

Go to the BEAUti window and click on File --> Import Alignment. You should select the [HBV.nex file](https://raw.githubusercontent.com/gwcbi/phylobang/master/HBV.nex). You should see something like:  

![load_nexus](https://github.com/gwcbi/phylobang/raw/master/img/load_nexus.png)

You can see that our alignment has 17 taxa (sequences in this case) and 3221 nucleotides. Aside from the alignment, we also need a [file with sampling dates](https://raw.githubusercontent.com/gwcbi/phylobang/master/HBV_dates.dat) of these sequences and another with [GPS coordinates](https://github.com/gwcbi/phylobang/blob/master/HBV_locations.dat).  

Now let's go to the 'Tip Dates' tab in BEAUti and click on 'Use tip dates'. 

![tipdates](https://github.com/gwcbi/phylobang/raw/master/img/tipdates.png)

We could make the program guess the sampling dates because they are part of the sequence names. Click on 'Guess'. A dialog pops up, where we can specify the dates as follows: the dates are encoded between underscores in the name, and it is the second part. So, we want to split on character (select this box) and take group 2. It should now look like this:

![guess](https://github.com/gwcbi/phylobang/raw/master/img/guess.png)

Click OK and you should see the dates correctly formatted on BEAUti

![correctdates](https://github.com/gwcbi/phylobang/raw/master/img/correctdates.png)

##### Setting up the substitution model

Next, we need to provide BEAUti with an indication as to how our genes in the alignment evolve. For this, we use the 'Site Model' tab, select HKY as substitution model, and frequency model to 'empirical'. You should see something like:  

![sitemodel](https://github.com/gwcbi/phylobang/raw/master/img/sitemodel.png)

##### Setting up the Priors tab

Here, simply change the configuration from 'Yule model' to 'Coalescent Constant Population' and go back to the Partitions tab.

##### Setting up the geographic model

Since we are trying to find out about origin and geographic spread, we need to let the program know where are the samples from. On the 'Partitions' tab click on the '+' button, select 'Add Spherical Geography', and click OK.

![spherical](https://github.com/gwcbi/phylobang/raw/master/img/spherical.png)

A new window pops up. Change 'Trait name' to geo and click OK

![geo](https://github.com/gwcbi/phylobang/raw/master/img/geo.png)

You should now see a new window showing taxon names and two empty columns, latitude and longitude. The idea here is similar to what we did with 'Tip Dates', i.e., infer the GPS coordinates from taxon names.

![location](https://github.com/gwcbi/phylobang/raw/master/img/location.png) 

So let's go ahead and click on 'Guess latitude', select 'split on character', and then select group 3, as shown below:

![lat](https://github.com/gwcbi/phylobang/raw/master/img/lat.png)

Do the same with 'Guess longitude' but instead of selecting group3, select group 4. You should now see something like:

![latlong](https://github.com/gwcbi/phylobang/raw/master/img/latlong.png)

We are almost there setting up the input file and before we run the analysis we need to make sure we are OK regarding a few more paramenters. Go to the 'Clock Model' tab and make sure it looks like the following screenshot:

![clockrate](https://github.com/gwcbi/phylobang/raw/master/img/clockrate.png)
![geoclock](https://github.com/gwcbi/phylobang/raw/master/img/geoclock.png)

And before we configure the MCMC, let's check the 'Priors' tab to make sure we are OK. The important parameter here is Tree.t:HBV, which needs to be set up to 'Coalescent Constant Population'
![priors](https://github.com/gwcbi/phylobang/raw/master/img/priors.png)

##### Setting up the MCMC

The next tab, MCMC, provides more general settings to control the length of the MCMC and the file names. Firstly we have the Length of chain. This is the number of steps the MCMC will make in the chain before finishing. Determining the length of the chain depends on things like the size of the dataset, the complexity of the models you want to run, and the accuracy of the answer. Remember that default values in BEAST, and probably in many programs, is completely arbitrary. Since we want the example to run quickly, let's initially set the chain length to 1,000,000 as this will run reasonably quickly on most modern computers (less than 5 minutes).  

The next options specify how often the parameter values in the Markov chain should be displayed on the screen and recorded in the log file. The screen output is simply for monitoring the programs progress so can be set to any value (although if set too small, the sheer quantity of information being displayed on the screen will actually slow the program down). For the log 
file, the value should be set relative to the total length of the chain. Sampling too often will result in very large files with little extra benefit in terms of the precision of the analysis. Sample too infrequently and the log file will not contain much information about the distributions of the parameters. You probably want to aim to store no more than 10,000 samples so this should be set to no less than chain length / 10,000.  

For this exercise we will set the screen log to 10000 and leave the file log to 1000. The final two options give the file names of the log files for the sampled parameters and the trees. These will be set to a default based on the name of the imported NEXUS le. **If you are using windows** then add the suffix .txt to both of these (so, HBV.log.txt and HBV.trees.txt) so that Windows recognizes these as text files.

![mcmc](https://github.com/gwcbi/phylobang/raw/master/img/mcmc.png)

Finally, we are ready to create the input file for BEAST, an xml file. Go to File --> Save. Use an appropriate name such as HBV.xml.  

#### Running BEAST

This is the easy part as you only have to open BEAST software, click on 'Choose File', and then click 'Run'.

![beast](https://github.com/gwcbi/phylobang/raw/master/img/beast.png)

If everything is running correctly, you should see something like the following screenshot:

![running](https://github.com/gwcbi/phylobang/raw/master/img/beastrunning.png)

Once BEAST has run, you will see two files: a .log and .tree files

#### Checking BEAST results for convergence

Within the BEAST directory, run the program Tracer to analyze the output of BEAST. Choose 'Import Trace File...' and select the .log file BEAST just created. Since the MCMC is a stochastic algorithm, you wouldn't see the exact same results as in the screenshot, however, they should be more or less similar. Tracer will give you a number of statistics (left-hand side of window) and distribution plots on the right. The key here is to identify whether the 'trace' has stabilized, whether there is good 'mixing', and whether the ESS is > 200.

![tracer](https://github.com/gwcbi/phylobang/raw/master/img/tracer.png)

##### Take a look at the distribution of trees and get a point estimate as a phylogenetic tree

One of the main outcomes of the BEAST analysis is a distribution of phylogenetic trees. We can obtain both a visualization of this distribution (DensiTree) and a point estimate (a phylogeny). Let's take a look at the posterior distribution of trees.

Open DensiTree (comes with the BEAST package) and go to File --> Load, and select the .trees file, e.g., HBV.trees. You should see something like:

![densi](https://github.com/gwcbi/phylobang/raw/master/img/densi.png)

The great thing about a DensiTree representation is that it shows the degree of uncertainty in phylogenetic clades, i.e., less suported clades tend to look more sparse and less dense than the more supported ones. Note how towards the root of the tree the lineages turn fuzzy.  
The traditional way of representing trees though is by showing a point estimate. For this, we will use the software TreeAnnotator, also part of the BEAST package. Open TreeAnnotator and set it up as in the screenshot below:

![tanno](https://github.com/gwcbi/phylobang/raw/master/img/tanno.png)

Once TreeAnnotator is done, open FigTree and take a look at the phylogenetic tree that you just created, i.e., HBV.tree. Go to File --> Open, and select the tree file.

![figtree](https://github.com/gwcbi/phylobang/raw/master/img/figtree.png)

#### Postprocessing BEAST output for geography inference

So far we've been going through the necessary steps of inferring a phylogeny using an explicit geographic model and summarizing a distribution of trees, however, one of the most exciting inferences is 'seeing' your results on a globe or map.  
Let's start by opening SPREAD, click on the 'Continuous tree' tab, and click on 'Open'. Select the HBV.tree file and change the latitude and longitud names on the left-side menu to 'locationsgeo1' and 'locationsgeo2', respectively. Finally, click on 'Output' and then on 'Plot'. You should get something like:

![spread](https://github.com/gwcbi/phylobang/raw/master/img/spread.png)

##### Visualizing on GoogleEarth

GoogleEarth is a widely used software for exploring and visualizing geographic information. Within SPREAD, click on 'Generate' to produce a .kml file (default is output.kml). You can open the kml file in GoogleEarth and see the spread of the epidemic animated through time.

![gearth](https://github.com/gwcbi/phylobang/raw/master/img/gearth.png)

This concludes the first half of the workshop. If you are interested in more advanced analysis using BEAST in general or phylogeography in particular, check out BEAST tutorial pages at [http://beast2.org/tutorials/](http://beast2.org/tutorials/).