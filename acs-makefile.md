---
layout: default
---

# [](#header-1) ACS Makefile Notes

Disclaimer: This are personal notes taken during development of ACS CB RPM's

According to the instructions presented on [acs-community.github.io](acs-community.github.io), and analyzing the ACS "Master" Makefile
the build process should be:

 - Group all the folders in the MODULES variable, this is every folder in LGPL folder, consisting in:
   - Tools
   - Kit
   - CommonSoftware
   - Benchmark (Optionally)
 
 - build definition: clean_log checkModuleTree prepare update
   - clean_log: deletes build.log
   - checkModuleTree: Applies acsBUILDCheckModuleTree.sh to each of the folders in MODULES
   - prepare: 
      - acsBUILDPrepareKit.sh script builds VLT and Doc stuff that belong to Kit Module and have a "circular" dependency. 
      - Then, Kit's acs and acstempl are built. Also Doxygen in Tool's module is built (Not necesary)
   - update: checkModuleTree Again and does almost the same as *Canned* with the difference, that instead of calling Make directly, it uses the defined instructions of MakeIt and MakeItAux, which add the measurement of time.

 - MakeIt and MakeItAux are called 3 times inside update, to make clean, make all, and make install
  * $1 = clean, $2 = build.log
  * $1 = all, $2 = build.log
  * $1 = install, $2 = build.log



 - Define Canned: For loop through folders grouped in MODULES variable, to select one of the 3 places where the Makefile of that particular module could be, which are:
  -  src/Makefile: 2nd level
  -  ws/src/Makefile: 3rd level
  -  Makefile: At same level
 And ejecutes make -C /path/to/Makefile. $(MAKE_FLAGS) appears to be empty.


