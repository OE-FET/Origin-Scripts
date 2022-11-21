# Origin-Scripts
LabTalk Scripts for automated data analysis of OFET and EG-OFET data, measured with the [Agilent-415X](https://github.com/OE-FET/Agilent-415X) LabVIEW programs. Compatible with Origin 2018 Pro or newer.

## Installation:
To install the scripts, perform the following steps:
1. Copy the `Scripts` folder to `C:\Users\<Account Name>\Documents\OriginLab`.
	- The filepath should **NOT** contain *spaces*!
	- The filenames of the scripts should also **NOT** contain *dashes* (`-`), as Origin cannot handle them.
2. Copy the `Themes` and `Templates` folders to `C:\Users\<Account Name>\Documents\OriginLab\User Files`.
	- In this case, there is no issue with *spaces* being in the filepath.
3. Copy the project files in the `Examples` folder into a folder of your choice.
	- The `Script examples` project file contains subfolders, each of which is designed to test the corresponding Origin script mentioned in the subfolder name. All scripts are tested in this file, except the `Experiment comparison` scripts.
	- The `Script examples - Experiment comparison` project file is only designed to test the `Experiment comparison` scripts, which compare experimental data between different experiments (and hence from different subfolders).
3. Run OriginLab and open the `Script examples` project file.
	- Each subfolder of the project file contains sample data and a readme file. The readme provides instructions on how to test the corresponding Origin script mentioned in the subfolder name.
4. Open the `Library` script with `CodeBuilder` and modify the `templatepath$` string variable at the top of the script. Make sure to save the script.
	- The `Library` is a script that contains functions that other scripts call. It will not meant to be executed.
5. Select `View` and then `Command Window`. The Command Window will appear. It is similar to the Linux command line and allows one to give commands directly to Origin, and run scripts, among other things.
6. In the `Command Window`, write `cd C:\Users\<Account Name>\Documents\OriginLab\Scripts;` to set the Scripts directory as the working directory (do not forget the semicolon!). Then press `ENTER`.
7. Now the scripts should be available. To verify this, start typing the name of a script in the `Command Window` (e.g. `EG`) and then press `TAB`. If the working directory has been set correctly, Origin will try to autocomplete the partially typed script name.
8. Test a script by going to a subfolder of the `Script examples` project file and following the instructions on the readme file. Usually all that is needed is to type the name of the corresponding script in the `Command Window` and press `ENTER`.

## Analysis of filename templates:


### Fields and their Values
Each filename template consists of different **fields**, separated by an *underscore* (`_`). The different **values** for each of the different **fields** can be seen in the `Library` file.
	- Not all filename templates contain all available fields. To determine the format of each type of measurement, see the [Filename templates](#FilenameTemplates) section.

The different **fields** are:
- `batchNo`: The number of the batch (e.g. 235th).
- `Architecture`: The architecture of the device (e.g. TGBC-OFET, TGBC-EGOFET).
- `material`: The semiconductor used to make the FET (e.g. IDTBT, C14-PBTTT).
- `concentration(No-units)`: The concentration of the material/semiconductor (e.g. 10-gl).
- `material`: The semiconductor used to make the FET (e.g. IDTBT, C14-PBTTT).
- `solvent`: The solvent(s) used to make the semiconductor solution (e.g. 100pDCB, 75pDCB-25pCF).
- `annealing`: The annealing conditions of the semiconductor thin films (e.g. 100C-1h).
- `additive`: The additives or SAMs used to in the FET (e.g. Pristine, TCNQ-40-nm).
	- For *evaporated* additives, the format is `AdditiveType-Thickness-Units` (e.g. TCNQ-20-nm).
	- For *solution-mixed* additives, the format is `AdditiveType-Concentration-Units` (e.g. F4TCNQ-10-pww, which will be converted to 10% w/w F4TCNQ).
- `dielectric`: The dielectric used to make the FET (e.g. Cytop-M, PMMA).
	- For details, look inside the `Library` script.
- `DielectricConcentration`: The format is either in a v/v ratio, or in g/l (e.g. 3-1, 50-gl).
	- For details, look inside the `Library` script.
- `sampleNo`: The number of the sample (e.g. 1st).
- `step`: The step of the measurement (see the [Step Convention](#StepConvention) section).
- `stepNo`: The step number (e.g. 1, 2, 3...)
- `deviceNo`: The number of the device, if there are multiple devices on the same chip.
- `length`: The length of the device (distance between the source-drain electrodes).
	- The format is `No-Units` (e.g. 20-um).
- `condition(air/N2_liquid)`: 
- `timelength`: The time that the sample has spent in the respective condition.
	- The format is `No-Units` (e.g. 20-days, 50-min, 3-h, or just "initial" for the initial day of measurements)
- `MeasurementType`: `T` for a transfer curve, `O` for an output curve, `S` for a sample (bias stress) measurement (for a `sample` measurement type (`S`), the suffix is: `S_Vg_Vd`).
	- For details, look inside the `Library` script.
- `MeasNo`: The measurement number (e.g. 1, 2, 3...)
- `MeasurementMode`: `C` for continuous mode, `P` for pulsed mode
	- For details, look inside the `Library` script.
- `IntegrationTime`: `S` for short, `M` for medium and `L` for large
	- For details, look inside the `Library` script.


### The `step` convention:<a name="StepConvention"></a>

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


## Filename templates:<a name="FilenameTemplates"></a>

**OFET (Transfer-Output)**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo_length(No-units)_condition(air/N2_liquid)_timelength_MeasurementType(T-O)_MeasNo_MeasurementMode_IntegrationTime`

**OFET (shelf-life stability)**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo_length(No-units)_"step"_stepNo_condition(air/N2_liquid)_minutesNo-"min"_MeasurementType(T-O)_MeasNo_MeasurementMode_IntegrationTime`

**OFET (Cryo)**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo_length(No-units)_condition(air/N2_liquid)_timelength_stepNo_air/vac_Temperature_MeasurementType(T-O)_MeasNo_MeasurementMode_IntegrationTime`

**OFET (Bias Stress)**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo_length(No-units)_condition(air/N2_liquid)_timelength_BiasStressTypeandNo(PBS1/NBS1)_minutesNo-"min"_MeasurementType(T-O-S)_MeasNo_MeasurementMode_IntegrationTime`
	- **NOTE**: For a `sample` measurement type (`S`), the suffix is: `S_Vg_Vd`

**EG-OGET (sensing-cycling)**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_sampleNo_condition(air/N2_liquid)_timelength_MeasurementType(I-vs-time_plunger,valve-port)`

`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_sampleNo_"step"_stepNo_condition(air/N2_liquid)_minutesNo-"min"_MeasurementType(T-O)_MeasNo_MeasurementMode_IntegrationTime`

**EG-OGET (operational stability)**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_sampleNo_"step"_stepNo_condition(air/N2_liquid)_timelength_MeasurementType(I-vs-time_plunger,valve-port)`

`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_sampleNo_"step"_stepNo_condition(air/N2_liquid)_minutesNo-"min"_MeasurementType(T-O)_MeasNo_MeasurementMode_IntegrationTime`

**NMR**:
`batchNo_sampleNo_Method (NMR/SSNMR)_Architecture (film)_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_condition(air/N2_liquid)_timelength_MeasNo_frequencyNo-frequencyUnits`

`batchNo_sampleNo_Method (NMR/SSNMR)_Architecture (solvent)_solvent_condition(air/N2_liquid)_timelength_MeasNo_frequencyNo-frequencyUnits`