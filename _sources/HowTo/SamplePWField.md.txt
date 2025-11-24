# SamplePWField.mac

-  水ファントム領域に磁場を印加する例題マクロファイルです。[exampleA11](../Tutorial/exampleA11.md)と基本的に同じです。
 
```
$ ./bin/PTSdemo -i  SamplePWField.mac
```

```{code-block} bash
#======================================================================================
# SamplePWField.mac
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
#                        <ParallelWorldName:s> <Layered:b> 
/My/physics/pwProcess     paraWorld0   true
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
/G4M/Module/Phantom/size   300. 300. 300.0 mm
/G4M/Module/Phantom/sdsize 300. 300. 300.0 mm
/G4M/Module/Phantom/dim     60.  60.  60.
/G4M/Module/select WaterPhantom
/G4M/Module/translate 0  0   0 mm
/G4M/Module/install WaterPhantom paraWorld0 true
#
/My/runaction/hist/enable      JST      true
/My/runaction/ntuple/addColumn JST de  F  keV
/My/runaction/ntuple/addColumn JST ix  I 
/My/runaction/ntuple/addColumn JST iy  I 
/My/runaction/ntuple/addColumn JST iz  I 
#----
/G4M/Module/FieldBox/field 0 -2 0 tesla
/G4M/Module/install FieldBox 
#/G4M/Module/FieldDisk/field 0 -2 0 tesla
#/G4M/Module/install FieldDisk 
#======================================================================================
# [MA] Beam Parameters
#======================================================================================
/control/execute macros/DynamicPort/particleGun2.mac
/gun/energy 200 MeV
#
#
# -------------------------------------------------------------------------------------
# [OP] ROOT Output file Name
# -------------------------------------------------------------------------------------
/My/runaction/dumpfile SamplePWField.root
#
/control/execute ./macros/common/vis.mac
#
/run/beamOn 100
#
# --EOF----------------------------------------------------------------------------------

```


