# Build Instructions for SpearSdkExample Desktop Application

SpearSdkExample desktop application needs to be built using the JavaFX widget toolkit and e(fx)clipse IDE.
This instructions were validated on Linux Mint 20.1.

## Install Java:

The SpearSdkExample application requires Java 8. Most Linux distributions come pre-installed with OpenJRE 8 which is sufficient to run the application. To build, you also need to install OpenJDK 8 with the corresponding OpenJFX package. (See Install JavaFx below).

### Install OpenJDK 8:

1. Download jdk-8u291-linux-x64.tar.gz from https://www.oracle.com/java/technologies/javase/javase8u211-later-archive-downloads.htm
2. Run the following commands:
```bash
$ sudo tar -xvzf jdk-8u291-linux-x64.tar.gz
$ sudo mkdir /opt/jdk
$ sudo mv jdk1.8.0_291 /opt/jdk/jdk1.8.0_291
$ sudo update-alternatives --install "/usr/bin/java" "java" "/opt/jdk/jdk1.8.0_291/bin/java" 1
$ sudo update-alternatives --install "/usr/bin/javac" "javac" "/opt/jdk/jdk1.8.0_291/bin/javac" 1
$ sudo update-alternatives --install "/usr/bin/javaws" "javaws" "/opt/jdk/jdk1.8.0_291/bin/javaws" 1
```

### Set Default Java:

1. Run the following command to make Oracle Java as default.
```bash
$ sudo update-alternatives --config java

There are 2 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                         Priority   Status
------------------------------------------------------------
* 0            /usr/lib/jvm/java-11-openjdk-amd64/bin/java   1111      auto mode
  1            /opt/jdk/jdk1.8.0_291/bin/java                1         manual mode
  2            /usr/lib/jvm/java-11-openjdk-amd64/bin/java   1111      manual mode

Press <enter> to keep the current choice[*], or type selection number: 1
```
2. Verify that it worked by checking the version.  Expected results:
```bash
$ java -version
java version "1.8.0_291"
Java(TM) SE Runtime Environment (build 1.8.0_291-b10)
Java HotSpot(TM) 64-Bit Server VM (build 25.291-b10, mixed mode)
```

### Set JAVA_HOME Environment variable:

1. Run the following command and copy the path from your Java installation. 
```bash
$ sudo update-alternatives --config java
```
2. Open `/etc/environment` using nano or your favorite text editor.
```bash
$ sudo nano /etc/environment
```
3. At the end of this file, add the following line, making sure to replace the highlighted path with your own copied path.
```bash
JAVA_HOME="/opt/jdk/jdk1.8.0_291/bin/java"
```
4. Save and exit the file, and reload it.
```bash
$ source /etc/environment
```
5. You can now test whether the environment variable has been set by executing the following command:
```bash
$ echo $JAVA_HOME
```

## Install e(fx)clipse:

To build, you'll need to install e(fx)clipse, a variant of Eclipse with plugins specialized for working with JavaFX applications.

1. Download the "all-in-one" solution from http://efxclipse.bestsolution.at/install.html#all-in-one
2. "Install" e(fx)clipse by simply unzipping the downloaded file.
```bash
$ tar -xvzf eclipse-SDK-4.6.0-linux-gtk-x86_64-distro-2.4.0.tar.gz
```
3. Run the `eclipse` executable to open eclipse
4. Click File -> Open Projects form File System...
5. Enter location of SpearSdkExample project. 

### Change Execution Environment:

To change Execution Environment to use Oracle JRE in Eclipse if it was already setup with OpenJDK

1. Click Window -> Preferences -> Java -> Installed JREs 
2. Click Add
3. Add Oracle JRE path (“/opt/jdk/jdk1.8.0_291”)
4. Right click the SpearSdkExample project -> Properties -> Java Build Path
5. Click Libraries -> Select JRE System Library 
6. Click Edit and select Execution Environment to be JavaSE-1.8 (jdk 1.8.0_291)

## Build in Eclipse:

1. Open `build.fxbuild`
2. Click `generate ant build.xml and run`
	- It will create a `build.xml` file and deployable package under the build directory
3. The deployable package and installer will be created under `build/deploy/bundles`

## Install Package:

Double click on the resulting .deb file or you can install via command line.

```bash
$ sudo dpkg -i <package>.deb
```

## Run Installed Package:

1. Run the installed application from the command line with this command:

```bash
$ /opt/SpearSdkExample/SpearSdkExample
```

OR 

2. Find it in your application browser and click on it.

