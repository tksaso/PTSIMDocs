# SampleRIbit.mac

- RI-tagging

```{code-block} bash
#======================================================================================
# SampleRIbit.mac
#======================================================================================#
#
#  (PreInit State)
#
# -------------------------------------------------------------------------------------
# [OP]Verbose settings
# -------------------------------------------------------------------------------------
/control/verbose 1
/run/verbose 0
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
/G4M/Module/Phantom/dim 150. 150. 250.
## Installation of WaterPhantom.
/G4M/Module/select WaterPhantom
/G4M/Module/translate 0. 0. 0. mm
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
#/G4M/Module/install SecondMonitor
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
#======================================================================================
# [MA] Beam Parameters
#======================================================================================
/My/PrimaryGenerator/select  ParticleGun
/control/execute macros/DynamicPort/particleGun.mac
#/gun/particle ion
#/gun/ion  11 22
#/gun/energy   0. MeV
#/gun/position 0 0 0. mm
##/gun/direction 0 0 0
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
##/control/execute ./macros/common/Hist.mac
/My/runaction/hist/enable DICOMCT false
/My/runaction/hist/enable DICOM   false
/My/runaction/hist/enable JSTEdep false
/My/runaction/hist/enable JSTDose false
#
/My/runaction/hist/enable JST     true
###/My/runaction/ntuple/create JST
/My/runaction/ntuple/addColumn JST evno I
/My/runaction/ntuple/addColumn JST pid I
/My/runaction/ntuple/addColumn JST parentpid I
/My/runaction/ntuple/addColumn JST de  F  keV
/My/runaction/ntuple/addColumn JST ix  I
/My/runaction/ntuple/addColumn JST iy  I
/My/runaction/ntuple/addColumn JST iz  I
/My/runaction/ntuple/addColumn JST riid  I
/My/runaction/ntuple/addColumn JST rix  F mm
/My/runaction/ntuple/addColumn JST riy  F mm
/My/runaction/ntuple/addColumn JST riz  F mm
###
/My/runaction/dumpfile SampleRIbit.root
#
#======================================================================================
# [MA] BeamOn
#======================================================================================
#
/My/MyTrackingActForTrkInfo/setBit 1 1 2
/My/MyTrackingActForTrkInfo/setBit 2 2 4
/My/MyTrackingActForTrkInfo/setBit 3 5 11
/My/MyTrackingActForTrkInfo/setBit 4 6 11
/My/MyTrackingActForTrkInfo/setBit 5 6 12
/My/MyTrackingActForTrkInfo/setBit 6 6 13
/My/MyTrackingActForTrkInfo/setBit 7 7 13
/My/MyTrackingActForTrkInfo/setBit 8 8 11
/My/MyTrackingActForTrkInfo/setBit 9 8 15
/My/MyTrackingActForTrkInfo/setBit 10 8 16
# HandsOn3
#
/process/had/rdm/thresholdForVeryLongDecayTime 1.e+60 year
#
/run/beamOn 10000
#
# --EOF----------------------------------------------------------------------------------

```


