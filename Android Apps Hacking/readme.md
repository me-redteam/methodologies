## 1. Android Mobile Applications Penetration Testing
> Android penetration testing is a process of finding security vulnerabilities in an android application. It is a systematic approach to searching for weaknesses in an Android app, verifying the app’s security, and making sure it abides by the security policies. It includes trying to attack the android application by using various methods and tools. 

### Android OS Basics
![image](https://user-images.githubusercontent.com/48615614/203702398-45e0a4f5-16ed-449b-a0fb-a564ea2cd916.png)


#### Activities

An activity represents a single screen with a user interface. For example, an email application might have one activity that shows a list of new emails, another activity to compose an email, and another activity for reading emails. If an application has more than one activity, then one of them should be marked as the activity that is presented when the application is launched. An activity is implemented as a subclass of Activity class as follows:
  public class MainActivity extends Activity {
  }



#### Services
A service is a component that runs in the background to perform long-running operations. For example, a service might play music in the background while the user is in a different application, or it might fetch data over the network without blocking user interaction with an activity. A service is implemented as a subclass of Service class as follows:
	public class MyService extends Service {
	}


#### Broadcast Receivers
Broadcast Receivers simply respond to broadcast messages from other applications or from the system. For example, applications can also initiate broadcasts to let other applications know that some data has been downloaded to the device and is available for them to use, so this is broadcast receiver who will intercept this communication and will initiate appropriate action. A broadcast receiver is implemented as a subclass of BroadcastReceiver class and each message is broadcasted as an Intent object.
	public class MyReceiver extends BroadcastReceiver {
	}


#### Content Providers
A content provider component supplies data from one application to others on request. Such requests are handled by the methods of the ContentResolver class. The data may be stored in the file system, the database or somewhere else entirely. A content provider is implemented as a subclass of ContentProvider class and must implement a standard set of APIs that enable other applications to perform transactions.
	public class MyContentProvider extends ContentProvider {
	}


#### Additional Components
There are additional components which will be used in the construction of above mentioned entities, their logic, and wiring between them. These components are:

Components	                    Description
Fragments	Represents a behavior or a portion of user interface in an Activity.
Views	UI elements that are drawn onscreen including buttons, lists forms etc.
Layouts	View hierarchies that control screen format and appearance of the views.
Intents	Messages wiring components together.
Resources	External elements, such as strings, constants and drawables pictures.
Manifest	Configuration file for the application.



### Testing App Permissions
Permissions of applications installed on a device can be retrieved using Drozer. The following extract demonstrates how to examine the permissions used by an application, in addition to the custom permissions defined by the app:

```
dz> run app.package.info  -a com.android.mms.service
Package: com.android.mms.service
  Application Label: MmsService
  Process Name: com.android.phone
  Version: 6.0.1
  Data Directory: /data/user/0/com.android.mms.service
  APK Path: /system/priv-app/MmsService/MmsService.apk
  UID: 1001
  GID: [2001, 3002, 3003, 3001]
  Shared Libraries: null
  Shared User ID: android.uid.phone
  Uses Permissions:
  - android.permission.RECEIVE_BOOT_COMPLETED
  - android.permission.READ_SMS
  - android.permission.WRITE_SMS
  - android.permission.BROADCAST_WAP_PUSH
  - android.permission.BIND_CARRIER_SERVICES
  - android.permission.BIND_CARRIER_MESSAGING_SERVICE
  - android.permission.INTERACT_ACROSS_USERS
  Defines Permissions:
  - None
```


• `Normal`: This permission gives apps access to isolated application-level features, with minimal risk to other apps, the user or the system. It is granted during the installation of the App. If no protection level is specified, normal is the default value. Example: `android.permission.INTERNET`

• `Dangerous`: This permission usually gives the app control over user data or control over the device that impacts the user. This type of permission may not be granted at installation time, leaving it to the user to decide whether the app should have the permission or not. Example: `android.permission.RECORD_AUDIO`

• `Signature`: This permission is granted only if the requesting app was signed with the same certificate as the app that declared the permission. If the signature matches, the permission is automatically granted. Example: `android.permission.ACCESS_MOCK_LOCATION`

• `SystemOrSignature`: Permission only granted to applications embedded in the system image or that were signed using the same certificate as the application that declared the permission. Example: `android.permission.ACCESS_DOWNLOAD_MANAGER`
![image](https://user-images.githubusercontent.com/48615614/203703832-305f14b2-203c-4bbd-80fe-f9aa7363792f.png)


### 2. Insecure Data Storage
#### 2.1 Hacking Android Apps Using Backup Technique

I will discuss how to examine the internal memory of Android apps on non-rooted devices using the backup feature. It is possible to get the backup of a specific application package or the whole device in order to examine it. This allows us to perform an analysis on the data that is being stored on the local device at runtime.

Prerequisites:
1. USB debugging should be enabled on the device.
2. `adb` – it comes along with Android SDK

Download adbextractor, which contains a set of tools required to do our job `http://sourceforge.net/projects/adbextractor/`.

![image](https://user-images.githubusercontent.com/48615614/203705552-25964501-9044-4b07-b3ee-4b23cff6084f.png)


I will use `adb.jar` only to perform the attack scenario.
First I will log in to the app, these information should be saved to the database
![image](https://user-images.githubusercontent.com/48615614/203705701-93a60c2c-9eb3-47c7-9ff0-1e47384f0412.png)


Now if the devices is not rooted, then I will not be able to access the database directory so I will use adb tool to take a back of the application.

Using `adb backup -f <filename><package name>`

I will create a new directory and move the `abe.jar` tool to the new dir, then I will take the backup from the app in the same dir with the `abe.jar`.

![image](https://user-images.githubusercontent.com/48615614/203705858-c593e31d-079d-4567-9665-58fba044e1f4.png)


The above command needs user interaction as it waits for the user to click the `backup my data` button on the device.

![image](https://user-images.githubusercontent.com/48615614/203707143-cf4d6089-dfa4-4dbe-af6b-a60b0cbb7603.png)

Now I have the backup file from the target app:

![image](https://user-images.githubusercontent.com/48615614/203707209-3101137b-f294-4256-969c-0bf84ee3adc1.png)

I cannot directly read the contents of this file. So, I need to convert it into a format which I can understand.
This is where I'm going to use the `abe.jar` tool, which is already there in my working directory.
Using the following command `java -jar unpack backuptest1.ab backuptest1.tar`

![image](https://user-images.githubusercontent.com/48615614/203712956-24b7add1-23b2-4766-b99f-084fc3d5a461.png)

Now I have got a new file backuptest1.tar:

![image](https://user-images.githubusercontent.com/48615614/203713044-2cd88ca1-b9e3-4e78-93b2-16f5598b3bf9.png)


After extracting this file I got a file named "apps"

![image](https://user-images.githubusercontent.com/48615614/203713107-240cf0b7-e9e3-46b2-a6b7-d2998fe8a137.png)

![image](https://user-images.githubusercontent.com/48615614/203713305-30b834ac-90ae-4fce-a076-33d34729173c.png)


Now I found a db folder that contain the application database:

![image](https://user-images.githubusercontent.com/48615614/203713395-f3ae6f31-2c61-4020-95db-50cf4cc3b567.png)

Now by using a SQLlite3 client I was able to access the database file and read it shown below:

![image](https://user-images.githubusercontent.com/48615614/203713480-657b06c3-24eb-46e6-959e-3616cc0ad5b9.png)








































