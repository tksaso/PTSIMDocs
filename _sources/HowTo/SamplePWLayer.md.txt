# SamplePWLayer.mac

- 水ファントムをマスワールドに配置して、パラレルワールドに物体を配置して水ファントムに物体を埋め込む例題マクロです。基本的に[exampleA7](../Tutorial/exampleA7.md)と同じです。

 
```
$ ./bin/PTSdemo -i  SamplePWLayer.mac
```

```{code-block} bash
#======================================================================================
# SamplePWLayer.mac
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
# [MA]Particle Therapy System Selection and Beam Module Registration
#======================================================================================
/G4M/System DynamicPort
/control/execute  ./macros/DynamicPort/beamModule.mac
#
#======================================================================================
# [OP]Parallel World
#======================================================================================
/My/DetConstruction/createPW  paraWorld0
/My/DetConstruction/listPW
#======================================================================================
# [OP]Process for Parallel World
#======================================================================================
#                        <ParallelWorldName:s> <Layered:b> 
/My/physics/pwProcess     paraWorld0  true
#======================================================================================
# [MA]PhysicsList
#======================================================================================
/control/execute ./macros/common/phys.mac
#
#======================================================================================
# [MA]Run Initialize
#======================================================================================
/run/initialize
#
#  (Idle State)
#
#======================================================================================
# [MA]Beam Module Implmentation
#======================================================================================
/G4M/Module/list
#
#
/My/DetConstruction/listPW
#
# -----------------------------------------
# Installation of the other modules
# ----------------------------------------- 
# Mass Geometry
/G4M/Module/Phantom/size   200. 200. 250.0 mm
/G4M/Module/Phantom/sdsize 200. 200. 250.0 mm
/G4M/Module/Phantom/dim    100. 100.  100.
/G4M/Module/select WaterPhantom
/G4M/Module/translate 0  0   0 mm
/G4M/Module/install WaterPhantom 
#
/My/runaction/hist/enable JST     true
/My/runaction/ntuple/addColumn JST de  F  keV
/My/runaction/ntuple/addColumn JST ix  I 
/My/runaction/ntuple/addColumn JST iy  I 
/My/runaction/ntuple/addColumn JST iz  I 
#
/G4M/Module/select Collimator
/G4M/Module/translate 0  0   0 mm
/G4M/Module/install  Collimator paraWorld0 true
#
/G4M/Module/dumpLV Collimator
/G4M/Module/selectLV Collimator 1
##/G4M/Module/setMaterial null
#======================================================================================
# [MA] Beam Parameters
#======================================================================================
/My/PrimaryGenerator/select   GPS
/gps/particle proton
/gps/pos/type  Plane
/gps/pos/shape  Square
/gps/pos/centre   0 0 500. mm
/gps/pos/halfx  200 mm
/gps/pos/halfy  200 mm
#/gps/pos/halfx  0 mm
#/gps/pos/halfy  0 mm
/gps/energy  200 MeV
#
#
# -------------------------------------------------------------------------------------
# [OP] ROOT Output file Name
# -------------------------------------------------------------------------------------
/My/runaction/dumpfile SamplePWLayer.root
#
#======================================================================================
# [MA] BeamOn
#======================================================================================
##/run/beamOn 50000
# --EOF----------------------------------------------------------------------------------

```


