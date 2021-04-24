---
title: "System update uninstall for Honor 9 Lite"
date: 2021-04-24T20:00:00-04:00
categories:
  - tools
tags:
  - smartphones
---

Manual how to fully remove System Updates application from Honor phones. Also useful for anoying bloatware withdrawal which cannot be done otherwise, for any android phone. 

The changes can be brought back by a factory reset.

### Manual

1. Install `adb` (android debug bridge) on your desktop. It is a part of Android Studio located
in `android_sdk/platform-tools/` or can be downloaded as a standalone package [here](https://developer.android.com/studio/releases/platform-tools).

2. Connect your phone to the desktop. The mode should be set to file transfer (MTP).

3. Enable USB debugging on the phone. Go to 
`About phone -> Software information -> Build number`
and tap it seven times. Doing this you have developer options enabled.
Go to `About phone -> Developer options -> Enable USB debugging`. When the device is
connected to the PC, choose "Allow" in the popup window "Allow USB debugging".

4. In the `adb` directory (or with adb in the system path) launch command prompt.

5. Ensure that the phone communicates with the PC by typing

```shell
adb devices
```

The device should be part of the list in a form of code and "device" attribute. If not try to
reconnect or check if the steps above are done correctly.

6. Run the commands

```shell
adb shell
pm uninstall -k –user 0 com.huawei.android.hwouc
```

instead of `com.huawei.android.hwouc` you may type any other package name (bloatware) and it will be removed alike.

7. You should be granted with a success message. Now is the system update fully uninstalled.
Safely unplug the phone. Exit the environment by executing two exits:

```shell
exit
exit
```

