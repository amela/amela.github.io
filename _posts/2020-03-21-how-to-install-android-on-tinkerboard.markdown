---
layout: post
title:  "How to install Android on Tinkerbord from macOS"
date:   2020-03-21 17:00:00 +0100
categories: Tinkerboard macOS Android GooglePlay Mediacenter
---
Here are the instructions on how to install Android and Google play on Tinkerboard. With this setup I created mini Media center. For this project you will need:

* Tinkerboard
* Compatible power supply (the one for charging the phone was not enough for me)
* Micro sd card (I used 128GB one)
* Peripherals (keyboard, mouse)
* macbook (with macOS), sd card adapter

## Flashing Android on micro sd card


1. Download latest Android from [here](https://www.asus.com/uk/Single-board-Computer/TINKER-BOARD/HelpDesk_Download/) and unzip the file
2. Insert micro sd card into macbook via adapter
3. Open disk utility and format sd card as FAT 32
4. Remember Device (in my case it was disk2s1, but we only need to know that it’s disk2)
5. Unmount the card via Disk Utility
6. Flash the image card: 
        
        sudo dd if=/Users/amela/Downloads/20191227-tinker-board-android-nougat-userdebug-v14.4.0.5.img of=/dev/rdisk2 bs=1m
It took about 2 minutes to complete the dd command
7. Plug sd card into Tinker Board and gooo :)

## Installing Google Play on Android

1. Find out the local IP address of your Tinker Board. (Mine was 192.168.1.113)
2. Install adb command with Homebrew:

        brew cask install android-platform-tools
3. On my system (macOS Catalina 10.15.1), adb command could not be used. I managed to fix this running: 

        cd /usr/local/Caskroom/android-platform-tools/29.0.6/platform-tools/ && open .
4. Then right-click on adb, select *Open*. This time, you can click *Open* instead of *Move to Bin*. To test if adb now works run:
   
        adb devices
5. Download [Google Play Services][play-services], [Google Services Framework][play-gsf], and [Google Play Store][play-store] apks
6. Rename apks into playservices.apk, gsf.apk and playstore.apk

        adb connect 192.168.1.133
        adb root
        adb connect 192.168.1.133
        adb shell
        mount -o rw,remount /system
        exit
        adb push playservices.apk /system/priv-app/
        adb push gsf.apk /system/priv-app/
        adb push playstore.apk /system/priv-app/
7. Reboot the Tinker Board
8. On my system, Google Play Services kept shutting down. An update of Google Play services fixes the problem.

        connect 192.168.1.133
        root
        install -r playservices.apk
9. Now you should be able to install apps you like.

## Credits
Check out [ossblog][flashing-instrucion] for more info on how to flash sd card and [x10.dev][google-play-instructions] on how to install GooglePlay. 

[google-play-instructions]: https://www.ossblog.org/installing-google-play-store-asus-tinker-board/
[flashing-instrucion]: https://www.x10.dev/flashing-tinkeros-onto-your-sd-card-from-macos/
[play-services]: http://www.apkmirror.com/apk/google-inc/google-play-services/google-play-services-10-5-62-release/google-play-services-10-5-62-438-153733333-android-apk-download/
[play-gsf]: http://www.apkmirror.com/apk/google-inc/google-services-framework/google-services-framework-6-0-1-release/google-services-framework-6-0-1-android-apk-download/
[play-store]: http://www.apkmirror.com/apk/google-inc/google-play-store/google-play-store-7-7-31-release/
