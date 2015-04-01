This repository is for demonstration purposes only. Some files are not included to protect privacy.

The heat file, or file that includes directory and component definitions for all files that are to be included in the installer
is not included in this repository to protect privacy. You can generate a heat file by opening the Windows Command Prompt and 
following these directions: 
Command to get heat file: 
1.Navigate to the WiX Toolset directory to access command line tools. 
cd C:\Program Files (x86)\WiX Toolset v3.9\bin 
2. type in heat.exe dir (Path to the folder with files you want included) dr TARGETDIR -cg NewFilesGroup -gg -g1 -srd -sreg 
-var "var.MyDir" - out ".\HeatFile.wxs"

dr refers to the TARGETDIR component specified in the Product.wxs file. -cg sets up a new component group named NewFilesGroup, 
which is referenced at the bottom of the Product.wxs file. -gg generates new guids. -g1 generates new guids without curly braces
around them. -sreg tells heat.exe not to harvest files to the directory they came from. -var "var.MyDir" refers to a pre-
processor variable we set up in the build section of the setup project's properties. Finally, -out ".\HeatFile" refers to the
name we give the heat file. 

Explanation of Product.wxs: 
On lines 8-10, include statements are provided for references used. In this case, they are for IIS Extension and Util Extension. References must also be added by navigating to the "References" section of the Setup Project in the Solution Explorer, right clicking, and selecting "Add Reference"

Inside the <Product> element on lines 13-14, the Id is set to "*", which does not change. This generates a new GUID each time the project is built. "Language = 1033" refers to "United States - English. Other language codes can be accessed on MSDN : https://msdn.microsoft.com/en-us/goglobal/bb895996.aspx. Manufacturer must be filled in, and is your name or your company's name. Upgrade Code is a GUID that does not change, as it is referred to in checking to see if the program already exists. InstallScope should be set to perMachine, but is perUser by default. 

More instructions coming...
