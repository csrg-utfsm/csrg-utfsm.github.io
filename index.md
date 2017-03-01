---
layout: default
---

# [](#header-1) Alma Common Software Community Branch RPM Repos for CentOS 7 (EL 7)
For now, only x86_64 is available, but, hopefully once the SRPMS are made available
ports can be created for aarch64, without cross compiling nightmares =D.
Versions 2016.10+ have a beta-working ExtProd RPM and repo with an ACS RPM in alfa.
Version is number-guided to keep clear the update order for the rpm package.

## [](#header-2) Use Instructions

* Update OS
> yum -y update

* Epel repo, centos-release-scl and apache-maven-fedora repo must be installed before:
> curl https://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -o /etc/yum.repos.d/epel-apache-maven.repo
> yum -y install epel-release centos-release-scl

* Install ACS CB and ExtProd Repo
> curl http://repo.csrg.cl/acs-cb.repo -o /etc/yum.repos.d/acs-cb.repo
* Install gcc (Not yet working as RPMs dependency)
> yum install -y gcc
* Install ACS CB ExtProds
> yum install -y ACS-ExtProds 

Currently gcc is not getting installed even when declared as a requirement
To load env vars you can do "source ~/.bashrc" or logout and login

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

Also rpmfind.net and OpenSuse pages are useful.

# [](#header-4) How was this ACS built

We have a forked [ACS Community Branch git](https://github.com/csrg-utfsm/acscb) which is regularly been merged against [ALMA ACS's git repository](http://git-dev.sco.alma.cl/cgit/)
Also, there are 2 RPMs:
 - ExtProd: Installs ACE+TAO 6.3.0_{ACS_Version} (Modified w/ACS Patches), OpenJDK 1.8.0, Apache Maven 3.2.5, boost 1.4.8, ... 
 - ACS Core: TBD

The main idea behind the RPMs is the use of system-available packages, which will in return allow an easier instalation/management of ACS

ExtProd is in a Beta state and ACS Core in an alfa state. The SPEC files can be found at [this github](https://github.com/LeoXDXp/acs-rpm)

ExtProd is built as a regular  RPM with the [check-rpaths](https://lists.fedoraproject.org/pipermail/users/2012-November/426600.html) commented out in .rpmmacros file.
Also AutoReq is set to "no"

The only packages not being from a RPM origin inside ExtProd are JacORB, Tctlk and Mico. Although, the manual Tctlk compilation build many packages already system available, but haven't
been replaced and tested. Mico is [expected to fail](http://acs-community.github.io/) on RHEL 6, so this behaviour is expected also on EL/RHEL 7+, although it appears to build correctly,
and includes the latest [gcc6 patch](https://github.com/csrg-utfsm/acscb/commit/9e930df9a0451e2b84344350520b6148a4460fd8).

As many ACS building/compiling needed-scripts are been left as Sources of the ExtProd RPM, also to allow modularity in their updates.

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


## [](#header-5) FAQ, Other Repositories and Useful Links

 - [CSRG oVirt Repo] (http://repo.csrg.cl/)
