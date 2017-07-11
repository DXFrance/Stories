# Bringing xcubes Desktop Application to Windows Store


Microsoft teamed up with XCubes Team to convert their application, XCubes, to the Universal Windows Platform (UWP), using Desktop Bridge.

By bringing its first software to the Windows Store, XCubes Team saw a great opportunity to increase the number of downloads, reach more than 400 million PC around the world, provide a richer user experience and benefit from a simpler installation and update process management. 
XCubes is a multidimensional spreadsheet system based on Business Intelligence concepts and technologies where data is stored in multidimensional cubes and the calculation logic in dimensions. More powerful than PivotTables it allows much easier data manipulation and analysis. It is perfectly suited for financial simulation, planning, reporting as well as marketing analysis and much more.
As more and more of their customers are running the current Desktop XCubes Application on Windows 10, XCubes decided to distribute this Software through the Windows Store</p>

 ![Company logo](../images/xcubes/xcubes.png) 

 ![Team](../images/xcubes/xcubes_2.png) 
 
 ![Team](../images/xcubes/xcubes_3.png)


The solution relies on :</p>
- [Desktop App Converter](https://www.microsoft.com/fr-fr/store/p/desktop-app-converter/9nblggh4skzw) : Used to automatically convert the Desktop Application into a package compliant with Windows Store</p>
- [Windows 10 SDK (10.0.15063.0)](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive ) : Windows 10 SDK which includes the tools </p>
- [Windows 10 Base Image 15063](http://aka.ms/converterimages/) : This Base Image is necessary to generate the package for Creator Update</p>
 

 ![Team](../images/xcubes/xcubes_1.png)


**Core Team:** </p>
- Marc Braun - XCubes Team Leader </p>
- Maud Tournay - Business Evangelist, Microsoft</p>
- Fred Le Coquil - Technical Evangelist, Microsoft</p>



## Customer profile ##

**XCubes** is an ISV specialized in data mining. Their main product is xcubes, a leading data mining software in France that provides tools for financial simulation, planning, reporting as well as marketing analysis. xcubes is a very comprehensive and high-performance tool, renowned as one of the best  data mining software program on the market. It offers several sample projects (import data, Consolidation, Timeline based journal...) you can use with your own data.
The xcubes software can be purchased online.

 ![Team](../images/xcubes/xcubes_4.png)

 [Xcubes Web Site](http://www.xcubes.net/)
  
## Problem statement ##

XCubes is a Win32 Desktop application available from their [official Web Site](http://www.xcubes.net/). The application is installed with a specific installation program. The Win32 application is not the best experience for Windows 10 users, but th XCubes Team did not want to rewrite all the code to create a new Universal Windows Platform (UWP) app to leverage Windows Store capabilities. They have been trying to integrate xcubes.com to the Windows Store since December 2016 and but they were facing deployments issues.

The easiest approach was to use the Desktop Bridge App Converter to automatically convert their Desktop Application into packages distributed with the Windows Store.
This effort did start last year using Windows 10 Anniversary Update, unfortunatelly the packages generated for this version of Windows 10 were not functional: the application didn't start when the user click on the application Tile.</p>
 
## Solution and steps ##

As the main objective of this initiative is to generate Application packages for the latest version of Windows 10 aka Creator Update, you need to prepare your configuration to support Windows 10 Creator Update.</p>


1. Install the latest version of Windows 10 (Creator Update) Build 15063 on your machine

2. Install on the same machine Windows 10 SDK (10.0.15063.0)


      https://developer.microsoft.com/en-us/windows/downloads/sdk-archive 


3. Install [DesktopAppConverter from Windows Store](https://www.microsoft.com/en-us/store/p/desktopappconverter/9nblggh4skzw) 

4. Switch your machine to developer mode under Settings - Update & Security - For Developers 

 ![Team](../images/xcubes/xcubes_ux_11.png)

5. Add the container feature

 ![Team](../images/xcubes/xcubes_ux_10.png)

6. Download the base image 15063 from there http://aka.ms/converterimages

7. Install the base Image 15063. Launch DesktopAppConverter.exe and enter the following command in the command shell window:</p>


       DesktopAppConverter.exe -Setup -BaseImage C:\temp\BaseImage-15063.wim


8. Your machine is now configured to generate Application packages for Windows 10 Creator Update.


You can now generate the package for xcubes application. As the application is only available as a Win32 (32 bits), the generated package will only support 32 bits binaries.</p> 

1. Copy the latest version of xcubes Installation Application on your machine. Don't forget to copy the content of the subfolders "Program files" and "Temp".</p>

 ![Team](../images/xcubes/xcubes_ux_12.png)

2. Create the Win32 package with DesktopAppConverter, enter the following command in the command shell window. The installation program uses the option "-PackageArch x86" to create a 32 bits package</p>


       DesktopAppConverter.exe -Installer C:\projects\xcubes\inputs\xcubes.msi  -PackageArch x86 -Destination "C:\projects\xcubes\outputs" -AppExecutable "C:\Program Files (x86)\XCubes\XCubes.exe" -PackageName "XCubes" -PackageDisplayName "XCubes"  -AppDisplayName "XCubes"  -AppDescription  "XCubes for financial simulation planning reporting as well as marketing analysis" -Version 1.0.0.0 -Publisher "CN=Marc Braun" - MakeAppx -Verbose  



3. After few minutes the new package is available under "C:\projects\xcubes\outputs\xcubes.appx" . The appx file is built using the files under "C:\projects\xcubes\outputs\PackageFiles". For troubleshooting, you can use the log files under "C:\projects\xcubes\outputs\Logs".

4. You can update the package file for instance to use updated logos. In the command Window, launch the following commands:</p>


       "C:\Program Files (x86)\Windows Kits\10\bin\x64\makeappx.exe" pack /d "C:\projects\xcubes\outputs\PackageFiles" /p "C:\projects\xcubes\outputs\xcubes.appx"

5. The Appx file is now ready to be published, you only need to sign the package wit the company certificate using the following command line: </p>


       "C:\Program Files (x86)\Windows Kits\10\bin\x64\signtool.exe" sign -f C:\projects\xcubes\outputs\Certificate.pfx -p [password] -fd SHA256 -v C:\projects\xcubes\outputs\xcubes.appx


6. Now the Appx file is ready to be published. You can test the installation using the following Powershell command: </p>


       Add-AppxPackage -Path C:\projects\xcubes\outputs\xcubes.appx

You can also install the package file in double-clicking on the file xcubes.appx in the file explorer.

1. Double-click on the file xcubes.appx in the file explorer, on the dialog box "Install G én éatique 2017", click on the button "Install"

 ![Team](../images/xcubes/xcubes_ux_1.png)

2. The installation program is copying the files on your machine.

 ![Team](../images/xcubes/xcubes_ux_2.png)

3. Once the installation is completed, click on the button "Launch" to launch the application.

 ![Team](../images/xcubes/xcubes_ux_3.png)

4. The xcubes Welcome dialog box is displayed on the screen.

 ![Team](../images/xcubes/xcubes_ux_4.png)


If you don't have a company certificate you can create your own test certificate using the command lines below:


1. Create a certificate using the command lines: </p>


       "C:\Program Files (x86)\Windows Kits\10\bin\x64\makecert.exe" -r -h 0 -n "CN=CENTRE DE DEVELOPPEMENT DE L''INFORMATIQUE PERSONNELLE, OU=Secure Application Development, O=CENTRE DE DEVELOPPEMENT DE L''INFORMATIQUE PERSONNELLE, L=OSNY, S=Val-d''Oise, C=FR"  -eku 1.3.6.1.5.5.7.3.3 -pe -sv C:\projects\xcubes\outputs\TempCert.pvk C:\projects\xcubes\outputs\TempCert.cer


2. Create the pfx file using the command lines:</p>


       "C:\Program Files (x86)\Windows Kits\10\bin\x64\pvk2pfx.exe" -pvk  C:\projects\xcubes\outputs\TempCert.pvk -spc C:\projects\xcubes\outputs\TempCert.cer -pfx C:\projects\xcubes\outputs\TempCert.pfx


3. Once the pfx file is created, you can import the certificat on the machine where you to test the application package. Double-click on the pfx file, select the  **local machine**  for the Store Location and click on the Next button.</p>

![](../images/xcubes/xcubes_ux_7.png)


4. Enter the password associated with your certificate and click on the Next button.</p>

![](../images/xcubes/xcubes_ux_9.png)


5. Finally select **trusted people** certificate store, click on the Next button and Finish button to complete the import.</p>

![](../images/xcubes/xcubes_ux_8.png)




## Conclusion ##

The main objective of this project was to enable the XCubes team to distribute his application through the Windows Store in order to address the 500 Million devices running Windows 10.
After a first unsuccessful attempt using Anniversary Update - the package generated with Desktop App Converter was unable to launch the application - we finally managed the migration while using the Creator Update.
The application generated and installed through the Windows Store is now completely functionning.


**Measurable impact/benefits resulting from the implementation of the solution:**</p>
With the current version of Windows 10 Creator Update, the generation of the xcubes package is automated. This package can be generated alongside the binaries for the Win32 Desktop application.

**General lessons:**</p>
Though xcubes is a very complex application with several third parties dependencies (40 MBytes), it is possible to generate an installation package compliant with the Windows Store using the Desktop App Converter.
Moreover, as the application is now available through the Windows Store, the XCubes team can now seize the opportunity to introduce a new business model using In App Purchase.


**Opportunities going forward**</p>
This project was the first step for the distribution of xcubes Application through Windows Store.
Below a list of extensions which could improve xcubes application running on Windows 10:
1. Calling the UWP API from the Win32 Application to support Tiles, Notification, Cortana...
2. Use Windows Store In App Purchase API to change the business model associated with the application.
3. Prepare a full UWP implementation

## Additional resources ##
Below a list of links to resources used by the team:
- Desktop Bridge:</p>
https://developer.microsoft.com/en-us/windows/bridges/desktop </p>
- Desktop app bridge to UWP Samples on github: </p>
https://github.com/Microsoft/DesktopBridgeToUWP-Samples </p>
- Startup Desktop Bridge extension for UWP application Sample Application:</p>
https://github.com/flecoqui/Windows10/tree/master/Samples/StartupUWP </p>

