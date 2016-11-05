##Roborio Control System

###Installation Instructions
**Chill Out 1778**

####Download the embedded JDK from Oracle:

Download the following version:

<table>
  <tr>
    <td>ARMv7 Linux - VFP, SoftFP ABI, Little Endian2</td>
    <td>116 </td>
  </tr>
</table>


[http://www.oracle.com/technetwork/java/embedded/embedded-se/downloads/javaseembedded8u6-2406243.html](http://www.oracle.com/technetwork/java/embedded/embedded-se/downloads/javaseembedded8u6-2406243.html)

Unzip the archive in a local temp directory on the PC.

In that same temp directory, switch to the ejdk/bin folder, and create a JRE distribution using the jrecreate.bat batch file:
```
jrecreate --vm client --dest C:\temp\JRE
```
Using an FTP utility like FileZilla, connect to the Roborio using anonymous user, no password.

Copy the entire C:\temp\JRE directory to /usr/local/frc on the Roborio. Make sure the JRE directory and children binaries are executable.  They should be by default, but if not, use an SSH tool like PuTTY to log in and change them via chmod commands.

####Reference Documentation

**Documentation for the 2015 FRC Control System:**

**[http://wpilib.screenstepslive.com/s/448**5](http://wpilib.screenstepslive.com/s/4485)

**2016 FRC Software Version Reference:**

**[https://wpilib.screenstepslive.com/s/4485/m/13503/l/305566-current-software-revision**s](https://wpilib.screenstepslive.com/s/4485/m/13503/l/305566-current-software-revisions)

**FRC Java Programming:**

**[http://wpilib.screenstepslive.com/s/4485/m/1380**9](http://wpilib.screenstepslive.com/s/4485/m/13809)

**Two links for the NI FRC software:**

**[http://www.ni.com/download/first-robotics-software-2015/5112/en**/](http://www.ni.com/download/first-robotics-software-2015/5112/en/)

**[https://decibel.ni.com/content/docs/DOC-3473**1](https://decibel.ni.com/content/docs/DOC-34731)

**Setting up Java SE Embedded JRE with jrecreate:**

**[http://docs.oracle.com/javase/8/embedded/develop-apps-platforms/jrecreate.ht**m](http://docs.oracle.com/javase/8/embedded/develop-apps-platforms/jrecreate.htm)

**Roborio FTP:**

**[http://wpilib.screenstepslive.com/s/4485/m/24166/l/282299-roborio-ft**p](http://wpilib.screenstepslive.com/s/4485/m/24166/l/282299-roborio-ftp)

**Roborio SSH and user accounts:**

**[https://wpilib.screenstepslive.com/s/4485/m/24166/l/286926-roborio-user-accounts-and-ss**h](https://wpilib.screenstepslive.com/s/4485/m/24166/l/286926-roborio-user-accounts-and-ssh)

###Steps to Configure Radio (D-Link DAP1522) - 2015 and before

1. **Configure the Wifi Radio.**

    1. **[http://wpilib.screenstepslive.com/s/4485/m/13503/l/242608-roborio-networkin**g](http://wpilib.screenstepslive.com/s/4485/m/13503/l/242608-roborio-networking)

    2. Look for the radio from PC under wifi sources after power up.  Connect to it from the PC.

    3.  In a web browser on the same PC, connect to the wifi radio (typically **192.168.0.50**).  For the wifi admin webpage, user=Admin, no password.

    4. Configure the Wifi Radio IP address to be **10.17.78.1**.

    5. Enable DHCP on the Wifi Radio, with distribution of addresses 10.17.78.20 through 10.17.78.199.

    6. Save settings.  Test access to the wifi radio by redirecting the browser to 10.17.78.1.

###Steps to Configure Radio (OpenMesh OM5P-AN) - 2016

1. **Instructions on configuration:**

    1. **[https://wpilib.screenstepslive.com/s/4485/m/13503/l/144986-programming-your-radio-for-home-us**e](https://wpilib.screenstepslive.com/s/4485/m/13503/l/144986-programming-your-radio-for-home-use)

2. **Download the radio configuration utility:**

    2. **[https://usfirst.collab.net/sf/frs/do/listReleases/projects.wpilib/frs.frc_radio_configuration_utilit**y](https://usfirst.collab.net/sf/frs/do/listReleases/projects.wpilib/frs.frc_radio_configuration_utility)

3. **Connect the radio to a PC via ethernet cable**

4. **Run the radio configuration utility**

    3. Set either Bridge mode (for competition) or Wireless mode (for practice)

5. **SSH and log into the OpenMesh radio using a tool like PuTTY**
```
    IP: 10.17.78.1  Login: root   Password: root
```
6. **To enable stdout for debugging after reconfiguring, reset the subnet mask on the radio:**
111
    5. uci set dhcp.lan.dhcp_option="1**,**255.255.255.0 28**,**10.17.78.255”
uci set dhcp.wan.dhcp_option=”1**,**255.255.255.0 28**,**10.17.78.255”
uci commit dhcp
/etc/init.d/dnsmasq restart
111
###Steps to Configure Roborio

1. **Connect Control System Hardware.**

    7. Connect power cabling to the Roborio and Wifi Radio.

    8. Connect Ethernet cable between Roborio and Wifi Radio.

    9. Configure Wifi radio switch for wireless (not bridge) operation.

    10. Power Roborio and Wifi Radio.

    11. Perform a factory reset on the Wifi Radio (hold recessed button in for greater than 10 sec).

2. **Updating Roborio firmware and Imaging the Roborio.**

    12. Download and Install the National Instruments FRC 2015 Update on the PC to be used in connecting to the Roborio (dev PC, Driver Station, etc) - **[http://wpilib.screenstepslive.com/s/4485/m/13503/l/144150?data-resolve-url=true&data-manual-id=1350**3](http://wpilib.screenstepslive.com/s/4485/m/13503/l/144150?data-resolve-url=true&data-manual-id=13503).

    13. Connect only the USB cable between Roborio and PC.  Make sure the ethernet cable is DISCONNECTED.  When connected via USB, the PC driver should install automatically, and the Roborio should be accessible via web browser at IP address **172.22.11.2.**

    14. Update Roborio firmware first following these instructions:

**[http://wpilib.screenstepslive.com/s/4485/m/13503/l/273817-updating-your-roborio-firmwar**e](http://wpilib.screenstepslive.com/s/4485/m/13503/l/273817-updating-your-roborio-firmware)

    15. Update the Roborio Image using the Roborio Imaging Tool instructions: **[http://wpilib.screenstepslive.com/s/4485/m/13503/l/144984-imaging-your-robori**o](http://wpilib.screenstepslive.com/s/4485/m/13503/l/144984-imaging-your-roborio)** ** If successful, the tool will indicate the version of Roborio image loaded, and if none, can reimage the Roborio.  You can also update the team number to 1778 using this tool.   

3. **Confirm Roborio communicating via PC web browser.**

    16. While still connected to the wifi radio with the PC, use a web browser to connect to **http://Roborio-1778.local/** .  If successful, the Roborio webpage will appear with status on the Roborio.

4. **Load Java Runtime Environment (JRE) on Roborio. **

    17. This step is required before loading any new programs on the Roborio - DO NOT SKIP THIS STEP.  While there is an upload utility, it is far more efficient to upload JRE to the Roborio using FTP.  Follow only one of the options below.

    18. First, from the Oracle website, download the embedded JDK (ejdk) version: ([http://www.oracle.com/technetwork/java/embedded/downloads/java-embedded-java-se-download-359230.html](http://www.oracle.com/technetwork/java/embedded/downloads/java-embedded-java-se-download-359230.html) )

        1.  **"****ARMv6/7 Linux - VFP, SoftFP ABI, Little Endian****2****"**

    19. **Option 1** **- Using the WPILib Java-Installer Utility (slow, less reliable method):**

        2. **[http://wpilib.screenstepslive.com/s/4485/m/13503/l/288822-installing-java-8-on-the-roborio-using-the-frc-roborio-java-installer-java-onl**y](http://wpilib.screenstepslive.com/s/4485/m/13503/l/288822-installing-java-8-on-the-roborio-using-the-frc-roborio-java-installer-java-only)

        3. **[http://wpilib.screenstepslive.com/s/4485/m/24166/l/282299-roborio-ft**p](http://wpilib.screenstepslive.com/s/4485/m/24166/l/282299-roborio-ftp)

    20. **Option 2 - FTP the JRE to the Roborio (faster, more direct method):**

        4. On your local PC, Use **WinRAR **to unzip the above EJDK tar.gz archive to a temporary directory (you will be deleting it later).  Note that this will likely be *two* uncompress steps - the first to uncompress the *.gz file, then the second to uncompress the *.tar file.  You may need to rename the file suffix to .tar between the first and second decompressions.  You know you are successful when you have a jdk folder hierarchy.

        5. In a command shell window on the PC, switch to **<EJDK folder>/bin**.  Run the following command in a shell window to create the JRE:  

            1. **jrecreate.bat --profile compact2 --dest JRE**

            2. This creates a directory structure named JRE in the current bin directory containing all necessary JRE files for the Roborio

            3. jrecreate reference links:

                1. [https://docs.oracle.com/javase/8/embedded/develop-apps-platforms/jrecreate.htm](https://docs.oracle.com/javase/8/embedded/develop-apps-platforms/jrecreate.htm) 

                2. [https://blogs.oracle.com/jtc/entry/introducing_the_ejdk](https://blogs.oracle.com/jtc/entry/introducing_the_ejdk) 

        6. Download and install an FTP client (like FileZilla) on your PC.

        7. Use FTP client to connect to the Roborio:

            4. host: Roborio-1778.local

            5. user: anonymous

            6. password: <none>

        8. Transfer the entire JRE directory above from the PC to **/usr/local/frc** location on the Roborio.

        9. Lastly, SSH into roborio-1778-frc.local on port 22 (using PuTTY or some similar SSH tool), username admin, no password.  Once at the Roborio command prompt, update the JRE files to executables by typing the following:  **chmod +x /usr/local/frc/JRE/bin/***

        10. At this point you should be able to load and run java code on the Roborio!

**Steps to Configure Development PC**

1. **[http://wpilib.screenstepslive.com/s/4485/m/13503/l/145002-installing-eclipse-c-jav**a](http://wpilib.screenstepslive.com/s/4485/m/13503/l/145002-installing-eclipse-c-java)

2. **Install Java Development Kit (Java Standard Edition 8  - Java 8 SE)**

    1. JDK1.8.0_40 or greater

    2. Ensure that Java paths are updated on the PC.  Insert **C:\Program Files\Java\JDK1.8_40\bin** into the PC path environment variable.

    3. Test paths are correct by typing ‘**java**’ or ‘**javac**’ from a command line prompt. If working correctly, a set of command parameters will be returned.

3. **Install Eclipse Integrated Development Environment (32-bit version, even if the OS is 64-bit).**

4. **Download and Install Eclipse FRC Plugin**

5. **Configure Eclipse FRC Plugin**

    4. Set Team number to 1778.  This is important - it directs Eclipse to the correct Roborio address.

6. **Download a test program to the Roborio.**

**Connecting to the Robot via mDNS**

### In some cases, you may be able to ping the Roborio’s IP address, but not the name (i.e. Roborio-1778.local).  This usually indicates a problem with mDNS.

### mDNS - Firewalls

To work properly mDNS must be allowed to pass through your firewall. Depending on your PC configuration, no changes may be required, this section is provided to assist with troubleshooting. Because the network traffic comes from the mDNS implementation and not directly from the Driver Station or IDE, allowing those applications through may not be sufficient. There are two main ways to resolve mDNS firewall issues:

* Add an application/service exception for the mDNS implementation (NI mDNS Responder is C:\Program Files\National Instruments\Shared\mDNS Responder\nimdnsResponder.exe)

* Add a port exception for traffic to/from UDP 5353 (IP ranges 10.0.0.0-10.255.255.255   172.16.0.0-172.31.255.255   192.168.0.0-192.168.255.255   169.254.0.0-169.254.255.255   224.0.0.251)

**_It also may be a problem with the PC’s IPv4 and IPv6 adapters conflicting with each other.  The Robot only uses IPv4, so make sure the PC’s IPv6 adapter is disabled._**

**Steps to Configure/Operate Driver Station**

1. **Start up DS application. ** NI FRC software must have already been installed, see section above.

2. **Configure Team Number in DS application.**

3. **Test Game Controllers (if any).**

