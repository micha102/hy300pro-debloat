# 🔍 Removing Hotack Silent SDK Proxy Malware from Android TV/Projector

Device: Magcubic HY300 Pro Android Projector (ui_Veng.projector)
Issue: Device was being used as a residential proxy node without consent, causing thousands of suspicious DNS requests and bandwidth usage.

## TDLR
The 3 following packages are malware installed on Magcubic HY300 Pro.
```
package:com.hotack.silentsdk
package:com.hotack.writesn
com.htc.htclauncherhighenglishd08
```
The first 2 makes the projector a proxy to other people navigating the Internet through a Proxy service called SOAX.

com.htc.htclauncherhighenglishd08 is also a malware (accessing to shortx.srafx.com which is a video shorts website and imasdk.googleapis.com), but to remove it you must first install an alternative Android TV launcher and create an app to launch the HDMI source which is com.softwinner.awlivetv.MainActivity


## 🚨 Symptoms

    Thousands of blocked DNS requests to app.appsflyer.com, *.onelink.me, iforgot.apple.com in Pi-hole

    Device registering with proxy networks at boot

    Suspicious traffic patterns as if strangers are browsing through your device

    APIPA (169.254.x.x) network connections

    Connections to ports 12341/12342

## 🔎 Identification

The culprit was found to be two packages from com.hotack:
bash
```
pm list packages | grep hotack
```
Output:
```
package:com.hotack.silentsdk
package:com.hotack.writesn
```

## 🛠️ Solution
Prerequisites

    ADB access enabled on your Android device

    USB debugging enabled

    Computer with ADB installed

Steps to Remove

    Connect to your device via ADB:

```
adb connect YOUR_DEVICE_IP:5555
```
Disable both malicious packages:
```
adb shell pm disable-user --user 0 com.hotack.silentsdk
adb shell pm disable-user --user 0 com.hotack.writesn
```
Verify they are disabled:
```
adb shell pm list packages -e | grep hotack
```
    (Should return nothing if successful)

    Monitor your Pi-hole/network traffic - the suspicious DNS requests should stop immediately.

## 📝 Technical Details

    com.hotack.silentsdk : Main proxy SDK that turns your device into a residential proxy node for SOAX.com

    com.hotack.writesn : Companion service for device identification and analytics

    These packages were pre-installed on the device and automatically registered with proxy networks at boot

    The device was being used as an exit node for unknown users, consuming bandwidth and potentially exposing your network

    _ Readme generated with Deepseek _
    
