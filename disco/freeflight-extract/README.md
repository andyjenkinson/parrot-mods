# Extracting FreeFlight

To stop FreeFlight nagging to update drone firmware after it's been downloaded, you can extract and modify the app data before restoring it to your phone. These instructions are for Mac.

## Requirements

* https://developer.android.com/studio/#downloads (download just the command line SDK)
* https://sourceforge.net/projects/adbextractor/
* https://sourceforge.net/projects/s-tar/ (install with brew)

## Prepare phone

1. [Enable USB debugging](https://www.technewscentral.com/how-to-enable-usb-debugging-and-developer-options-in-android-4-2-and-higher-android-4-2android-4-3android-4-4/id_7250)
2. Connect via USB

## Install tools
```
brew install star
unzip android-backup-tookit-20180521.zip
unzip sdk-tools-darwin-4333796.zip
tools/bin/sdkmanager --install platform-tools
wget https://github.com/andyjenkinson/parrot-mods/raw/master/disco/freeflight-extract/firmware_checker.patch
```
## Backup and unpack
```
platform-tools/adb backup -f freeflight3.ab com.parrot.freeflight3
java -jar android-backup-tookit/android-backup-extractor/android-backup-extractor-20180521-bin/abe.jar unpack freeflight3.ab freeflight3.tar
tar -tf freeflight3.tar > freeflight3.list
tar -xf freeflight3.tar
```
## Make modifications
```
cp -p apps/com.parrot.freeflight3/r/app_embedded_firmware/plfFolder/090e/disco_update.plf apps/com.parrot.freeflight3/f/plfFolder/090e/disco_update.plf
patch -p0 <../firmware_checker.patch apps/com.parrot.freeflight3/sp/firmware_checker.xml
```
## Repack and restore
```
star -c -v -f freeflight3-discofirmware1.4.1.tar -no-dirslash list=freeflight3.list
java -jar android-backup-tookit/android-backup-extractor/android-backup-extractor-20180521-bin/abe.jar pack freeflight3-discofirmware1.4.1.tar freeflight3-discofirmware1.4.1.ab
platform-tools/adb restore modified/freeflight3-discofirmware1.4.1.ab
```
