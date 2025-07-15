---
layout: default
title: Running an example
parent: The restG4 package
nav_order: 20
---

## Running an example with restG4
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

This page follows day2/session2 of the [rest-school](https://github.com/rest-for-physics/rest-school/tree/master/day2/session2) where codes can be found to run the following code. 


### Exercise 1. Create a simple geometry using GDML
In this exercise you will learn how to create a simple geometry using GDLM. Remember that this file has different sections:

**XML Declaration**
```
<?xml version="1.0" encoding="utf-8" ?>
```
**External files to be included** (in this case we are including material file)
```
<!DOCTYPE gdml [
<!ENTITY materials SYSTEM "https://rest-for-physics.github.io/materials/rest.xml">
]>
```
**XML Schema and GDML Schema location**

```
<gdml xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="http://service-spi.web.cern.ch/service-spi/app/releases/GDML/schema/gdml.xsd">
```
**Numerical definitions**
```
<define>
       <variable name="world_size" value="120" />
       
        <!-- The gas target -->
	<variable name="quencherDensity" value="2.51*0.01"/> <!-- mg/cm3 -->
        <variable name="quencherFraction" value="0.01"/>
	<variable name="targetGasDensity" value="1.66201e-3"/> <!-- in g/cm3 -->
        <variable name="gasTemperature" value="300"/> <!-- K -->
        <variable name="gasPressure" value="1"/> <!-- bar -->

        <!-- The vessel definitions -->  
        <constant name="vesselThickness" value="10" />    <!--in mm's -->
        <constant name="vesselXSize" value="100" />
	      <constant name="vesselYSize" value="100" />
        <constant name="vesselZSize" value="100"/>
        <position name="vesselPosition" unit="mm" x="0" y="0" z="0"/>
</define>
```
**Material definitions**
As we have included a file with material definitions, just need to referenciate it.
```
&materials;
```
**Geometry**

*Solid declarations*
```
<solids>
    <box name="WorldSolid" x="world_size" y="world_size" z="world_size" lunit="mm"/>
    <box name="solidBox" x="vesselXSize" y="vesselYSize" z="vesselZSize" lunit="mm" />
     <box name="gasSolid"  x="vesselXSize-2*vesselThickness" y="vesselYSize-2*vesselThickness" z="vesselZSize-2*vesselThickness" lunit="mm" />
    
    <subtraction name="vesselSolid">
        <first ref="solidBox"/>
        <second ref="gasSolid"/>
    </subtraction>      
</solids>
```
*Structure*
```
<structure>
    <!-- {{{ Volumes definition (material and solid assignment) -->
    <volume name="gasVolume">
        <materialref ref="PureArgon"/>
        <solidref ref="gasSolid"/>
    </volume>

    <volume name="vesselVolume">
        <materialref ref="G4_Cu"/>
        <solidref ref="vesselSolid"/>
    </volume>

    <!-- {{{ Physical volume definition (volume and position assignment) -->
     <volume name="World">
        <materialref ref="G4_AIR"/>
        <solidref ref="WorldSolid"/>

        <physvol name="gas">
            <volumeref ref="gasVolume"/>
            <position name="gasPosition"  x="0" y="0" z="0"/>
        </physvol>
       
        <physvol name="vessel">
            <volumeref ref="vesselVolume"/>
		<positionref ref="vesselPosition"/>
        </physvol>
    </volume>  

</structure>
```
**Definition of top volume**
```
<setup name="Default" version="1.0">
        <world ref="World"/>
    </setup>
```
**Final mark of GDM file**
```
</gdml>
```
**Activity 1:** Identify in the material repository (https://rest-for-physics.github.io/materials/rest.xml) the materials appearing in the GDML file. 
**Activity 2:** Create your own **box.gdml** file including all the sections and visualize the geometry using the REST macro  ```REST_Geant4_ViewGeometry("box.gdml")``` after launching a restRoot session `restRoot -m 1`. 

**NOTE:** The argument `-m 1` is equivalent to use the command`restRootMacros` and it will load in your ROOT interactive shell environment the REST macros installed in the installation directory.

### Exercise 2. Create a simple *RML* file to launch simulations

In this exercise you will learn how to create an RML file to launch a restG4 simulation.

**XML Declaration**
```
<?xml version="1.0" encoding="utf-8" ?>
```
**Initial tag**
```
<restG4>
```
**Parameters for the Run section**
```
   <TRestRun name="TestRun" title="A basic test with 2 active volumes">
        <parameter name="experimentName" value="Test"/>
        <parameter name="readOnly" value="false"/>
        <parameter name="runNumber" value="auto"/>
        <parameter name="runDescription" value=""/>
        <parameter name="user" value="${USER}"/>
        <parameter name="verboseLevel" value="1"/>
        <parameter name="overwrite" value="off"/>
        <parameter name="outputFileName" value="Run[fRunNumber]_[fRunType]_[fRunTag]_[fRunUser].root"/>
    </TRestRun>
```
 **TRestGeant4 Metadata**
```
 <TRestGeant4Metadata name="restG4 run" title="CosmicMuonTest">
        <parameter name="gdmlFile" value="box.gdml"/>
        <parameter name="subEventTimeDelay" value="100us"/>

        <parameter name="nRequestedEntries" value="1000"/>
        <parameter name="seed" value="137"/>

        <parameter name="saveAllEvents" value="false"/>
        <parameter name="printProgress" value="true"/>

       <generator type="surface" shape="wall" position="(0,10,0)cm" size="(30,30,0)cm" rotationAngle="3.1416/2"
                   rotationAxis="(1,0,0)">
            <source particle="mu-" fullChain="on">
                <energy type="TH1D" file="Muons.root" name="cosmicmuon"/>
                <angular type="formula" name="Cos2" direction="(0,-1,0)"/>
            </source>
        </generator>
        <detector>
            <parameter name="activateAllVolumes" value="false"/>

            <volume name="gas" sensitive="true" maxStepSize="1mm"/>
            <volume name="vessel" maxStepSize="1mm"/>
        </detector>

    </TRestGeant4Metadata>
```
**Physics List**
```
<TRestGeant4PhysicsLists name="default" title="First physics list implementation." verboseLevel="warning">
        <parameter name="cutForGamma" value="1" units="um"/>
        <parameter name="cutForElectron" value="1" units="um"/>
        <parameter name="cutForPositron" value="1" units="um"/>

        <parameter name="cutForMuon" value="1" units="mm"/>
        <parameter name="cutForNeutron" value="1" units="mm"/>
        <parameter name="minEnergyRangeProductionCuts" value="1" units="keV"/>
        <parameter name="maxEnergyRangeProductionCuts" value="1" units="GeV"/>

        <!-- EM Physics lists -->
        <physicsList name="G4EmLivermorePhysics"/>
        <!-- <physicsList name="G4EmPenelopePhysics"> </physicsList> -->
        <!-- <physicsList name="G4EmStandardPhysics_option3"> </physicsList> -->

        <!-- Decay physics lists -->
        <physicsList name="G4DecayPhysics"/>
        <physicsList name="G4RadioactiveDecayPhysics"/>
        <physicsList name="G4RadioactiveDecay">
            <option name="ICM" value="true"/>
            <option name="ARM" value="true"/>
        </physicsList>

        <!-- Hadron physics lists -->
        <physicsList name="G4HadronElasticPhysicsHP"/>
        <physicsList name="G4IonBinaryCascadePhysics"/>
        <physicsList name="G4HadronPhysicsQGSP_BIC_HP"/>
        <physicsList name="G4EmExtraPhysics"/>

    </TRestGeant4PhysicsLists>
 ```
 **Final Tag**
 ```
    </restG4>
 ```
 **Activity 1:** Create your own **basicSim.rml** file and launch simulation ```restG4 basic.rml```. 
 
 **Activity 2:** Once a root file has been created, open a root session ```restRoot -m 1``` and use the macro ```REST_Geant4_ViewEvent("fileName.root")``` to visualise the geometry and the particle tracks; use the ```new TBrowser``` to explore the Tree inside the file.
 
 **Extra activity:** Go to [examples](https://github.com/rest-for-physics/restG4/tree/master/examples) and to the [documentation](https://sultan.unizar.es/rest/classTRestGeant4Metadata.html) to choose any other *generator* or a different geometry.
 
 ### Exercise 3. Create an  *RML* file to launch a g4Analysis
In this exercise you will learn how to create a simple *rml* file for g4Analysis.
**XML Declaration**
```
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
```
**Initial label for TRestManager**
```
<TRestManager name="RESTManagerSim" title="Template manager to process a simulation generated by restG4.">
```
**Global definitions**
```
    <globals><parameter name="mainDataPath" value="."/><parameter name="verboseLevel" value="warning"/> %options are : essential silent, warning, info, debug
    </globals>
```
**Parameters for the Run section**
```
    <TRestRun name="Process" title="Geant4 basic analysis">
        <parameter name="experimentName" value="preserve"/>
        <parameter name="readOnly" value="false"/>
        <parameter name="runNumber" value="preserve"/>
        <parameter name="runTag" value="preserve"/>
        <parameter name="runType" value="G4Analysis"/>
        <parameter name="runDescription" value=""/>
        <parameter name="user" value="${USER}"/>
        <parameter name="verboseLevel" value="1"/>
        <parameter name="overwrite" value="off"/>
        <parameter name="outputFileName" value="Run[fRunNumber]_[fRunType]_[fRunTag]_[fRunUser].root"/>
    </TRestRun>
 ```
 **Initial label for the TRestProcessRunner and parameter's declarations**
 ```
    <TRestProcessRunner name="TemplateEventProcess" verboseLevel="info"><parameter name="eventsToProcess" value="0"/>
    <parameter name="inputEventStorage" value="OFF"/>
    <parameter name="outputEventStorage" value="OFF"/>
```
**Adding a G4Analysis Process and declaring observables**
```
	<addProcess type="TRestGeant4AnalysisProcess" name="g4Ana" observable="all">
	<!-- Custom observables -->
	 <observable name="gasVolumeEDep" value="ON"
        description="Energy deposited in the gas volume in keV" /> 
        <observable name="vesselVolumeEDep" value="ON"
	description="Energy deposited in the vessel volume 1 in keV" /> 
       <observable name="gasMeanPosX" value="ON"
        description="Mean position X of the energy deposited in the gas volume " />
         <observable name="gasMeanPosY" value="ON"
        description="Mean position Y of the energy deposited in the gas volume " />
          <observable name="gasMeanPosZ" value="ON"
      description="Mean position Z of the energy deposited in the gas volume " />
	</addProcess>
```
**Final mark for the TRestProcess Runner**
```
    </TRestProcessRunner>
```
**Adding a task**
```
    <addTask type="processEvents" value="ON"/>
```
**Final mark for TRestManager**
```
</TRestManager>
```
 **Activity 1:** Create your own **g4analyis.rml** file and launch analysis ```restManager  --f fileName.root -c g4analysis.rml```. 
 
 **Activity 2:** Once a root file has been created, open a root session ```restRoot -m 1``` and use the ```new TBrowser``` to explore the Tree inside the Analysis file you have created.
 
 **Extra activity:** Explore the [TRestGeant4Analysis documentation](https://sultan.unizar.es/rest/classTRestGeant4AnalysisProcess.html) to know more about G4Analysis observables.
