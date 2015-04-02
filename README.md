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

Inside the <Product> element on lines 13-14, the Id is set to "*", which does not change. This generates a new GUID each time the project is built. "Language = 1033" refers to "United States - English. Other language codes can be accessed on MSDN : https://msdn.microsoft.com/en-us/goglobal/bb895996.aspx. Manufacturer must be filled in, and is your name or your company's name. Upgrade Code is a GUID that does not change, as it is referred to in checking to see if the program already exists. InstallScope should be set to perMachine, but is perUser by default. Platform should be set to x86, otherwise it will install to Program Files(x86) folder. 

On line 16, we give an instruction to initialize, or make changes to the system necessary for the program to run. Then, a condition is set, checking to see if the program has already been installed. If so, the dialog changes to one allowing users to repair files or uninstall the program. 

On line 17, set EmbedCab="yes" to include the cabinet file with the .msi file. Otherwise, it will be separate. It is recommended to set it to yes unless you have a reason you need it to be separate. 

On lines 20-24, we set up variables for our custom logo and our company's software license.

Lines 27-34 check for .NET version 4.5 or higher, and IIS 7 or 8. If they aren't installed, a bootstrapper attached to this project installs them. 

Next, on lines 36-39, we find the IIS directory in the registry. If it isn't installed, the user is given a warning that they need to have it installed for the program to work correctly.

On lines 41-43, we set up properties for app pool and virtual directory name changes, should the user decide to change them on dialogs given (Dialogs not in use at this time, properties left as a stub for the next version). We also set up a property for our custom installer dialogs.

On lines 45-48, we tell the installer the folder we want to install to. The first line always is TARGETDIR and SourceDir. After this, we tell the installer to install to the IIS Root we already found. 

On lines 49-51, we set up a Start menu shortcut to the program. This will default to the ProgramFiles(x86) folder unless "Platform=x64" is specified in the Package element on line 14.

Lines 55-59 use the IIS reference we added to the setup project earlier to set up a website for our application in the Default Web Site folder. This can also be changed to add a new website for the program. However, it is important this part is left outside of a component and component group. If it is not left outside of a component group, the entire Default Website and it's contents, including external programs, can be deleted on uninstall. 

On lines 61-70, we set up our app pool. In versions of .NET above 3.5, it is recommended to set this to "Integrated", meaning the IIS pipeline runs right along with the ASP.NET pipeline in handling requests. "Classic" should be used for versions of IIS6 and below. 

Lines 74-90 give details on setting up the shortcut for our application in the Start menu, and for uninstalling the Start menu shortcut along with the rest of the program when the user chooses to uninstall it. It refers back to the IIS root directory in the registry, which we referred to earlier. 

All components we want to install must be included in a feature at the end of the file. Here, I've included the component group name given to the components included in the heat file generated for this program's files, "NewFilesGroup", the app pool, which is included in Product Components section, as well as our shortcut. 

