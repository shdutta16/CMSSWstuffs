# CMSSWstuffs
Contains instructions on various tasks done within CMSSW


## Extracting Xsecs using MINIAOD files
1. Setup the analyzer to extract the Xsec
   ```
   cmsrel CMSSW_10_6_0
   cd CMSSW_10_6_0/src
   cmsenv
   curl https://raw.githubusercontent.com/cms-sw/genproductions/master/Utilities/calculateXSectionAndFilterEfficiency/genXsec_cfg.py -o ana.py
   ```
   Do ```voms cms``` before doing the following.

2. Locate the MINIAOD file's logical path from DAS. As an example, one MINIAOD file corresponding to 2HDMa model for Run2Fall17 94X is done here. File: ```/store/mc/RunIIFall17MiniAODv2/ggTomonoH_aa_sinp0p35_tanb1p0_MXd10_MH3_800_MH4_150_TuneCP3_madgraph-pythia/MINIAODSIM/PU2017_12Apr2018_94X_mc2017_realistic_v14-v2/140000/185FCA4F-39C0-EB11-B8A0-0242AC1C050C.root```

3. Get the site where the file is located in CMSSW:
   ```
   dasgoclient -query="site file=/store/mc/RunIIFall17MiniAODv2/ggTomonoH_aa_sinp0p35_tanb1p0_MXd10_MH3_800_MH4_150_TuneCP3_madgraph-pythia/MINIAODSIM/PU2017_12Apr2018_94X_mc2017_realistic_v14-v2/140000/185FCA4F-39C0-EB11-B8A0-0242AC1C050C.root"
   ```

4. Prepend ```/store/test/xrootd/SITENAME``` to the logical path where ```SITENAME``` is one of the outputs from the previous operation. More details [here](https://twiki.cern.ch/twiki/bin/view/CMSPublic/WorkBookXrootdService) under the topic titled ```Access a file from one specific site``` .

5. Copy the file using ```xrdcp``` command
   ```
   xrdcp root://cmsxrootd.fnal.gov//store/test/xrootd/T1_US_FNAL/store/mc/RunIIFall17MiniAODv2/ggTomonoH_aa_sinp0p35_tanb1p0_MXd10_MH3_800_MH4_150_TuneCP3_madgraph-pythia/MINIAODSIM/PU2017_12Apr2018_94X_mc2017_realistic_v14-v2/140000/185FCA4F-39C0-EB11-B8A0-0242AC1C050C.root ./2HDMa_mA800_ma150_Run2Fall17_94X_MINIAOD.root
   ```

6. Extract the Xsec running ```cmsRun```
   ```
   cmsRun ana.py inputFiles="file:2HDMa_mA800_ma150_Run2Fall17_94X_MINIAOD.root" maxEvents=-1 &> 2HDMa_mA800_ma150_Run2Fall17_94X_MINIAOD_Xsec.txt
   ```
   This will store the stdout and stderr in the file ```2HDMa_mA800_ma150_Run2Fall17_94X_MINIAOD_Xsec.txt``` where, the Xsec can be found.



## Adding package from CMSSW
1. From the ```CMSSW/src``` do ```cmsenv```
2. Run ```git cms-addpkg <pkg-name>``` 



## Getting the list of files of a microAOD dataset
``` dasgoclient -query="file dataset=/DiPhotonJetsBox_MGG-80toInf_13TeV-sherpa/phys_higgs-Era2018_legacy_v1_Summer20UL-legacyRunII-v0-RunIISummer20UL18MiniAODv2-106X_upgrade2018_realistic_v16_L1v1-v2-cfbcbdcc72837b89d1f6329ff10bae09/USER instance=prod/phys03"```

By default the instance is global. The instance needs to specified, if it's other than global. 
