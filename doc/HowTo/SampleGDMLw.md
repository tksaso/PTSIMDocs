# SampleGDMLw.mac

-  実体化したビームモジュールのオメトリをGDMLファイルに出力する例題マクロファイルです。

 
```
$ ./bin/PTSdemo -m  SampleGDMLw.mac
```

- Collimator.gdml, MainMonitor.gdml, Scatter.gdml, Extract.gdml, MLC.gdml, WaterPhantom.gdmlを出力します。

```{code-block} bash
#======================================================================================
# SampleGDMLw.mac
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
# ----------------------------------------
# Water Phantom settings and installation
# ----------------------------------------- 
## Water Phantom Size (Half length)
/G4M/Module/Phantom/size 150. 150. 200.0 mm
## Water Phantom SD Size (Half length) 
/G4M/Module/Phantom/sdsize 150.0 150.0 200.0 mm
## Segmentation of water phantom ( Nx, Ny, Nz )
/G4M/Module/Phantom/dim 50. 50. 50.
## Installation of WaterPhantom. 
/G4M/Module/install WaterPhantom
#
# -----------------------------------------
# Installation of the other modules
# ----------------------------------------- 
/G4M/Module/install Extract
/G4M/Module/install Scatter
/G4M/Module/install WobblerX
/G4M/Module/install WobblerY
/G4M/Module/install MainMonitor
/G4M/Module/install Collimator
/G4M/Module/install MLC
#
#======================================================================================
# [MA] Beam Parameters
#======================================================================================
/control/execute macros/DynamicPort/particleGun.mac
#
#======================================================================================
# [MA] BeamOn
#======================================================================================
#
/run/beamOn 0
#			   mname   SD    RefPointer
/Dynamic/Module/gdml/write Extract false true
/Dynamic/Module/gdml/write Scatter false true
/Dynamic/Module/gdml/write MainMonitor false true
/Dynamic/Module/gdml/write Collimator false true
/Dynamic/Module/gdml/write MLC false true
/Dynamic/Module/gdml/write WaterPhantom false true
# --EOF----------------------------------------------------------------------------------

```
