# SampleRotating.mac

- 回転モジュレータのビームラインの例題マクロファイルです。<br>
可視化して確認しましょう。
 
```
$ ./bin/PTSdemo -i  SampleRotating.mac
Session: /vis/viewer/set/viewpointThetaPhi 0. 0.
Session: /vis/viewer/zoomTo 5
Session: /vis/scene/endOfEventAction refresh
Session: /run/beamOn 50 ./macros/common
```

- 水ファントムでのワブリングビームを確認できます。
SampleWobbler_t0.root, SampleWobbler_t1.rootを出力します。

```{code-block} bash
#======================================================================================
# SampleWobbler.mac
#======================================================================================
#  (PreInit State)
#
# -------------------------------------------------------------------------------------
# [OP]Verbose settings
# -------------------------------------------------------------------------------------
/control/verbose 1
/run/verbose 1
/My/physics/verbose 0
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
/My/physics/verbose 0
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
# Wobbling Field activation
# ----------------------------------------- 
/Dynamic/Module/updateEvent/wobbling  WobblerX  WobblerY  true
#
/G4M/Module/select WobblerX
/G4M/Module/typeid ./data/Sample/G4MWobblerMagnet/wobblerX.dat
/G4M/Module/install WobblerX
#
/G4M/Module/select WobblerY
/G4M/Module/typeid ./data/Sample/G4MWobblerMagnet/wobblerY.dat
/G4M/Module/install WobblerY
#
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
#======================================================================================
# [MA] Beam Parameters
#======================================================================================
/My/PrimaryGenerator/select  ParticleGun
###/control/execute ./macros/DynamicPort/P2_beam.mac
/control/execute ./macros/DynamicPort/particleGun.mac
#
#======================================================================================
# [OP] Scoring
#======================================================================================
/My/runaction/hist/enable JSTEdep true
##/My/runaction/ntuple/create JST 
/My/runaction/hist/enable JST true
/My/runaction/ntuple/addColumn JST evno I
/My/runaction/ntuple/addColumn JST pid I
/My/runaction/ntuple/addColumn JST de  F  keV
/My/runaction/ntuple/addColumn JST ix  I 
/My/runaction/ntuple/addColumn JST iy  I 
/My/runaction/ntuple/addColumn JST iz  I 
#
/analysis/verbose 1
#
/My/runaction/dumpfile SampleWobbler.root
#
##/run/beamOn 5000
###/run/beamOn 10
# --EOF----------------------------------------------------------------------------------


```


