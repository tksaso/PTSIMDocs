# SampleScan.mac

- スポットスキャニングの例題マクロファイルです。<br>
基本的な構造は、[exampleA8](../Tutorial/exampleA8.md)と同じです。

- `eidFile`, `weightFile`,`scanFile`が必要です。
 
```
$ cp ./data/Sample/G4MScanBeam/EIDSample.dat .
$ cp ./data/Sample/G4MScanBeam/WeightSample.dat .
$ cp ./data/Sample/G4MScanBeam/ScanSample.dat .
$ ./bin/PTSdemo -m  SampleScan.mac
```

- SampleScan_t0.root, SampleScan_t1.rootを出力します。

```{code-block} bash
#======================================================================================
# SampleScan.mac
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
/control/execute  ./macros/DynamicPort/beamModuleScan.mac
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
# ScanBeam and Scanning Magnets
# ----------------------------------------- 
/My/PrimaryGenerator/select  ScanBeam
/control/execute macros/DynamicPort/scanBeamBase.mac
/G4M/ScanBeam/scanMagXY ScanMagnetX ScanMagnetY
/G4M/Module/install ScanMagnetX
/G4M/Module/install ScanMagnetY
#
/G4M/ScanBeam/eidFile    ./EIDSample.dat
/G4M/ScanBeam/weightFile ./WeightSample.dat
/G4M/ScanBeam/scanFile   ./ScanSample.dat
#
/G4M/ScanBeam/calcProb
#
/G4M/ScanBeam/show eid
/G4M/ScanBeam/show spot
/G4M/ScanBeam/show weight
#
# -----------------------------------------
# Water Phantom settings and installation
# -----------------------------------------
## Water Phantom Size (Half length)
/G4M/Module/Phantom/size 200. 200. 250.0 mm
## Water Phantom SD Size (Half length)
/G4M/Module/Phantom/sdsize 200.0 200.0 250.0 mm
## Segmentation of water phantom ( Nx, Ny, Nz )
/G4M/Module/Phantom/dim 400. 400. 500.
## Installation of WaterPhantom.
/G4M/Module/select WaterPhantom
/G4M/Module/upEdgeZ 0. mm
/G4M/Module/install WaterPhantom
#
#
/My/runaction/hist/enable JST     true
/My/runaction/ntuple/addColumn JST evno I
/My/runaction/ntuple/addColumn JST de  F  keV
/My/runaction/ntuple/addColumn JST ix  I 
/My/runaction/ntuple/addColumn JST iy  I 
/My/runaction/ntuple/addColumn JST iz  I 
#
/My/tracking1/killZUpStream -1000. mm
/My/tracking1/killSecondary true
# -------------------------------------------------------------------------------------
# [OP] ROOT Output file Name
# -------------------------------------------------------------------------------------
/My/runaction/dumpfile SampleScan.root
#
#======================================================================================
# [OP] Visualization Settings
#======================================================================================
#/control/execute ./macros/common/vis.mac
#
#======================================================================================
# [MA] BeamOn
#======================================================================================
#
/run/beamOn 50000
#
# --EOF----------------------------------------------------------------------------------
```


