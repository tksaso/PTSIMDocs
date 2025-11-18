# SampleICRP145.mac

- ICRP145メッシュファントムジオメトリの例題マクロファイルです。<br>
ICRP145dataが必要です。

- Geant4付属のexamples/advanced/ICRP145_HumanPhantomでサンプルデータにアクセスできます。
サンプルデータが実行ディレクトリのICRP145dataフォルダに格納されているとします。
 
```
$ ./bin/PTSdemo -i  SampleICRP145.mac
Session: /run/beamOn 50
```

```{code-block} bash
#======================================================================================
# SampleICRP145.mac
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
# ** materials.mac defines "Liver" which may conflict with ICRP145 material definition.
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
# BM-Registration        Name     Class        Parameter file                    Position    Rotation
/Dynamic/Module/register Room     G4MRoom      ./data/Sample/G4MRoom/room10.dat  0. 0. 0. mm 0. 0. 0. deg
/Dynamic/Module/register ICRP145  G4MICRP145   ./data/Sample/G4MICRP145/AM.dat      0. 0. 0. mm 0. 0. 0. deg
#
#======================================================================================
# [MA]Run Initialize
#======================================================================================
/run/initialize
#
#  (Idle State)
#======================================================================================
# [MA]Beam Module Implmentation
#======================================================================================
/G4M/Module/list
#--
/G4M/ICRP145/vis true
/G4M/ICRP145/path ./ICRP145data
/G4M/ICRP145/type MRCP_AF
#--
/G4M/Module/select ICRP145
/G4M/Module/rotate 0. 90. 0.
/G4M/Module/install ICRP145
#
#======================================================================================
# [OP] Scoring 
#======================================================================================
/My/runaction/hist/enable DICOMCT false
/My/runaction/hist/enable DICOM   false
/My/runaction/hist/enable JSTEdep false
/My/runaction/hist/enable JSTDose false
/My/runaction/ntuple/create ICRP ICRP145/HitsCollection
/My/runaction/ntuple/addColumn ICRP evno I
/My/runaction/ntuple/addColumn ICRP pid I
/My/runaction/ntuple/addColumn ICRP de  F  keV
/My/runaction/ntuple/addColumn ICRP iext  I
/My/runaction/ntuple/addColumn ICRP x  F 
/My/runaction/ntuple/addColumn ICRP y  F 
/My/runaction/ntuple/addColumn ICRP z  F 
#
#/My/runaction/dumpfile SampleICRP145.root
#
#======================================================================================
# [MA] Beam Parameters
#======================================================================================
/control/execute macros/DynamicPort/particleGun.mac
#
#======================================================================================
# [OP] Visualization Settings
#======================================================================================
#
# ** Following Vis commands should be off in the PTSIM batch-mode. 
#
/vis/open
/vis/viewer/set/autoRefresh false
/vis/verbose errors
#
/vis/viewer/set/specialMeshRendering
/vis/viewer/set/specialMeshRenderingOption surfaces
/vis/viewer/set/style surface
/vis/viewer/set/hiddenMarker true
/vis/viewer/set/background 1 1 1 1
#
/vis/geometry/set/visibility Room 0 false
/vis/geometry/set/visibility ICRP145LV 0 false
#
/vis/drawVolume
#
/vis/viewer/set/autoRefresh true
/vis/verbose warnings
#======================================================================================
# [MA] BeamOn
#======================================================================================
#
##/run/beamOn 5
#
# --EOF----------------------------------------------------------------------------------

```


