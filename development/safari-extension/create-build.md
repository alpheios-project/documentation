
# How To Create a QA Build 

**1. Pull latest code from master**

**2. Increment build number**

Keep the version and build numbers consistent across the App and the App Extension. Eventually we would like to automate this but for now we will do it manually.

*Update Package.json*

`build`: increment when producing a build for QA or for release

`version`: only increment with a new release

*Update Info.plist files*

Increment the build number (`CFBundleVersion`) for both the App and the Extensionto match the build number in package.json

Make sure the release version number (`CFBundleShortVersionString`) for both the App and the Extension matches that of the package.json version.

**3. Build javascript code**

 `npm run build-safari`

(Js/Css files are made using a standard npm build process with using `config-content-safari.mjs` file and `alpheios-node-build` tool.)

**4. Commit to GitHub**

Commit the built code to GitHub

**5.Build and Archive MacOS application**

In Xcode:

**Product -> Clean Build Folder**
**Product -> Build**
**Product -> Archive**

In the left Panel you will see all saved builds for macOS Apps, when you choose the one of them at the right panel you will be able to see the Details:

   ![Screenshot](../../images/safari-create-build1.png)

Click **Distribute App**, select **Development** and click **Next**

   ![Screenshot](../../images/safari-create-build2.png)

Make sure the Team on the signing page is The Alpheios Project, Ltd. and choose your Mac Developer certificate, then the select the **Alpheios App Developer Profile** profile for the Alpheios App and the **Alpheios Safari Developer Profile** for the Alpheios Safari App Extension and click **Next**

   ![Screenshot](../../images/safari-create-build-3b.png)

You will be shown the screen with final properties, click **Export**

   ![Screenshot](../../images/safari-create-build4.png)

Define Archive Folder name and click **Export**

   ![Screenshot](../../images/safari-create-build5.png)

Finally you should have the ready folder in SafariBuild with AlpheiosSafariApp (names from my example).

## 6. Pack MacOS application to .dmg file

For creating .dmg from archive application folder:

1. Open Applications -> Utilities -> Disk Utility

   ![Screenshot](../../images/safari-create-build6.png)

2. Click File -> New Image -> Image from Folder

   ![Screenshot](../../images/safari-create-build7.png)

3. Choose created archive application folder and click **Chose**

   ![Screenshot](../../images/safari-create-build8.png)

4. Click **Save** (in this dialog window you could define dmg file name)

   ![Screenshot](../../images/safari-create-build9.png)

5. Copy the dmg file to the Alpheios-Safari Dropbox folder
