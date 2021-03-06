# IMgui-electron-packages
Front end for IMa suite of evolutionary biology analysis tools, developed using Electron to generate apps that require no installation or dependencies.

##Required Software
For linux/mac:
* g++
* (optional) mpicxx for multi-threaded IMa

##How To Use

###Linux
To use the app in linux, extract the .tgz with the following command in a directory of your choice, then "cd" into the extracted directory:
```
tar -xvzf IMgui-linux-x64.tgz
cd IMgui-linux-x64
```
Before running, IMa2 must be compiled. This is done by running the install_ima.sh script:
```
bash install_ima.sh
```
This will check for an mpicxx installation and compile with it if found, otherwise the single-thread version will be compiled with g++. If no IMa2 executable is created in the target folder, it can be created manually or the install_ima.sh script can be edited. 

Once IMa2 has been compiled, the app can be run by starting the IMgui executable:
```
./IMgui
```


###Mac
To use the app on Mac OS, extract the .tgz archive with this command in a directory of your choice, then "cd" into the extracted directory:
```
tar -xvzf IMgui-darwin-x64.tgz
cd IMgui-darwin-x64
```
Before running, IMa2 must be compiled. This is done by running the install_ima.sh script:
```
bash install_ima.sh
```
This will check for an mpicxx installation and compile with it if found, otherwise the single-thread version will be compiled with g++. If no IMa2 executable is created in the target folder, it can be created manually or the install_ima.sh script can be edited. 

Before starting the app from the command line, it usually must be granted permission to run by OSX. This is done by using the "Finder" app to find the extracted directory, then "right-clicking" on the IMgui.app/ folder and selecting "Open", then selecting "Yes" for the security question. After this, the app can be opened via commandline by running the following command in the extracted directory:
```
open IMgui.app/
```


###Windows
To use the app on Windows, simply extract the zip archive to a folder, then double-click on the IMgui executable. 

##How to Use
###Job Running
####Providing input parameters
In order to start a job, input parameters must be provided in accordance with the [IMa2 manual](https://bio.cst.temple.edu/~hey/program_files/IMa2/Using_IMa2_8_24_2011.pdf). For new users, the  manual will need to read through in order to understand the required and optional input files. Required parameters are noted on the input form. For a basic MCMC mode run, the required parameters are an input file in IMa format, an output prefix, a parameter controlling the number of burn-in steps for the Markov chain, a run duration to determine how many genealogies to save, and upper bounds for the priors of three variables: population sizes, migration rates, and population split times. (These priors can be included in a priorfile, which can include specific priors for different populations/migrations)

The "Demo Job" consists of the following input and parameters:
* Input file: ima2_testinput.u. This file contains data for a four-population, five locus run where the loci are split between an Infinite Sites model, HKY model, Stepwise Mutation model and Joint Infinite Sites/Stepwise model.
* Burn duration: 1000. Since this is an integer, the burn-in will run the Markov chain for 1000 steps. Once done it will switch to run-mode.
* Run duration: 1000. The program will save 1000 genealogies.
* Max Population Size: 3. This value is the upper bound on the population sizes in the model, in terms of 4Nu where N is population size and u is migration rate. Note the migration rate is not per base pair.
* Migration Prior Values: 1. This is the upper bound on migration rates between populations.
* Maximum time of population splitting: 2. This is the upper bound on splitting times for the populations.
* Heating mode: Geometric. The formula for heating terms for a given chain can be found in the IMa2 manual.
* Number of Chains: 5. This is the number of markov chains in the simulation, of which one will be unheated and others will have heating terms in accordance to the heating scheme.
* First heating term: .9. This is the first heating parameter for the model.
* Second heating term: .3. This is the minimum heating term for the final Markov chain. All in-between chains will have heating terms according to the formula specified in the IMa2 manual.

Once all parameters have been input, clicking the "Execute" button will run through validation steps:
* The app will check that all required fields have been provided, and that no incompatible options have been selected
* If "Validate that input files are present before run" box is checked, the node.js server will check to make sure all input files that have been provided are present on the user's computer (it will not validate the file content, just the name)
* The app will check that the output prefix provided is not being used by another job

Any errors will be reported under the "Execute" button. 

Once a job is validated, the node.js server will add the job to the job list, then start the job. STDOUT from jobs will be stored, and the user can select which job's stdout stream to view in the app. There are up to six buttons that can be used to control a job: 
* Kill Job: Stops active job. Job can then be restarted or removed from job list.
* Send IMburn/run signal: If either burn or run duration is set to be user-controlled ("Duration of burn" and/or "Run duration" values are floating point, not integer, and their respective file creation check boxes have been selected), this button will be enabled when the program is waiting for a user signal to end the current phase. Clicking will signal program to advance to next stage.
* Restart Job: Restarts job with same parameters as initially provided (changing them in the app form will NOT change the job information).
* Remove Job Information: Will remove job from job list, also frees up output prefix for use with a different job.
* Refresh: Will refresh the output from the current job. Use if output appars out of order.

###Output XML File Analysis
To start, click "Choose File" and select one of the output XML files from a job. Note: the app will check the suffix of the file to determine what output file it is, so if you rename output files be sure to keep the ".out.xml" or ".out.histograms.xml" suffix.
####Tables (*.xml, *.intervals.xml)
Once the XML file is processed, the tables included in the file will be added to the drop-down menu, with the first table present automatically generated and displayed. To save the active table, provide a filename or prefix (optional) and click either "Save as .csv" or "Save as .png". If no prefix is provided, a default will be used.
####Histograms(*.histograms.xml)
Once the XML file is processed, there will be two dropdown menus. The first dropdown menu will have a list of the types of variables contained in the file, the second dropdown will have a list of the available variables in the given list. Clicking "Make Graph" will render the graph in the app window.
#####Downloading Image/Data
To download the information from the chart selected, provide a filename or prefix in the input field (optional) and click either "Save Image as .png" or "Save Columns as .csv". The .png option will save an image of the graph, the .csv option will output a two-column, comma separated file.
#####Generating Code for Plot
Below the graph there is a code window that will generate code in either Matplotlib (Python), R, or Matlab. Clicking "Generate Code" will create a basic script that can be run to generate a simple plot, allowing the user to add additional information or features to the plot. The code can either be copied and pasted into an IDE or downloaded as a separate script. If a value is provided in the "Maximum line length" field, the arrays storing the data will be spread over multiple lines.
###IMfig
IMfig is a python script that has been converted to executable for for use on platforms without Python and GraphicsMagick. It generates a summary plot describing migration rates between populations. To generate the plot, provide the path to the non-XML output file in the "Input File" field. The output prefix is optional but must NOT contain any directory prefix, since get requests for the final image must be done from the internally specified output folder. All other options are optional. When submit is clicked, values are validated to ensure they fall in a proper range, with error messages being presented under options that have invalid values. The job is then sent to the server, which will create an EPS file and two JPG files (one high-res, one low-res) for the given input. The small JPG image will be posted to the app, the other files will be present in the app's home directory.
