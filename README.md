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
