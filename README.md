# Reactive Load Box for Guitar Amps
![IMG_6397](https://github.com/user-attachments/assets/4eb43f84-0238-4890-9c8d-f29357dec935)

To avoid making the neighbors hate me, I designed and built a reactive load box inspired by commercial solutions like the Universal Audio OX Box and Two Notes Captor X.
This allows my 50W Marshall JMP 2204 guitar amp to be safely driven at proper output levels while feeding a clean, isolated signal directly into my computer without sacrificing the Marshall feel or tone.

# Resistive vs Reactive 
There are two main types of load boxes on the market for amplifier attenuation attentuation being reactive and resistive. While both work, each option varies greatly in amplifier response accuracy when attentuating.

## Resistive
A resistive load uses high power resistors or an L-pad in a voltage divider configuration to attentuate the connected amplifier. While resistive loads are cheap to build since it only requires two resistors, there are important caveats that affect the amplifier's tone. These include reduced dynamics and a flat response that will make the amplifier sound different than it would with a speaker cabinent.

Models on the market include: </br>
**Bugera PS1 Power Attentuator**
<img width="860" height="860" alt="image" src="https://github.com/user-attachments/assets/ba0344d9-59bc-47ef-b124-3fa96de667e0" />

**Carl's Custom Guitars Speaker Soak**
<img width="749" height="800" alt="image" src="https://github.com/user-attachments/assets/d44a2949-0fd3-4f5e-a37e-bc5ed8a57dc3" />

**Dr. Z Airbrake**
<img width="600" height="800" alt="image" src="https://github.com/user-attachments/assets/8bbc41e7-1751-449b-956e-705873bc95a2" />

## Reactive
The reason comes down to how the amplifier responds to a speaker vs resistors. With a resistor divider, the impedance is going to be constant through the entire frequency spectrum as shown through my LTSpice simulation below.

<img width="1911" height="425" alt="image" src="https://github.com/user-attachments/assets/eeb73bac-f440-4b7c-91cf-08bb2ac2489b" />

Whereas a speaker has a varying impedance across the entire frequency spectrum. Such a trend can be seen from this graph below that shows the [impedance chart for a 4 ohm speaker](https://audiojudgement.com/speaker-impedance-curve-explained/)

<img width="974" height="607" alt="image" src="https://github.com/user-attachments/assets/cf67a72f-4b31-4c64-b93f-ebf0c8a57740" />

To emulate this, a reactive load can be created with inductors, capacitors, and resistors to accurately represent a speaker's impedance curve. While more complex and expensive, the benefit is a more realistic, dynamic, and natural amplifier response that allows for an authentic playing experience, even at full cranked volumes. 

Models on the market include:
**Universal Audio OX Reactive Amp Attenuator**
<img width="750" height="291" alt="image" src="https://github.com/user-attachments/assets/7d9d9394-d71b-4ea9-bf5f-1861bd8e660f" />

**Two Notes Torpedo Captor X Reactive Loadbox DI and Attenuator**
<img width="750" height="615" alt="image" src="https://github.com/user-attachments/assets/b07cd3b7-094b-4533-a399-43d0b5dd18cf" />

**Suhr Reactive Load**
<img width="850" height="649" alt="image" src="https://github.com/user-attachments/assets/e3bb0502-a6a8-4742-820d-cb2a48da9d3c" />

# Design
I decided to go with a reactive load similar to the OX box or Captor X vs a resistive load for a more realistic and dynamic feel and tone. For the reactive load part, I referenced [this website for Aiken's reactive speaker load](https://www.aikenamps.com/index.php/designing-a-reactive-speaker-load-emulator) for the basis of the design. 

To step down the amplifier voltage enough, I found a way to use a voltage divider in parallel with the RLC network that feeds an isolated audio transformer without affecting the load seen by the amp.

I used a Lehle LTHZ transformer for isolation and line-level output. It’s dead quiet and sounds fantastic straight into my Focusrite. I handle all cab simulation with IRs (Neural Amp Modeler + Tone3000), so there’s no onboard cabinet filtering.

The resistors are high power chassis mount resistors noted in the schematics. These mount directly to the metal enclosure to dissipate the heat from the attenuation.

For the inductors I used air core inductors normally used in crossovers, they have a really high saturation current and the magnetic field won't break down with the high wattage amplifier output. Parts Express has a whole bunch of these for a decent price and is very high quality. 

</br>
</br>

Accounting for cost of certain components and making adjustments accordingly, I calculated alternate component values to reduce the inductance of Aiken's 50mH air core inductor, as these are either very pricy or unavaliable. 

Using the RLC resonance formula, f₀ ≈ 1 / (2π√(LC)), the original Aiken design with the 50mH inductor and 100uF bipolar capacitor:

f₀ ≈ 1 / (2π√(LC)) = 1 / (2π√(50mH)(100uF)) = 71.2 Hz

I found that using a 10mH inductor and a 500uF capacitor accurately represented the lower peak well enough that there would not be a huge difference in sound. 

f₀ ≈ 1 / (2π√(LC)) = 1 / (2π√(10mH)(500uF)) = 71.2 Hz

This helped make this design more feasible and greatly reduced the potential cost of the reactive load.

I came up with this diagram that accurately emulates Aiken's design and includes all of the additions to safely run a stereo out to my interface.

<img width="1083" height="837" alt="image" src="https://github.com/user-attachments/assets/e60b576f-c125-4e77-97e0-94e59327d888" />

Below is the impedance curve generated by the circuit measured at the amplifier input, showing that with all of the components needed, the impedance curve is not affected by the parallel voltage divider and line transformer:
<img width="1916" height="424" alt="image" src="https://github.com/user-attachments/assets/60a97c8e-2f2e-489a-bb44-3590dbeb98d2" />

# Parts List
To make this design, the completed parts list is below. All of the parts are rated and designed to withstand 100W of power from an amp:

- 47 Ohm Chassis Mount Resistor 15W
- 8 Ohm Chassis Mount Resistor 100W
- 68 Ohm CHassis Mount Resistor 10W
- 82 Ohm Chassis Mount Resistor 10W
- 8.2 Ohm Chassis Mount Resistor 10W
- 12 Ohm 1/2W Resistor
- Dayton Audio 10mH 18 AWG Air Core Inductor
- Dayton Audio 1mH 18 AWG Air Core Inductor
- 2x 250uF Electrolytic Bipolar Capacitor (I used two in parallel to equal 500uF to increase ripple current rating of the capacitors)
- Lehle LTHZ Line Transformer
- Dual Gang Potentiometer 10k Audio
- 2x 1/4" Mono Phone Jack
- 1/4" Stereo Phone Jack
- Zulkit Metal Project Box (Any metal enclosure works here)

# Layout
The resistors are high power chassis mount resistors noted in the schematics. These mount directly to the metal enclosure to dissipate the heat from the attenuation.

For mounting, I did not mount directly on the metal enclosure face since the inductance will be affected, so I used a 1/2” block of wood as a buffer under the inductors.
![IMG_6346](https://github.com/user-attachments/assets/37344320-4b74-4406-a47c-fd8f52927488)
![IMG_5392](https://github.com/user-attachments/assets/680b7913-2485-43e9-a28e-cbb5ca9648d9)

# Conclusion
The result was amazing! The sound coming out of the attentuator is noise free, dynamic, and perfect for running into my computer for impulse responses.

I'd love for anyone to be able to build their own reactive load without having to spend 500 - 1000+. Feel free to use my design to make your own reactive load!
