---
layout: default
---

# [](#header-1) Alma Common Software Community Branch RPM Repos for CentOS 7 (EL 7)
For now, only x86_64 is available, but, hopefully once the SRPMS are made available
ports can be created for aarch64, without cross compiling nightmares =D.
Versions 2016.10+ have a beta-working ExtProd RPM and repo with an ACS RPM in alfa.
Version is number-guided to keep clear the update order for the rpm package.



## Current Status: 15/5/2017

* ACS-2017.04 (Apr) has no mayor deferences with 2017.02 (Feb)
* ACS-ExtProds uses castor package without acs-patches. Patch is for castor 0.9.6, and most of it is already included, with different names in the code, however, I can't build xmljbind due to the missing method "setDefaultURIResolver(AlmaURIResolver)" added in the patch -> Patch Needs Updating
* ACS-ExtJars is OK
* ACS-eclipse-plugins is built with Autoreq = no, otherwise, it has never ending java dependencies.
* ACS-ToolsKit no longer build benchmarks, which needs to be build after all the acs-core packages.
* ACS-jACSutil Compilation is OK, Unittests Passed 12/15
* ACS-xmljbind: Patch Needs Updating
* ACS-xmlpybind Compilation is OK, Unittests, 1 Passed, 2 Failed, 3 broken


* Tat Tests:
 > grepFile doesn't exist
 > /diska/home/almadev/introot/idl/*.xml: No such file or directory*
 > Some tests don't compile




## [](#header-2) Use Instructions

* Update OS
> yum -y update

* Apache-maven-fedora repo must be installed before:
> curl https://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -o /etc/yum.repos.d/epel-apache-maven.repo
* Epel and centos-release-scl Repos
> yum -y install epel-release centos-release-scl

* Install ACS CB and ExtProd Repo
> curl http://repo.csrg.cl/acs-cb.repo -o /etc/yum.repos.d/acs-cb.repo
* Install ACS CB ExtProds and ACS CB Extra Jars
> yum install -y ACS-ExtProds ACS-ExtJars

After the instalation of ACS-ExtProds, and before de ACS instalation, a session logout-login is needed
in order to load the environment variables (now in profile.d files)

## [](#header-3) Places to look for packages:
In order to save time, the lookup of existing RPMs in other flavors of RPM using S.O. is highly recommended.

* SRPMS: 
  - [http://fedora.gtdinternet.com/releases/{Fedora Version}/Everything/source/SRPMS/]
  - [https://dl.fedoraproject.org/pub/fedora/linux/releases/23/Server/source/SRPMS/]

* x86_64: 
  - [http://ftp.inf.utfsm.cl/fedora/linux/releases/{Fedora Version}/Everything/x86_64/os/Packages/]
  - [http://ftp.inf.utfsm.cl/fedora/linux/updates/{Fedora Version}/x86_64/]

* aarch64:
  - [http://mirror.chpc.utah.edu/pub/fedora-secondary/updates/{Fedora Version}/aarch64/]
  - [http://mirror.chpc.utah.edu/pub/fedora-secondary/releases/{Fedora Version}/Everything/aarch64/os/Packages/]

Also [http://www.rpmfind.net](RPMFind.net) and OpenSuse pages are useful.

# [](#header-4) How was this ACS built

We have a forked [ACS Community Branch git](https://github.com/csrg-utfsm/acscb) which is regularly been merged against [ALMA ACS's git repository](http://git-dev.sco.alma.cl/cgit/)
Also, there are 3 RPMs:
 - ExtProd: Installs ACE+TAO 6.3.0_{ACS_Version} (Modified w/ACS Patches), OpenJDK 1.8.0, Apache Maven 3.2.5, boost 1.4.8, ... 
 - ExtJars: Includes jar libraries not found on current RPMs: activation.jar, (apache-)common-math.jar, (apache-)common-xml-resolver, ehcache-core, jackarta-regexp (deprecated by apache), jchart2d, junit-dep, prevayler, sqltool
 - ACS Core: TBD

The main idea behind the RPMs is the use of system-available packages, which will in return allow an easier instalation/management of ACS

ExtProd is in a Beta state and ACS Core in an alfa state for 2016.10. 2017.02 hasn't been released. The SPEC files can be found at [this github](https://github.com/LeoXDXp/acs-rpm)

Initially ExtProd was built as a regular  RPM with the [check-rpaths](https://lists.fedoraproject.org/pipermail/users/2012-November/426600.html) commented out in .rpmmacros file. It happened because
Eclipse has [https://fedoraproject.org/wiki/RPath_Packaging_Draft](libs in non-system paths). This appears no longer needed.
Also AutoReq is set to "no"

The only packages not being from a RPM origin inside ExtProd are JacORB, Tctlk and Mico. Although, the manual Tctlk compilation build many packages already system available, but haven't
been replaced and tested. Mico is [expected to fail](http://acs-community.github.io/) on RHEL 6, so this behaviour is expected also on EL/RHEL 7+, although it appears to build correctly,
and includes the latest [gcc6 patch](https://github.com/csrg-utfsm/acscb/commit/9e930df9a0451e2b84344350520b6148a4460fd8).

As many ACS building/compiling needed-scripts are been left as Sources of the ExtProd RPM, also to allow modularity in their updates. This scripts are left in system default path's
such as /usr/local/bin, in order to prevent the need for path extension and modifications on the user side.

Another important point is the intend to deprecate the use of .bash_profile.acs file, leaving enviroment variables in /etc/profile.d files. For example,
ExtProd RPM creates jacorb.sh, which contains and exports:

> JACORB_HOME=/home/almamgr/ACS-%{version}/JacORB
> export JACORB_HOME

Source0 in Extprod RPM consist in the following:
> ACS-ExtProd-{ACS_Version}
>  - INSTALL
>    - buildEclipse
>  - PRODUCTS
>    - eclipse-SDK-4.2.2-linux-gtk-x86_64.tar.gz
>    - eclipse-4.2.2-delta-pack.zip

The traditional files from the old instalation of ACS are kept as legacy but not used. All of this can be installed via the Extprod-devel rpm is available from 2016.10+.
ExtProds RPM creates user almamgr with uid 550 and the /alma symlink, which point to /home/almamgr. ExtProds-devel creates user almadevel. This way, ExtProds and devel can be installed/uninstalled
without colliding over the same user.

Of the Tools Module built by ACS the yet not RPM encapsulated are (getopt ignored as is indicated for SunOS only):
> tat xsddoc extidl vtd-xml oAW scxml_apache



### [](#Packages_ExtProd ) Package selection in ExtProd
All of the python packages in acs.req and acs.req.0 have been included as requirements of the rpm or installed through pip. Every package is kept as closed as possible to the one indicated
in acs.req/acs.req.0 files, with the consideration that if a newer version is available, and the changelog indicates no mayor changes, then the newer version will be prefered.
Also, if the version in the acs.req files is deprecated, the oldest system available version is prefered.
Python 2.7 is not being compiled as it comes with CentOS 7.

As CentOS 7 is compatible with mainly every package between fedora21 and fedora24, and further for packages not involving gcc, the srpms are fetch and recompiled, allowing an interesting
number of versions per package to be selected. Also RPMs from OpenSUSE and other rpm based SO is considered.
The spirit of the RPM is initally to replace with system available packages as much as possible and the fix whatever may have been broken.

### [](#ACE-TAO-2008aPatch) ACE-TAO Extra Patch
The following replacement/patch has been aplied to ACE-TAO 6.3.0_{ACS_Version} 
> sed -i 's/IOP::TaggedComponentList create_ior_components/IOP::TaggedComponentSeq create_ior_components/g' /usr/include/orbsvcs/SecurityReplaceable.idl

For information about this change, please see [2008a Changelog of TAO](http://www.dre.vanderbilt.edu/~schmidt/DOC_ROOT/TAO/ChangeLogs/ChangeLog-2008a): Removed TaggedComponentList and use TaggedComponentSeq, thefirst one is not part of the CORBA spec, the second is.

## [](https://hub.docker.com/r/leoxdxp/acs/) Docker Containers

In order to create re-usable building and testing environments, there are a couple of [available docker containers](https://hub.docker.com/r/leoxdxp/acs/)

 - 2016.10-beta: Container with ACS 2016.10 hand compiled. Created to compare/analize expected behaviour.
 - 2017.02: Containers with ExtProds + ExtJars. Can be used to hand compile ACS 2017.02


## [](#FAQ-others) FAQ, Other Repositories and Useful Links

 - [Source RPMs](http://repo.csrg.cl/)
 - [CSRG oVirt Repo](http://repo.csrg.cl/)
 - [Personal Development Notes](https://csrg-utfsm.github.io/acs-makefile.md)

## [](#Todo) To-Do List

 - Create JacORB 3.6.1 RPM. JacORB 2.3.1 is available in fedora.
 - Create Mico RPM
 - Replace tctlk 8.5 manual compilation with system available packages. Almost all the packages manually built exist. Some version checking is needed.
 - Instead of using /home/<user> leave the code in /usr/share/{alma,almadevel,etc}
 - Create symlinks in a cleaner way in ExtProds Spec file
 - Sign the RPMs: http://giovannitorres.me/how-to-setup-an-rpm-signing-key.html
 - Complete the Systemd daemons of acs commands
 - Pack al java libs properly. [http://www.mojohaus.org/rpm-maven-plugin/usage.html](Maven RPM creation plugin) can be used

## [](#Deprecated) Deprecated Packages used in ACS

 - [http://jakarta.apache.org/regexp/](The Jakarta Project). Recomendation is to replace with regexp in java.util.regexp
  
