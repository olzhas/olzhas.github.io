---
layout: post
title: Cross compilation in Qt Creator on Ubuntu 16.04 for BeagleBone Black with Debian Jessie
date: '2017-04-02T03:00:00.001+06:00'
comments: true
categories: [code]
---

The tutorial consists of 2 sections. The first will provide instructions for the PC and the second section presents instruction for BeagleBone Black.
<!--more-->

If you encounter the following block
```bash
$ echo "Hello world"
```
you should type it in the terminal (GNOME Terminal, Konsole, xterm, etc)

## BeagleBone Black side
Firstly, let us prepare BeagleBone Black flashing it with the latest Debian image. Download latest image from https://beagleboard.org/latest-images. I used Debian Jessie 8.7.

```bash
$ wget https://debian.beagleboard.org/images/bone-debian-8.7-lxqt-4gb-armhf-2017-03-19-4gb.img.xz
```

After that please insert SD card in your PC and identify which device it is your system, in my system it was ```/dev/sdb```

The next step is to write image file to your SD card. There are two options of doing that: using p7zip or xz-tools.

### 7zip
Install ```7zip``` using the following line.
```bash
$ sudo apt-get install p7zip
```
Now, you can unpack it.
```bash
$ 7z x bone-debian-8.7-lxqt-4gb-armhf-2017-03-19-4gb.img.xz
```
Write image file to your SD card using dd.
```bash
$ sudo if=bone-debian-8.7-lxqt-4gb-armhf-2017-03-19-4gb.img of=/dev/sdb bs=4M
```

### xz-tools
Install ```xz-tools``` using the following line.
```bash
$ sudo apt-get install xz-tools
```
Now you can write it to your SD card.
```bash
$ xzcat bone-debian-8.7-lxqt-4gb-armhf-2017-03-19-4gb.img.xz > /dev/sdb
```

By default the image downloaded will not flash your eMMC. To flash new operating system you have to edit ```/boot/uEnv.txt```

Insert your SD card into BeagleBone Black and boot it, BBB will load from SD card.

Now, after the OS was booted you can do it for example with nano.

```
$ ssh debian@192.168.7.2
$ sudo nano /etc/uEnv.txt
```

Remove the '#' on the line with 'cmdline=init=/opt/scripts/tools/eMMC/init-eMMC-flasher-v3.sh'. For more details please refer to [elinux.org/Beagleboard:BeagleBoneBlack_Debian](elinux.org/Beagleboard:BeagleBoneBlack_Debian).

Reboot and wait until flash light turn off. You can unplug you microSD card and boot into a fresh OS.

The final step on your BBB is to install **gdbserver** such that it would be possible to debug your application remotely with Qt Creator.

To install GDB server on BBB enter the following in your terminal
```bash
$ ssh debian@192.168.7.2
$ sudo apt-get install gdbserver
```

## Developer's computer side

Install necessary compilers and tools for ARM architecture
```bash
$ sudo apt-get install crossbuild-essential-armhf gcc-4.9-arm-linux-gnueabihf
```

Install GDB debugger to enable remote debug
```bash
$ sudo apt-get install gdb-multiarch
```

Download Qt Creator from the official website
Easiest path would be to follow the link below and select the latest version.
https://download.qt.io/official_releases/qtcreator/

at the moment of writing of the document the latest version was 4.2.1 therefore
```sh
$ cd ~/Downloads
$ wget https://download.qt.io/official_releases/qtcreator/4.2/4.2.1/qt-creator-opensource-linux-x86_64-4.2.1.run
$ chmod +x ./qt-creator-opensource-linux-x86_64-4.2.1.run
$ ./qt-creator-opensource-linux-x86_64-4.2.1.run
```

Now let us configure Qt Creator to work with BeagleBone Black

Press **Tool->Options**
![Tools]({{ site.url }}/assets/images/tutorial1/first.png)

Select on the side bar **Build and Run**
![Build and Run]({{ site.url }}/assets/images/tutorial1/second.png)

After that add a compiler

![Add a compiler]({{ site.url }}/assets/images/tutorial1//third.png)

To do so - press **Add** and select GCC, since we use this compiler.

![Add a compiler hint]({{ site.url }}/assets/images/tutorial1//add_compiler_hint.png)

Fill accordingly the form.

![Fill GCC C compiler]({{ site.url }}/assets/images/tutorial1//add_compiler_form.png)

Set *Compiler path* to
```
/usr/bin/arm-linux-gnueabihf-gcc-4.9
```
And give the name to this compiler **GCC 4.9 arm-linux-gnueabihf**.

The add C++ compiler too
![Add GCC C++ compiler]({{ site.url }}/assets/images/tutorial1//fourth.png)


![Fill GCC C++ compiler]({{ site.url }}/assets/images/tutorial1//fifth.png)


Let us add debugger as well, switch to **Debugger** tab.
![Add GDB]({{ site.url }}/assets/images/tutorial1/sixth.png)
Press **Add** and fill it accordingly

![Fill GDB]({{ site.url }}/assets/images/tutorial1/seventh.png)

Go to **Kits** tab and press **Add**

![Kits overview]({{ site.url }}/assets/images/tutorial1/eighth.png)

Fill the form accordingly
![BeagleBone Black]({{ site.url }}/assets/images/tutorial1/kit_form.png)

Now, let us add our BeagleBone Black Device to Qt Creator.

![Devices overview]({{ site.url }}/assets/images/tutorial1/eleventh.png)

![First step]({{ site.url }}/assets/images/tutorial1/first_step_device.png)

Set IP - *192.168.7.2* and username - *debian* and password - *temppwd* .
![Second step]({{ site.url }}/assets/images/tutorial1/second_step_device.png)

![Final step]({{ site.url }}/assets/images/tutorial1/final_step_device.png)

Connection success!
![Connection Success]({{ site.url }}/assets/images/tutorial1/connection_success.png)


Let's test if everything works creating a new C++ project in Qt Creator.

![non Qt C++ application project]({{ site.url }}/assets/images/tutorial1/new-project-1.png)

Name the project and select appropriate project location:
![Project location]({{ site.url }}/assets/images/tutorial1/select-project-location.png)

Select CMake as a build system.
![Select building system]({{ site.url }}/assets/images/tutorial1/select-cmake.png)


Select **BeagleBone Black** Kit
![Select **BeagleBone Black** Kit]({{ site.url }}/assets/images/tutorial1/select-kit.png)

Select GIT as source control systems
![Select GIT]({{ site.url }}/assets/images/tutorial1/select-scm.png)

Press finish and now you have a C++ project runnable on BeagleBone Black.

Now you can develop your C++ program in Qt Creator, at the moment you can compile it, but to run it and debug you should do additional steps.
![C++ project]({{ site.url }}/assets/images/tutorial1/cpp-project-overview.png)

Select **Default->cross_compilation_test (on Remote Device)**

![Select needed configuration default ]({{ site.url }}/assets/images/tutorial1/bbb-default-run.png)

This will compile and run **Release** configuration on your BBB. You can test it pressing *Play* button or pressing **CTRL-R**.

![Test run ]({{ site.url }}/assets/images/tutorial1/bbb-first-run.png)

Congratulation you have cross-compiled and executed your first program on BeagleBone Black.

![Select needed configuration debug]({{ site.url }}/assets/images/tutorial1/bbb-default-run.png)

Now to debug you should select **Debug->cross_compilation_test (on Remote Device)**

![Select debug configuration]({{ site.url }}/assets/images/tutorial1/bbb-debug-select.png)

Put a breakpoint and press Debug button or **F5**.
![Stop at breakpoint]({{ site.url }}/assets/images/tutorial1/bbb-breakpoint.png)

Done!

I would be happy to receive comments and feedback.
