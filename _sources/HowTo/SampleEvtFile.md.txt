# SampleEvtFile.mac

- 独自フォーマットのPHSPファイルを読み込む例題マクロファイルです。

 
```
$ ./bin/PTSdemo -m  SampleEvtFile.mac
```

```{code-block} bash
#======================================================================================
# SampleEvtFile.mac
#======================================================================================
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
#
########
# Evt Interface Generator
/My/PrimaryGenerator/select EvtGun
########
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
# -----------------------------------------
# Water Phantom settings and installation
# ----------------------------------------- 
## Water Phantom Size (Half length)
/G4M/Module/Phantom/size 150. 150. 250.0 mm
## Water Phantom SD Size (Half length) 
/G4M/Module/Phantom/sdsize 150.0 150.0 250.0 mm
## Segmentation of water phantom ( Nx, Ny, Nz )
/G4M/Module/Phantom/dim 300. 300. 500.
## Installation of WaterPhantom. 
##/G4M/Module/install WaterPhantom
#
# -----------------------------------------
# Installation of the other modules
# ----------------------------------------- 
#/G4M/Module/install Extract
#/G4M/Module/install Scatter
#/G4M/Module/install WobblerX
#/G4M/Module/install WobblerY
#/G4M/Module/install MainMonitor
#/G4M/Module/install SECMonitor
#/G4M/Module/install FlatMonitor
#/G4M/Module/install Collimator
#/G4M/Module/install MLC
#
#======================================================================================
# [MA] Beam Parameters
#======================================================================================
/control/execute macros/DynamicPort/particleGun.mac
#
# -------------------------------------------------------------------------------------
# [OP] ROOT Output file Name
# -------------------------------------------------------------------------------------
/control/execute ./macros/common/Hist.mac
/My/runaction/dumpfile SampleEvtFile.root
#======================================================================================
# [MA] BeamOn
#======================================================================================
#
### 
# Event Interface open
/G4M/EvtIF/open trackDump.data
#/G4M/EvtIF/verbose 2
#
/run/beamOn 1000
#
### 
# Event Interface close
/G4M/EvtIF/close
#
# --EOF----------------------------------------------------------------------------------

```


