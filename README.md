# ESXi-Customizer

Requirements
A Windows computer (XP or newer) with Powershell 2.0 or newer
VMware PowerCLI version 5.1 or newer

Instructions

ESXi-Customizer-PS is a Powershell script that you launch from within a Powershell or a PowerCLI console window. It accepts various command line options. One of them is -help that will display this help screen:

Basically the script has three different operating modes:
Create an ESXi installation ISO or Offline Bundle from the VMware Online depot (standard mode)
Create an ESXi installation ISO or Offline Bundle from a local ESXi Offline Bundle (-izip mode)
Update a local ESXi Offline Bundle with an ESXi patch bundle from the VMware Online depot (-izip -update mode)
With all three modes you can optionally add bundles from the V-Front Online Depot, any other Online Depot (by URL) or locally stored Offline Bundles and VIB files (e.g. downloaded drivers or software packages).

Highly recommended - If you like learning by watching videos then give it a try!

To get the full picture though you should read through the following documentation before you start using the script. Here are some examples to get you going:

a) The simplest use case: Create a vanilla ESXi installation ISO with the latest patch level
.\ESXi-Customizer-PS-v2.6.0.ps1
Calling the script without any parameters will create an ESXi installation ISO of the latest ESXi version (6.5 as of now) and its latest patch level. The ISO file will be created in the script directory. You can modify this behavior by using one or more of the following parameters:
-v50 : Create the latest ESXi 5.0 ISO
-v51 : Create the latest ESXi 5.1 ISO
-v55 : Create the latest ESXi 5.5 ISO
-v60 : Create the latest ESXi 6.0 ISO
-v65 : Create the latest ESXi 6.5 ISO
-v67 : Create the latest ESXi 6.7 ISO
-outDir : Write the ISO file to a custom directory. If this switch is used then the script's log file will also be moved here and named after the Imageprofile name and timestamp.
-sip : Do not automatically use the latest image profile (= patch level), but display all in a menu and let me select a specific one. The menu will be sorted by date starting with the latest one. It will also list the image profiles that contain only security fixes and/or no VMware Tools.
-ozip : Do not output an installation ISO but an ESXi Offline Bundle that you can use for importing into Update Manager, command line patching with esxcli or as input for further customizations

b) Use an ESXi Offline Bundle as input (instead of the VMware Online depot):
.\ESXi-Customizer-PS-v2.6.0.ps1 -izip .\VMware-ESXi-6.0.0-2494585-depot.zip
ESXi Offline Bundles can be downloaded from the VMware Patch Download portal. Some hardware vendors (e.g. HP) also provide their customized ESXi version as Offline Bundles (these can be found on the vSphere download pages), and you could finally create your own customized ESXi Offline bundle using this script.

The command above will create an ESXi installaton ISO from the ESXi 6.0 GA Offline bundle. Again you can modify the behavior by using -outDir, -sip and/or -ozip like described under a).
c) Adding additional packages from connected Online Depots
.\ESXi-Customizer-PS-v2.6.0.ps1 -v55 -load net-r8168,net-r8169,net-sky2
This script call will build the latest ESXi 5.5 installation ISO with the NIC drivers added that were dropped with ESXi 5.5. These packages are still in the VMware Online Depot, because they are part of all ESXi 5.0 and 5.1 image profiles, and the script just re-adds them to the latest ESXi 5.5 image profile.

Please note that this does not work for ESXi 6.0, because these drivers are blacklisted there. for details see my ultimate guide to upgrade your white box to ESXi 6.0

d) Connect the V-Front Online Depot and other depots
.\ESXi-Customizer-PS-v2.6.0.ps1 -v60 -vft -load sata-xahci,net55-r8168
This will make the script connect to the V-Front Online Depot and add the sata-xahci and net55-r8168 packages from there to the latest ESXi 6.0 Imageprofile.

e) Adding local Offline Bundles and/or VIB files
.\ESXi-Customizer-PS-v2.6.0.ps1 -pkgDir C:\temp\pkg
This command will add all Offline bundles and VIB files that are stored in the directory C:\temp\pkg. This way you can add 3rd party or community supported device drivers and software packages.

f) Updating an ESXi Offline bundle from the VMware Online depot
.\ESXi-Customizer-PS-v2.6.0.ps1 -v60 -izip .\VMware-ESXi-6.0.0-2494585-HP-600.9.1.39-Mar2015-depot.zip -update
With this script call you update a local ESXi Offline bundle (in this example: the HP customized version of ESXi 6.0 GA) with the latest ESXi 6.0 patch from the VMware Online depot. When using -update please also specify the ESXi version that matches the one of the local Offline bundle (-v65, -v60, -v55, -v51 or -v50).

g) Advanced options

For the sake of completeness here are the remaining optional parameters:
-log: Specify a custom log file (the default is in %TEMP% or in outDir if specified)
-test : this is for testing the validity and effect of the specified parameters without actually building the output file. Can save a lot of time, because it also skips most downloads from the VMware Online depot.
-nsc : Use the -noSignatureCheck option with the export function. Try this if you get an error message like "Could not find trusted signer." because of packages with missing or invalid electronic signatures.
-ipname
-ipdesc
-ipvendor : Override the image profile's name, description and vendor attributes. By default the name and description are derived from the original image profile by adding the string "customized", and the vendor is kept as is.
-remove vib1[,...] : Remove one or more VIB packages from the custom Imageprofile
Licensing

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

A copy of the GNU General Public License is available at http://www.gnu.org/licenses/.


Support

Please check the "Known Bugs and Issues" section first to see if your issue is covered there!

If you have trouble using the script then please send an email to ESXi-Customizer-PS@v-front.de. Be sure to include a transcript of the script execution (create it by using the Start-Transcript cmdlet before calling the script). Otherwise I might just ignore your message.
