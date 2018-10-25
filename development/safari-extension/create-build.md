# How To Create a Build (all steps)

The update and build process for Safari App Extension contains the following stages:

1) *(Optional)* Update content part (js, css files)
2) *(Optional)* Change version and rebuild MacOS application
3) Archive MacOS application
4) Pack MacOS application to .dmg file

## 1. Update content part (js, css files)

Js/Css files are made using a standard npm build process with using `config-content-safari.mjs` file and `alpheios-node-build` tool.

The command is `npm run build-safari` (details of steps and parameters you could see in the `package.json`, `scripts` section).

Entry file for starting a build process you could see in `config-content-safari.mjs` (webpack -> common -> entry).

Current versions of all used libraries you could see in `package.json`.

## 2. Change version and rebuild MacOS application

Alpheios Safari App Extension consists of Application and Safari Extension. 
Each of them has its own `Info.plist` file with the following fields ([apple doc](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/)):

Field name (XCode) | Field Key | Description
------------ | ------------- | -----------------
Bundle name | CFBundleName | Specifies the short name of the bundle, which may be displayed to users in situations such as the absence of a value for CFBundleDisplayName. This name should be less than 16 characters long.
Bundle Display name | CFBundleDisplayName | Specifies the display name of the bundle, visible to users and used by Siri
Executable file | CFBundleExecutable | Name of the bundle’s executable file.
Bundle identifier | CFBundleIdentifier | An identifier string that specifies the app type of the bundle. The string should be in reverse DNS format using only the Roman alphabet in upper and lower case (A–Z, a–z), the dot (“.”), and the hyphen (“-”).
Bundle version | CFBundleVersion | The build-version-number string for the bundle.
Bundle versions string, short | CFBundleShortVersionString | The release-version-number string for the bundle.
Bundle OS Type Code | CFBundlePackageType | The four-letter code identifying the bundle type. The type code for apps is APPL; for frameworks, it is FMWK; for loadable bundles, it is BNDL.
Copyright (human-readable) | NSHumanReadableCopyright | This key contains a string with the copyright notice for the bundle; for example, © 2016, My Company. You can load this string and display it in an About dialog box. The system uses this string in the app’s Info window in Finder.
NSHumanReadableDescription | NSHumanReadableDescription | When the app extension is installed, the string value of this key appears in Safari Extensions Preferences.
