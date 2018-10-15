# Installation Instructions

## Windows

Supported Windows versions: Windows 7, Windows 8, Windows 10.

To install Nuitrack on Windows, follow the steps below: 

1. Download [nuitrack-win32.zip](http://download.3divi.com/Nuitrack/platforms/nuitrack-win32.zip) (for Windows 32-bit) or [nuitrack-win64.zip](http://download.3divi.com/Nuitrack/platforms/nuitrack-win64.zip) (for Windows 64-bit) and extract the archive in any folder that you like, which should be named as *install-folder*. 
2. [Required] Make sure that you have installed Microsoft Visual C++ Redistributable for Visual Studio on your computer. If not, install this package depending on your VS version and architecture:
    * [Visual C++ Redistributable 2015 (x86)](https://download.microsoft.com/download/9/3/F/93FCF1E7-E6A4-478B-96E7-D4B285925B00/vc_redist.x86.exe)
    * [Visual C++ Redistributable 2015 (x64)](https://download.microsoft.com/download/9/3/F/93FCF1E7-E6A4-478B-96E7-D4B285925B00/vc_redist.x64.exe)
    * [Visual C++ Redistributable 2017 (x86)](https://aka.ms/vs/15/release/VC_redist.x86.exe)
    * [Visual C++ Redistributable 2017 (x64)](https://aka.ms/vs/15/release/VC_redist.x64.exe)
3. [Required] Install OpenNI 1.5 (*install-folder*\OpenNI-Win32-1.5.7-Dev.msi, *install-folder*\OpenNI-Win64-1.5.7-Dev.msi for 64-bit).
4. Set up environment variables:
    * Create an environment variable with the name of **NUITRACK_HOME** and value of *<install-folder>*\nuitrack.
    * Add *<install-folder>*\nuitrack\bin to the **PATH** environment variable.

### Note
To add a new environment variable or change the existing environment variable manually, use the "Environment Variables" dialog.
To access it, open the "System" dialog (Win + Break), then select **Advanced system settings → Environment Variables...**
![alt text](https://github.com/OlgaUtochka/Nuitrack-docs/blob/master/images/install_windows.png "Editing environment variables in Windows 10")

5. [Optional] Install OpenNI-1.5 Primesense/ORBBEC driver (*install-folder*\Primesense-Sensor-5.1.6.6-Win-x86.msi, *install-folder*\Primesense-Sensor-5.1.6.6-Win-x64.msi for 64-bit).
6. [Optional] To use OpenNI 1.5 API, register the Nuitrack dynamic module for OpenNI 1.5:
   
  32-bit:
  ```
  "C:\Program Files (x86)\OpenNI\Bin\niReg.exe" -r <install-folder>\nuitrack\bin\libnuitrack_ni.dll <install-folder>\nuitrack\data
  ```

  64-bit:
  ```
  "C:\Program Files\OpenNI\Bin64\niReg64.exe" -r <install-folder>\nuitrack\bin\libnuitrack_ni.dll <install-folder>\nuitrack\data
  ```

### Note
To get started with a new device, you must first install the drivers for it. Contact the device vendor to get the drivers.

## Ubuntu Linux 

Supported Ubuntu version is 14.04 and above. Supported architectures are AMD64 and ARM 32-bit.

To install Nuitrack on Ubuntu, follow the steps below:

1. Download one of the following Debian packages, depending on the target architecture:
    * [nuitrack-ubuntu-amd64.deb](http://download.3divi.com/Nuitrack/platforms/nuitrack-ubuntu-amd64.deb) for AMD64
    * [nuitrack-persee-ubuntu-arm32.deb](http://download.3divi.com/Nuitrack/platforms/nuitrack-persee-ubuntu-arm32.deb) for ARM 32-bit
2. Install the downloaded package using the following command: 
```
sudo dpkg -i <downloaded-package-name>.deb
```
3. Log out to let the system changes take effect. 
4. Check that the environment variables **NUITRACK_HOME** and **LD_LIBRARY_PATH** are set correctly using the following commands:
```
echo $NUITRACK_HOME
echo $LD_LIBRARY_PATH
```
NUITRACK_HOME should be equal to */usr/etc/nuitrack*. LD_LIBRARY_PATH should include */usr/local/lib/nuitrack* path.

If the environment variables are empty, set them manually using the following commands (as root):
```
echo "export NUITRACK_HOME=/usr/etc/nuitrack" > /etc/profile.d/nuitrack_env.sh
echo "export LD_LIBRARY_PATH=/usr/local/lib/nuitrack" >> /etc/profile.d/nuitrack_env.sh
. /etc/profile.d/nuitrack_env.sh
```
5. [For Ubuntu 18.04] Install the [libpng12-0](https://packages.ubuntu.com/xenial/amd64/libpng12-0/download) package.

### Note
If you see "ERROR: Couldn't open device ..." message when trying to use Nuitrack, try to set permissions for USB devices with the following command:
```
sudo chmod -R 777 /dev/bus/usb/
```
## Android

1. Allow your device to install applications from unknown sources. To do this, go to **Settings → Security and tick "Unknown sources"**.
2. Download [Nuitrack.apk](http://download.3divi.com/Nuitrack/platforms/Nuitrack.apk) and install it. To install the APK package, locate it in a file manager, open and tap "INSTALL".
3. Launch the Nuitrack application. 
![alt text](https://github.com/OlgaUtochka/Nuitrack-docs/blob/master/images/install_1.png)
4. Wait for Nuitrack installation. If the Nuitrack installation is successful, the message will be displayed as shown in the picture below:
![alt text](https://github.com/OlgaUtochka/Nuitrack-docs/blob/master/images/install_2.png)
