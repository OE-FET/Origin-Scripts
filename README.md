# Origin-Scripts
This repository contains LabTalk Scripts for automated data analysis of organic field-effect transistor (OFET) and electrolyte-gated organic field-effect transistor (EG-OFET) data. The scripts are compatible with `Origin 2018 Pro` or newer.

For these scripts to work:
- The data files need to have a certain internal structure, otherwise the scripts will not work properly. See the [Script examples](https://github.com/OE-FET/Origin-Scripts/blob/master/Examples/Script%20examples.opju) project file for examples. If the data were acquired using the [Agilent-415X](https://github.com/OE-FET/Agilent-415X) LabVIEW programs, then the scripts will work.
- The data filenames have to be in a certain format. If the format is different the scripts will not work. See the [Data filenames](#Data-filenames) section for details.  Alternatively, look at the [Script examples](https://github.com/OE-FET/Origin-Scripts/blob/master/Examples/Script%20examples.opju) project file and save your experimental data accordingly.
- The graph templates that the scripts use to plot the data have to have filenames in a certain format. If the format is different, or if the template does not exist, a generic template will be used but the scripts will work.

## Installation
To install the scripts, perform the following steps:
1. Copy the `Scripts` folder to `C:\Users\<Account Name>\Documents\OriginLab`.
	- The `<Account Name>` is the name of your user account in your PC.
	- The filepath should **NOT** contain *spaces*!
	- The filenames of the scripts should also **NOT** contain *dashes* (`-`), as Origin cannot handle them.
2. Copy the `Themes` and `Templates` folders to `C:\Users\<Account Name>\Documents\OriginLab\User Files`.
	- These folders contain the graph themes and templates.
	- In this case, there is no issue with *spaces* being in the filepath.
3. Copy the project files in the `Examples` folder into a folder of your choice.
	- The [Script examples](https://github.com/OE-FET/Origin-Scripts/blob/master/Examples/Script%20examples.opju) project file contains separate folders, each of whom tests a particular script. Each of these scripts only processes files that exist in the same folder as the script. All the scripts are tested in this file, except the `Experiment comparison` scripts.
	- The [Script examples - Experiment comparison](https://github.com/OE-FET/Origin-Scripts/blob/master/Examples/Script%20examples%20-%20EG-OFET%20Experiment%20comparison.opju) project file is only designed to test the `Experiment comparison` scripts, which compare experimental data between different EG-OFET experiments (and hence from different folders).
3. Run OriginLab and open the [Script examples](https://github.com/OE-FET/Origin-Scripts/blob/master/Examples/Script%20examples.opju) project file.
	- Each folder contains sample data and a readme file. The readme provides instructions on how to test the corresponding Origin script mentioned in the folder name.
4. Open the `Library` script with `CodeBuilder` and modify the `templatepath$` string variable at the top of the script, so that it points to the Templates folder in your PC. Make sure to save the script.
	- The `Library` is a script that contains functions that other scripts call. It will not meant to be executed by itself.
5. Select `View` and then `Command Window`. The Command Window will appear. It is similar to the Linux command line and allows one to give commands directly to Origin, and run scripts.
6. In the `Command Window`, write `cd C:\Users\<Account Name>\Documents\OriginLab\Scripts;` to set the Scripts directory as the working directory (do not forget the semicolon!). Then press `ENTER`.
7. Now the scripts should work. To verify this, start typing the name of a script in the `Command Window` (e.g. `EG`) and then press `TAB`. If the working directory has been set correctly, Origin will try to autocomplete the partially typed script name with one of the EG-OFET scripts.
8. Test a script by going to a folder of the `Script examples` project file, and following the instructions in the readme file. To run a script, type the name of the script in the `Command Window` and press `ENTER`.


## Data filenames

### Fields and their Values
As mentioned earlier, the scripts expect to see data filenames in a certain format. Each filename consists of different **fields**, separated by an *underscore* (`_`). The different **values** for each of the different **fields** can be seen in the `Library` file.
- Depending on the type of measurement, each filename will contain different fields. The [Filename formats](#filename-formats) section presents the different file formats for each type of measurement.
- When a script is executed, the data filename is parsed and the values of the different fields are used to select the appropriate graph template, as well as to calculate different performance metrics (e.g., mobility).

The different **fields** are:
- `batchNo`: The number of the batch (e.g. 235th).
- `Architecture`: The architecture of the device (e.g. TGBC-OFET, TGBC-EG-OFET).
- `material`: The semiconductor used to make the FET (e.g. IDTBT, C14-PBTTT).
- `concentration(No-units)`: The concentration of the material/semiconductor (e.g. `10-gl` means 10 g/L).
- `material`: The semiconductor used to make the FET (e.g. IDTBT, C14-PBTTT).
- `solvent`: The solvent(s) used to make the semiconductor solution (e.g. 100pDCB, 75pDCB-25pCF).
- `annealing`: The annealing conditions of the semiconductor thin films (e.g. 100C-1h).
- `additive`: The additives or self-assembled monolayers (SAMs) used in the FET (e.g. Pristine, TCNQ-40-nm).
	- For *evaporated* additives, the format is `AdditiveType-Thickness-Units` (e.g. TCNQ-20-nm).
	- For *solution-mixed* additives, the format is `AdditiveType-Concentration-Units` (e.g. `F4TCNQ-10-pww` means 10% w/w F4TCNQ).
- `dielectric`: The dielectric used to make the FET (e.g. Cytop-M, Cytop-S, PMMA).
	- For details, look inside the `Library` script.
	- **NOTE**: The `dielectric` field is used as an input in the `DielectricThicknessCalc` function of the `Library` script. The latter has some preset dielectric thicknesses that can be used to extract the mobility.
	- To set the dielectric thickness manually, open the script to be executed (not the `Library` script itself), comment out `DielectricThicknessCalc` function and set the `dielectricthickness` parameter manually.
- `DielectricConcentration`: The format is either in a `v/v ratio`, or in `g/L` (e.g. 3-1, 50-gl).
	- For details, look inside the `Library` script.
- `sampleNo`: The number of the sample (e.g. 1st).
- `step`: The step of the measurement (see the [Step Convention](#the-step-convention) section). **Applies to EG-OFETs only.**
- `stepNo`: The step number (e.g. 1, 2, 3...)
- `deviceNo`: The number of the device, if there are multiple devices on the same sample.
- `length`: The length of the device (distance between the source-drain electrodes). Usually a sample will contain FETs with different channel lengths.
	- The format is `No-Units` (e.g. 20-um).
	- The value of the `width` is a **parameter** that can be adjusted in the script itself. There is no field in the filename from which to extract this value.
- `condition(air/N2_liquid)`: The condition in which the sample has been stored (for shelf-life stability), or is being measured (for operational stability).
	- For example, `up-H2O` is used for ultrapure water, `ss` for saline solution, `1-x-PBS` for 1xPBS, `NaP` for NaP buffer. For details, look inside the `Library` script.
- `timelength`: The time that the sample has been stored or measured in the respective condition.
	- The format is `No-Units` (e.g. 20-days, 50-min, 3-h, or just "initial" for the initial day of measurements).
- `MeasurementType`: `T` for a transfer curve, `O` for an output curve, `S` for a sample (bias stress) measurement (for a `sample` measurement type (`S`), the suffix is: `S_Vg_Vd`).
	- For details, look inside the `Library` script.
- `MeasNo`: The measurement number (e.g. 1, 2, 3...). There could be multiple transfer curves taken at a specific time point, so it is necessary to number them.
- `MeasurementMode`: `C` for continuous mode, `P` for pulsed mode
- `IntegrationTime`: `S` for short, `M` for medium and `L` for large


### Filename formats
**OFET (Transfer-Output)**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo_length(No-units)_condition(air/N2_liquid)_timelength_MeasurementType(T-O)_MeasNo_MeasurementMode_IntegrationTime`

**OFET (shelf-life stability)**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo_length(No-units)_"step"_stepNo_condition(air/N2_liquid)_minutesNo-"min"_MeasurementType(T-O)_MeasNo_MeasurementMode_IntegrationTime`

**OFET (Cryo)**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo_length(No-units)_condition(air/N2_liquid)_timelength_stepNo_air/vac_Temperature_MeasurementType(T-O)_MeasNo_MeasurementMode_IntegrationTime`

**OFET (Bias Stress)**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_dielectric_DielectricConcentration_sampleNo_deviceNo_length(No-units)_condition(air/N2_liquid)_timelength_BiasStressTypeandNo(PBS1/NBS1)_minutesNo-"min"_MeasurementType(T-O-S)_MeasNo_MeasurementMode_IntegrationTime`

- **NOTE**: For `S` measurement type (`sample`) the suffix is: `S_Vg_Vd`.

**EG-OGET (sensing-cycling)**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_sampleNo_condition(air/N2_liquid)_timelength_MeasurementType(I-vs-time_plunger,valve-port)`

`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_sampleNo_"step"_stepNo_condition(air/N2_liquid)_minutesNo-"min"_MeasurementType(T-O)_MeasNo_MeasurementMode_IntegrationTime`

**EG-OGET (operational stability)**:
`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_sampleNo_"step"_stepNo_condition(air/N2_liquid)_timelength_MeasurementType(I-vs-time_plunger,valve-port)`

`batchNo_Architecture_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_sampleNo_"step"_stepNo_condition(air/N2_liquid)_minutesNo-"min"_MeasurementType(T-O)_MeasNo_MeasurementMode_IntegrationTime`

**NMR**:
`batchNo_sampleNo_Method (NMR/SSNMR)_Architecture (film)_material_concentration(No-units)_solvent_annealing_additive (type-thickness-units)_condition(air/N2_liquid)_timelength_MeasNo_frequencyNo-frequencyUnits`

`batchNo_sampleNo_Method (NMR/SSNMR)_Architecture (solvent)_solvent_condition(air/N2_liquid)_timelength_MeasNo_frequencyNo-frequencyUnits`


### The `step` convention
The `step` data filename convention is only used on EG-OFET data filenames. This convention aims to keep filenames short while keeping track of the sequence of liquids that have been injected in an EG-OFET. Suppose, for example, that we inject ultrapure water for the first 10 minutes, then saline solution for the next 5 hours, and then finally ultrapure water for the next day. Using the step convention, the data filenames would look like this (if we assume one measurement per minute):
- `<transistor parameters>_step_1_up-H2O_1-min_<measurement parameters>`
- `<transistor parameters>_step_1_up-H2O_2-min_<measurement parameters>`
- ...
- `<transistor parameters>_step_2_ss_1-min_<measurement parameters>`
- `<transistor parameters>_step_2_ss_2-min_<measurement parameters>`
- ...
- `<transistor parameters>_step_3_up-H2O_1-min_<measurement parameters>`
- ...


## Script parameters
**Parameters** are **variables** inside the scripts that need to be set up manually, using the `CodeBuilder`, before the script is run. These parameters are then used to extract various metrics, or to select the correct graph template. We mentioned earlier that the channel `width` and the `dielectricthickness` are parameters.
- If the parameters are set up incorrectly, the script will not run properly, or may miscalculate the performance metrics.

The parameters for each script are the following:


### OFET parameters
- `Width (W)` [um]: The transistor width.
- `Length (L)` [um]: The transistor length. This is not usually set manually, as a parameter, but is extracted from the `length` field of the filename. Usually a sample will contain FETs with different channel lengths.
- `Thickness of accumulation layer (d)` [nm]: The thickness of the accumulation layer.
- `Dielectric constant (er)`: The dielectric constant. Common values include `2.1` for `Cytop`, `3.6` for `PMMA` and `3.9` for `SiO2`.
- `Vacuum permittivity (e0)` [F/m]: The dielectric permittivity of vacuum is a constant with a value of `8.854E-12`.
- `Dielectric thickness` [nm]: The thickness of the dielectric. This parameter is not usually set manually. It is calculated using the `DielectricThicknessCalc` function, using the `dielectric` field as input.
	- **NOTE**: To set the dielectric thickness manually, open the script to be executed (not the `Library` script itself), comment out `DielectricThicknessCalc` function and set the `dielectricthickness` variable manually.
- `Dielectric capacitance (Ci)` [F/m<sup>2</sup>]: The capacitance per unit area of the dielectric. This is calculated by the dielectric thickness and the dielectric constant, using the formula `Ci=e0*er/(dielectricthickness*10^(-9)`.
	- **NOTE**: This is not completely correct. In reality, there is metal penetration inside the dielectric during the gate evaporation. Hence, we should not take into account the entire dielectric thickness into account when calculating the dielectric capacitance. The correct way to extract the dielectric capacitance is to measure it with an impedance analyzer by short-circuiting the source and drain. Typical values for dielectrics are `3.2` nF/cm<sup>2</sup> for `Cytop` and `5.2` nF/cm<sup>2</sup> for `PMMA`. From that capacitance, we can extract the effective dielectric thickness, compare it with the measured dielectric thickness and estimate the extent of metal penetration into the dielectric.


#### Mobility correction (power law fit) parameters
There is a [publication](https://aip.scitation.org/doi/10.1063/1.4876057) that demonstrates how to extract the contact and channel resistance as a function of the gate voltage from the transfer curves, in the linear and saturation regime. The process involves fitting a exponential function to the mobility curve. The fit is performed using the `PowerLawFit` function that can be called from the `Library` script.
- To disable this fit, open the script to be executed (not the `Library` script itself), and comment out the line calling the `PowerLawFit` function.

The parameters of the `PowerLawFit` function are:

- `minRangeLengthPLF` [V]: The minimum voltage range over which the fit will be applied. The fit starts at least `minRangeLengthPLF` volts after the `Vgmax` point. This number is usually `20`.
- `errorwindowPLF`: The maximum Root Mean Square Error allowed for the fit. If the error is larger than this value then the voltage range in which the fit is performed increases by a single row. Hence, this error defines the "window" for performing the power law fit. The default value is usually `0.001`.


#### Rolling Regression (Vt fit) parameters
The `RollingRegression` function is used to to calculate the threshold voltage (`Vt`). It performs a linear fit on part of the drain current (`Id`) curve, or the <span>&#8730;</span>Id curve, and finds the intercept of the fit with the voltage (horizontal) axis. This intercept is `Vt`.

The question is how to determin the voltage range on which to perform the linear fit. One option would be to choose two fixed limits on the voltage axis (e.g. a 20 Volt range) and perform the linear fit in those. If the fit is not good, we can manually change the fit limits and try again. Instead of doing this, the `RollingRegression` function tries to find the maximum voltage range it can perform a linear fit on before the slope of the linear fit surpasses a pre-determined value that is set in the `slopewindow` parameter.

- In our FET measurements, the transfer curves are usually scanned with a forward and reverse sweep. Hence, in a typical transfer curve data file, Vgmax would be at `maxrows/2` (the middle row) and the minimum voltages would be at the first and last rows and (row `0` and `maxrows`). The script performs the linear fit in the **reverse sweep** (i.e. after `Vg` has reached `Vgmax` and is reducing).
- The function initially sets the left voltage limit for the linear fit at `Vgmax` (or `Vgmax+offset`) and the right voltage limit at a distance of `minRangeLength` volts away (usually 20 V). The right voltage limit is at a gate voltage `Vg(i)`, where `i` is the corresponding row number. `i` is larger than `maxrows/2` because we are fitting in the reverse sweep. A linear fit is performed at that range, and the value of the slope is stored inside the variable `firstfit`, since it is the first linear fit performed.
- The left limit is held constant at `Vgmax` (or `Vgmax + offset`), while the right limit is iteratively moving further away from it (the row number `i` increases by `1` with each iteration). At every iteration, a new linear fit is performed in the new voltage range. The percentage difference between the new slope and `firstfit` is then calculated and compared against `slopewindow`. If it is smaller than `slopewindow` the iteration continues, with the right voltage limit moving further away, and a new linear fit being made. Once the percentage difference surpasses `slopewindow`, the fitting stops, and we know the voltage range over which to perform the linear fit.
- The iteratively increasing row index `i` determines the right voltage limit for the linear fit. With each iteration, the right limit moves from the minimum `i` value `wks.maxrows/2 + minRangeLength` (or `wks.maxrows/2 + minRangeLength + offset`) to the end of the voltage range (`maxrows`). Moving `i` to the right (via the loop) constitutes the *rolling window*.

So far two **parameters** have been introduced:

- `minRangeLength` [V]: The minimum voltage range (not row index range!) over which the linear fit will be applied. The more curvy the `Id/SQRT(Id)` curve, the smaller `minRangeLength` has to be, in order to be able to fit linearly. Typical values are between `5` and `20`, with the default values being `5` for the Linear and `10` for the Saturation regime.

- `slopewindow` [%]: The percentage of slope variation (relative to the first linear fit) allowed before the final linear fit is performed. This is the "window" for performing the rolling regression. The default values are `1.2` for the Linear and `3` for the Saturation regime.


#### The `offset` parameters

##### Offset 1: When `Idmax` and `Vgmax` do not coincide
There are cases cases when taking the left integration limit to be `Vgmax` will cause the Rolling Regression to fail. One such case is when `Idmax` and `Vgmax` do not coincide. This is presented in the following image:

<img src="https://github.com/OE-FET/Origin-Scripts/blob/master/Images/Vtlin%20offset%201%20wrong%20fit.png" alt="VtlinOffset1WrongFit" height="400">

In this case, `Idmax` occurs at a different voltage: `Vg(Idmax)`. After `Vg(Idmax)`, the slope of the `dId/dV` curve turns positive and the resulting linear fit rises to infinity.

To correct for this, the `RollingRegression` function checks if `Idmax` and `Vgmax` coincide. If not, the left voltage limit for the linear fit is shifted from `Vgmax` to `Vgmax+offset1`, where `offset1` is the voltage difference between `Vgmax` and `Vg(Idmax)`. The left the left voltage limit will then start from `Vg(Idmax)`.

`offset1` ensures that the linear fit is applied at the voltage range around `Idmax`. This is presented in the following image:

<img src="https://github.com/OE-FET/Origin-Scripts/blob/master/Images/Vtlin%20offset%201%20correct%20fit.png" alt="VtlinOffset1CorrectFit" height="400">

##### Offset 2: When `gm,max` and `Vgmax` do not coincide
Another case when the Rolling Regression can fail is when `gm,max` and `Vgmax` do not coincide. This is presented in the following images:

<img src="https://github.com/OE-FET/Origin-Scripts/blob/master/Images/Vtlin%20offset%202%20wrong%20fit.png" alt="VtlinOffset2WrongFit" height="400">

<img src="https://github.com/OE-FET/Origin-Scripts/blob/master/Images/Vtsat%20offset%202%20wrong%20fit.png" alt="VtsatOffset2WrongFit" height="400">

In this case, `gm,max` occurs at a different voltage: `Vg(gm,max)`. The `RollingRegression` function will start at `Vgmax` (60 Volts), perform the first linear fit over a voltage range of `minRangeLength` Volts and then keep iterating until the slope diverges. The divergence happens roughly ~15 Volts away from `Vgmax`, at 45 Volts. However, this linear fit obviously underestimates the threshold voltage `Vt`. This is because beyond the 45 Volts the slope (i.e. the transconductance, `gm`) becomes even larger. The maximum value of the slope (and of the transconductance) occurs at roughly 35 Volts, and this is where the linear fit should have taken place.

To correct for this, the `RollingRegression` function checks if `gm,max` and `Vgmax` coincide. If not, the left voltage limit for the linear fit is shifted from `Vgmax` to `Vgmax+offset2`, where `offset2` is the voltage difference between `Vgmax` and `Vg(gm,max)`. The left the left voltage limit will then start from `Vg(gm,max)`.

`offset2` ensures that the linear fit is applied at the voltage range around `gm,max`. This is presented in the following image:

<img src="https://github.com/OE-FET/Origin-Scripts/blob/master/Images/Vtlin%20offset%202%20correct%20fit.png" alt="VtlinOffset2CorrectFit" height="400">


##### Summary
The `RollingRegression` function checks and applies both offsets, first `offset1` and then `offset2`. If `offset1` is not accounted for, then there is a chance that `Vg(gm,max)` is occuring before `Idmax`. The offsets are summed into a new variable (`offset = offset1 + offset2`) and the left voltage limit for the linear fit is shifted from `Vgmax` to `Vgmax+offset`.

The offset-related **parameters** are:

- `autooffset`: Setting it to `1` will enable the automatic offset calculation. Setting it to `0` will disable it.
- `offset`: Manual setting of the offset value, in case `autooffset` is set to `0`. Common offset values include `20` for `IDTBT` and `0` for `N2200`. The default value is `10`. If `autooffset` is set to `1` this parameter is meaningless, as the offset will be automatically determined.


#### Reliability factor
The **reliability factor** is calculated as described in the relevant [publication](https://www.nature.com/articles/nmat5035). A point of uncertainty exists with respect to the denominators of formulas (B1) and (B2) of the publication. The denominators are the transconductances, which depend on the gate voltage. The question is, which value of the transconductance should be used for the calculation of the reliability factor. We could use the maximum transconductance (g<sub>m, max</sub>), or the transconductance at the maximum gate voltage (g<sub>m, VGmax</sub>). The scripts have been coded to use the maximum transconductance (g<sub>m, max</sub>) value (which may not be at the maximum gate voltage), thus calculating a worst case scenario for the reliability factor.


### EG-OFET parameters

#### Steady state time `Tss`
- To be completed.