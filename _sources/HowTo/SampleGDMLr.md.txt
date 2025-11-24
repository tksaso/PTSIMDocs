# SampleGDMLr.mac

- GDMLファイルのジオメトリを構築する例題マクロファイルです。

 
```
$ ./bin/PTSdemo -i  SampleGDMLr.mac
Session: /run/beamOn 50
```

- SampleGDMLr_t0.root, SampleGDMLr_t1.rootを出力します。

```{code-block} bash
#======================================================================================
# SampleGDMLr.mac
#======================================================================================#
#
#  (PreInit State)
#
# -------------------------------------------------------------------------------------
# [OP]Verbose settings
# -------------------------------------------------------------------------------------
/control/verbose 2
/run/verbose 2
/tracking/verbose 0
#
#======================================================================================
# [MA]Material
#======================================================================================
/control/execute ./macros/common/materials.mac
#
#======================================================================================
# [MA]PhysicsList
#======================================================================================
/control/execute ./macros/common/phys.mac
#
#======================================================================================
# [MA]Particle Therapy System Selection and Beam Module Registration
#======================================================================================
/G4M/System DynamicPort
/control/execute  ./macros/DynamicPort/beamModule.mac
/Dynamic/Module/unregister Extract
/Dynamic/Module/unregister Scatter
/Dynamic/Module/unregister MainMonitor
/Dynamic/Module/unregister Collimator
/Dynamic/Module/unregister MLC
/Dynamic/Module/unregister Bolus
/Dynamic/Module/unregister WaterPhantom
###----GDML-----
/Dynamic/Module/gdml/schema  ./data/schema/gdml.xsd
##-- USE Defined material in PTSIM.
/Dynamic/Module/gdml/reader/skipMaterials  true
/Dynamic/Module/register Extract G4MBMGdml ./data/Sample/G4MBMGdml/Extract.gdml 0. 0. 3500. mm 0. 0. 0. deg
/Dynamic/Module/register MainMonitor G4MBMGdml ./data/Sample/G4MBMGdml/MainMonitor.gdml 0. 0. 2200. mm 0. 0. 0. deg
/Dynamic/Module/register Collimator G4MBMGdml ./data/Sample/G4MBMGdml/Collimator.gdml 0. 0. 1500. mm 0. 0. 0. deg
/Dynamic/Module/register MLC G4MBMGdml ./data/Sample/G4MBMGdml/MLC.gdml 0.  0.  1000. mm 0. 0. 0. deg
###---GDML w/ SD -----
/Dynamic/Module/register Scatter G4MBMGdml ./data/Sample/G4MBMGdml/ScatterSD.gdml 0. 0. 2700. mm 0. 0. 0. deg
/Dynamic/Module/register WaterPhantom G4MBMGdml ./data/Sample/G4MBMGdml/WaterPhantomSD.gdml 0. 0. 0. mm 0. 0. 0. deg
#
#======================================================================================
# [MA]Run Initialize
#======================================================================================
/run/initialize
#
#
#  (Idle State)
#
#======================================================================================
# [MA]Beam Module Implmentation
#======================================================================================
/G4M/Module/list
#
# -----------------------------------------
# Installation of the other modules
# ----------------------------------------- 
/G4M/Module/install Extract
/G4M/Module/install Scatter
/G4M/Module/install MainMonitor
/G4M/Module/install Collimator
/G4M/Module/install MLC
/G4M/Module/install WaterPhantom
#
# -------------------------------------------------------------------------------------
# [OP] ROOT Output file Name
# -------------------------------------------------------------------------------------
/My/runaction/hist/enable DICOMCT false
/My/runaction/hist/enable DICOM   false
/My/runaction/hist/enable JSTEdep false
/My/runaction/hist/enable JSTDose false
#####
/My/runaction/ntuple/create SCT DetectorSD/HitsCollection
/My/runaction/ntuple/addColumn SCT evno I
##/My/runaction/ntuple/addColumn SCT trkid I
/My/runaction/ntuple/addColumn SCT pid I
/My/runaction/ntuple/addColumn SCT de  F  keV
#/My/runaction/ntuple/addColumn SCT dose  F  gray
#/My/runaction/ntuple/addColumn SCT ix  I 
#/My/runaction/ntuple/addColumn SCT iy  I 
/My/runaction/ntuple/addColumn SCT x  F mm 
/My/runaction/ntuple/addColumn SCT y  F mm
#/My/runaction/ntuple/addColumn SCT iz  I 
#/My/runaction/ntuple/addColumn SCT w  F
#/My/runaction/ntuple/addColumn SCT proc I
#/My/runaction/ntuple/addColumn SCT ke F MeV
####
/My/runaction/ntuple/create WP WaterPhantomSD/HitsCollection
/My/runaction/ntuple/addColumn WP evno I
##/My/runaction/ntuple/addColumn WP trkid I
/My/runaction/ntuple/addColumn WP pid I
/My/runaction/ntuple/addColumn WP de  F  keV
#/My/runaction/ntuple/addColumn WP dose  F  gray
/My/runaction/ntuple/addColumn WP ix  I 
/My/runaction/ntuple/addColumn WP iy  I
/My/runaction/ntuple/addColumn WP iz  I 
/My/runaction/ntuple/addColumn WP x  F mm 
/My/runaction/ntuple/addColumn WP y  F mm
/My/runaction/ntuple/addColumn WP z  F mm
#/My/runaction/ntuple/addColumn WP w  F
#/My/runaction/ntuple/addColumn WP proc I
#/My/runaction/ntuple/addColumn WP ke F MeV
####
/My/runaction/dumpfile SampleGDMLr.root
###
#======================================================================================
# [MA] Beam Parameters
#======================================================================================
/control/execute macros/DynamicPort/particleGun.mac
#
#======================================================================================
# [MA] BeamOn
#======================================================================================
#
#
###/run/beamOn 1000
##
# --EOF----------------------------------------------------------------------------------
```
