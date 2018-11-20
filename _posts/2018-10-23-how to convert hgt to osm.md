#how to convert hgt from SRTM to osm

tools used:[phyghtmap](https://wiki.openstreetmap.org/wiki/Phyghtmap) [homePage](http://katze.tfiu.de/projects/phyghtmap/)  [manPage](https://www.mankier.com/1/phyghtmap)

###environment reference：ubuntu16.04+python3.5

## Download

At the moment, you have the choice between two different distribution types:

- Source distibution: [phyghtmap_2.20.orig.tar.gz](http://katze.tfiu.de/projects/phyghtmap/phyghtmap_2.20.orig.tar.gz)
- Debian package: [phyghtmap_2.20-1_all.deb](http://katze.tfiu.de/projects/phyghtmap/phyghtmap_2.20-1_all.deb)
- Debian .debian.tar.xz: [phyghtmap_2.20-1.debian.tar.xz](http://katze.tfiu.de/projects/phyghtmap/phyghtmap_2.20-1.debian.tar.xz)
- Debian .dsc: [phyghtmap_2.20-1.dsc](http://katze.tfiu.de/projects/phyghtmap/phyghtmap_2.20-1.dsc)

 

## Dependencies

I tested the installation process under Windows. If you want to install phyghtmap on a Windows machine, go down to the [Windows installation section](http://katze.tfiu.de/projects/phyghtmap/#WindowsInstallation).

The installation process requires python3 and a working python3 setuptools installation.

- python3-setuptools, if you want to install from the source distribution (on Debian-like systems, say `apt-get install python3-setuptools`, but I guess you want to use the Debian package instead. For other operating systems, have a look [here](http://pypi.python.org/pypi/setuptools/))

The program itself should run with python >= 3.0 (Windows users, take a look [here](http://www.python.org/download/)). Some additional dependencies have to be installed on your system in order to run phyghtmap:

- python3-matplotlib (on Debian-like systems, say `apt-get install python3-matplotlib`; Windows users, have a look [here](http://sourceforge.net/projects/matplotlib/files/))
- python3-bs4 (on Debian-like systems, say `apt-get install python3-bs4`
- python3-numpy (on Debian-like systems, say `apt-get install python3-numpy`; Windows users, have a look [here](http://sourceforge.net/projects/numpy/files/))
- python3-gdal (since version 1.70; if you don't want to use GeoTiff input files, you may be happy withput python-gdal; on Debian-like systems, say `apt-get install python3-gdal`).

 Installation

### Source Distribution

If you are not running a Debian-like system, you will want to have the source distribution. This is especially true, if you want to use phyghtmap on a Windows machine. If so, go down to the seperate [Windows installation section](http://katze.tfiu.de/projects/phyghtmap/#WindowsInstallation).

To install phyghtmap, unpack the source file, chdir to the unpacked source directory and then say

> `sudo python3 setup.py install`

which will install the stuff on your system.

If you are not a Windows user and have no `sudo` installed, I guess you know what to do.

If you don't have the permissions to globally install phyghtmap but want to use it anyway, you may want to set up a virtual python environment and install phyghtmap there. If so, take a look [here](http://svn.python.org/projects/sandbox/branches/setuptools-0.6/virtual-python.py).



usages:

i.e. convert a folder of hgt files to one osm file

```c
phyghtmap -s 5 --start-node-id=NODE-ID --start-way-id=WAY-ID --max-nodes-per-tile=0 <your hgt files>
```

*start-node-id = $((1 << 33))*

*start-way-id = $((1 << 33))*





 