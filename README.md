# Python3.11 service template 

This is a fork from [https://github.com/info-beamer/package-python3](https://github.com/info-beamer/package-python3) for [info-beamer](https://info-beamer.com) which is created by Florian Wesch. 
The changes are just to use Python 3.11 instead of Python 3.7. 
Tested on a Raspberry Pi with Raspbian (Debian Bookworm).

[![Import](https://cdn.infobeamer.com/s/img/import.png)](https://info-beamer.com/use?url=https://github.com/info-beamer/package-python3)

This package bundles a Python3 runtime into a
[overlay.squashfs file](https://info-beamer.com/doc/package-services#customoverlay).
info-beamer OS will detect this file and mount it as an overlay into the
package service's filesystem. This makes Python3.7 available for use in
your `service`.

The first time python3 is invoked it will precompile some python modules
and save the result in the package's
[scratch directory](https://info-beamer.com/doc/package-services#scratchdirectory).

## Using in your own package

1. Copy `overlay.squashfs` into your package. 
1. Invoke `python3` as your package service's interpreter.

## Building this package

The included build-overlay and Makefile can be used to create the included
`overlay.squashfs`. Right now this packages uses Raspbian OS (bookworm) packages
instead of building its own binaries. Place the following files from the
Raspbian deb archive into the directory `debs`, for example by using
`apt-get download`:

```
libpython3.11-minimal_3.11.2-6+deb12u3_armhf.deb
libpython3.11-stdlib_3.11.2-6+deb12u3_armhf.deb
libpython3-stdlib_3.11.2-1+b1_armhf.deb
python3.11_3.11.2-6+deb12u3_armhf.deb
python3.11-minimal_3.11.2-6+deb12u3_armhf.deb
python3_3.11.2-1+b1_armhf.deb
python3-certifi_2022.9.24-1_all.deb
python3-chardet_5.1.0+dfsg-2_all.deb
python3-idna_3.3-1+deb12u1_all.deb
python3-pkg-resources_66.1.1-1_all.deb
python3-pyinotify_0.9.6-2_all.deb
python3-requests_2.28.1+dfsg-1_all.deb
python3-six_1.16.0-4_all.deb
python3-tz_2022.7.1-4_all.deb
python3-urllib3_1.26.12-1_all.deb
```

The packages are listed in `debs/download.txt`. You can download them all at once with `cat download.txt | xargs apt download`.

Then invoke `make clean` (remove the existing overlay.squashfs so make will create the overlay folder with data for the copyright) and `make` and you'll end up with a new version of `overlay.squashfs`.
