# RF Power Measurements Using Scope

<p align="center">
<img src="./img/measurement_hints.jpg" width="1000" height="600"/>
</p> 

## Setup overview

When using osciloscope to measure output power we rather measure voltage level on known resistive dummy load (i.e. load with low reactance) and then use simple formulas to convert obtained results to power in Watts or dBm's.

Prior to measuring power levels exceeding a few dBm's always consult scope manaual and confirm maximum input voltage allowed by your probing system!

Typically maximum DC + AC peak value shall not be lower than 300 Volts making a probe with short ground connector absolutely fine to use to measure voltage levels directly on the dummy load (at least for signal frequencies within HF band).

However due to authors personal experience with damaged scope during such measurements, alternative setup utilizing rf tap has been described below.

It shall be aslo stressed that to obtain results, which can be easily interpreted or compared with formal specifications, we shall always use sinusoidal waveform stimulus.

In case of HF transceivers, this requires switching to CW mode - for commonly used SSB emissions carrier has been surpressed. 

Moreover when using osiloscope to measure output power it is very important to ensure that both output of the device under test (on the picture above we use functional generator) and the output of the rf tap/attenuator are matched to 50ohm. Most budget scopes will have input impedance in the range of 1Mohm, that is why we cannot connect such scope directly to rf tap output. Instead we must use tee adapter with one of the outputs termninated with 50ohm low power dummy load (see hint #2) and another one connected to our osciloscope. 

In the scenario when the scope input impedance can be set to 50ohm, we can connect the scope directly to rf tap/attenuator input. In this configuration it is important to check scopes manual for maximum allowed input voltage, which internal 50ohm terminating resistor can sustain.

Most functional generators do have option to select device output impedance, in our case we need to set it to 50ohm. Connecting osciloscope to the output of the functional generator does not impact measurement results since again input impedance of the scope is in the range of 1Mohm i.e. is much greater then output impedane of the generator or device under test (see hint #1). Of course we assume that dummy load, attenduator/rf tap, and cabeling are all 50ohm rated as well.

Characteristics of the load used during power measurments can heavily impact results! That is why even the best antenna cannot replace the proper dummy load!

When measuring high power levels (above 50W/47dBm), limit the time dummy load is subjected to such power. Prolonged exposure to high power levels do impact dummy load characteristics and ultimately may cause permanent damage.

## Useful formulas

### Voltage

<p align="center">
<img src="./img/Sinwave.jpg" width="600" height="400"/>
</p>

where:

$Upp$ - Peak to Peak Voltage

$Up$ - Peak Voltage

$Up = Upp / 2$

Both, $Up$ and $Upp$ can be directly read from the scope screen.

$Urms$ - Root Mean Square Voltage 

For a periodic AC voltage, the $Urms$ value represents the equivalent DC voltage that would deliver the same amount of power to a resistive load.

In case of sinusoidal waveform specifically, the RMS voltage can be expressed as:

$Urms = \frac{Up}{\sqrt{2}}$        (*)

### Power

Power is the measure  of the energy per unit of time transfered by the electric circuit and can be expressed as:

$P=U \times I$      (**)

From Ohm's law:

$R=U \times I$, 

which gives:

$I= U \times R$ 

If we substitute I in equation (**) with equation for I derived from Ohm's law above, we get the formula expressing power dicipating on the load R:

$P = \frac{U^2}{R}$

In RF measurements we sometime discuss peak power, which is the maximum instantaneous power that a system can deliver or consume at any given moment:

$Pp = \frac{Up^2}{R}$

and even more often average power, representing the consistent power output or consumption over time. 

In case of sine waveform, average power can be expressed as:

$Pavg = \frac{Urms^2}{R}$

$Urms$ is not directly visible on the screen of osciloscope. That is why we need to convert above formula by substituting $Urms$ with formula (*), which gives:

$Pavg = P = \frac{Up^2}{2 \times R}$

RF Power Output listed in radio equipment specifications refers to average power. That is why for simplicity we will refer to average power as power.

### Finding peak voltage coresponding to given power Level 

Other useful equations, allow to find peak voltage $Up$ on load R for given power or peak power level:

$Up = \sqrt{2 \times P \times R}$

or

$Up = \sqrt{Pp \times R}$


### Converting power between dBm and Watts

To convert power given in dBm to Watts we use:

$P[W] = 10^{\left(\frac{P[dBm]}{10} - 3\right)}$

To convert power given in Watts to dBm we use:

$P[dBm] = 10 \times \log{\frac{P[W]}{0,001}}$


## Measurement example

We measure sinusoidal waveform generated by functional generator set to the ouput power level of 20dBm. Generator's output is connected to 50ohm dummy load through -40dBm RF tap. For the reference signal frequency was set to 3.65MHz (80m band). Coresponding osciloscope readings have been shown below.
<p align="center">
<img src="./meas/Combined_DSO_2024-05-11 11-47-25.png" width="600" height="400"/>
</p>
<p align="center">
<img src="./meas/Uin_DSO_2024-05-11 11-46-20.png" width="400" height="200"/>
<img src="./meas/Uout_DSO_2024-05-11 11-46-20.png" width="400" height="200"/>
</p>
Blue waveform represents the functional generator output voltage $U$, while yellow curve is the RF Tap output voltage $Utap$ 

$Upp = 6.4V$

$Upptap = 64.8mV$


$Up = Upp / 2 = 3.2V$

$Uptap = Upptap / 2 = 32.4mV$


$P =  \frac{Up^2}{2 \times R} = \frac{3.2^2}{2 \times 50} = 0.1024W$

and 

$P[dBm] = 10 \times \log{\frac{P[W]}{0,001}} = \log{\frac{0.1024}{0,001}} = 10 \times log{102.4} = 20,1dBm$


similarly:

$Ptap =  \frac{Up^2}{2 \times R} = \frac{(32.4 \times 10^{-3})^2}{2 \times 50} = 1.05 \times 10^{-5}W$

and

$Ptap[dBm] = 10 \times \log{\frac{Ptap[W]}{0,001}} = \log{\frac{(1.05 \times 10^{-5})}{0,001}} = -19.8dBm$

To verify obtained results we can confirm that measured voltage attenuation is as expected approximately 1:100 and that the sum of generator output power and attenuation indeed equals to measured tap output power for all powers expressed in dBm/dB (see test setup picture for test configuration details). Finally ratio of generator output power and power measured at RF tap output is as expected approximately 1:1000. See below for more exact calculations:

Voltage ratio: 

$\frac{Up}{Uptap} = \frac{3.2}{32.4 \times 10^{-3}} = 98.8$ (expected ratio = 100)

Power ratio:

$\frac{P}{Ptap} = \frac{0,1024}{1,05 \times 10^{-5}} = 9752$ (expected ratio = 10 000)

Power attenuation:

$P + Attenuation = Ptap$ => $20,1dBm + (-40dB) = -19.9dBm$ (compared to measured value of $Ptap$ = -19.8dBm)

## Power and voltage conversion to dB or dBm

To express power ratio in dB we use the following formula:

$[dB] = 10 \times \log{\frac{P2[W]}{P1[W]}}$

in case of P1 equal 1mW we call the ratio to be expressed in dBm, i.e.:

$[dBm] = 10 \times \log{\frac{P2[W]}{0,001W}}$

We can calculate the relation between power ratio and equivalent voltage ratio on given load R as:

$10 \times \log{\frac{P2[W]}{P1[W]}} = 10 \times \log{\frac{\frac{(U2[V])^2}{R}}{\frac{(U1[V])^2}{R}}} = 10 \times \log{(\frac{U2[V]}{U1[V]})^2}  = 20 \times \log{\frac{U2[V]}{U1[V]}}$

As can be seen from the above, the relation expressed in logarythmic scale between ratios of power and coresponding voltage ratios on given load is:

$10 \times \log{\frac{P2[W]}{P1[W]}} = 20 \times \log{\frac{U2[V]}{U1[V]}}$     (***)

In practice this means that if use attenuator, which attenuates power by -40dB, the coresponding voltage attenuation will be equal to:

$Att[dB] = 10 \times \log{\frac{P2[W]}{P1[W]}} = -40dB$

then from (***):

$\frac{U2}{U1} = 10^{(\frac{Att[dB]}{20})} = 10^{-2} = 0.01$ (!)


## Deriving the formula to convert power expressed in dBm to to one in Watts

As we know already:

$P[dBm] = 10 \times \log{\frac{P[W]}{0,001}}$

$P[dBm] / 10 = \log{\frac{P[W]}{0,001}}$

$\log{10^{\frac{P[dBm]}{10}}} = \log{\frac{P[W]}{0,001}}$

$P[W] = 0.001 \times 10^{\frac{P[dBm]}{10}}$

$P[W] = 10^{-3} \times 10^{\frac{P[dBm]}{10}}$

$P[W] = 10^{\frac{P[dBm]}{10} - 3}$

## Acknowledgments 

Special thanks to: 
* Miroslaw (Miro) Sadowski, SP5GNI
* Marcin Boboli, SP5IOU
* Piotr Skarpetowski, SQ5J
* Adam Zalewski, SP5UFK  

for the review of this article and valuable technical comments!

Otwock, August 2024

## References

[1] [The definitive guide to RF Power measurement](<https://qrp-labs.com/qcx/rfpower.html>)

[2] [Is It OK to Use an External 50 Ohm Terminator with an Oscilloscope?](<https://blog.teledynelecroy.com/2022/06/is-it-ok-to-use-external-50-ohm.html>)


