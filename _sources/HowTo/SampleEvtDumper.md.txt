# SampleEvtDumper.mac

-   独自フォーマットのPHSPファイル出力の例題マクロファイルです。

 
```
$ ./bin/PTSdemo -m  SampleEvtDumper.mac
```

```{code-block} bash
#======================================================================================
# SampleEvtDumper.mac
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
#####
# EVENT DUMPER Module
/Dynamic/Module/register DiskDumper        G4MDiskDumper      ./data/Sample/G4MDiskDumper/dumper.dat         0.   0. 1000. mm 0. 0. 0. deg
/Dynamic/Module/register BoxDumper1        G4MBoxDumper      ./data/Sample/G4MBoxDumper/dumper.dat         200.   0.  0. mm 0. 90. 0. deg
/Dynamic/Module/register BoxDumper2        G4MBoxDumper      ./data/Sample/G4MBoxDumper/dumper.dat        -200.   0.  0. mm 0. 90. 0. deg
#####
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
/G4M/Module/install DiskDumper
/G4M/Module/install BoxDumper1
/G4M/Module/install BoxDumper2
#
#
#======================================================================================
# [MA] Beam Parameters
#======================================================================================
/control/execute macros/DynamicPort/particleGun.mac
#
# -------------------------------------------------------------------------------------
# [OP] ROOT Output file Name
# -------------------------------------------------------------------------------------
/My/runaction/dumpfile SampleEvtDumper.root
#
#======================================================================================
# [MA] BeamOn
#======================================================================================
#
####
# EVENT Interface File open
/G4M/DumpInfo/DiskDumper/open DumpDisk.asc  ASCII
/G4M/DumpInfo/BoxDumper1/open DumpBox1.bin  BINARY
/G4M/DumpInfo/BoxDumper2/open DumpBox2      IAEAPHSP
####
/run/beamOn 5000
####
# EVENT Interface File close
/G4M/DumpInfo/DiskDumper/close
/G4M/DumpInfo/BoxDumper1/close
/G4M/DumpInfo/BoxDumper2/close
#
# --EOF----------------------------------------------------------------------------------

```


