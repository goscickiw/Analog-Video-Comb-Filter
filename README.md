# Analog-Video-Comb-Filter
Analog Composite Video to S-Video comb filter board using SAA4960 or SAA4961 integrated circuit

## **WARNING**
**This project uses end-of-life components that are no longer manufactured. If you want to build this, make sure that you can obtain all components before starting. Used or new-old-stock parts can sometimes be found online.**

## Description

This is a fully analog alternative to digital comb filters such as Extron YCS Transcoder or YCS 100. That doesn't mean it will give better results though. I just made it because I didn't find any fully analog device like that on the market and I wanted one.

The comb filter is used to convert a composite video signal into an S-Video (Y/C) signal. Comb filters are much better than passive LC filters usually built into composite video displays, as they can preserve the frequencies of luminance that overlap with chrominance, providing higher image detail while removing dot crawl, and remove those overlapping luminance frequencies from the chrominance signal which reduces color artifacts.

When using the SAA4960 integrated circuit, PAL B, D, G, H and I systems are supported.

SAA4961 provides aditional support for PAL M, N and NTSC M systems.

The board is designed to fit inside a KRADEX Z-76 enclosure.

### Circuit description

A composite video signal is fed in through an RCA connector. The signal is terminated with a 75 ohm resistor and fed to U2, U3 and U5 through 100 nF capacitors.

The LM1881 sync separator (U5) uses the composite video signal to generate a burst gate signal. The burst gate signal passes through a 74HC04 inverter (U4) to the MC44144 subcarrier PLL (U3).

U3 uses the composite video signal and the burst gate signal to generate a subcarrier frequency synchronized to the colorburst of the composite video signal. This subcarrier signal is then passed to the FSC input of U2.

The SAA4960/61 comb filter (U2) is fed with the composite video signal and the synchronized subcarrier signal. The jumpers SYS1 and SYS2 set the video standard, and the jumper LPF can be used to disable the input anti-alias low pass filter. This circuit outputs a filtered luminance and chrominance signals and a delayed composite video passthrough signal. Those signals are then fed to the output amplifiers built using bipolar transistors.

Each output amplifier is built from two BC548 NPN transistors and a BC558 PNP transistor. The output signals from the SAA4960/61 are DC-biased by around 1V, so DC decoupling capacitors are not required to properly DC-bias the base of the transistors. This allows the signal black level to stay at the same voltage regardless of what is being displayed.

## Pictures

Assembled prototype:
![Assembled prototype](https://github.com/goscickiw/Analog-Video-Comb-Filter/blob/main/pictures/IMG_20240814_213257.JPG)

Completed, operating device in enclosure:
![Completed device](https://github.com/goscickiw/Analog-Video-Comb-Filter/blob/main/pictures/IMG_20240814_220529.JPG)

Image comparison on a monochrome display - with and without comb filter:
![Comparison](https://github.com/goscickiw/Analog-Video-Comb-Filter/blob/main/pictures/Comparison.JPG)

## LEDs

D1 shows that the device is receiving power.

D2 shows that the comb filter is operating (U2 is in COMB mode). I'm not sure if U2 will ever *not* be in COMB mode when used in this circuit, so this LED might be unnecessary.

## Solder jumper settings

### Standard selection jumpers (SYS1, SYS2)

SAA4961 only. When using SAA4960, SYS1 and SYS2 jumpers should be left open.

Appropriate crystal for the subcarrier oscillator should be installed depending on the jumper setting. If the standard is going to be changed frequently, it may be possible to install a socket for the crystal, however it should be taken into account that MC44144 is sensitive to additional capacitance at the crystal.

| Standard      | SYS1  | SYS2  | Crystal required | 
| ------------- | ----- | ----- | ---------------- |
| PAL B/D/G/H/I | Open  | Open  | 17,734475 MHz    |
| PAL M         | Open  | Short | 14,302444 MHz    |
| PAL N         | Short | Open  | 14,328225 MHz    |
| NTSC M        | Short | Short | 14,31818 MHz     |

### Low pass filter jumper (LPF)

The SAA4960 and SAA4961 integrated circuits have a built-in low-pass filter on the composite video input. This filter can be disabled by shorting the LPF jumper, but it's recommended to leave it on.

| LPF   | Filter mode     |
| ----- | --------------- |
| Open  | Filter enabled  |
| Short | Filter disabled |

## Adjustment

After assembly, including setting the jumpers and installing the correct crystal depending on the analog video standard, the variable capacitor C24 will have to be adjusted so that MC44144 properly locks onto the subcarrier. Use the following procedure for adjustment:

1. Set C24 to minimum capacitance, then slowly increase capacitance until the PLL locks on. Mark that position.
2. Set C24 to maximum capacitance, then slowly decrease capacitance until the PLL locks on. Mark that position.
3. Set C24 to a value between the two positions.

C24 will have to be readjusted if the crystal and the standard selection jumper settings are changed.

## Parts list

### Resistors

| Resistance | Power  | Qty |
| ---------- | ------ | --- |
| 27 Ω       | 0,25 W | 3   |
| 47 Ω       | 0,25 W | 4   |
| 75 Ω       | 0,25 W | 4   |
| 560 Ω      | 0,25 W | 6   |
| 1 kΩ       | 0,25 W | 8   |
| 2,2 kΩ     | 0,25 W | 1   |
| 4,7 kΩ     | 0,25 W | 4   |
| 10 kΩ      | 0,25 W | 1   |
| 47 kΩ      | 0,25 W | 1   |
| 680 kΩ     | 0,25 W | 1   |

### Ceramic and MLCC capacitors 

| Capacitance | Pin pitch | Type             | Qty |
| ----------- | --------- | ---------------- | --- |
| 4-20 pF     | 5,08 mm   | Ceramic variable | 1   |
| 470 pF      | 2,5 mm    | Ceramic          | 1   |
| 1 nF        | 5 mm      | Ceramic          | 1   |
| 100 nF      | 2,5 mm    | Ceramic          | 18  |
| 1 μF        | 7,5 mm    | MLCC             | 3   |

### Electrolytic capacitors

| Capacitance | Voltage | Pin pitch | Diameter | Qty |
| ----------- | ------- | --------- | -------- | --- |
| 100 μF      | ≥ 6,3 V | 2 mm      | 5 mm     | 6   |
| 100 μF      | ≥ 16 V  | 2 mm      | 5 mm     | 1   |
| 220 μF      | ≥ 16 V  | 2,5 mm    | 6,3 mm   | 2   |

### Inductors

| Inductance | Max. Current | Exact part        | Qty |
| ---------- | ------------ | ----------------- | --- |
| 22 μH      | 0,285 A      | Ferrocore DLA22-N | 6   |

### Semiconductors

| Type           | Model     | Qty |
| -------------- | --------- | --- |
| LED            | 3mm Green | 1   |
| LED            | 3mm Red   | 1   |
| NPN Transistor | BC548     | 7   |
| PNP Transistor | BC558     | 3   |

### Integrated circuits

| Model              | Package | Qty |
| ------------------ | ------- | --- |
| L7805              | TO220   | 1   |
| SAA4960 or SAA4961 | DIP28   | 1   |
| MC44144            | DIP8    | 1   |
| 74HC04             | DIP14   | 1   |
| LM1881             | DIP8    | 1   |

### Other

| Type               | Model                           | Qty |
| ------------------ | ------------------------------- | --- |
| Crystal            | HC-49U, depends on video system | 1   |
| RCA Connector      | Keystone Electronics 973        | 2   |
| Mini-DIN Connector | MDC-204, unshielded             | 1   |
| DC Barrel Jack     | Generic 5,5/2,5mm               | 1   |
| Enclosure          | KRADEX Z-76                     | 1   |

## License

This work is licensed under CC BY-SA 4.0 license.
