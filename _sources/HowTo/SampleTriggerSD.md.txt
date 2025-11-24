# SampleTriggerSD.mac

-  基本的に[exampleA10](../Tutorial/exampleA9.md)と同じです。

```{code-block} bash
#======================================================================================
# SampleTriggerSD.mac
#======================================================================================#
#
#  (PreInit State)
#
# -------------------------------------------------------------------------------------
# [OP]Verbose settings
# -------------------------------------------------------------------------------------
/control/verbose 1
/run/verbose 1
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
/control/execute  ./macros/DynamicPort/beamModule0.mac
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
/G4M/Module/install Extract
/G4M/Module/install Scatter
/G4M/Module/install WobblerX
/G4M/Module/install WobblerY
/G4M/Module/install MainMonitor
/G4M/Module/install SECMonitor
/G4M/Module/install FlatMonitor
/G4M/Module/install Collimator
/G4M/Module/install MLC
#
#======================================================================================
# [MA] Beam Parameters
#======================================================================================
/control/execute macros/DynamicPort/particleGun.mac
#
# -------------------------------------------------------------------------------------
# [OP] ROOT Output file Name
# -------------------------------------------------------------------------------------
/My/runaction/dumpfile SampleTriggerSD.root
#
# Geometrical Trigger ----
/G4M/Module/dumpLV     MLC
/G4M/Module/selectLV   MLC   1
/G4M/Module/attachTrgID  1
#
#======================================================================================
# [MA] BeamOn
#======================================================================================
/My/runaction/hist/enable JST     true
/My/runaction/ntuple/addColumn JST de  F  keV
/My/runaction/ntuple/addColumn JST ix  I 
/My/runaction/ntuple/addColumn JST iy  I 
/My/runaction/ntuple/addColumn JST iz  I 
/My/runaction/ntuple/addColumn JST triggerid I
/My/runaction/ntuple/addColumn JST triggerx F mm
/My/runaction/ntuple/addColumn JST triggery F mm
/My/runaction/ntuple/addColumn JST triggerz F mm
#
##/run/beamOn 1
/run/beamOn 10000
#
# --EOF----------------------------------------------------------------------------------

```


