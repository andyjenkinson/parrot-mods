## Requirements

https://developer.android.com/studio/#downloads (just the command line SDK)
https://sourceforge.net/projects/adbextractor/
https://sourceforge.net/projects/s-tar/

## Install tools
```
brew install star
unzip android-backup-tookit-20180521.zip
unzip sdk-tools-darwin-4333796.zip
tools/bin/sdkmanager --install platform-tools
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