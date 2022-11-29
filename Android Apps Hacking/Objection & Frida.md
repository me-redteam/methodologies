### Frida:

It’s a dynamic code instrumentation toolkit. 
It lets you inject snippets of JavaScript or your own library into native apps on Windows, macOS, GNU/Linux, iOS, Android, and QNX. 
Frida also provides you with some simple tools built on top of the Frida API. These can be used as-is, tweaked to your needs, or serve as examples of how to use the API.

Frida’s core is written in C and injects Google’s V8 engine into the target processes, where your JS gets executed with full access to memory, hooking functions and even calling native functions inside the process. 

<br>

### Objection:

Components:

objection is made up of three primary components:

	1. Frida Gadget that runs in embedded mode and starts up with a patched mobile application. 
Frida is responsible for most of the magic under the hood, running the hooks provided by objection ultimately making this tool possible in the first place.

	1. Objection tool itself. 
This is a python software component that provides the objection commands and explore REPL (read-eval-print-loop).
It is responsible for interacting with a loaded Frida Gadget, running hooks and parsing the output generated by those.

	1. Objection hooks themselves.
These hooks are run using Frida within their respective runtimes to make all of the objection features you can use possible.

Prerequisites:

To run objection, all you need is the python3 interpreter to be available. Installation via pip should take care of all of the dependencies needed. For more details, please see the prerequisites section on the project wiki.

As for the target mobile applications though, for iOS, an unencrypted IPA is needed and Android just the normal APK should be fine. If you have the source code of the iOS application you want to explore, then you can simply embed and load the FridaGadget.dylib from within the Xcode project.

Installation:

Installation is simply a matter of `pip3 install objection`. This will give you the objection command.

<img width="713" alt="image" src="https://user-images.githubusercontent.com/48615614/204358616-14e20c6b-a7bf-4001-b88a-d1e36fcf0310.png">

#### Getting Started:

You have two options:

  1. Working with unrooted device ==> you will need to patch your apk with frida gadget.
  2. Working with rooted device ==> you will need to copy frida-server to your mobile and run it.

<br><br>

### Frida-server for rooted device

First off, download the frida-server for Android from our releases page and get it running on your device:
https://github.com/frida/frida/releases

```
$ adb push frida-server /data/local/tmp/ 
$ adb shell "chmod 755 /data/local/tmp/frida-server"
$ adb shell "/data/local/tmp/frida-server &"
```

Note: make sure that you download the frida-server matching with version of frida "12.0.3 in my case" and your mobile Arch "arm64 for Lenovo A7000"

![image](https://user-images.githubusercontent.com/48615614/204359843-a8d78b20-d10c-42c3-b8b7-81d8ca471e18.png)

<br>

#### Android apk Patching 

The first thing is to patch the apk, embedding a Frida gadget that can be used with objection or just Frida itself.

The `objection patchapk`  is a command that basically wraps around several other system commands, automating the patching process as far as possible. 

Patching - dependencies:

```
• aapt - from: http://elinux.org/Android_aapt
• adb - from: https://developer.android.com/studio/command-line/adb.html
• jarsigner - from: http://docs.oracle.com/javase/7/docs/technotes/tools/windows/jarsigner.html
• apktool - from: https://ibotpeaches.github.io/Apktool/
```

They can be installed using apt or manually but make sure that they are available in your current PATH.

If your application is already installed on the device and you want to get the apk, please follow the below steps:

1. Locate the package name for the application you downloaded:

`$ adb shell pm list packages | grep <application name>`

![image](https://user-images.githubusercontent.com/48615614/204360550-ff5838a1-15b0-450a-9b35-250cb11d2f7e.png)

2. determine its installed path on the device:

`$ adb shell pm path <package name>`

![image](https://user-images.githubusercontent.com/48615614/204360633-83c2b63c-d83e-410d-b89d-e3972bb8fcd9.png)

	Note:  you will find the any application apk at /data/app/<packagename>/base.apk 

3. Pull the apk from the device:

`$ adb pull /data/app/<packagename>/base.apk appname.apk`

![image](https://user-images.githubusercontent.com/48615614/204360827-c32afed6-c84e-454f-93b2-203105db4e37.png)

After you got the apk file it's time to patch it with `objection patchapk`.

`objection patchapk -s <apk file>`

![image](https://user-images.githubusercontent.com/48615614/204360998-0838bd69-e8d2-433c-b872-26002fe929db.png)

Once you have a patched APK ready, its time to install it.

![image](https://user-images.githubusercontent.com/48615614/204361051-461cec00-f1df-4557-b50f-3f790da45a6a.png)

<br>

### Getting started with android

#### Unrooted Device:

With objection installed, a patched APK installed to your Android device and with the device connected and authorized to your computer via USB you may start using objection:

`objection explore`

![image](https://user-images.githubusercontent.com/48615614/204361448-27e5cfa9-0586-4c6e-bb5d-9fae07fc02c3.png)


#### Rooted Device:

With frida-server runnig on your rooted device: 
Get your application process name:

`frida-ps -Ua`

![image](https://user-images.githubusercontent.com/48615614/204381931-e90fbd43-4133-4b9d-b191-1259cb6cfaf5.png)

`objection --gadget "com.cohaesus.mafshare" explore`

![image](https://user-images.githubusercontent.com/48615614/204382051-5dc62140-aedd-4ab5-afcf-035c500b3e4b.png)

Android Commands: 

Root:
Disable:
Attempts to disable root detection
`android root disable`

![image](https://user-images.githubusercontent.com/48615614/204382267-38a2766b-f3fc-4eb3-a897-e2bee61682b3.png)

Simulate:
Attempts to simulate a rooted android envirnment, by responding positively to common checks that are performed within the application.

`android root simulate`

![image](https://user-images.githubusercontent.com/48615614/204382412-e4076ba7-8943-477f-bf4a-300e2215b7a9.png)

Hooking:
List:
Activities:
you can list all the application registered activities using the following command:

`android hooking list activities`

![image](https://user-images.githubusercontent.com/48615614/204382662-faf6c930-ee8d-40d0-808c-51321b9eb4e7.png)

Class Methods:
you can list all the methods in a class using the following command:

`android hooking list class_methods <ClassName>`

![image](https://user-images.githubusercontent.com/48615614/204382739-228aa189-e423-4b6a-921a-92cf6eb6ae3d.png)


Broadcast Receivers:
you can list all the Broadcast Receivers using the following command:

`android hooking list receivers`

![image](https://user-images.githubusercontent.com/48615614/204382814-487af96d-7d82-406e-b2d1-6740090edce7.png)

Services:
you can list all the services using the following command:

`android hooking list services`

![image](https://user-images.githubusercontent.com/48615614/204382939-d2a06069-bebe-4f50-b6c2-1499f70df369.png)


Set Return Value:

You can sets a method return value to always be true/false, this could be useful to use in case of SSL binning or root detection.
using the following command:
`android hooking set return_value "<fully qualified class name>" "<method>" <true/false>`

![image](https://user-images.githubusercontent.com/48615614/204383111-3917cd55-3a90-47d9-be3e-f10cb8141838.png)
	
Note: This only possible on methods returns a boolean. 

Watch: 
Class: 
You can hook a class and watch for methods calling using the following command:
`android hooking watch class <ClassName>`

![image](https://user-images.githubusercontent.com/48615614/204383274-bbbc4b93-ef5d-4ad7-ba8c-abbed4db9c09.png)


Class_methods:
You can hook a specified method in a class and watch for calling  using the following command:
`android hooking watch class <ClassName> <method>`

![image](https://user-images.githubusercontent.com/48615614/204383367-cdaa4776-cd9c-44e3-9496-d7b78c0e916c.png)

you can use the following options for both watch class and watch class_methods: 
			
	--dump-args: for dumbing the arguments value that the methods called with.
	--dump-return: for dumbing the method return value 
	--dump-backtrace: provide a full stack trace that lead to methods invocation will also be dumped. 

Intent Launch activity:
Launch an exported activity
`android intent luanch_activity <activity name>`

![image](https://user-images.githubusercontent.com/48615614/204383607-678e968e-92d4-4046-9abb-6dc428eedc89.png)

- Keystore:

List: list certificates or keys in the current application keystore
`android keystore list`

![image](https://user-images.githubusercontent.com/48615614/204383930-622799b5-23db-40d7-8ee0-df7d19335a15.png)

Clear: clear current application keystore
`android keystore clear`

![image](https://user-images.githubusercontent.com/48615614/204384089-efa294b1-984f-4dd7-8706-1c25f8ca482b.png)

Sslpinning disable: Attemps to disable sslpinnig using the following command: 
`android sslpinning disable`

![image](https://user-images.githubusercontent.com/48615614/204384165-20077c6a-e879-4315-b1ab-9fe2ddc189db.png)

Shell_exec: could be used as a shell to execute command but use `su` command to gain root permission

![image](https://user-images.githubusercontent.com/48615614/204384235-179422ac-f3b5-4afe-b9cf-f94b37ecde65.png)






























