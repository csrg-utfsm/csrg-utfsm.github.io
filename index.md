# [](#header-1) Alma Common Software Community Branch RPM Repos for CentOS 7 (EL 7)
For now, only x86_64 is available, but, hopefully once the SRPMS are made available
ports can be created for aarch64, without cross compiling nightmares =D


# [](#header-2) Use Instructions

* Update OS
> yum -y update

* Epel repo, centos-release-scl and apache-maven-fedora repo must be installed before:
> curl https://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -o /etc/yum.repos.d/epel-apache-maven.repo
> yum -y install epel-release centos-release-scl

* Install ACS CB and ExtProd Repo
> curl http://repo.csrg.cl/acs-cb.repo -o /etc/yum.repos.d/acs-cb.repo
> yum install -y gcc
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
