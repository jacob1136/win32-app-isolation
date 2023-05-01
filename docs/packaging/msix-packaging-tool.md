# Table of Contents
1. [Convert an existing win32 installer into an .msix app](#Win32)
2. [Convert an existing .msix app to run isolated](#MSIX)

## Overview

This page will cover everything needed to package an existing MSIX or win32 application into
Isolated Win32 App. This will be done through the [MSIX Packaging Tool](https://learn.microsoft.com/en-us/windows/msix/packaging-tool/tool-overview) 

## Win32 -> MSIX <a name="Win32"></a>

1. Select "Application Pacakge" on the far left and choose where the package will be created.
This flow will follow the "Create package on this computer" option.

    ![image](https://github.com/microsoft/win32-app-isolation/blob/main/docs/packaging/images/01-packaging-main-menu.png)

2. Wait for the "MSIX Packaging Tool Driver" field to finish checking

    ![image](https://github.com/microsoft/win32-app-isolation/blob/main/docs/packaging/images/02-packaging-prepare.png)

3. Use the browse button to navigate to and select the win32 installer. Leave signing preference
blank as we will need to edit the manifest and sign it again.

    ![image](https://github.com/microsoft/win32-app-isolation/blob/main/docs/packaging/images/03-packaging-installer.png)

4. Enter the package information. The publisher name must match the name on the certificate used to sign in later steps.

    ![image](https://github.com/microsoft/win32-app-isolation/blob/main/docs/packaging/images/04-packaging-package-info.png)

5. Go through the win32 installer as normal

6. If there are additional entry points besides the main one, launch or browse to them. If the app
has options for File Type Association in settings/config/preferences, toggle them at this step so
MSIX will pick up on them

7. Repeat the same process if there are services in the package

8. Clicking Create will save the package as a full trust package. Click the "Package Editor" button
to go to the "Package Editor" flow from the main menu
    
    ![image](https://github.com/microsoft/win32-app-isolation/blob/main/docs/packaging/images/05-packaging-create-package.png)

## MSIX -> Isolated Win32 <a name="MSIX"></a>
1. Select the far right option "Application Pacakge" and browse to the .msix file and click the
"Open package" button.

    ![image](https://github.com/microsoft/win32-app-isolation/blob/main/docs/packaging/images/01-packaging-main-menu.png)

2. Scroll down to the "Manifest file" section and click "Open file"

    ![image](https://github.com/microsoft/win32-app-isolation/blob/main/docs/packaging/images/10-packaging-package-editor.png)

    In the manifest, the following changes will need to be made.
   
    * Add `xmlns:previewsecurity2="http://schemas.microsoft.com/appx/manifest/preview/windows10/security/2"`
    to the `<Package>` element
 
    * Add `previewsecurity2` to `IgnorableNamespaces` at the end of the `<Package>` element

    * In `<Dependencies>` change `TargetDeviceFamily` to 
    `<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.22622.0" MaxVersionTested="10.0.22622.0" />`
  
    * In `<Application>` replace any existing entrypoint/trustlevel/runtimebehavior with
    `uap10:TrustLevel="appContainer" previewsecurity2:RuntimeBehavior="appSilo"`

    ![image](https://github.com/microsoft/win32-app-isolation/blob/main/docs/packaging/images/11-packaging-manifest.png)

3. The app might need additional capabilities to function correctly now that it has been isolated.

    These capabilities directly add functionality back to isolated apps.

    * `isolatedWin32-print` - Print documents 
    * `isolatedWin32-sysTrayIcon` - Display notifications from the Systray
    * `isolatedWin32-shellExtensionContextMenu` - Display COM-based context menu entries
    * `isolatedWin32-promptForAccess` - Prompt Users for file access
    * `isolatedWin32-accessToPublisherDirectory` - Access to directories that end with the publisher ID

    These capabilities allow minimal access to libraries such as MSVC runtime or other Windows/3rd 
    Party DLLs for applications that don't support prompting.

    * `isolatedWin32-dotNetBreadcrumbStore`
    * `isolatedWin32-profilesRootMinimal`
    * `isolatedWin32-userProfileMinimal`
    * `isolatedWin32-volumeRootMinimal`

4. Save and close the manifest window. If there are any errors in the manifest, MPT will display 
them. Select Create/Save to generate the .msix file.