## Continuous Phylogeography Tutorial
Welcome to the Continuous phylogeography tutorial for the [International Conference on Bioinformatics and Biostatistics for Agriculture, Health, and Environment](http://dept.ru.ac.bd/stat/bio-conference/) at the [University of Rajshahi](http://www.ru.ac.bd), Bangladesh. 

In this tutorial, we will learn how to use phylogenetics and statistics to infer the most probably origin and geographic spread of a pathogen outbreak. In your computers, you should have the following software installed:  

* **BEAST** - this package contains the BEAST program, BEAUti, TreeAnnotator and other utility programs. This tutorial is written for BEAST v2.3. It is available for download from http://beast2.org/.
* **Tracer** - this program is used to explore the output of BEAST (and other Bayesian MCMC programs). It graphically and quantitively summarizes the distributions of continuous parameters and provides diagnostic information. It is available for download from http://beast.bio.ed.ac.uk/.
* **Spread** for summaryzing the geographic spread in a KML (available from http://www.kuleuven.ac.be/aidslab/phylogeography/SPREAD.html.
* **GoogleEarth** for displaying the KML(just google for it, if you have not already have it installed).

The objective of the tutorial is to walk you through the necessary steps to get you from sequence alignment to GoogleEarth animation to understand the spread of infectious agents. In this case, we will use Hepatitis B virus sequences isolated throughout Europe and Africa. The data is described in an article [here](https://peerj.com/articles/2406/). However, we will use a subset for the sake of time and demonstration that consists of 17 sequences of 3221 characters.  

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

The next tab, MCMC, provides more general settings to control the length of the MCMC and the file names. Firstly we have the Length of chain. This is the number of steps the MCMC will make in the chain before finishing. Determining the length of the chain depends on things like the size of the dataset, the complexity of the models you want to run, and the accuracy of the answer. Remember that default values in BEAST, and probably on in many programs, is completely arbitrary. Since we want the example to run quickly, let's initially set the chain length to 1,000,000 as this will run reasonably quickly on most modern computers (less than 5 minutes).  

