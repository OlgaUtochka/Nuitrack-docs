# TVico User Guide

**TVico** is an interactive Android computer with a 3D sensor and RGB camera. You can use **TVico** as a standalone device or as a device for development with a PC (using Wi-Fi connection). Android, Windows and Linux platforms are supported. Please see the installation instructions for both cases below.

## Standalone case

1. Download [Nuitrack.apk](http://download.3divi.com/Nuitrack/platforms/Nuitrack.apk).
2. Install *Nuitrack.apk* on TVico following the [installation instructions for Android](http://download.3divi.com/Nuitrack/doc/Installation_page.html#install_android_sec).
3. Launch the Nuitrack application.
4. Click **Compatibility test** and wait until the test is complete.
5. Enter your secret key and press **Upgrade to Pro**.
6. Your device will get license from the activation server and after that Nuitrack will be fully-functional on this device (without 3 minute time limit).
7. Press **Test/Run** to test Nuitrack middleware.

## Wireless case 

###Installation of TVico.apk on TVico 

1. Allow your device to install applications from unknown sources. To do this, go to **Settings → Security** and tick **Unknown sources**.
2. Download [TVico.apk](http://download.3divi.com/Nuitrack/platforms/TVico.apk)(beta) and install it. To install the APK package, locate it in the file manager, open and tap **INSTALL**.
3. Launch the Nuitrack application.

\mbox{\includegraphics{TVico_1.png}}

4. Wait for Nuitrack installation. If the Nuitrack installation is successful, the message will be displayed as shown in the picture below:

\mbox{\includegraphics{TVico_2.png}}

### Setting Up TVico

1. Click **Compatibility test** and wait until the test is complete.

\mbox{\includegraphics{TVico_3.png}}

2. Enter your Activation Key and click **Get available licenses**.

\mbox{\includegraphics{TVico_4.png}}

3. Choose the **TVico** license type and click **Activate**. After that, the Nuitrack app will be restarted.

\mbox{\includegraphics{TVico_5.png}}

4. Turn on a Wi-Fi access point (go to **Android Settings → Wireless & Networks → More → Tethering & Portable Hotspot** and tick the **Portable Wi-Fi Hotspot**)

\mbox{\includegraphics{TVico_6.png}}

5. To turn on the server, run the Nuitrack app. When the server is running, the notification is displayed. To turn off the server, just click on the notification.

\mbox{\includegraphics{TVico_7.png}}

### Setting Up Your Device

#### Android 

1. Download and install [VicoVR.apk](https://play.google.com/store/apps/details?id=com.vicovr.manager) on your device.
2. Connect to the TVico Wi-Fi access point (its default name is **AndroidAP**).

\mbox{\includegraphics{TVico_8.png}}

3. Run the VicoVR app.
4. Go to **Settings → Developer Options**, change mode to **Wi-Fi** and edit the IP Address to **192.168.43.1** (the default address for access points on TVico).

\mbox{\includegraphics{TVico_9.png}}

5. In Settings, click on the **Test Sensor** button. You will see the window with the depth map, user mask and skeleton.

\mbox{\includegraphics{TVico_10.png}}

#### Windows / Linux {#TVico_page_TVico_Desktop}

##### Note
*Note In this case, **only skeleton data** can be transferred.*

1. Connect your PC to the Wi-Fi access point (its default name is **AndroidAP**).

2. **Unity**

Import **NuitrackSDK.unitypackage** to your project and drag-and-drop the **Nuitrack Scripts** prefab to the Scene. In the **Nuitrack Manager** section, select **Wifi Connect From PC --$>$ TVico**. After that, **Build and run** the project.

\mbox{\includegraphics{TVico_11.png}}

**С\# sample with nuitrack.net.dll**

Use the following code to initialize Nuitrack:
```
Nuitrack.Init("", Nuitrack.NuitrackMode.DEBUG);    
Nuitrack.SetConfigValue("Settings.IPAddress", "192.168.43.1"); // TVico IP-address
```
