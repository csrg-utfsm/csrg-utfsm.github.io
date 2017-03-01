---
layout: default
---

# [](#header-1) Alma Common Software Community Branch RPM Repos for CentOS 7 (EL 7)
For now, only x86_64 is available, but, hopefully once the SRPMS are made available
ports can be created for aarch64, without cross compiling nightmares =D


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
  - []http://fedora.gtdinternet.com/releases/{Fedora Version}/Everything/source/SRPMS/
  - []https://dl.fedoraproject.org/pub/fedora/linux/releases/23/Server/source/SRPMS/
* x86_64: 
  - []http://ftp.inf.utfsm.cl/fedora/linux/releases/{Fedora Version}/Everything/x86_64/os/Packages/
  - []http://ftp.inf.utfsm.cl/fedora/linux/updates/{Fedora Version}/x86_64/
* aarch64:
  - []http://mirror.chpc.utah.edu/pub/fedora-secondary/updates/{Fedora Version}/aarch64/
  - []http://mirror.chpc.utah.edu/pub/fedora-secondary/releases/{Fedora Version}/Everything/aarch64/os/Packages/

Also rpmfind.net and OpenSuse pages are useful.

# [](#header-4) How was this ACS built

We have a forked [https://github.com/csrg-utfsm/acscb](ACS Community Branch git) which is regularly been merged against [http://git-dev.sco.alma.cl/cgit/](ALMA ACS's git).
Also, there are 2 RPMs:
 - ExtProd: Installs ACE+TAO 6.3.0_{ACS_Version} (Modified w/ACS Patches), OpenJDK 1.8.0, Apache Maven 3.2.5, boost 1.4.8, ... 
 - ACS Core: TBD


The main idea behind the RPMs is the use of system-available packages, which will in return allow an easier instalation/management of ACS
ExtProd is in a Beta state and ACS Core in an alfa state. The SPEC files can be found at [](https://github.com/LeoXDXp/acs-rpm)
ExtProd is built as a regular  RPM with the [https://lists.fedoraproject.org/pipermail/users/2012-November/426600.html](/usr/lib/rpm/check-rpaths) commented out in .rpmmacros file

## [](#header-5) FAQ, Other Repositories and Useful Links

CSRG oVirt Repo: []http://repo.csrg.cl/
