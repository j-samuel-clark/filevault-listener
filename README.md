# filevault-listener
A tool which checks for real-time status of the FileVault process and submits a Jamf recon in order to keep encryption statuses up to date as soon as encryption finishes.

## How filevault-listener works
This tool is designed to be installed as a component of a Jamf policy to enable FileVault encryption. The build of this project is designed to work within Jamf's Composer application. 

### Installed payloads:
`/usr/local/orgname/bin/fileVaultListener`
`/Library/LaunchDaemons/com.orgName.fileVaultListener.plist`

### What the payloads do:
When deployed as a PKG, the fileVaultListener executable will be installed as an executable payload into `/usr/local/orgName/bin`, and a postinstall script will write a global LaunchDaemon to check the status  run on a schedule defined by a LaunchDaemon. The LaunchDaemon runs by default every 30 seconds to trigger the `fileVaultListener` executable script, which checks for progress of the computer during the encryption process. Once encrypted, and only when encryption completes, the `fileVaultListener` reports the device status back to Jamf Pro. This gives the most accurate information about the encryption status to Jamf Pro where an admin would otherwise have to wait for a recon to take place on its own.

## How to use this tool
The filevault-listener tool is designed to leverage Jamf's Composer application in order to easily build this workflow as a deployable PKG within a Jamf Policy. To use this tool, simply:

1. Set the `orgName` variable within the `fileVaultListener` executable and `postinstall` scripts
2. Change the orgName folder.
3. Move the folder for the filevault-listener to your Composer.app Work Directory.
	- The default directory for Composer.app is `/Library/Application Support/JAMF/Composer/Sources`
4. Open Composer.app and change the name of 
5. Ensure the proper permissions are set for the deployment directory
	- The default owner and group permissions for the deployed directory should be `root:wheel 775`
6. Build the package and upload it to Jamf Pro
7. Deploy this package with a Jamf Pro Policy which enables FileVault. 

When the user's computer is finished encrypting, the `fileVaultListener` script will trigger a recon and the listener will remove itself.

NOTE: The framework for your org will not be removed after the `removeListener` function runs. You can modify this yourself if you want to remove all traces of the filevault-listener. For my purposes, having a working directory for "orgName" is kept in place. 