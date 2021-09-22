# Build Instructions for SpearSdkExample Desktop Application

SpearSdkExample desktop application needs to be built using the JavaFX widget toolkit, e(fx)clipse IDE.

## Install Java:

The SpearSdkExample application requires Java 8.

### On Linux Mint/Ubuntu:

Most Linux distributions come pre-installed with OpenJRE 8 which is sufficient to run the application. To build, you also need to install OpenJDK 8 with the corresponding OpenJFX package. (See Install JavaFx below).

#### Install OpenJDK 8:

```bash
$ apt install openjdk-8-jdk
```

#### Install Oracle JDK 8:

On Ubuntu 16.04 LTS, You need to install Oracle Java SE 8 to build the application. The deployable package (.deb) fails to launch JVM with OpenJDK.

See https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-get-on-ubuntu-16-04 to install Oracle JDK 8 and change its default Java version.

```bash
$ sudo add-apt-repository ppa:webupd8team/java
$ sudo apt-get update
$ sudo apt-get install oracle-java8-installer
```

### Set Default Java:

#### On Linux Mint/Ubuntu:

Run the following command to make Oracle Java as default.

```bash
$ sudo update-alternatives --config java

There are 2 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                            Priority   Status
------------------------------------------------------------
  0            /usr/lib/jvm/java-8-oracle/jre/bin/java          1081      auto mode
  1            /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java   1081      manual mode
* 2            /usr/lib/jvm/java-8-oracle/jre/bin/java          1081      manual mode

Press <enter> to keep the current choice[*], or type selection number:
```

Verify that it worked by checking the version.  Expected results:

```bash
$ java -version
java version "1.8.0_201"
Java(TM) SE Runtime Environment (build 1.8.0_201-b09)
Java HotSpot(TM) 64-Bit Server VM (build 25.201-b09, mixed mode)

```

### Set JAVA_HOME Environment variable:

#### On Linux Mint/Ubuntu:

Run the following command and copy the path from your Java installation. 

```bash
$ sudo update-alternatives --config java
```

Open `/etc/environment` using nano or your favorite text editor.

```bash
$ sudo nano /etc/environment
```
At the end of this file, add the following line, making sure to replace the highlighted path with your own copied path.

```
JAVA_HOME="/usr/lib/jvm/java-8-oracle"
```

Save and exit the file, and reload it.

```bash
$ source /etc/environment
```

You can now test whether the environment variable has been set by executing the following command:

```bash
echo $JAVA_HOME
```

## Install JavaFX:

### On Linux Mint/Ubuntu:

Run the following command if you're using OpenJDK:

```bash
apt install openjfx
```

If you're using Oracle JAVA SE 8, you don't need to install JavaFX separately as it already contains JavaFX package.


## e(fx)clipse:

To build, you'll need to install e(fx)clipse, a variant of Eclipse with plugins specialized for working with JavaFX applications.

1. Download the "all-in-one" solution from http://efxclipse.bestsolution.at/install.html#all-in-one
2. "Install" e(fx)clipse by simply unzipping the downloaded file.
3. Run the `eclipse` executable to open eclipse

### Change Execution Environment:

#### On Linux Mint/Ubuntu:

To change Execution Environment to use Oracle JRE in Eclipse if it was already setup with OpenJDK

1. Eclipse -> Preferences -> Java -> Installed JRE 
2. Click Add
3. Add Oracle JRE path (“/usr/lib/jvm/java-8-oracle”)
4. Right click the project -> Properties -> Java Build Path -> Libraries -> Select JRE System Library -> Click Edit and update Execution Environment to be Oracle JRE


## Add SpearSdk Dependency:

### On Linux:

1. Add new SpearSdk-<version>.jar to SpearSdkExample/lib folder
2. Right-click on the SpearSdkExample project in Eclipse
3. Choose Properties
4. Select Java Build Path from the left pan
5. Select the Libraries tab from the right pan
6. Click Add JARs...
7. Select SpearSdkExample -> lib -> SpearSdk-<version>.jar from the JAR Selection widow.
8. Select OK
9. Select the old SpearSdk jar file in the Libraries tab
10. Click the Remove button
11. Click Apply
12. Click OK


## Build in Eclipse:

### On Linux:

1. Open `build.fxbuild`
2. Click `generate ant build.xml and run`
	- It will create a `build.xml` file and deployable package under the build directory
3. The deployable package and installer will be created under `build/deploy/bundles`


## Install Package:

### On Linux:

Double click on the resulting .deb file or you can install via command line.

On Linux Mint/Ubuntu:

```bash
$ sudo dpkg -i <package>.deb
```


## Run Installed Package:

### On Linux:

To run the installed application from the command line, use this command:

```bash
/opt/SpearSdkExample/SpearSdkExample
```

OR you can find it in your application browser and click on it.

