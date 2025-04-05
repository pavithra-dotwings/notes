Step 1 : add awesome_notifications: ^0.10.1  package to pubspec yaml file

Step 2: Go to android/app/src/main/AndroidManifest.xml and apply

🔹 **Add this inside the `<manifest>` tag**:
```xml
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
<uses-permission android:name="android.permission.VIBRATE" />

```

🔹 **Add this inside `<application>` tag**:
```xml
<receiver android:name="me.carda.awesome_notifications.notifications.receivers.ActionReceiver" />

<receiver android:name="me.carda.awesome_notifications.notifications.receivers.DismissedReceiver" /><receiver

android:name="me.carda.awesome_notifications.notifications.receivers.ScheduledNotificationReceiver" />

<receiver

android:name="me.carda.awesome_notifications.notifications.receivers.KeepAliveReceiver"

android:exported="true">

<intent-filter>

<action android:name="android.intent.action.BOOT_COMPLETED"/>

<action android:name="android.intent.action.MY_PACKAGE_REPLACED"/>

</intent-filter>

</receiver>

     

```

**Step 2: Update `main.dart` Initialization**

`main.dart` initialization to ensure \*\*background execution is enabled

```dart
void main() async {
WidgetsFlutterBinding.ensureInitialized();

await AwesomeNotifications().initialize(
  null,
  [
    NotificationChannel(
      channelKey: 'basic_channel',
      channelName: 'Basic Notifications',
      channelDescription: 'Channel for scheduled notifications',
      defaultColor: Colors.blue,
      ledColor: Colors.white,
      importance: NotificationImportance.High,
      enableLights: true,
      enableVibration: true,
    ),
  ],
  debug: true,
);

runApp(MyApp());
}

```

Create UI or set notifications function where you need.
**Ensure Permissions Are Granted:**

```dart
@override
 .    void initState() {
     super.initState();
     requestNotificationPermission();
     }

	void requestNotificationPermission() async {
			  bool isAllowed = await       AwesomeNotifications().isNotificationAllowed();
	  if (!isAllowed) {
    AwesomeNotifications().requestPermissionToSendNotifications();
      }
     }

```

**Basic Notification example:**

```dart
void showSimpleNotification() {
     AwesomeNotifications().createNotification(
	   content: NotificationContent(
		id: 1,
		channelKey: 'basic_channel',
		title: '🎉 Hello!',
		body: 'This is a simple Awesome Notification!',
		),
	);
}
```

**scheduled notification function**:

```dart
void scheduleNotification() async {
  bool isAllowed = await AwesomeNotifications().isNotificationAllowed();
  if (!isAllowed) {
    AwesomeNotifications().requestPermissionToSendNotifications();
  }

  await AwesomeNotifications().createNotification(
    content: NotificationContent(
      id: 2,
      channelKey: 'basic_channel',
      title: '⏰ Scheduled Notification',
      body: 'This notification was scheduled!',
      notificationLayout: NotificationLayout.Default,
    ),
    schedule: NotificationCalendar(
      year: DateTime.now().year,
      month: DateTime.now().month,
      day: DateTime.now().day,
      hour: DateTime.now().hour,
      minute: DateTime.now().minute + 1, // Schedule 1 minute later
      second: 0,
      timeZone: await AwesomeNotifications().getLocalTimeZoneIdentifier(),
      preciseAlarm: true,
    ),
  );
}

```

**Example code for notification:**

```dart
import 'package:flutter/material.dart';

import 'package:awesome_notifications/awesome_notifications.dart';



void main() async {

WidgetsFlutterBinding.ensureInitialized();



await AwesomeNotifications().initialize(null, [

NotificationChannel(

channelKey: 'basic_channel',

channelName: 'Basic Notifications',

channelDescription: 'Channel for simple notifications',

defaultColor: Colors.blue,

ledColor: Colors.white,

),

], debug: true);



runApp(MyApp());

}



class MyApp extends StatelessWidget {

@override

Widget build(BuildContext context) {

return MaterialApp(

title: 'Awesome Notification Demo',

home: NotificationHomePage(),

);

}

}



class NotificationHomePage extends StatefulWidget {

@override

State<NotificationHomePage> createState() => _NotificationHomePageState();

}



class _NotificationHomePageState extends State<NotificationHomePage> {

@override

void initState() {

super.initState();

requestNotificationPermission();

}



void requestNotificationPermission() async {

bool isAllowed = await AwesomeNotifications().isNotificationAllowed();

if (!isAllowed) {

AwesomeNotifications().requestPermissionToSendNotifications();

}

}



void showSimpleNotification() {

AwesomeNotifications().createNotification(

content: NotificationContent(

id: 1,

channelKey: 'basic_channel',

title: '🎉 Hello!',

body: 'This is a simple Awesome Notification!',

),

);

}



void scheduleNotification() async {

AwesomeNotifications().createNotification(

content: NotificationContent(

id: 2,

channelKey: 'basic_channel',

title: '⏰ Scheduled Notification',

body: 'This notification was scheduled!',

notificationLayout: NotificationLayout.Default,

),

schedule: NotificationCalendar(

year: DateTime.now().year,

month: DateTime.now().month,

day: DateTime.now().day,

hour: DateTime.now().hour,

minute: DateTime.now().minute + 1,

second: 0,

timeZone: await AwesomeNotifications().getLocalTimeZoneIdentifier(),

preciseAlarm: true,

),

);

}

@override

Widget build(BuildContext context) {

return Scaffold(

appBar: AppBar(title: Text('Awesome Notification')),

body: Column(

children: [

Row(

children: [

Center(

child: ElevatedButton(

onPressed: showSimpleNotification,

child: Text('Show Notification'),

),

),

],

),

SizedBox(height: 20),

ElevatedButton(

onPressed: scheduleNotification,

child: Text('Schedule Notification (1 min later)'),

),

],

),

);

}

}
```

