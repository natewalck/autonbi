 AutoNBI.py - A tool to automate (or not) the building and modifying
 --------------------------------------------------------------------
  of Apple NetBoot NBI bundles.
  -------------------------------
 Requirements:
   * OS X 10.9 Mavericks - This tool relies on parts of the SIUFoundation
     Framework which is part of System Image Utility, found in
     /System/Library/CoreServices in Mavericks.

   * Munki tools installed at /usr/local/munki - needed for FoundationPlist.

 Thanks to: Greg Neagle for overall inspiration and code snippets (COSXIP)
            Per Olofsson for the awesome AutoDMG which inspired this tool
            Tim Sutton for further encouragement and feedback on early versions

 This tool aids in the creation of Apple NetBoot Image (NBI) bundles.
 It can run either in interactive mode by passing it a folder, installer
 application or DMG or automatically, integrated into a larger workflow.

 Required input is:

 * [--source][-s] The valid path to a source of one of the following types:

   - A folder (such as /Applications) which will be searched for one
     or more valid install sources
   - An OS X installer application (e.g. "Install OS X Mavericks.app")
   - An InstallESD.dmg file

 * [--destination][-d] The valid path to a dedicated build root folder:

   The build root is where the resulting NBI bundle and temporary build
   files are written. If the optional --folder arguments is given an
   identically named folder must be placed in the build root:

   ./AutoNBI <arguments> -d /Users/admin/BuildRoot --folder Packages
   |
   +-> Causes AutoNBI to look for /Users/admin/BuildRoot/Packages

 * [--name][-n] The name of the NBI bundle, without .nbi extension

 * [--folder] *Optional* The name of a folder to be copied onto
   NetInstall.dmg. If the folder already exists, it will be overwritten.
   This allows for the customization of a standard NetInstall image
   by providing a custom rc.imaging and other required files,
   such as a custom Runtime executable. For reference, see the
   DeployStudio Runtime NBI.

 * [--auto][-a] Enable automated run. The user will not be prompted for
   input and the application will attempt to create a valid NBI. If
   the input source path results in more than one possible installer
   source the application will stop. If more than one possible installer
   source is found in interactive mode the user will be presented with
   a list of possible InstallerESD.dmg choices and asked to pick one.

 To invoke AutoNBI in interactive mode:
   ./AutoNBI -s /Applications -d /Users/admin/BuildRoot -n Mavericks

 To invoke AutoNBI in automatic mode:
   ./AutoNBI -s ~/InstallESD.dmg -d /Users/admin/BuildRoot -n Mavericks -a

 To replace "Packages" on the NBI boot volume with a custom version:
   ./AutoNBI -s ~/InstallESD.dmg -d ~/BuildRoot -n Mavericks -f Packages -a