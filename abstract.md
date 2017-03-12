---
layout: default
---

# [](#header-1) Abstract

- Seen the dependencies as a tree structure, attacking the problem from the leafs, making system packaging and system control through daemons, an accessible option to initiate the transformation from big binary to real modularized components.

- Using off-the-shelf tools available in other scenarios where Big Data is handled (Stock Exchange, Social Networks, e-shopping)

- Analyzing the paradigm change of the last 15+ years, from heterogeneous architectures and OSes to Linux and its tools as an homogeneous solution.

## Dependency Tree

 - 

## Off-the-Shelf Approach

In the specific case of ACS, everything not considered ACS Source Code, was re-grouped as External Tools, and packaged in one or more RPMs.
Each RPM handled 2 specific things:
 - Dependencies of the External Tools. (Ex: Doxygen requires c++)
 - Environment Variables: Replace the manual setup, and prefer the system method (Ex:Profile.d folder in CentOS)
 
## Paradigm Shift
Other issue to be considered, is the change in paradigms since the days ACS was initially coded. 15+ years ago, the existence of heterogeneous CPU
architectures (SPARC, x86, s390), each one with it's corresponding O.S., implicated the challenge of creating code that should deploy and execute in as much of the OSes it could, which meant, handling many options.

Today, Linux is available for almost every existing architecture, with the OSes available for servers, narrowed down to Debian, CentOS or OpenSUSE.


## [](#Benefits) Benefits
 - Easier Updating/Patching
 - Easier Deployment
 - Easier resource control via Systemd
  - Memory Control (Memory Max)
  - Behaviour Control (OnRestart, FailureAction)
  - System Supervision
  - Environment Variable Handling (Environment, EnvironmentFile)
