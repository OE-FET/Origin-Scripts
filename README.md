# Origin-Scripts
LabTalk Scripts for OFET and EG-OFET analysis. Compatible with Origin 2018 Pro or newer.

## Filename templates:
- Each filename template consists of different *fields*, separated by an *underscore* (`_`).
- **NOTE**: Units are separated by a dash (e.g. 20-nm).

**OFET (Transfer/Output)**:
batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo_length(No-units)_condition(air/N2_liquid)_daysNo-"days"_MeasurementType(T-O)_MeasNo_MeasurementMode_IntegrationTime

**OFET (C-V, C-f)**:
batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo/CapNo (e.g. Cap-2)_condition(air/N2_liquid)_daysNo-"days"_MeasurementType(C-f)_MeasNo_Vdc

batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo/CapNo_condition(air/N2_liquid)_daysNo-"days"_MeasurementType(C-V)_MeasNo_frequencyNo-frequencyUnits_Forward/Reverse sweep

**OFET (stability)**:
batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo_length(No-units)_"step"_stepNo_condition(air/N2_liquid)_minutesNo-"min"_MeasurementType(T-O)_MeasNo_MeasurementMode_IntegrationTime

**OFET (Cryo)**:
batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo_length(No-units)_condition(air/N2_liquid)_daysNo-"days"_stepNo_air/vac_Temperature_MeasurementType(T-O)_MeasNo_MeasurementMode_IntegrationTime

**OFET (Bias Stress)**:
batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo_length(No-units)_condition(air/N2_liquid)_daysNo-"days"_BiasStressTypeandNo(PBS1/NBS1)_minutesNo-"min"_MeasurementType(T-O-S)_MeasNo_MeasurementMode_IntegrationTime
 - **NOTE**: For "S" measurement type ("sample") the suffix is: S_Vg_Vd

**EG-OGET (stability)**:
batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_sampleNo_"step"_stepNo_condition(air/N2_liquid)_minutesNo-"min"_MeasurementType(T-O)_MeasNo_MeasurementMode_IntegrationTime

batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_sampleNo_"step"_stepNo_condition(air/N2_liquid)_daysNo-"days"_minutesNo-"min"_MeasurementType(T-O)_MeasNo_MeasurementMode_IntegrationTime

 - **EG-OGET Origin Template filename**:
batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_"cycle"_cycleLength (in mins)_"step"_stepNo_condition(air/N2_liquid)_daysNo-"days"_minutesNo-"min"_MeasurementType(T-O)_Vd/Vg_Vd/Vg value_Vd/Vg_Vd/Vg sweep limits

**CAS UV/VIS**:
batchNo_Method (CAS-UVVIS/UVVIS)_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_"step"_stepNo_condition(air/N2_liquid)_daysNo-"days"_Nomins-"min"_Pressure-units_Temperature-units_meas_MeasNo_MeasurementType(A-T-S-B)_"Vg"_Vg-V_"Vd"_Vd-V

**FTIR (film/CAS)**:
batchNo_Method (CAS-FTIR/FTIR)_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_stepNo_condition(air/N2_liquid)_daysNo-"days"_Nomins-"min"_Pressure-units_Temperature_MeasNo_MeasurementType(A-T-S-B)_Vg_Vd

**FTIR (EG-OFET)**:
batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_sampleNo_stepNo_condition(air/N2_liquid)_daysNo-"days"_Nomins-"min"_Pressure-units_Temperature_MeasNo_MeasurementType(A-T-S-B)_Vg_Vd

**Cyclic Voltammetry**:
FileNo_batchNo_Architecture_material_concentration/thickness_solvent-annealingT/time_additive_sampleNo_WE_CE_RE_liquid_"Start"_StartV_"Stop"_StopV_"V1"_LowerVertex_"V2"_UpperVertex_"step"_stepV_"rate"_Rate_"Scans"_ScanNo_MeasNo

**NMR**:
(data)
batchNo_sampleNo_Method (NMR/SSNMR)_Architecture (film)_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_condition(air/N2_liquid)_daysNo-"days"_MeasNo_frequencyNo-frequencyUnits

batchNo_sampleNo_Method (NMR/SSNMR)_Architecture (solvent)_solvent_condition(air/N2_liquid)_daysNo-"days"_MeasNo_frequencyNo-frequencyUnits


(exported data in csv format)
batchNo_sampleNo_Method (NMR/SSNMR)_Architecture (film)_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_condition(air/N2_liquid)_daysNo-"days"_MeasNo_frequencyNo-frequencyUnits_scale

batchNo_sampleNo_Method (NMR/SSNMR)_Architecture (solvent)_solvent_condition(air/N2_liquid)_daysNo-"days"_MeasNo_frequencyNo-frequencyUnits_scale


(exported image)
batchNo_sampleNo_Method (NMR/SSNMR)_Architecture (film)_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_condition(air/N2_liquid)_daysNo-"days"_frequencyNo-frequencyUnits_peak-"ppm"

batchNo_sampleNo_Method (NMR/SSNMR)_Architecture (solvent)_solvent_condition(air/N2_liquid)_daysNo-"days"_frequencyNo-frequencyUnits_peak-"ppm"


**Mass Spectrometry**:
batchNo_sampleNo_Method (LCMS/GCMS)_SampleType(Priming Waste/Experiment Waste)_Architecture (film)_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_condition(air/N2_liquid)_daysNo-"days"_MeasNo_IonMeasurementMode(Pos/Neg)_LowEnergy/HighEnergy Channel (LE/HE)


**Profilometer**:
batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo_length(No-units)_condition(air/N2_liquid)_daysNo-"days"_workbooktype(T-O)_MeasNo_MeasurementMode_IntegrationTime

material_sampleNo_deviceNo_MeasNo_length(No-units)_duration(No-units)_force(No-units)_thickness(No-units)
