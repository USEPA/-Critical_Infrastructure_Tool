# Introduction 
The Stochastic Infrastructure Remediation Model (SIRM) tool allows for a series of interconnected infrastructure sectors to be modeled and considers the realistic variability of the impact of a CBRN event. The SIRM's mechanics are based on the Gillespie Algorithm of stochastically modeling chemical kinetic systems, with adjustments made to suit the modeling of infrastructure remediation after an event that incapacitates infrastructure sectors (e.g. a CBRN event).

The SIRM examines the interactions of 9 different infrastructure sectors: Water, Energy, Transportation, Communication, Government, Food/Agriculture, Emergency Services, Waste Management and Healthcare. Based on the initial operating efficiency after the event, the model calculates an estimated time of recovery for each sector. The tool doesn’t account for auxiliary infrastructure such as power lines, water pipes, etc. that may impact operations/recovery

The SIRM examines the interactions of 9 different infrastructure sectors: Water, Energy, Transportation, Communication, Government, Food/Agriculture, Emergency Services, Waste Management and Healthcare. Based on the initial operating efficiency after the event, the model calculates an estimated time of recovery for each sector.

Python and ArcGIS were used to design the graphical user interface for the Stochastic Infrastructure Remediation Model (SIRM). The tool allows for the user to (1) load or define the inputs for a specific contamination scenario (2) choose the desired outputs from the scenario (3) output the results in reports allowing for further analysis.

# Getting Started

## Required Software Installations
- [Python 3.7 or more recent] (https://www.python.org/downloads/)
  - Used for development of the tool
  - Will need the following packages:
	- pyinstaller
	- os
	- pandas
	- json
	- sys
	- requests
	- urllib
	- urllib.request
	- subprocess
	- shutil
  - pip install <package> after downloading Python
	
- [ArcGIS Pro] (https://pro.arcgis.com/en/pro-app/latest/get-started/download-arcgis-pro.htm)
  - Used for the GIS capabilities. The Python tool can be run separately if ArcGIS is not available. 

## Initial Steps

1) Clone the repository in the folder of your choice.
2) Once the repository is cloned, open ArcGIS Pro. Users should sign into their ArcGIS Online (AGO) account if planning to leverage ArcGIS Online resources. For EPA users, access your Esri Enterprise login credentials and use your portal ID or PIV card access credentials
3) Navigate within ArcGIS Pro and create a New Project (new Map). 
4) Click on the “Insert” tab on the top left. 
5) Navigate to the Toolboxes icon in the top left corner.
7) Click on it. Select “Add Toolbox”. 
8) Navigate to the file location of the Infrastructure Remediation Repository, then select the "dist" folder, then select the "infrastructures_gui" folder. Select "SIRM Tool.tbx".
9) The toolbox will appear in the pane on the side. 
10) Expand the SIRM Tool, and right click the "Effiency Calculator" script. 
11) Select "Properties". 
12) Select "Store tool with relative path".  Change the path of the script file to "[location of Infrastructure Remediation Repository]/EfficiencyCalculatorMod.py". Click Ok. 
13) Right click the "Import HIFLD" script. 
14) Select "Properties". 
15) Select "Store tool with relative path". Change the path of the script file to the location of the "[location of Infrastructure Remediation Repository]/ImportHIFLD.py". Click Ok. 
16) Ensure that the "Battelle.EPA.WideAreaDecon.Launcher.zip" files are unzipped, and add the exe to the same folder with the zip. There are two instances of this file - in the main repository and in the infrastructures_gui folder in the dist folder. 
17) Add the exe to the gitignore if it is not there already. This file, if not ignored, will cause issues with pushes since it is too large to push over github.

## Running the Python script

The Python exe is contained within the "[location of Infrastructure Remediation Repository]/dist/infrastructures_gui" folder. This exe can be run separate from the ArcGIS tool. 

**Note:** The tool can be packaged as an independent zip file, if there are no plans to edit the code.

## Making edits to the code

Any time edits are made to the code, run the following command from the command line in the repository folder. 

**pyinstaller infrastructures_gui.spec infrastructures_gui.py**

The toolbox will automatically update. 

**Note:** If you are editing the sensitivity analysis GUI, then an additional step is required. First you must run the command: **pyinstaller sensitivity_GUI.spec sensitivity_GUI.py**. Then, run the original command. 

# Independently Packaging Tool

Every time a pull request is completed, a build artifact entitled "SIRMExe" is created. This zip file can be unzipped onto any machine, and contains the exe of the infrastructures gui, and the Toolbox. The setup is identical to the rest of the tool, however the underlying code cannot be edited. 

The zip file can be handed off to another machine. To add an additional layer of isolation and make the Toolbox independent of the scripts, before subitting a pull reuqest the following steps must be followed. 

1) Right click on the "Import HIFLD" ascript within the Toolbox pane in ArcGIS. 
2) Select "Properties".
3) In the Options section, select "Import script" and deselect "Store tool with relative path". 
4) Follow the same process with the "Efficiency Calculator" script. 

# Common Errors

1) If the ImportHIFLD cannot import a certain file, ensure that the URL is correct in the Python file. The URLS can be found here: ttps://hifld-geoplatform.opendata.arcgis.com/. In the ImportHIFLD.py file, replace "HIFLD_URL" in the relevant line in the function "importDataset" (createLayerFromAPI(r"<HIFLD_URL>", "<infrastructure_type>_HIFLD", Temporary_Output_Path))
2) If the Internet connection is weak, either Import HIFLD or Efficiency Calculator may time out and throw an error. Ensure your connection is strong, and rerun the tool. Typically, each function takes around 10-15 minutes, but the Efficiency Calculator can take more time for larger incidents. 
3) If the dist folder does not exist, you will need to run the command: **pyinstaller sensitivity_GUI.spec sensitivity_GUI.py** in the code folder, then **pyinstaller infrastructures_gui.spec infrastructures_gui.py**. 
 
