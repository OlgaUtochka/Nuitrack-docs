Installation Instructions
============================

Windows
-------

Supported Windows versions: Windows 7, Windows 8, Windows 10.

To install Nuitrack on Windows, follow the steps below: 

1. Download `nuitrack-win32.zip <http://download.3divi.com/Nuitrack/platforms/nuitrack-win32.zip>`_ (for Windows 32-bit) or `nuitrack-win64.zip <http://download.3divi.com/Nuitrack/platforms/nuitrack-win64.zip>`_ (for Windows 64-bit) and extract the archive in any folder that you like, which should be named as *<install-folder>*. 
2. [Required] Make sure that you have installed Microsoft Visual C++ Redistributable for Visual Studio on your computer. If not, install this package depending on your VS version and architecture:
    * `Visual C++ Redistributable 2015 (x86) <https://download.microsoft.com/download/9/3/F/93FCF1E7-E6A4-478B-96E7-D4B285925B00/vc_redist.x86.exe>`_
    * `Visual C++ Redistributable 2015 (x64) <https://download.microsoft.com/download/9/3/F/93FCF1E7-E6A4-478B-96E7-D4B285925B00/vc_redist.x64.exe>`_
    * `Visual C++ Redistributable 2017 (x86) <https://aka.ms/vs/15/release/VC_redist.x86.exe>`_
    * `Visual C++ Redistributable 2017 (x64) <https://aka.ms/vs/15/release/VC_redist.x64.exe>`_
3. [Required] Install OpenNI 1.5 (*<install-folder>*/OpenNI-Win32-1.5.7-Dev.msi, *<install-folder>*/OpenNI-Win64-1.5.7-Dev.msi for 64-bit).
4. Set up environment variables:
    * Create an environment variable with the name of **NUITRACK_HOME** and value of *<install-folder>*/nuitrack.
    * Add *<install-folder>*/nuitrack/bin to the **PATH** environment variable.

.. note:: To add a new environment variable or change the existing environment variable manually, use the "Environment Variables" dialog. To access it, open the "System" dialog (Win + Break), then select **Advanced system settings → Environment Variables...**

.. image:: http://download.3divi.com/Nuitrack/doc/install_windows.png
   :width: 600 px
   :alt: Editing environment variables in Windows 10
   :align: center

**Editing environment variables in Windows 10**

5. [Optional] Install OpenNI-1.5 Primesense/ORBBEC driver (*<install-folder>*/Primesense-Sensor-5.1.6.6-Win-x86.msi, *<install-folder>*/Primesense-Sensor-5.1.6.6-Win-x64.msi for 64-bit).
6. [Optional] To use OpenNI 1.5 API, register the Nuitrack dynamic module for OpenNI 1.5:
   
  32-bit: ::

  "C:\Program Files (x86)\OpenNI\Bin\niReg.exe" -r <install-folder>\nuitrack\bin\libnuitrack_ni.dll <install-folder>\nuitrack\data

  64-bit: ::

  "C:\Program Files\OpenNI\Bin64\niReg64.exe" -r <install-folder>\nuitrack\bin\libnuitrack_ni.dll <install-folder>\nuitrack\data

.. note:: To get started with a new device, you must first install the drivers for it. Contact the device vendor to get the drivers.

Ubuntu Linux 
------------

Supported Ubuntu version is 14.04 and above. Supported architectures are AMD64 and ARM 32-bit.

To install Nuitrack on Ubuntu, follow the steps below:

1. Download one of the following Debian packages, depending on the target architecture:
    * `nuitrack-ubuntu-amd64.deb <http://download.3divi.com/Nuitrack/platforms/nuitrack-ubuntu-amd64.deb>`_ for AMD64
    * `nuitrack-persee-ubuntu-arm32.deb <http://download.3divi.com/Nuitrack/platforms/nuitrack-persee-ubuntu-arm32.deb>`_ for ARM 32-bit

2. Install the downloaded package using the following command: ::

    sudo dpkg -i <downloaded-package-name>.deb

3. Log out to let the system changes take effect. 
4. Check that the environment variables **NUITRACK_HOME** and **LD_LIBRARY_PATH** are set correctly using the following commands: ::

    echo $NUITRACK_HOME
    echo $LD_LIBRARY_PATH

**NUITRACK_HOME** should be equal to ``/usr/etc/nuitrack``. **LD_LIBRARY_PATH** should include ``/usr/local/lib/nuitrack`` path.

If the environment variables are empty, set them manually using the following commands (as root): ::

    echo "export NUITRACK_HOME=/usr/etc/nuitrack" > /etc/profile.d/nuitrack_env.sh
    echo "export LD_LIBRARY_PATH=/usr/local/lib/nuitrack" >> /etc/profile.d/nuitrack_env.sh
    . /etc/profile.d/nuitrack_env.sh

5. [For Ubuntu 18.04] Install the `libpng12-0 <https://packages.ubuntu.com/xenial/amd64/libpng12-0/download>`_ package.

.. note:: If you see "ERROR: Couldn't open device ..." message when trying to use Nuitrack, try to set permissions for USB devices with the following command: `sudo chmod -R 777 /dev/bus/usb/`

Android
---------

1. Allow your device to install applications from unknown sources. To do this, go to **Settings → Security and tick "Unknown sources"**.
2. Download `Nuitrack.apk <http://download.3divi.com/Nuitrack/platforms/Nuitrack.apk>`_ and install it. To install the APK package, locate it in a file manager, open and tap "INSTALL".
3. Launch the Nuitrack application. 

.. image:: http://download.3divi.com/Nuitrack/doc/install_1.png
   :width: 400 px
   :alt: Editing environment variables in Windows 10
   :align: center
    
4. Wait for Nuitrack installation. If the Nuitrack installation is successful, the message will be displayed as shown in the picture below:

.. image:: http://download.3divi.com/Nuitrack/doc/install_2.png
   :width: 400 px
   :alt: Editing environment variables in Windows 10
   :align: center

License Activation
---------------------

There are two Nuitrack versions: **Nuitrack Trial** and **Nuitrack Pro**.

**Nuitrack Trial** is free and has the time limit. This Nuitrack version stops working after running for three minutes, so you need to restart it. **Nuitrack Trial** is provided by default and you can use it without entering the license key. It is intended for demo and evaluation purposes only.

**Nuitrack Pro** is for commercial applications. It allows to develop and sell applications based on Nuitrack. There are two types of **Nuitrack Pro** licenses: annual (which is valid for 1 year) and perpetual (with unlimited period of validity).

You can upgrade **Nuitrack Trial** to **Nuitrack Pro** by entering a license key. The license is purchased `here <https://nuitrack.com/#rec38627247>`_. After you purchase the license, we send you an email with your activation key and activation instructions. You need a license key for each sensor used with an application based on Nuitrack.

The Nuitrack license is associated with a sensor serial number. Besides, the hardware that you use is also checked at Nuitrack runtime. The license is non-portable, but you can reactivate the license if you use new hardware with the same sensor.

3D Sensor Known Issues 
-----------------------
All Sensors
~~~~~~~~~~~
* Make sure that the date and time settings on your device are correct.
* [For Windows 10] Make sure that you allowed apps to access your camera: select **Settings → Privacy → Camera** and turn on **"Allow apps to access your camera"**.

Kinect V1
~~~~~~~~~~
To install the driver for Kinect V1, download `Kinect SDK v1.8 <https://www.microsoft.com/en-us/download/details.aspx?id=40278>`_ and follow `Install Instructions <https://www.microsoft.com/en-us/download/confirmation.aspx?id=40278>`_.

.. note:: If you use Windows 10, we recommend to run *KinectSDK-v1.8-Setup.exe* in compatibility mode for Windows 8.

Intel® RealSense™ Depth Camera D415
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Supported OS versions: Windows 8.1, Windows 10 and Ubuntu 14.04 or higher
* Before using the sensor, you need to download and install Intel® RealSense™ SDK 2.0 `for Windows <https://goo.gl/hkhUdR>`_ or `for Linux <https://goo.gl/wmFSuG>`_.
* Supported camera firmware version: 5.8.15 or higher. To update the camera firmware, please, download the latest firmware from `the official Intel website <https://downloadcenter.intel.com/download/27514/Windows-Device-Firmware-Update-Tool-for-Intel-RealSense-D400-Product-Family?v=t>`_.
