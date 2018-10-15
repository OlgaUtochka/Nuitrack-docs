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