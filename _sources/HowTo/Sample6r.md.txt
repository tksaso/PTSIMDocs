# Sample6r.mac

- IAEAphspデータ出力ファイルを読み込む例題マクロファイルです。

 
```
$ ./bin/PTSdemo -m  Sample6r.mac
```

- Sample6_t0_00.IAEAphsp, Sample6_t0_00.IAEAheader etc.を読み込みます。

```{code-block} bash
#======================================================================================
# Sample6r.mac
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
/G4M/Module/install WaterPhantom
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
# -------------------------------------------------------------------------------------
# [OP] Change Beam Module Parameters
# -------------------------------------------------------------------------------------
#/G4M/Module/select WaterPhantom
#/G4M/Module/translate 0. 0. -107.5 mm
#/G4M/Module/select Scatter
#/G4M/Module/typeid ./data/Sample/G4MDisk/016.dat
#
#
#======================================================================================
# [OP] Optional settings
#======================================================================================
# -------------------------------------------------------------------------------------
# [OP] Production Cuts, Region Production Cuts, for WaterPhantom or DICOM.
# -------------------------------------------------------------------------------------
# This setting must be done after installation of Module.
#/run/setCut 1. mm
#/run/setCutForRegion WaterPhantom  30. micrometer
#/run/setCutForRegion WaterPhantom  15. micrometer
#/My/physics/stepLimitForRegion WaterPhantom 10. micrometer
#
# -------------------------------------------------------------------------------------
# [OP] Secondary Track Suppression
# -------------------------------------------------------------------------------------
#/My/tracking1/killZUpStream 1000. mm
#/My/tracking1/killNeutral true
#/My/tracking1/killLepton true
#/My/tracking1/killSecondary true
#/My/tracking2/killZUpStream -1000. mm
#/My/tracking2/killNeutral true
#
# -------------------------------------------------------------------------------------
# [OP] Initial Random Seed
# -------------------------------------------------------------------------------------
#/random/resetEngineFrom ./LastSeed.info
#
# -------------------------------------------------------------------------------------
# [OP] ROOT Output file Name
# -------------------------------------------------------------------------------------
/control/execute ./macros/common/Hist.mac
/My/runaction/dumpfile Sample6r.root
#
#======================================================================================
# [MA] Beam Parameters
#======================================================================================
/My/PrimaryGenerator/select IAEAphsp
/G4M/IAEAphsp/file Sample6 00
/G4M/IAEAphsp/recycle 0
/G4M/IAEAphsp/totalParaRun 1
/G4M/IAEAphsp/paraRunId 1
/G4M/IAEAphsp/verbose 1
#
#======================================================================================
# [MA] BeamOn
#======================================================================================
#
/run/beamOn 100
#
# --EOF----------------------------------------------------------------------------------

```


