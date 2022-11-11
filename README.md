# Origin-Scripts
LabTalk Scripts for automated data analysis of OFET and EG-OFET data. Compatible with Origin 2018 Pro or newer.

## Installation:
To install the scripts perform the following steps:
1. Copy the `Scripts` folder to `C:\Users\<Account Name>\Documents\OriginLab`.
	- The filepath should **NOT** contain *spaces*!
	- The names of the scripts should also **NOT** contain *dashes* (`-`), as Origin cannot handle them.
2. Copy the `Themes` and `Templates` folders to `C:\Users\<Account Name>\Documents\OriginLab\User Files`.
3. Run OriginLab
4. Open the `Library` script with `CodeBuilder` and modify the `templatepath$` string variable at the top of the script. Make sure to save the script.
5. Select `View` and then `Command Window`. The Command Window will appear. It is similar to the Linux command line and allows one to give commands directly to Origin.
6. Write `cd C:\Users\<Account Name>\Documents\OriginLab\Scripts;` to set the Scripts directory as the working directory (do not forget the semicolon!). Then press `ENTER`.
7. Now the scripts should be available. To verify this, start typing the name of a script in the command line (e.g. `EG`) and then press `TAB`. If the working directory has been set correctly, Origin will try to autocomplete the partially typed script name.

## Analysis of filename templates:

### Fields
Each filename template consists of different **fields**, separated by an *underscore* (`_`).

The different **fields** are:
- `batchNo`: The number of the batch (e.g. 235th)
- `Architecture`: The architecture of the device (e.g. TGBC-OFET, TGBC-EGOFET).
- `material`: The semiconductor used to make the FET (e.g. IDTBT, C14-PBTTT).
- `concentration(No-units)`: The concentration of the material/semiconductor (e.g. 10-gl).
- `material`: The semiconductor used to make the FET (e.g. IDTBT, C14-PBTTT).
- `solvent`: The solvent(s) used to make the semiconductor solution (e.g. 100pDCB, 75pDCB-25pCF).
- `annealing`: The annealing conditions of the semiconductor thin films (e.g. 100C-1h).
- `additive (type-thickness-units)`: The additives or SAMs used to in the FET (e.g. Pristine, TCNQ-40-nm).
	- **NOTE**: Units are separated by a dash (e.g. 20-nm).
- `dielectric`: The dielectric used to make the FET (e.g. Cytop-M, PMMA).


### Values
The different **values** for each of the different **fields** can be seen in the `Library` file.



### The `step` convention:

Some of the filename templates employ the `step` convention. This is simply a convention to keep filenames short while keeping track of the sequence of liquids that have been injected in an EG-OFET. Suppose, for example, that we inject first ultrapure water (10 minutes), then saline solution (5 hours), and then ultrapure water again (1 day). How would we distinguish the measurement files?

Let's suppose that we have finished the 10 minute ultrapure water injection and are 2 minutes in the saline solution experiment: One method is to name the file e.g. `<transistor parameters>_up-H2O_10-min_ss_2-min_<measurement parameters>`. But with such a convention the filenames would get very big, once we injected more and more liquids.

With the step convention, we would simply write `step_2_ss_2-min`. `Step 2` means that this liquid (saline solution in this case) is the **second** liquid to be injected and the rest of the filename (2-min) represents the **exposure time** in that specific liquid.

Since we mentioned that our sequence consists of ultrapure water (10 minutes), then saline solution (5 hours), and then ultrapure water again (1 day). The overall measurements would look like this (if we assume one measurement per minute):
- `<transistor parameters>_step_1_up-H2O_1-min_<measurement parameters>`
- `<transistor parameters>_step_1_up-H2O_2-min_<measurement parameters>`
- ...
- `<transistor parameters>_step_2_ss_1-min_<measurement parameters>`
- `<transistor parameters>_step_2_ss_2-min_<measurement parameters>`
- ...
- `<transistor parameters>_step_3_up-H2O_1-min_<measurement parameters>`
- ...

## Script parameters:

**OFET (Transfer/Output)**:


## Filename templates:

**OFET (Transfer/Output)**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo_length(No-units)_condition(air/N2_liquid)_daysNo-"days"_MeasurementType(T-O)_MeasNo_MeasurementMode_IntegrationTime`

**OFET (C-V, C-f)**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo/CapNo (e.g. Cap-2)_condition(air/N2_liquid)_daysNo-"days"_MeasurementType(C-f)_MeasNo_Vdc`

`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo/CapNo_condition(air/N2_liquid)_daysNo-"days"_MeasurementType(C-V)_MeasNo_frequencyNo-frequencyUnits_Forward/Reverse sweep`

**OFET (shelf-life stability)**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo_length(No-units)_"step"_stepNo_condition(air/N2_liquid)_minutesNo-"min"_MeasurementType(T-O)_MeasNo_MeasurementMode_IntegrationTime`

**OFET (Cryo)**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo_length(No-units)_condition(air/N2_liquid)_daysNo-"days"_stepNo_air/vac_Temperature_MeasurementType(T-O)_MeasNo_MeasurementMode_IntegrationTime`

**OFET (Bias Stress)**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo_length(No-units)_condition(air/N2_liquid)_daysNo-"days"_BiasStressTypeandNo(PBS1/NBS1)_minutesNo-"min"_MeasurementType(T-O-S)_MeasNo_MeasurementMode_IntegrationTime`
	- **NOTE**: For a `sample` measurement type (`S`), the suffix is: `S_Vg_Vd`

**EG-OGET (sensing-cycling)**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_sampleNo_condition(air/N2_liquid)_daysNo-"days"_MeasurementType(I-vs-time_plunger,valve-port)`

`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_sampleNo_"step"_stepNo_condition(air/N2_liquid)_minutesNo-"min"_MeasurementType(T-O)_MeasNo_MeasurementMode_IntegrationTime`

**EG-OGET (operational stability)**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_sampleNo_"step"_stepNo_condition(air/N2_liquid)_daysNo-"days"_MeasurementType(I-vs-time_plunger,valve-port)`

`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_sampleNo_"step"_stepNo_condition(air/N2_liquid)_minutesNo-"min"_MeasurementType(T-O)_MeasNo_MeasurementMode_IntegrationTime`

**CAS UV/VIS**:
`batchNo_Method (CAS-UVVIS/UVVIS)_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_"step"_stepNo_condition(air/N2_liquid)_daysNo-"days"_Nomins-"min"_Pressure-units_Temperature-units_meas_MeasNo_MeasurementType(A-T-S-B)_"Vg"_Vg-V_"Vd"_Vd-V`

**FTIR (film/CAS)**:
`batchNo_Method (CAS-FTIR/FTIR)_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_stepNo_condition(air/N2_liquid)_daysNo-"days"_Nomins-"min"_Pressure-units_Temperature_MeasNo_MeasurementType(A-T-S-B)_Vg_Vd`

**FTIR (EG-OFET)**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_sampleNo_stepNo_condition(air/N2_liquid)_daysNo-"days"_Nomins-"min"_Pressure-units_Temperature_MeasNo_MeasurementType(A-T-S-B)_Vg_Vd`

**Cyclic Voltammetry**:
`FileNo_batchNo_Architecture_material_concentration/thickness_solvent-annealingT/time_additive_sampleNo_WE_CE_RE_liquid_"Start"_StartV_"Stop"_StopV_"V1"_LowerVertex_"V2"_UpperVertex_"step"_stepV_"rate"_Rate_"Scans"_ScanNo_MeasNo`

**NMR**:
`batchNo_sampleNo_Method (NMR/SSNMR)_Architecture (film)_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_condition(air/N2_liquid)_daysNo-"days"_MeasNo_frequencyNo-frequencyUnits`

`batchNo_sampleNo_Method (NMR/SSNMR)_Architecture (solvent)_solvent_condition(air/N2_liquid)_daysNo-"days"_MeasNo_frequencyNo-frequencyUnits`

**Mass Spectrometry**:
`batchNo_sampleNo_Method (LCMS/GCMS)_SampleType(Priming Waste/Experiment Waste)_Architecture (film)_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_condition(air/N2_liquid)_daysNo-"days"_MeasNo_IonMeasurementMode(Pos/Neg)_LowEnergy/HighEnergy Channel (LE/HE)`

**Profilometer**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo_length(No-units)_condition(air/N2_liquid)_daysNo-"days"_workbooktype(T-O)_MeasNo_MeasurementMode_IntegrationTime`

`material_sampleNo_deviceNo_MeasNo_length(No-units)_duration(No-units)_force(No-units)_thickness(No-units)`