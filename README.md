## \[PATCH-02.04.2024\]--LINUX-VaudTax\_2023

:heavy_exclamation_mark: Fix missing LibWebKit2Gtk-4.0 error on RHEL/Fedora systems

---

The "vaudtax-2023" script is intended only for systems running Ubuntu (or Debian in a pinch).  
Unfortunately, there are many other distributions, and it's certain that many people won't be able to take advantage of this software.

To run VaudTax, you need the WebKitGTK library, so you can display web content.  
On Debian-based distributions, the package is actually LibWebKit2Gtk, but on RHEL systems, the package has a different name.  
The directories containing the binary files are also different, but the script only checks /usr/lib/x86\_64-linux-gnu.  
Even if the package has been installed on another distribution, the script will **still display the error that WebKitGTK is missing**.

\----------------------------------------------------------------------------------------------------------------------  
 

[https://packages.fedoraproject.org/pkgs/webkitgtk/](https://packages.fedoraproject.org/pkgs/webkitgtk/) \<= It is the equivalent of libwebkit2gtk on Fedora/RHEL OSes.  
[https://src.fedoraproject.org/rpms/webkit2gtk3/blob/f35/f/webkit2gtk3.spec](https://src.fedoraproject.org/rpms/webkit2gtk3/blob/f35/f/webkit2gtk3.spec)  
 

\----------------------------------------------------------------------------------------------------------------------

#### **.:: DEBUGGING FROM MISSING LIBWEBKIT2GTK-4.0 ERROR ON FEDORA :**

##### SYSTEM INFO :

> NAME="Fedora Linux"
> 
> VERSION="39 (Workstation Edition)"
> 
> ID=fedora
> 
> VERSION\_ID=39
> 
> VERSION\_CODENAME=""
> 
> PLATFORM\_ID="platform:f39"
> 
> PRETTY\_NAME="Fedora Linux 39 (Workstation Edition)"
> 
> ANSI\_COLOR="0;38;2;60;110;180"
> 
> LOGO=fedora-logo-icon
> 
> CPE\_NAME="cpe:/o:fedoraproject:fedora:39"
> 
> DEFAULT\_HOSTNAME="fedora"
> 
> HOME\_URL="[https://fedoraproject.org/](https://fedoraproject.org/)"
> 
> DOCUMENTATION\_URL="[https://docs.fedoraproject.org/en-US/fedora/f39/system-administrators-guide/](https://docs.fedoraproject.org/en-US/fedora/f39/system-administrators-guide/)"
> 
> SUPPORT\_URL="[https://ask.fedoraproject.org/](https://ask.fedoraproject.org/)"
> 
> BUG\_REPORT\_URL="[https://bugzilla.redhat.com/](https://bugzilla.redhat.com/)"
> 
> REDHAT\_BUGZILLA\_PRODUCT="Fedora"
> 
> REDHAT\_BUGZILLA\_PRODUCT\_VERSION=39
> 
> REDHAT\_SUPPORT\_PRODUCT="Fedora"
> 
> REDHAT\_SUPPORT\_PRODUCT\_VERSION=39
> 
> SUPPORT\_END=2024-11-12
> 
> VARIANT="Workstation Edition"
> 
> VARIANT\_ID=workstation

##### VaudTax version :

```plaintext
<appName>VaudTax 2023</appName>
<version>1.2</version>
```

---

##### Webkit2gtk search :

```bash
sudo dnf search webkit2gtk
```

```plaintext
====================================== Name & Summary matches : webkit2gtk =======================================
webkit2gtk4.0-devel.i686 : Development files for webkit2gtk4.0
webkit2gtk4.0-devel.x86_64 : Development files for webkit2gtk4.0
webkit2gtk4.0-doc.noarch : Documentation files for webkit2gtk4.0
webkit2gtk4.1-devel.i686 : Development files for webkit2gtk4.1
webkit2gtk4.1-devel.x86_64 : Development files for webkit2gtk4.1
webkit2gtk4.1-doc.noarch : Documentation files for webkit2gtk4.1
=========================================== Name matches: webkit2gtk ===========================================
webkit2gtk4.0.x86_64 : WebKitGTK for GTK 3 and libsoup 2
webkit2gtk4.0.i686 : WebKitGTK for GTK 3 and libsoup 2
webkit2gtk4.1.x86_64 : WebKitGTK for GTK 3 and libsoup 3
webkit2gtk4.1.i686 : WebKitGTK for GTK 3 and libsoup 3
========================================= Summary matches: webkit2gtk ==========================================
javascriptcoregtk4.0.x86_64 : JavaScript engine from webkit2gtk4.0
javascriptcoregtk4.0.i686 : JavaScript engine from webkit2gtk4.0
javascriptcoregtk4.0-devel.i686 : Development files for JavaScript engine from webkit2gtk4.0
javascriptcoregtk4.0-devel.x86_64 : Development files for JavaScript engine from webkit2gtk4.0
javascriptcoregtk4.1.x86_64 : JavaScript engine from webkit2gtk4.1
javascriptcoregtk4.1.i686 : JavaScript engine from webkit2gtk4.1
javascriptcoregtk4.1-devel.i686 : Development files for JavaScript engine from webkit2gtk4.1
javascriptcoregtk4.1-devel.x86_64 : Development files for JavaScript engine from webkit2gtk4.1
rubygem-webkit2-gtk.noarch : Ruby binding of WebKit2GTK+
webkitgtk6.0-doc.noarch : Documentation files for webkit2gtk5.0
```

##### Installation :

```bash
sudo dnf install webkit2gtk4.0 -y
```

##### Installation and version check :

```bash
sudo dnf list webkit2gtk*
```

```plaintext
Metadata expiration last checked 1:22:26 ago on Tue Apr 02, 2024 19:12:33.
Installed packages
webkit2gtk4.0.x86_64                                         2.44.0-2.fc39                                   @updates
webkit2gtk4.1.x86_64                                         2.44.0-2.fc39                                   @updates
Paquets disponibles
webkit2gtk4.0.i686                                           2.44.0-2.fc39                                   updates
webkit2gtk4.0-devel.i686                                     2.44.0-2.fc39                                   updates
webkit2gtk4.0-devel.x86_64                                   2.44.0-2.fc39                                   updates
webkit2gtk4.0-doc.noarch                                     2.44.0-2.fc39                                   updates
webkit2gtk4.1.i686                                           2.44.0-2.fc39                                   updates
webkit2gtk4.1-devel.i686                                     2.44.0-2.fc39                                   updates
webkit2gtk4.1-devel.x86_64                                   2.44.0-2.fc39                                   updates
webkit2gtk4.1-doc.noarch                                     2.44.0-2.fc39                                   updates
```

---

##### In the script, the paths (for LibWebKit2Gtk-4.0) are exclusively for Ubuntu :

```bash
case "$(uname -i)" in
i686|x86)
libWebKitFile="/usr/lib/i386-linux-gnu/libwebkit2gtk-4.0.so.37" #    <= UBUNTU
MY_LIBPATH="$(pwd)/lib/ubuntu/usr/lib/i386-linux-gnu"
;;
*)
libWebKitFile="/usr/lib/x86_64-linux-gnu/libwebkit2gtk-4.0.so.37" #  <= UBUNTU ALSO
MY_LIBPATH="$(pwd)/lib/ubuntu/usr/lib/x86_64-linux-gnu"
;;
esac
if [ ! -f "${libWebKitFile}" ] ; then
echo "LibWebKit2Gtk-4.0 Does not exist. Add it." #                    <= Yes, since the installation
```

##### Terminal errors :

```plaintext
┌=[SunL0w@7321ACS300]:[~/Dev/VaudTax/VaudTax_2023]
└=>$ ./vaudtax-2023
LibWebKit2Gtk-4.0 Does not exist. Add it.
The LibWebKit2Gtk library path is :/home/sunlow/Documents/01-FINANCES/04-IMPOTS/01-VaudTax/VaudTax_2023/lib/ubuntu/usr/lib/x86_64-linux-gnu
libwebkit2gtk-4.0.so.37  libwebkit2gtk-4.0.so.37.53.4  webkit2gtk-4.0
If it is still not working, try: sudo apt-get install libwebkit2gtk-4.0-37
```

Installing webkit2gtk4.0-devel doesn't help either, because in all cases, it's the path to the binaries that's wrong in the script.

On Fedora, libwebkit2gtk-4.0.so.37 is in **/usr/lib64/**.

The complete path is therefore :

```plaintext
/usr/lib64/libwebkit2gtk-4.0.so.37
```

---


#### **.:: ERROR CORRECTION :**

I just changed the path of the "libWebKitFile" variable from :

```plaintext
/usr/lib/x86_64-linux-gnu/libwebkit2gtk-4.0.so.37
```

to :

```plaintext
/usr/lib64/libwebkit2gtk-4.0.so.37
```

##### **Here is the modified script for testing :**

```bash
#!/bin/bash
# This script starts the application on Linux (like .exe in windows)

# FIXED (FOR FEDORA) ON 02.04.2024

# Unset OS Java-Paths
unset JAVA_HOME
unset JDK_HOME

export PATH="$(dirname "$0")/jre/bin:$PATH"

JAVA_NOT_FOUND_MSG="Aucun interpréteur Java n'a pu être trouvé sur la ligne de commande.
Merci d'installer Java à l'aide du gestionnaire de paquet de votre distribution,
ou sur http://www.java.com/fr/download"

classpath='lib/dvbern-lib-update.jar:lib/cryptutil.jar:lib/jaxb-api.jar:lib/activation.jar:lib/jaxb-impl.jar'

if [ ! -f ///$classpath ] ; then
cd "$(dirname "$0")"
else
cd //
fi

type java &> /dev/null
if [ "$?" -ne "0" ] ; then
        echo $JAVA_NOT_FOUND_MSG
        which zenity &> /dev/null && zenity --no-wrap --no-markup --warning --window-icon=error --text="$JAVA_NOT_FOUND_MSG"
        exit 1
fi

case "$(uname -i)" in
i686|x86)
libWebKitFile="/usr/lib32/libwebkit2gtk-4.0.so.37" # PATH CORRECTION
MY_LIBPATH="$(pwd)/lib/ubuntu/usr/lib/i386-linux-gnu"
;;
*)
libWebKitFile="/usr/lib64/libwebkit2gtk-4.0.so.37" # PATH CORRECTION
MY_LIBPATH="$(pwd)/lib/ubuntu/usr/lib/x86_64-linux-gnu"
;;
esac


if [ ! -f "${libWebKitFile}" ] ; then
echo "LibWebKit2Gtk-4.0 Does not exist. Add it."

        export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:${MY_LIBPATH}"
        echo "The LibWebKit2Gtk library path is $LD_LIBRARY_PATH"
        ls $MY_LIBPATH

        echo "If it is still not working, try: sudo apt-get install libwebkit2gtk-4.0-37 <[ON UBUNTU/DEBIAN]" # MODIFIED
echo "Or, try: sudo dnf install webkit2gtk4.0 <[ON FEDORA/RHEL]" # ADDED
fi

java -cp $classpath ch.dvbern.lib.update.Launcher
```

In the [patch](https://github.com/SunL0w/PATCH-02.04.2024--LINUX-VaudTax_2023) ([vaudtax-2023--PATCHED](https://github.com/SunL0w/PATCH-02.04.2024--LINUX-VaudTax_2023/blob/main/vaudtax-2023--PATCHED)) I made, the script checks which operating system is being used, and sets the libWebKitFile variable according to the directory to be used.

___

## Patch made by :
<a href="https://github.com/SunL0w/" alt="SunLow GitHub Link">
<img src="https://img.shields.io/badge/SunL0w-Dind%20Thibault-blue"  alt="Developper SunL0w"/></a>
