# Origin-Scripts
LabTalk Scripts for automated data analysis of OFET and EG-OFET data, measured with the [Agilent-415X](https://github.com/OE-FET/Agilent-415X) LabVIEW programs. Compatible with Origin 2018 Pro or newer.

## Installation
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
4. Open the `Library` script with `CodeBuilder` and modify the `templatepath$` string variable at the top of the script, so that it points to the Templates folder in your PC. Make sure to save the script.
	- The `Library` is a script that contains functions that other scripts call. It will not meant to be executed.
5. Select `View` and then `Command Window`. The Command Window will appear. It is similar to the Linux command line and allows one to give commands directly to Origin, and run scripts, among other things.
6. In the `Command Window`, write `cd C:\Users\<Account Name>\Documents\OriginLab\Scripts;` to set the Scripts directory as the working directory (do not forget the semicolon!). Then press `ENTER`.
7. Now the scripts should be available. To verify this, start typing the name of a script in the `Command Window` (e.g. `EG`) and then press `TAB`. If the working directory has been set correctly, Origin will try to autocomplete the partially typed script name with one of the EGOFET scripts.
8. Test a script by going to a subfolder of the `Script examples` project file and following the instructions on the readme file. Usually all that is needed is to type the name of the corresponding script in the `Command Window` and press `ENTER`.


## Analysis of filename templates

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
	- **NOTE**: The `dielectric` field is used as an input in the `DielectricThicknessCalc` function of the `Library` script. The latter has some preset dielectric thicknesses that can be used to extract the mobility. To set the dielectric thickness manually, open the script to be executed (not the `Library` script itself), comment out `DielectricThicknessCalc` function and set the `dielectricthickness` variable manually.
- `DielectricConcentration`: The format is either in a v/v ratio, or in g/l (e.g. 3-1, 50-gl).
	- For details, look inside the `Library` script.
- `sampleNo`: The number of the sample (e.g. 1st).
- `step`: The step of the measurement (see the [Step Convention](#StepConvention) section).
- `stepNo`: The step number (e.g. 1, 2, 3...)
- `deviceNo`: The number of the device, if there are multiple devices on the same chip.
- `length`: The length of the device (distance between the source-drain electrodes).
	- The format is `No-Units` (e.g. 20-um).
- `condition(air/N2_liquid)`: The condition in which the sample has been stored (for shelf-life stability), or is being measured (for operational stability).
	- Example: `up-H2O` is used for ultrapure water, `ss` for saline solution, `1-x-PBS` for 1xPBS, `NaP` for NaP buffer.
	- For details, look inside the `Library` script.
- `timelength`: The time that the sample has spent in the respective condition.
	- The format is `No-Units` (e.g. 20-days, 50-min, 3-h, or just "initial" for the initial day of measurements)
- `MeasurementType`: `T` for a transfer curve, `O` for an output curve, `S` for a sample (bias stress) measurement (for a `sample` measurement type (`S`), the suffix is: `S_Vg_Vd`).
	- For details, look inside the `Library` script.
- `MeasNo`: The measurement number (e.g. 1, 2, 3...)
- `MeasurementMode`: `C` for continuous mode, `P` for pulsed mode
	- For details, look inside the `Library` script.
- `IntegrationTime`: `S` for short, `M` for medium and `L` for large
	- For details, look inside the `Library` script.


### The `step` convention<a name="StepConvention"></a>
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


## Script parameters
**Parameters** are **variables** inside the scripts that need to be set up manually, using the `CodeBuilder`, before the script is run. These parameters are then used to extract various metrics (e.g. the dielectric thickness parameter is used to extract the mobility), or to select the correct template. If the parameters are set up incorrectly, the script may also not run properly. The parameters for each script are the following:


### OFET parameters
- `Width (W)` [um]: The transistor width.
- `Length (L)` [um]: The transistor length. This is not usually set manually, as a parameter, but is extracted from the `length` field of the filename. This is because a sample can contain devices of different lengths.
- `Thickness of accumulation layer (d)` [nm]: The thickness of the accumulation layer.
- `Dielectric constant (er)`: The dielectric constant. Common values include `2.1` for `CYTOP`, `3.6` for `PMMA` and `3.9` for `SiO2`.
- `Vacuum permittivity (e0)` [F/m]: The dielectric permittivity of vacuum is a constant with a value of `8.854E-12`.
- `Dielectric thickness` [nm]: The thickness of the dielectric. This parameter is not usually set manually. It is calculated using the `DielectricThicknessCalc` function, using the `dielectric` field as input.
	- **NOTE**: To set the dielectric thickness manually, open the script to be executed (not the `Library` script itself), comment out `DielectricThicknessCalc` function and set the `dielectricthickness` variable manually.
- `Dielectric capacitance (Ci)` [F/m<sup>2</sup>]: The capacitance per unit area of the dielectric. This is calculated by the dielectric thickness and the dielectric constant, using the formula `Ci=e0*er/(dielectricthickness*10^(-9)`. However, this is not completely correct. In reality, there is metal penetration inside the dielectric during the gate evaporation. Hence, we should not take into account the entire dielectric thickness into account when calculating the dielectric capacitance. The correct way to extract the dielectric capacitance is to measure it with an impedance analyzer by short-circuiting the source and drain. Typical values for dielectrics are `3.2` nF/cm<sup>2</sup> for `CYTOP` and `5.2` nF/cm<sup>2</sup> for `PMMA`. From that capacitance, we can extract the effective dielectric thickness, compare it with the measured dielectric thickness and estimate the extent of metal penetration into the dielectric.


#### Mobility correction (power law fit) parameters
There is a [publication](https://aip.scitation.org/doi/10.1063/1.4876057) that demonstrates how to calculate the gate voltage dependent contact and channel resistance from the transfer curves, in the linear and saturation regime. The process involves fitting a exponential function into the mobility curve. The fit is performed using the `PowerLawFit` function that can be called from the `Library` script. To disable the power law fit, open the script to be executed (not the `Library` script itself), and comment out the line calling the `PowerLawFit` function. If performing The parameters of the `PowerLawFit` function are:

- `minRangeLengthPLF` [V]: The minimum voltage range over which the Power Law fit will be applied. The fit starts at least `minRangeLengthPLF` volts after the `Vgmax` point. This number is usually `20`.
- `errorwindowPLF`: The maximum Root Mean Square Error allowed for the power law fit to be performed. If the error is larger than this value then the voltage range in which the fit is performed increases by a single row. Hence, this error defines the "window" for performing the power law fit. The default value is usually `0.001`.


#### Rolling Regression (Vt fit) parameters
The `RollingRegression` function is used to perform a linear fit on part of the `Id` (or <span>&#8730;</span>Id) curve in order to calculate the threshold voltage `Vt`, which is the intercept of the fit with the voltage (horizontal) axis. One option would be to choose two fixed limits on the voltage axis (e.g. a 20 Volt range) and perform the linear fit in those. If the fit was not good, we would manually change the fit limits and try again.

Instead, the `RollingRegression` function tries to find the maximum voltage range it can perform a linear fit on before the slope of the linear fit surpasses a pre-determined **parameter** called `slopewindow`.

- Note that in our measurements, the transfer curves are usually scanned with a forward and reverse sweep. Hence, in a typical transfer curve data file, Vgmax would be at `maxrows/2` (the middle row) and the minimum voltages would be at the first and last rows and (row `0` and `maxrows`). We perform the linear fit in the reverse sweep (i.e. after `Vg` has reached `Vgmax` and is reducing).	
- The function initially sets the left voltage limit for the linear fit at `Vgmax` (or `Vgmax+offset`) and the right voltage limit at a distance of `minRangeLength` volts away, at a value `Vg(i)`, where `i` is the row number corresponding to that particular row number `Vg(i)`. `i` is larger than `maxrows/2` because we are fitting in the reverse sweep. A linear fit is performed at that range, with the value stored inside the variable `firstfit`, since it is the first linear fit performed.	
- The left limit is held constant at `Vgmax` (or `Vgmax + offset`), while the right limit is iteratively moving further away from the first one (the row number `i` increases by `1` with each iteration). After every iteration, a new linear fit is performed in the new voltage range. This limit moving and linear re-fitting continues until the slope of the linear fit diverges beyond a certain percentage from the original `firstfit`. The divergence limit is called `slopewindow` and it is a **parameter** to be set in the script.	
- Hence, the row index `i` shows the right limit of the voltage range for the linear fit. The iterative loop extends from the minimum `i` value `wks.maxrows/2 + minRangeLength` (or `wks.maxrows/2 + minRangeLength + offset`) to the end of the voltage range (`maxrows`). Moving `i` to the right (via the loop) constitutes the *rolling window*.

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


### EGOFET parameters

#### Steady state time `Tss`
- To be completed.


## Filename templates:<a name="FilenameTemplates"></a>
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