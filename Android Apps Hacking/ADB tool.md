ADB gives you huge flexibility while interacting with the device. Some of the most commonly used commands that might help you are:

	○ adb shell—Starts a remote shell in the target emulator and you can work on device as if you are physically using it.
	○ adb install<apk_file>—Installs the given APK file into the device. –swill make it install on the /sdcard.
	○ adb push—Copies a file from machine to your device.
	○ adbpull—Copies a file from device to your machine.
	○ adblogcat—Prints log data on the screen.

1. Get the full path name of the APK file for the desired package.
`adb shell pm path com.example.someapp`

This gives the output as:
`package:/data/app/com.example.someapp-2.apk`


2. Pull the APK file from the Android device to the development box.
`adb pull /data/app/com.example.someapp-2.apk`


Ref: http://www.droidviews.com/push-pull-files-android-using-adb-commands/
