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
InfoDictionary version | CFBundleInfoDictionaryVersion | (Recommended) Version information for the Info.plist format. Identifies the current version of the property list structure. This key exists to support future versioning of the information property list file format. Xcode generates this key automatically when you build a bundle and you should not change it manually. The value for this key is currently 6.0.
Bundle name | SFBundleName | Specifies the short name of the bundle, which may be displayed to users in situations such as the absence of a value for CFBundleDisplayName. This name should be less than 16 characters long.