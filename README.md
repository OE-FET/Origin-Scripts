# Origin-Scripts
LabTalk Scripts for OFET and EG-OFET analysis. Compatible with Origin 2018 Pro or newer.

## Installation:
To install the scripts perform the following steps:
1. Copy the `Scripts` folder to `C:\Users\<Account Name>\Documents\OriginLab`. Note that the filepath should **NOT** contain spaces!
2. Copy the `Themes` and `Templates` folders to `C:\Users\<Account Name>\Documents\OriginLab\User Files`.
3. Open OriginLab and select `View` and `Command Window`. The Command Window is similar to the Linux command line. It is where we can give commands directly to Origin.
4. Write `cd C:\Users\<Account Name>\Documents\OriginLab\Scripts` to set the Scripts directory as the working directory. Then press `ENTER`.
5. Now the scripts should be available. To verify this, start typing the name of a script in the command line (e.g. `EG`) and then press `TAB`. If the working directory has been set correctly, Origin will try to autocomplete the partially typed script name.

## Analysis of filename templates:
- Each filename template consists of different *fields*, separated by an *underscore* (``_``).

The different fields are:
- `batchNo`: The number of the batch.

### The `step` convention:

Some of the filename templates employ the `step` convention. This is simply a convention to keep filenames short while keeping track of the sequence of liquids that have been injected in an EG-OFET. Suppose, for example, that we inject first ultrapure water (10 minutes), then saline solution (5 hours), and then ultrapure water again (1 day). How would we distinguish the measurement files?

Let's suppose that we have finished the 10 minute ultrapure water injection and are 2 minutes in the saline solution experiment: One method is to name the file e.g. `<transistor parameters>`_`up-H2O`_`10-min`_`ss`_`2-min`_`<measurement parameters>`. But with such a convention the filenames would get very big, once we injected more and more liquids.

With the step convention, we would simply write `step`_`2`_`ss`_`2-min`. `Step 2` means that this liquid (saline solution in this case) is the **second** liquid to be injected and the rest of the filename (2-min) represents the **exposure time** in that specific liquid.

Since we mentioned that our sequence consists of ultrapure water (10 minutes), then saline solution (5 hours), and then ultrapure water again (1 day). The overall measurements would look like this (if we assume one measurement per minute):
- `<transistor parameters>`_`step`_`1`_`up-H2O`_`1-min`_`<measurement parameters>`
- `<transistor parameters>`_`step`_`1`_`up-H2O`_`2-min`_`<measurement parameters>`
- ...
- `<transistor parameters>`_`step`_`2`_`ss`_`1-min`_`<measurement parameters>`
- `<transistor parameters>`_`step`_`2`_`ss`_`2-min`_`<measurement parameters>`
- ...
- `<transistor parameters>`_`step`_`3`_`up-H2O`_`1-min`_`<measurement parameters>`
- ...


## Filename templates:
- **NOTE**: Units are separated by a dash (e.g. 20-nm).

**OFET (Transfer/Output)**:
`batchNo`_`Architecture`_`material`_`concentration(No-units)`_`solvent`_`annealing`_`additive (type-thickness-units)`_`dielectric`_`DielectricConcentration`_`sampleNo`_`deviceNo`_`length(No-units)`_`condition(air/N2`_`liquid)`_`daysNo-"days"`_`MeasurementType(T-O)`_`MeasNo`_`MeasurementMode`_`IntegrationTime`

**OFET (C-V, C-f)**:
`batchNo`_`Architecture`_`material`_`concentration(No-units)`_`solvent`_`annealing`_`additive (type-thickness-units)`_`dielectric`_`DielectricConcentration`_`sampleNo`_`deviceNo/CapNo (e.g. Cap-2)`_`condition(air/N2`_`liquid)`_`daysNo-"days"`_`MeasurementType(C-f)`_`MeasNo`_`Vdc`

`batchNo`_`Architecture`_`material`_`concentration(No-units)`_`solvent`_`annealing`_`additive (type-thickness-units)`_`dielectric`_`DielectricConcentration`_`sampleNo`_`deviceNo/CapNo`_`condition(air/N2`_`liquid)`_`daysNo-"days"`_`MeasurementType(C-V)`_`MeasNo`_`frequencyNo-frequencyUnits`_`Forward/Reverse sweep`

**OFET (shelf-life stability)**:
`batchNo`_`Architecture`_`material`_`concentration(No-units)`_`solvent`_`annealing`_`additive (type-thickness-units)`_`dielectric`_`DielectricConcentration`_`sampleNo`_`deviceNo`_`length(No-units)`_`"step"`_`stepNo`_`condition(air/N2`_`liquid)`_`minutesNo-"min"`_`MeasurementType(T-O)`_`MeasNo`_`MeasurementMode`_`IntegrationTime`

**OFET (Cryo)**:
`batchNo`_`Architecture`_`material`_`concentration(No-units)`_`solvent`_`annealing`_`additive (type-thickness-units)`_`dielectric`_`DielectricConcentration`_`sampleNo`_`deviceNo`_`length(No-units)`_`condition(air/N2`_`liquid)`_`daysNo-"days"`_`stepNo`_`air/vac`_`Temperature`_`MeasurementType(T-O)`_`MeasNo`_`MeasurementMode`_`IntegrationTime`

**OFET (Bias Stress)**:
`batchNo`_`Architecture`_`material`_`concentration(No-units)`_`solvent`_`annealing`_`additive (type-thickness-units)`_`dielectric`_`DielectricConcentration`_`sampleNo`_`deviceNo`_`length(No-units)`_`condition(air/N2`_`liquid)`_`daysNo-"days"`_`BiasStressTypeandNo(PBS1/NBS1)`_`minutesNo-"min"`_`MeasurementType(T-O-S)`_`MeasNo`_`MeasurementMode`_`IntegrationTime`
- **NOTE**: For `S` measurement type (`sample`) the suffix is: `S`_`Vg`_`Vd`

**EG-OGET (sensing-cycling)**:
`batchNo`_`Architecture`_`material`_`concentration(No-units)`_`solvent`_`annealing`_`additive (type-thickness-units)`_`sampleNo`_`condition(air/N2`_`liquid)`_`daysNo-"days"`_`MeasurementType(I-vs-time`_`plunger,valve-port)`

`batchNo`_`Architecture`_`material`_`concentration(No-units)`_`solvent`_`annealing`_`additive (type-thickness-units)`_`sampleNo`_`"step"`_`stepNo`_`condition(air/N2`_`liquid)`_`minutesNo-"min"`_`MeasurementType(T-O)`_`MeasNo`_`MeasurementMode`_`IntegrationTime`

**EG-OGET (operational stability)**:
`batchNo`_`Architecture`_`material`_`concentration(No-units)`_`solvent`_`annealing`_`additive (type-thickness-units)`_`sampleNo`_`"step"`_`stepNo`_`condition(air/N2`_`liquid)`_`daysNo-"days"`_`MeasurementType(I-vs-time`_`plunger,valve-port)`

`batchNo`_`Architecture`_`material`_`concentration(No-units)`_`solvent`_`annealing`_`additive (type-thickness-units)`_`sampleNo`_`"step"`_`stepNo`_`condition(air/N2`_`liquid)`_`minutesNo-"min"`_`MeasurementType(T-O)`_`MeasNo`_`MeasurementMode`_`IntegrationTime`

**CAS UV/VIS**:
`batchNo`_`Method (CAS-UVVIS/UVVIS)`_`Architecture`_`material`_`concentration(No-units)`_`solvent`_`annealing`_`additive (type-thickness-units)`_`dielectric`_`DielectricConcentration`_`sampleNo`_`"step"`_`stepNo`_`condition(air/N2`_`liquid)`_`daysNo-"days"`_`Nomins-"min"`_`Pressure-units`_`Temperature-units`_`meas`_`MeasNo`_`MeasurementType(A-T-S-B)`_`"Vg"`_`Vg-V`_`"Vd"`_`Vd-V`

**FTIR (film/CAS)**:
`batchNo`_`Method (CAS-FTIR/FTIR)`_`Architecture`_`material`_`concentration(No-units)`_`solvent`_`annealing`_`additive (type-thickness-units)`_`dielectric`_`DielectricConcentration`_`sampleNo`_`stepNo`_`condition(air/N2`_`liquid)`_`daysNo-"days"`_`Nomins-"min"`_`Pressure-units`_`Temperature`_`MeasNo`_`MeasurementType(A-T-S-B)`_`Vg`_`Vd`

**FTIR (EG-OFET)**:
`batchNo`_`Architecture`_`material`_`concentration(No-units)`_`solvent`_`annealing`_`additive (type-thickness-units)`_`sampleNo`_`stepNo`_`condition(air/N2`_`liquid)`_`daysNo-"days"`_`Nomins-"min"`_`Pressure-units`_`Temperature`_`MeasNo`_`MeasurementType(A-T-S-B)`_`Vg`_`Vd`

**Cyclic Voltammetry**:
`FileNo`_`batchNo`_`Architecture`_`material`_`concentration/thickness`_`solvent-annealingT/time`_`additive`_`sampleNo`_`WE`_`CE`_`RE`_`liquid`_`"Start"`_`StartV`_`"Stop"`_`StopV`_`"V1"`_`LowerVertex`_`"V2"`_`UpperVertex`_`"step"`_`stepV`_`"rate"`_`Rate`_`"Scans"`_`ScanNo`_`MeasNo`

**NMR**:
`batchNo`_`sampleNo`_`Method (NMR/SSNMR)`_`Architecture (film)`_`material`_`concentration(No-units)`_`solvent`_`annealing`_`additive (type-thickness-units)`_`condition(air/N2`_`liquid)`_`daysNo-"days"`_`MeasNo`_`frequencyNo-frequencyUnits`

`batchNo`_`sampleNo`_`Method (NMR/SSNMR)`_`Architecture (solvent)`_`solvent`_`condition(air/N2`_`liquid)`_`daysNo-"days"`_`MeasNo`_`frequencyNo-frequencyUnits`

**Mass Spectrometry**:
`batchNo`_`sampleNo`_`Method (LCMS/GCMS)`_`SampleType(Priming Waste/Experiment Waste)`_`Architecture (film)`_`material`_`concentration(No-units)`_`solvent`_`annealing`_`additive (type-thickness-units)`_`condition(air/N2`_`liquid)`_`daysNo-"days"`_`MeasNo`_`IonMeasurementMode(Pos/Neg)`_`LowEnergy/HighEnergy Channel (LE/HE)`


**Profilometer**:
`batchNo`_`Architecture`_`material`_`concentration(No-units)`_`solvent`_`annealing`_`additive (type-thickness-units)`_`dielectric`_`DielectricConcentration`_`sampleNo`_`deviceNo`_`length(No-units)`_`condition(air/N2`_`liquid)`_`daysNo-"days"`_`workbooktype(T-O)`_`MeasNo`_`MeasurementMode`_`IntegrationTime`

`material`_`sampleNo`_`deviceNo`_`MeasNo`_`length(No-units)`_`duration(No-units)`_`force(No-units)`_`thickness(No-units)`