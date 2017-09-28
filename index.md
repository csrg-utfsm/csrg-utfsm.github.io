---
layout: default
---

# [](#header-1) Alma Common Software Community Branch RPM Repos for CentOS 7 (EL 7)
For now, only x86_64 is available, but, hopefully once the SRPMS are made available
ports can be created for aarch64, without cross compiling nightmares =D.
Versions 2016.10+ have a beta-working ExtProd RPM and repo with an ACS RPM in alfa.
Version is number-guided to keep clear the update order for the rpm package.

## Current Status: 28/9/2017

* ACS-2017.06 (Jun) has a good number of changes with 2017.04.
* ACS-ExtProds uses castor package without acs-patches. 
 - 15/5/2017: Patch is for castor 0.9.6, and most of it is already included, with different names in the code, however, I can't build xmljbind due to the missing method "setDefaultURIResolver(AlmaURIResolver)" added in the patch -> Patch Needs Updating
 - 18/5/2017: Created Castor-ACS-0.9.6.2017.02. Only xmljbind makes direct use of it. Leavind ACS-ExtProds as it was. Patch Update is STILL Needed if castor is to be used.
 - 30/5/2017: Created log4cpp-ACS, devel package used by ACS-logging
 - Updated to 2017.06
* ACS-ExtJars is OK (Updated to 2017.06)
* ACS-eclipse-plugins is built with Autoreq = no, otherwise, it has never ending java dependencies. (Updated to 2017.06)
* ACS-ToolsKit no longer build benchmarks, which needs to be build after all the acs-core packages.
 - Added AutoReq no. (Updated to 2017.06)
* ACS-jACSutil Compilation is OK, Unittests Passed 12/15 (Updating to 2017.06)
* ACS-xmljbind: Compilation is OK, Unittests Passed 1. 1 Broken (diska)
* ACS-xmlpybind Compilation is OK, Unittests, 1 Passed, 2 Failed, 3 broken
* ACS-acserridl OK
* ACS-acsidlcommon OK
* ACS-acsutil OK, Tat Test require baciidl
* ACS-acsstartup under construction
* ACS-loggingidl is OK
* ACS-logging is OK, Tat Test require maciidl
* ACS-acserr is OK, Unittests Failed
* ACS-maciidl under construction, needs acserrType
* ACS-baciidl OK , Tat Test require maciidl
* tcltk-ACS created. Compiles tcltk with ACS patch, leaving products in /home/acs-tcltk/tcltk. In the future, products will be left in /usr/local. In the meanwhile, is where ACS-Extprod will call tcltk
* ACS-acserrTypes is OK.

* Tat Tests:
 > grepFile doesn't exist - sedFile doesn't exist: Extend temporary rpm path to Kit/acs/src (not bin, intentionally)
 > /diska/home/almadev/introot/idl/*.xml: No such file or directory*
 > Some tests don't compile

* False Failures:
 > ERROR: ----> ../lib/libbaselogging.a  does not exist. (It does)
 > ERROR: ----> ../lib/libacserrHandlersErrStubs.a  does not exist. (It does)
 > ERROR: ----> ../lib/libbaciErrTypePropertyStubs.a  does not exist. (It does. baciidl)

* Deprecation Warnings:
 >  warning: ThreadInfoCompositeData is internal proprietary API and may be removed in a future release (jacsutil)
 > Note: Some input files use or override a deprecated API.
 > Note: Recompile with -Xlint:deprecation for details
 > Note: ./alma/ACSErrTypeCppNative/wrappers/AcsJCppAnyEx.java uses or overrides a deprecated API (acserrTypes)
 > Note: ./alma/ACSErrTICS/wrappers/AcsJAnyTICSErrorEx.java uses or overrides a deprecated API. (acserrTypes)

* Usual compilation issues in packages:
 > Missing argument for option: -XSL | Update paths of xalan.jar in acserrGen scripts : commit 32f1ac415da730
 > fatal error: acserr.idl: No such file or directory | acserr not in global path : commit 73f733b
 > No rule to make target acserrExceptionManager.h | Usually needs to symlink that header with the ones in commit 3868fc0
  - The same applies to logging.h, commit 1528c5d, acsutilTimeStamp.h commit d416ce0,  loki stuff commit 03de50d, acsutil.h commit ff045c0

* Non Fatal Errors
 > /idl/baciErrTypeProperty.xml: src-resolve: Cannot resolve the name 'common:nameType' to a(n) 'type definition' component

# [](#Rules) Building Rules

The instalation destination of the compiled products of the main rpm package of each acs component is /usr/local, where:
 * /usr/local/bin has the products in the acs component /bin directory
 * /usr/local/share/java has the jar products in the acs component /lib directory
 * /usr/local/lib64/ has the shared objets (.so) products in the acs component /lib directory
 * /usr/local/lib/python/site-packages has the python products in the acs component /lib directory

Some packages  generates elements in /object folder, such as Skeleton, Stubs and/or static libraries (.a files). 
Those elements are packaged in the devel rpm and are also installed in /usr/local, where:
 * /usr/local/include has the .h, .cpp and .inl files originated in the acs component /object directory
 * /usr/local/lib64/ has the static linked (.a) products in the acs component /lib directory

Other products, such as man pages and documentation should also be in their respective /usr/local path.

The Source0 element for each acs component rpm package consists in the complete git repository of ACSCB, which is, the result
of the merges explained below.
The main contents of Source0, the LGPL folder is available with the acs-cb-ToolKit-devel rpm package.

The main idea applied in each package is to:
 - Replace the use of .bash\_profile.acs declarations with several separated files in /etc/profile.d, ensuring the loading
of them on boot time
 - Inside each SPEC file, make calls acsMakefile, encapsulating the results inside the rpm without big modifications of the way it works.
 - Each acs rpm package replaces the list of packages to be compiled with the package's name, in order to trick acsMakefile to only compile the single acs component at a time.

Once an rpm package is succesfully manually built, it undergoes a "strict" build under Jenkins CI, where env vars and other elements are deleted, 
creating a clean building environment.
Lastly, each package can be tested in centos docker containers.

Currently, the main focus is to succesfully compile and package the acs components, with tat tests compiling but being allowed to temporary fail. 
Once the first iteration is concluded, tat tests will be fixed where necesary, considering that some tat test dependencies are different than those
of the main acs component package.

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


We have a forked [ACS Community Branch git](https://github.com/csrg-utfsm/acscb) which is regularly been merged against [ALMA ACS's git repository](http://git-dev.sco.alma.cl/cgit/). This is what Source0 of each acs component package contains.

The main idea behind the RPMs is the use of system-available packages, which will in return allow an easier instalation/management of ACS

ExtProd is in a Beta state and ACS Core in an alfa state for 2016.10. 2017.02 hasn't been released. The SPEC files can be found at [this github](https://github.com/LeoXDXp/acs-rpm)

Initially ExtProd was built as a regular  RPM with the [check-rpaths](https://lists.fedoraproject.org/pipermail/users/2012-November/426600.html) commented out in .rpmmacros file. It happened because
Eclipse has [https://fedoraproject.org/wiki/RPath_Packaging_Draft](libs in non-system paths). This appears no longer needed.
Also AutoReq is set to "no"

The only packages not being from a RPM origin inside ExtProd are JacORB and Mico. Tcltk has a first version of its own package.
Mico is [expected to fail](http://acs-community.github.io/) on RHEL 6, so this behaviour is expected also on EL/RHEL 7+, although it appears to build correctly, and includes the latest [gcc6 patch](https://github.com/csrg-utfsm/acscb/commit/9e930df9a0451e2b84344350520b6148a4460fd8).

As many ACS building/compiling needed-scripts are been left as Sources of the ExtProd RPM, also to allow modularity in their updates. This scripts are left in system default path's such as /usr/local/bin, in order to prevent the need for path extension and modifications on the user side.

Another important point is the intend to deprecate the use of .bash_profile.acs file, leaving enviroment variables in /etc/profile.d files. For example, ExtProd RPM creates jacorb.sh, which contains and exports:

> JACORB_HOME=/home/almamgr/ACS-%{version}/JacORB
> export JACORB_HOME

Source0 in Extprod RPM consist in the following:
> ACS-ExtProd-{ACS_Version}
>  - INSTALL
>    - buildEclipse
>  - PRODUCTS
>    - eclipse-SDK-4.2.2-linux-gtk-x86_64.tar.gz
>    - eclipse-4.2.2-delta-pack.zip

The traditional files from the old instalation of ACS are kept as legacy but not used. All of this can be installed via the acs-cb-ToolKit-devel rpm, which is available from 2016.10+.
ExtProds RPM creates user almamgr with uid 550 and the /alma symlink, which point to /home/almamgr. 

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
 - Instead of using /home/<user> leave the code in /usr/share/{alma,almadevel,etc}
 - Create symlinks in a cleaner way in ExtProds Spec file
 - Sign the RPMs: http://giovannitorres.me/how-to-setup-an-rpm-signing-key.html
 - Complete the Systemd daemons of acs commands
 - Pack al java libs properly. [http://www.mojohaus.org/rpm-maven-plugin/usage.html](Maven RPM creation plugin) can be used

## [](#Deprecated) Deprecated Packages used in ACS

 - [http://jakarta.apache.org/regexp/](The Jakarta Project). Recomendation is to replace with regexp in java.util.regexp
 - tktable: This package is obsolete (CentOS7) https://pkgs.org/download/tktable.
