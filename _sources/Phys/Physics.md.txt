```text
#######################################################################
# EM processes including generic ions.
########################################################################
#/My/physics/register G4EmStandardPhysics
#/My/physics/register G4EmStandardPhysics_option1
#/My/physics/register G4EmStandardPhysics_option2
/My/physics/register G4EmStandardPhysics_option3
#/My/physics/register G4EmStandardPhysics_option4
#/My/physics/register G4EmDNAPhysics
#/My/physics/register G4EmLivermorePhysics
#/My/physics/register G4EmPenelopePhysics
#/My/physics/register G4EmLowEPPhysics
#/My/physics/register G4EmStandardPhysicsGS
#/My/physics/register G4EmStandardPhysicsSS
#/My/physics/register G4EmStandardPhysicsWVI
########################################################################
# Elastic process for hadrons.
########################################################################
/My/physics/register G4HadronElasticPhysics
#/My/physics/register G4HadronDElasticPhysics
#/My/physics/register G4HadronDElasticPhysicsHP
#/My/physics/register G4HadronDElasticPhysicsHPT
#/My/physics/register G4HadronDElasticPhysicsLEND
#/My/physics/register G4HadronDElasticPhysicsPHP
#/My/physics/register G4HadronDElasticPhysicsVI
#/My/physics/register G4HadronDElasticPhysicsXS
########################################################################
# Elastic process for other ions.
########################################################################
#/My/physics/register G4IonElasticPhysics
########################################################################
# Elatic process for thermal neutrons
########################################################################
#/My/physics/register G4ThermalNeutrons
########################################################################
# Inelastic processes except for ions.
########################################################################
#/My/physics/register G4HadronPhysicsQBBC
#/My/physics/register G4HadronPhysicsQBBC_ABLA
#/My/physics/register G4HadronPhysicsFTFP_BERT
#/My/physics/register G4HadronPhysicsFTFP_BERT_ATL
#/My/physics/register G4HadronPhysicsFTFP_BERT_HP
#/My/physics/register G4HadronPhysicsFTFP_BERT_TRV
#/My/physics/register G4HadronPhysicsFTFPQGSP_BERT
/My/physics/register G4HadronPhysicsFTF_BIC
#/My/physics/register G4HadronPhysicsINCLXX
#/My/physics/register G4HadronPhysicsQGSP_BERT
#/My/physics/register G4HadronPhysicsQGSP_BERT_HP
#/My/physics/register G4HadronPhysicsQGSP_BIC
#/My/physics/register G4HadronPhysicsQGSP_BIC_AllHP
#/My/physics/register G4HadronPhysicsQGSP_BIC_HP
#/My/physics/register G4HadronPhysicsQGSP_FTFP_BERT
#/My/physics/register G4HadronPhysicsQGS_BIC
#/My/physics/register G4HadronPhysicsShielding
#/My/physics/register G4HadronPhysicsShieldingLEND
########################################################################
# Stopping Physics  (Capture)
########################################################################
/My/physics/register G4StoppingPhysics
#/My/physics/register G4StoppingPhysicsFritiofWithBinaryCascade
#/My/physics/register G4StoppingPhysicsWithINCLXX
########################################################################
# Radioactive decay
########################################################################
/My/physics/register G4RadioactiveDecayPhysics
########################################################################
# Inelastic Ions physics including Generic Ions.
########################################################################
/My/physics/register G4IonBinaryCascadePhysics
#/My/physics/register G4IonINCLXXPhysics
#/My/physics/register G4IonPhysics
#/My/physics/register G4IonQMDPhysics
#/My/physics/register G4LightIonQMDPhysics
########################################################################
# Decay Physics
########################################################################
/My/physics/register G4DecayPhysics
########################################################################
# VERBOSE
########################################################################
/My/physics/verbose 0
##
# Enable production cut for gamma intractions
# (Photoelectron and compton electron )
###/process/em/applyCuts true
# RdaioactiveDecay time window (V11.2 default 1 year)
###/process/had/rdm/thresholdForVeryLongDecayTime 1.e+60 year
#------------------------------------------------------------------------
# (Updates)
# 2024-08-28 thresholdForVeryLongDecayTime. (V11.2 or later)
# 2024-08-31 Add physicsconstructors available in v11.2.2
###
```