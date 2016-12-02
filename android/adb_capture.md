# adb로 스크린 캡쳐하기 
Grab Android screenshot to computer via ADB

There are many ways to take a screen shot on Android device. One simple way to capture the screen on Galaxy Nexus is to simultaneously press and hold Power and Volume Down buttons. The image will be saved in a "Screenshot" directory and accessible via Gallery.

Quite often, however, I need to copy the captured image over to my computer. Usually, I do this with adb pull command, but if I'm going to use CLI to retrieve the file, why don't I take the screen shot with it as well?

One method is to use screencap command via adb shell like so:
```
adb shell screencap -p /sdcard/screen.png
adb pull /sdcard/screen.png
adb shell rm /sdcard/screen.png
```
