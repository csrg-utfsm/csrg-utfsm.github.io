---
layout: default
---

# [](#header-1) Alma Common Software Community Branch RPM Repos for CentOS 7 (EL 7)
For now, only x86_64 is available, but, hopefully once the SRPMS are made available
ports can be created for aarch64, without cross compiling nightmares =D.
Versions 2016.10+ have a working ExtProd RPM and repo with an ACS RPM in alfa.
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
  - [](http://fedora.gtdinternet.com/releases/{Fedora Version}/Everything/source/SRPMS/)
  - [](https://dl.fedoraproject.org/pub/fedora/linux/releases/23/Server/source/SRPMS/)
* x86_64: 
  - [](http://ftp.inf.utfsm.cl/fedora/linux/releases/{Fedora Version}/Everything/x86_64/os/Packages/)
  - [](http://ftp.inf.utfsm.cl/fedora/linux/updates/{Fedora Version}/x86_64/)
* aarch64:
  - [](http://mirror.chpc.utah.edu/pub/fedora-secondary/updates/{Fedora Version}/aarch64/)
  - [](http://mirror.chpc.utah.edu/pub/fedora-secondary/releases/{Fedora Version}/Everything/aarch64/os/Packages/)

Also rpmfind.net and OpenSuse pages are useful.

# [](#header-4) How was this ACS built

We have a forked [https://github.com/csrg-utfsm/acscb](ACS Community Branch git) which is regularly been merged against [http://git-dev.sco.alma.cl/cgit/](ALMA ACS's git).
Also, there are 2 RPMs:
 - ExtProd: Installs ACE+TAO 6.3.0_{ACS_Version} (Modified w/ACS Patches), OpenJDK 1.8.0, Apache Maven 3.2.5, boost 1.4.8, ... 
 - ACS Core: TBD

The main idea behind the RPMs is the use of system-available packages, which will in return allow an easier instalation/management of ACS

ExtProd is in a Beta state and ACS Core in an alfa state. The SPEC files can be found at [](https://github.com/LeoXDXp/acs-rpm)

ExtProd is built as a regular  RPM with the [https://lists.fedoraproject.org/pipermail/users/2012-November/426600.html](/usr/lib/rpm/check-rpaths) commented out in .rpmmacros file.
Also AutoReq is set to "no"

The only packages not being from a RPM origin inside ExtProd are JacORB, Tctlk and Mico. Although, the manual Tctlk compilation build many packages already system available, but haven't
been replaced and tested. Mico is [http://acs-community.github.io/](expected to fail) on RHEL 6, so this behaviour is expected also on EL/RHEL 7+.

### [](#Python_Packages_ExtProd ) Python Packages in ExtProd
All of the python packages in acs.req and acs.req.0 have been included as requirements of the rpm or installed through pip. Every package is kept as closed as possible to the one indicated
in acs.req/acs.req.0 files, with the consideration that if a newer version is available, and the changelog indicates no mayor changes, then the newer version will be prefered.
Also, if the version in the acs.req files is deprecated, the oldest system available version is prefered.
As CentOS 7 is compatible with mainly every package between fedora21 and fedora24, and further for packages not involving gcc, the srpms are fetch and recompiled, allowing an interesting
number of versions per package to be selected. Also RPMs from OpenSUSE and other rpm based SO is considered.

### [](#ACE-TAO-2008aPatch) ACE-TAO Extra Patch
The following replacement/patch has been aplied to ACE-TAO 6.3.0_{ACS_Version} 
> sed -i 's/IOP::TaggedComponentList create_ior_components/IOP::TaggedComponentSeq create_ior_components/g' /usr/include/orbsvcs/SecurityReplaceable.idl
For information about this change, please see [http://www.dre.vanderbilt.edu/~schmidt/DOC_ROOT/TAO/ChangeLogs/ChangeLog-2008a](2008a Changelog of TAO): Removed TaggedComponentList and use TaggedComponentSeq, the first one is not part of the CORBA spec, the second is.



## [](#header-5) FAQ, Other Repositories and Useful Links

CSRG oVirt Repo: []http://repo.csrg.cl/
