### Summary on how to install dependencies and run PathoScope2 on Windows machines  

**Install Python in Windows**  
* Go to -> https://www.python.org/downloads/windows/  
* Search for -> "Python 2.7.13rc1 - 2016-12-04"  
* Select -> "Download Windows x86 MSI installer"  
* Go to Downloads folder and run python windows installer  

**Install PathoScope in Windows**  
* To download PathoScope go to -> https://github.com/PathoScope/PathoScope -> “Clone or download” -> "Download ZIP"  
* Go to "Downloads" folder and extract PathoScope in appropiate folder (C:\)  
* Go to PathoScope folder and run the install python file  

**Install Perl in Windows**  
* Go to -> https://www.perl.org/get.html -> Windows "Strawberry Perl" -> select appropiate 64bit or 32bit  
* Go to Download folder and run Windows Installer Package "strawberry-perl-5.24.0.1"  

**Install Bowtie in Windows**  
* Go to Bowtie link in tutorial and select bowtie2-2.3.0-windows-legacy-x86_64.zip  
* Go to Downloads folder and extract bowtie in appropiate folder (C:\)  
* Go to "bowtie2-2.3.0-legacy" folder and run bowtie2 Windows Batch File  
* "target" and "filter library" take time to download (~30-45min) !  
* Go to "Command Prompt" in Windows  

        C:\Python27\python C:\PathoScope-master\pathoscope\pathoscope2.py -h  
        C:\Strawberry\perl\bin\perl C:\bowtie2-2.3.0-legacy\bowtie2 -h  

**Add python and perl path in windows**  

*   Go to -> Control Panel -> System -> Advanced system settings -> Environment Variables -> in "System variables" clic on "New..." -> in "Variable name" write "python" and in "Variable value" write the path "C:\Python27\" -> clic "OK" -> in "Variable name" write "perl" and in "Variable value" write the path "C:\Strawberry\perl\bin\" -> clic "OK" 
* Add bowtie2 path: -btHome C:\bowtie2-2.3.0-legacy\  

        C:\Python27\python C:\PathoScope-master\pathoscope\pathoscope2.py MAP -U ES_211.fastq -outDir C:\Users\Ashley\Documents\Katterinne -outAlign ES_211.sam -indexDir C:\Users\Ashley\Documents\Katterinne\index_HMP-human-phix -targetIndexPrefixes HMP_ref_ti_0,HMP_ref_ti_1 -filterIndexPrefixes genome,phix174 -btHome C:\bowtie2-2.3.0-legacy\ -expTag Bangladesh  

        C:\Python27\python C:\PathoScope-master\pathoscope\pathoscope2.py ID -alignFile ES_211.sam -fileType sam -outDir C:\Users\Ashley\Documents\Katterinne -expTag DAV -thetaPrior 1000000  
                
**Install R and Rstudio**  

*       Go to -> https://cran.r-project.org/bin/windows/base/ -> select "Download R 3.3.2 for Windows" -> go to Downloads folder and double click on "R-3.3.2-win"  
*       Go to -> https://www.rstudio.com/products/rstudio/download/ -> search for "Installers for Supported Platforms" and select "RStudio 1.0.136 - Windows Vista/7/8/10" -> go to Downloads folder and double click on "RStudio-1.0.136"  
