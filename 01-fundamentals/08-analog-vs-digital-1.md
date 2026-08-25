# Analog vs. Digital — Two Ways of Representing Reality

> The physical world is fundamentally continuous, but computers process information in discrete form.

Chapter 07 followed a signal from a raw sensor output through conditioning to a standardized 4–20 mA loop. This chapter asks a more fundamental question underneath all of that: what does it actually mean for information to be *analog*, what does it mean for it to be *digital*, and how — precisely — does a continuous physical quantity like temperature become a number sitting in a PLC register?

```
REAL WORLD
                     │
          Temperature / Pressure
          Position / Speed / Light
                     │
                     ▼
                  SENSOR
                     │
              ┌──────┴──────┐
              ▼             ▼
           ANALOG         DIGITAL
        representation   representation
              │             │
              ▼             ▼
         Continuous      Discrete
              │             │
              └──────┬──────┘
                     ▼
                CONTROLLER
```

**Hero picture to hold in mind:** a temperature rising smoothly from 20 °C to 80 °C — first as a continuous analog waveform, then as a staircase of discrete digital samples. Everything in this chapter is really explaining the gap between those two pictures.

---

## Table of Contents

1. [What Does "Analog" Actually Mean?](#1-what-does-analog-actually-mean)
2. [What Does "Digital" Actually Mean?](#2-what-does-digital-actually-mean)
3. [Continuous vs. Discrete](#3-continuous-vs-discrete)
4. [Analog Is Not the Same as "Electrical"](#4-analog-is-not-the-same-as-electrical)
5. [Real-World Examples](#5-real-world-examples)
6. [The Central Architecture — Conversion](#6-the-central-architecture--conversion)
7. [ADC — Analog to Digital Conversion](#7-adc--analog-to-digital-conversion)
8. [Sampling and Sampling Frequency](#8-sampling-and-sampling-frequency)
9. [The Nyquist Concept and Aliasing](#9-the-nyquist-concept-and-aliasing)
10. [Quantization](#10-quantization)
11. [ADC Resolution](#11-adc-resolution)
12. [Quantization Error](#12-quantization-error)
13. [Binary Representation — Down to Electronics](#13-binary-representation--down-to-electronics)
14. [Why Digital Electronics Uses Binary](#14-why-digital-electronics-uses-binary)
15. [Noise — Analog vs. Digital](#15-noise--analog-vs-digital)
16. [DAC — Digital to Analog Conversion](#16-dac--digital-to-analog-conversion)
17. [Analog and Digital Control, Side by Side](#17-analog-and-digital-control-side-by-side)
18. [Analog vs. Digital — Industrial Comparison](#18-analog-vs-digital--industrial-comparison)
19. [Digital Does Not Always Mean Better](#19-digital-does-not-always-mean-better)
20. [Hybrid Systems](#20-hybrid-systems)
21. [PWM — A Powerful Hybrid Concept](#21-pwm--a-powerful-hybrid-concept)
22. [Digital Pulses as Information](#22-digital-pulses-as-information)
23. [Encoders — Analog World to Digital Data](#23-encoders--analog-world-to-digital-data)
24. [Digital Communication, Briefly](#24-digital-communication-briefly)
25. [Where the "0 and 1" Really Come From](#25-where-the-0-and-1-really-come-from)
26. [Complete Real-World Example — Tank Temperature Control](#26-complete-real-world-example--tank-temperature-control)
27. [What If Resolution Is Too Low?](#27-what-if-resolution-is-too-low)
28. [What If Sampling Is Too Slow?](#28-what-if-sampling-is-too-slow)
29. [Troubleshooting Across the Analog/Digital Boundary](#29-troubleshooting-across-the-analogdigital-boundary)
30. [The Big Mental Model](#30-the-big-mental-model)
31. [Research Questions](#31-research-questions)
32. [References](#32-references)

---

## 1. What Does "Analog" Actually Mean?

Resist the urge to jump straight to voltage and current — the concept comes first.

> Analog means that one physical quantity is represented by another quantity that varies continuously in correspondence with it.

```
Temperature
20°C ─────────────── 80°C
       continuous
          ↓
Voltage
2 V ─────────────── 8 V
```

The important idea is *continuous correspondence*: as temperature rises smoothly, the representing voltage rises smoothly along with it, with no gaps or jumps. Nothing in that definition requires electricity at all — it's the continuity of the relationship that matters.

---

## 2. What Does "Digital" Actually Mean?

> Digital representation divides information into discrete states or numerical values.

```
Analog temperature:      Digital system:
20.0                     20.0 °C
20.1                     20.1 °C
20.2          →          20.2 °C
20.3                     20.3 °C
20.4                     20.4 °C
...                      ...
```

But an important correction to a common misconception:

> Digital does not simply mean "0 and 1."

Zero and one are the fundamental binary symbols digital electronics happens to use internally, but by combining many bits together, digital systems can represent enormous numerical ranges — a 32-bit integer can represent over four billion distinct values. "Digital" describes *discreteness*, not a specific tiny alphabet.

---

## 3. Continuous vs. Discrete

This is the core visual distinction underneath everything else in the chapter.

**Continuous** — a value exists at every instant throughout the interval:

```
Value
 ↑
 │       ╭────╮
 │     ╭─╯    ╰──╮
 │   ╭─╯         ╰──
 └──────────────────→ Time
```

**Discrete** — a value is represented only at distinct points or states:

```
Value
 ↑
 │       •
 │    •     •
 │  •         •
 │ •           •
 └──────────────────→ Time
```

![Digital storage oscilloscope displaying a sampled waveform](https://commons.wikimedia.org/wiki/Special:FilePath/Agilent%20Technologies%20DSO6052A%20Oscilloscope.jpg)
*A digital storage oscilloscope — internally it samples the input at discrete instants and reconstructs the display from those digital samples, unlike an analog scope's continuous trace. (Source: Wikimedia Commons)*

Every ADC in every PLC on earth is performing exactly this conversion, from the top picture to the bottom one, continuously, many times per second.

---

## 4. Analog Is Not the Same as "Electrical"

An important misconception worth eliminating early: **analog ≠ voltage**, and **digital ≠ computer**.

Analog representation can take many physical forms: voltage, current, mechanical position, pressure, fluid level, or physical displacement — a simple float rising with tank level is an entirely analog system with no electronics involved at all. Likewise, digital information can be transmitted electrically, optically, magnetically, or even mechanically (think of the discrete notches on a mechanical counter).

```
Analog ≠ voltage
Digital ≠ computer
```

These words describe *how information is represented* — continuously or discretely — not which physical medium happens to carry it.

---

## 5. Real-World Examples

| Physical quantity | Analog representation | Digital representation |
|---|---|---|
| Temperature | Voltage / current | Digital number |
| Position | Voltage | Encoder counts |
| Speed | Voltage / frequency | Pulse count / data |
| Pressure | 4–20 mA | Digital communication |
| Switch state | — | ON / OFF |
| Shaft rotation | — | Pulse sequence |

Notice that some quantities (temperature, position, speed) naturally have *both* an analog and digital representation, while a simple switch is inherently discrete from the start — there's no meaningful "analog voltage" representation of a push-button that adds real information beyond ON/OFF.

---

## 6. The Central Architecture — Conversion

This is the diagram the rest of the chapter builds on.

**Forward — sensing:**

```
REAL WORLD
    ↓
  Sensor
    ↓
Analog electrical signal
    ↓
   ADC
    ↓
Digital number
    ↓
PLC / Computer
```

**Reverse — acting:**

```
PLC / Computer
    ↓
Digital number
    ↓
   DAC
    ↓
Analog electrical signal
    ↓
Drive / Actuator
    ↓
Physical world
```

Every industrial control loop is, at bottom, this pair of conversions happening over and over: analog reality → digital computation → analog action.

---

## 7. ADC — Analog to Digital Conversion

Three conceptual stages, every time, regardless of the specific hardware:

```
Analog signal
      ↓
1. Sampling      — measure at discrete instants in time
      ↓
2. Quantization  — round each measurement to the nearest representable level
      ↓
3. Encoding      — express that level as a binary number
      ↓
Digital value
```

![10-bit ADC sampling and conversion timing](https://commons.wikimedia.org/wiki/Special:FilePath/10-Bit%20ADC%20Track%20and%20Conversion%20Example%20Timing.png)
*An ADC tracking a continuous input and converting each sample to a digital code at fixed intervals. (Source: Wikimedia Commons)*

Sections 8 through 12 walk through each of these three stages in turn.

---

## 8. Sampling and Sampling Frequency

> Sampling converts a continuous-time signal into a sequence of measurements taken at discrete times.

```
20°C ─── 30°C ─── 40°C ─── 50°C     (continuous reality)

  t1    t2    t3    t4    t5
  •     •     •     •     •          (sampled measurements)
```

```
f_s = 1 / T_s
```

where **f_s** is sampling frequency and **T_s** is the sampling period. 1000 samples per second is 1 kHz.

Different physical quantities need very different sampling rates: temperature changes slowly and can be sampled infrequently; vibration and audio change far faster and demand much higher sampling rates; a high-speed encoder tracking a fast-spinning shaft can require extremely rapid sampling to keep up.

---

## 9. The Nyquist Concept and Aliasing

The basic requirement for faithfully capturing a band-limited signal:

```
f_s > 2 × f_max
```

> The sampling frequency needs to be sufficiently high relative to the highest frequency of interest, or the digital record will misrepresent the signal.

This shouldn't be oversimplified into "exactly twice is always enough" — real systems need margin above the theoretical minimum, plus proper anti-aliasing filtering, to work reliably in practice.

**Aliasing** is what happens when this requirement is violated:

```
REAL SIGNAL
╭╮  ╭╮  ╭╮  ╭╮
╯╰──╯╰──╯╰──╯╰

LOW SAMPLING
•       •       •

APPARENT SIGNAL
╭────────╮
          ╰──────
```

> An inadequately sampled high-frequency signal can be interpreted by the digital system as a completely different, lower-frequency signal.

The fix is a hardware **anti-aliasing filter** placed ahead of the ADC — a low-pass filter that removes frequency content the chosen sampling rate can't represent correctly, so it never reaches the sampler to be misrepresented in the first place.

---

## 10. Quantization

After sampling fixes *when* a measurement is taken, quantization fixes *what value* gets recorded.

```
Analog range:        0 ─────────────────── 10 V
Quantized levels:    0, 1, 2, 3, 4, 5, 6, 7, ...
```

> Quantization replaces an infinitely precise theoretical range of amplitudes with a finite set of numerical levels.

No matter how fine the steps, the analog world's true infinite precision is always, unavoidably, rounded to the nearest available digital level.

---

## 11. ADC Resolution

Resolution is set by how many bits the converter uses to encode each sample:

| ADC | Levels |
|---|---|
| 8-bit | `2⁸ = 256` |
| 12-bit | `2¹² = 4096` |
| 16-bit | `2¹⁶ = 65,536` |

> More bits generally allow finer quantization, but resolution is not the same as total measurement accuracy.

A 16-bit ADC connected to a noisy, poorly calibrated sensor is not automatically more *accurate* than a 12-bit ADC on a clean, well-calibrated one — resolution describes the theoretical fineness of the digital steps, while accuracy describes how close the final number is to physical truth, which depends on the whole measurement chain from Chapters 05 through 07.

---

## 12. Quantization Error

```
Actual value              5.37 V
Digital representation    5.36 V
                           ↓
                    Quantization error
```

Real ADCs add further imperfections on top of this unavoidable rounding: **offset error** (a constant shift), **gain error** (a scaling mismatch), **nonlinearity** (uneven step sizes across the range), electronic **noise**, and errors in the converter's own internal reference voltage. Resolution tells you the *best possible* case; these additional errors are why real-world accuracy is always somewhat worse than the ideal quantization step alone would suggest.

---

## 13. Binary Representation — Down to Electronics

```
1 bit  →  0 or 1
2 bits →  00, 01, 10, 11
n bits →  2ⁿ possible combinations
```

This is where the digital number inside a PLC ultimately connects back to physical electronics:

```
Transistors
   ↓
Logic gates
   ↓
Flip-flops / registers
   ↓
Binary numbers
   ↓
Digital processors
   ↓
PLC / computer
```

![TI 7400 integrated circuit containing four NAND logic gates](https://commons.wikimedia.org/wiki/Special:FilePath/TexasInstruments%207400%20chip%2C%20view%20and%20element%20placement.jpg)
*A 7400-series integrated circuit — four NAND logic gates, the physical building block underneath every binary number a PLC ever computes with. (Source: Wikimedia Commons)*

![Siemens S7-200 PLC CPU module](https://commons.wikimedia.org/wiki/Special:FilePath/Siemens%20PLC%20S7-200%20CPU.jpg)
*A PLC CPU module — the physical processor where transistor states ultimately become ladder logic decisions. (Source: Wikimedia Commons)*

Every digital value your PLC program ever touches is, physically, a pattern of transistor states switching between two ranges of voltage.

---

## 14. Why Digital Electronics Uses Binary

Electronic circuits can reliably distinguish *ranges* of physical states far more easily than exact values:

```
LOW voltage  → 0
HIGH voltage → 1
```

The exact voltage thresholds separating "low" from "high" depend on the specific technology, but the principle is universal. Binary systems are attractive for several compounding reasons: strong **noise tolerance** (a circuit only needs to tell "low-ish" from "high-ish," not measure an exact value), **reliable switching**, the ability to **regenerate** a clean signal at every stage (Section 15), straightforward **storage**, clean **logic operations**, and — critically — **scalable processing**, since the same two-state building block composes into arbitrarily complex computation.

---

## 15. Noise — Analog vs. Digital

This comparison is one of the most practically important ideas in the whole chapter.

**Analog:**

```
Original:   ──────────────
Noise:      ~~~~~~
Received:   ─~─~~─~────~~
```

Noise directly and permanently alters the represented value — there's no way to separate the "real" signal back out from a corrupted analog waveform after the fact.

**Digital:**

```
Original:      00011100
Small noise →  still interpreted as 00011100
```

> Digital systems can often regenerate clean logical states, so small amounts of noise may not change the interpreted data.

This is *not* immunity, though — digital signals absolutely can be corrupted by sufficiently large noise, timing errors, or dropped bits. The advantage is a matter of degree and threshold behavior, not a magic exemption from physics.

---

## 16. DAC — Digital to Analog Conversion

Reversing the whole process:

```
Digital value
      ↓
     DAC
      ↓
Analog voltage / current
      ↓
Drive / actuator
```

**Example:**

```
PLC command = 50%
       ↓
      DAC
       ↓
     5 V
       ↓
  VFD reference
       ↓
  Motor speed
```

**Signal regeneration**, mentioned above, works the same way on the digital side of any transmission:

```
Digital signal → noise → receiver threshold → clean 0 / 1
```

A receiver only has to decide which side of a threshold a noisy pulse landed on — not reconstruct its exact original amplitude — which is precisely why digital communication tends to remain reliable over distances and conditions where an equivalent analog signal's amplitude accuracy would visibly degrade.

---

## 17. Analog and Digital Control, Side by Side

**Analog control — a VFD:**

```
PLC → Analog output → 0–10 V → VFD → Motor

0 V  → minimum command
5 V  → 50% command
10 V → maximum command
```

![Variable frequency drive (VFD)](https://commons.wikimedia.org/wiki/Special:FilePath/Siemens%20VFD.jpg)
*A variable-frequency drive — a real destination for an analog 0–10 V speed-command signal. (Source: Wikimedia Commons, CC BY-SA 4.0)*

(Actual VFD scaling depends entirely on how the drive is configured — the numbers above illustrate the concept, not a universal standard.)

**Digital control — a contactor:**

```
PLC → Digital output → 24V ON/OFF → Contactor → Motor
```

Placed side by side, it becomes clear why real automation systems use *both*: continuous speed commands are naturally analog, while simple start/stop commands are naturally digital.

---

## 18. Analog vs. Digital — Industrial Comparison

| Feature | Analog | Digital |
|---|---|---|
| Representation | Continuous | Discrete |
| Typical PLC signal | 4–20 mA, 0–10 V | ON/OFF, pulses, data |
| Noise sensitivity | Can directly alter the value | Often tolerates small noise |
| Resolution | Continuous in the ideal model; limited by real electronics | Determined by bit depth |
| Processing | Analog circuitry | Digital logic / software |
| Long-distance integrity | Can degrade gradually | Can often be regenerated |
| Typical example | A temperature value | A sensor's ON/OFF state |
| Communication | Analog loops | Industrial networks |

---

## 19. Digital Does Not Always Mean Better

An important engineering lesson, easy to overlook in an era where "digital" is often used as a synonym for "modern" or "good."

Analog can be genuinely advantageous for: simple systems, low-latency signal paths, certain instrumentation applications, and simple control interfaces. Digital can be genuinely advantageous for: computation, data storage, diagnostics, communication, complex control algorithms, and networking.

> The correct question is never "analog or digital, which is better?" — it is: **what representation is appropriate for this specific application?**

---

## 20. Hybrid Systems

Modern industrial automation is neither purely analog nor purely digital — it's a continuous alternation between the two, at every boundary between the physical world and computation.

```
Physical world
      ↓
Analog sensor
      ↓
Signal conditioning
      ↓
     ADC
      ↓
Digital PLC
      ↓
Digital logic
      ↓
DAC / PWM / Network
      ↓
   Drive
      ↓
Analog / physical response
      ↓
   Motor
```

This is arguably the single most important diagram in the chapter — nearly every real automation system, from a small machine to a full plant, is this exact pattern repeated at every measurement and every actuation point.

---

## 21. PWM — A Powerful Hybrid Concept

**Pulse Width Modulation** is where "analog" and "digital" stop being opposites and start working together directly.

```
25% duty:   __|‾|____|‾|____|‾|__
50% duty:   __|‾‾|__|‾‾|__|‾‾|__
75% duty:   __|‾‾‾|_|‾‾‾|_|‾‾‾|_
```

![PWM signal with varying duty cycle](https://commons.wikimedia.org/wiki/Special:FilePath/Pwm%20duty%20cycle.gif)
*Pulse-width modulation — the same digital high/low switching, with duty cycle carrying continuous, analog-like information. (Source: Wikimedia Commons)*

> PWM is a digital switching waveform whose duty cycle represents information or controls average power.

The switching itself is purely digital — the output is either fully ON or fully OFF at any instant — but by controlling *how much time* is spent ON versus OFF, PWM lets a digital output deliver an effectively continuous, analog-like average power. This is the bridge between digital PLC logic and genuinely analog physical effects like motor torque, LED brightness, or heater power — and it's the working principle behind most modern motor drives and power electronics.

---

## 22. Digital Pulses as Information

The same physical waveform — a train of digital pulses — can carry entirely different kinds of information depending on what property you read from it:

```
Pulse count  → position
Frequency    → speed
Duty cycle   → command
Pulse width  → information
```

This is exactly why "digital" is a much richer idea than simply ON/OFF: the same binary electrical event, encoded differently, becomes position data, speed data, or a command value.

---

## 23. Encoders — Analog World to Digital Data

A rotary encoder is the cleanest possible illustration of the whole chapter compressed into one device.

```
Motor shaft
    ↓
 Encoder
    ↓
 A/B pulses
    ↓
Digital input
    ↓
   PLC
    ↓
Position + direction + speed
```

The shaft's rotation is a purely continuous, analog physical quantity — an angle that can take any value at all. The encoder converts that continuous rotation directly into a train of discrete pulses, with no intermediate analog voltage stage at all. Counting those pulses gives position; measuring their frequency gives speed; comparing the relative timing of the A and B channels gives direction. This is analog-to-digital conversion happening mechanically and optically, rather than through a conventional ADC chip.

![Incremental rotary encoder](https://commons.wikimedia.org/wiki/Special:FilePath/Rotary%20encoder.jpg)
*An incremental rotary encoder — continuous shaft rotation converted directly into a discrete pulse train, with no conventional ADC involved. (Source: Wikimedia Commons, CC BY 4.0)*

---

## 24. Digital Communication, Briefly

```
Sensor
 ↓
Digital electronics
 ↓
  Data
 ↓
Communication
 ↓
  PLC
```

At this level, worth knowing only by name for now: **serial communication**, **fieldbus**, and **industrial Ethernet**. Specific protocols — EtherNet/IP, DeviceNet, ControlNet, and the rest — belong to a dedicated communications chapter later in this sequence; introducing them here would dilute both topics.

---

## 25. Where the "0 and 1" Really Come From

The complete stack, connecting raw physics to a running PLC program and back out to physical action:

```
Physical voltage
      ↓
Transistor states
      ↓
Logic gates
      ↓
Binary logic
      ↓
Registers
      ↓
Processor
      ↓
Digital value
      ↓
PLC program
      ↓
Output state
      ↓
Transistor / relay
      ↓
Electrical signal
      ↓
Actuator
```

This is the complete physics → semiconductor → automation bridge: every PLC decision, no matter how abstract it feels while writing ladder logic, ultimately reduces to transistors switching between voltage ranges.

---

## 26. Complete Real-World Example — Tank Temperature Control

```
Tank
 ↓
Temperature
 ↓
   RTD
 ↓
Resistance
 ↓
Transmitter
 ↓
4–20 mA
 ↓
Analog Input
 ↓
   ADC
 ↓
Digital value
 ↓
   PLC
 ↓
Control algorithm
 ↓
Digital output / analog output
 ↓
Heater / SCR / VFD
 ↓
Tank temperature
 ↺
```

Label every stage by category, and the whole chapter collapses into one sentence: **physical → analog → digital → computation → analog → physical.** This loop is the flagship diagram of the entire chapter — everything above is really just zooming into one link of this chain.

---

## 27. What If Resolution Is Too Low?

Suppose you need to measure the range 0–100 °C, but the digital system's resolution is too coarse:

```
Real temperature:      Digital system:
50.00 °C                50 °C
50.01 °C                51 °C
50.02 °C          →     52 °C
50.03 °C                ...
...
```

> The system cannot represent changes smaller than its available resolution.

A control loop trying to hold a setpoint tighter than the digital system can even represent will visibly hunt or oscillate — not because the control algorithm is wrong, but because the feedback itself is too coarse to see the small errors it's meant to correct.

---

## 28. What If Sampling Is Too Slow?

Suppose a vibration signal changes rapidly:

```
Real:            /\/\/\/\/\/\/\/\/\
Slow sampling:   •       •       •
```

The controller may interpret the signal as something it isn't — slower, smaller, or entirely absent — connecting directly back to Sections 8 and 9: sampling rate, the Nyquist requirement, and aliasing. A vibration monitoring system sampled too slowly doesn't just lose precision; it can miss the fault signature entirely.

---

## 29. Troubleshooting Across the Analog/Digital Boundary

Suppose the sensor genuinely reads 50 °C, but the HMI displays 73 °C. Where's the fault?

```
   Sensor
      ↓
Analog signal
      ↓
Signal conditioning
      ↓
      ADC
      ↓
  Raw count
      ↓
   Scaling
      ↓
  PLC tag
      ↓
    HMI
```

Potential faults live at *any* stage: a drifted sensor, incorrect transmitter configuration, a wiring fault, a misconfigured analog input range, faulty ADC/input hardware, wrong scaling parameters, an error in PLC logic, or a mislinked HMI tag. This is the same discipline from Chapter 07's troubleshooting section, applied specifically to the analog/digital boundary rather than the whole signal path — the fault could be purely on the analog side, purely on the digital side, or exactly at the ADC seam between them.

---

## 30. The Big Mental Model

```
PHYSICAL REALITY
                        │
                        ▼
                     SENSOR
                        │
                        ▼
                ANALOG SIGNAL
                        │
                        ▼
              SIGNAL CONDITIONING
                        │
                        ▼
                       ADC
                        │
                        ▼
                 DIGITAL DATA
                        │
                        ▼
              DIGITAL PROCESSING
                        │
                        ▼
                       DAC
                  / PWM / OUTPUT
                        │
                        ▼
                    ACTUATOR
                        │
                        ▼
                 PHYSICAL REALITY
                        ↺
```

> Analog describes continuously varying representation. Digital describes discrete representation. Modern automation combines both because the physical world is continuous while controllers and computers are fundamentally digital.

---

## 31. Research Questions

1. What exactly makes a signal analog?
2. What exactly makes a signal digital?
3. Is the physical world itself analog?
4. Why do computers use binary?
5. What is sampling?
6. Why is sampling necessary at all?
7. What is sampling frequency?
8. What is the Nyquist principle?
9. What is aliasing?
10. What is quantization?
11. What is ADC resolution?
12. Why doesn't higher resolution automatically mean higher accuracy?
13. What is quantization error?
14. What is a DAC?
15. Why are ADCs required in PLC systems?
16. Why are DACs required in some control systems?
17. How does noise affect an analog signal?
18. Why can digital signals tolerate some noise?
19. What is signal regeneration?
20. How does a transistor ultimately participate in digital logic?
21. What is PWM?
22. How can a digital pulse train represent speed?
23. How does an encoder convert motion into digital information?
24. Why do industrial systems use both analog and digital signals, rather than picking one?
25. What happens at every single stage from a physical temperature to a PLC temperature tag?

---

## 32. References

- ISA (International Society of Automation) — instrumentation and signal standards.
- *Nyquist–Shannon sampling theorem*, Wikipedia — sampling rate requirements and aliasing.
- *Quantization (signal processing)*, Wikipedia — quantization error and resolution.
- *Pulse-width modulation*, Wikipedia — duty cycle and average-power control.
- *Rotary encoder*, Wikipedia — incremental vs. absolute position encoding.
- *Logic gate*, Wikipedia — binary logic and transistor-level implementation.
- *Variable-frequency drive*, Wikipedia — analog and digital motor control interfaces.
- AutomationDirect and NI (National Instruments) technical libraries — ADC/DAC and DAQ fundamentals.
- Wikimedia Commons — photographs and diagrams, credited individually above each image.

---

## Suggested Repository Structure for This Chapter

```
01-fundamentals/
│
├── 07-signals-and-signal-conditioning.md
├── 08-analog-vs-digital.md   ← this file
│
└── assets/
    └── 08-analog-vs-digital/
        ├── images/
        │   ├── adc-sampling-timing.png
        │   ├── digital-storage-oscilloscope.jpg
        │   ├── ti-7400-logic-gate-chip.jpg
        │   ├── siemens-s7-200-cpu.jpg
        │   ├── siemens-vfd.jpg
        │   ├── pwm-duty-cycle.gif
        │   └── rotary-encoder.jpg
        │
        └── animations/
            ├── continuous-vs-discrete/
            ├── sampling/
            ├── aliasing/
            ├── quantization/
            ├── adc-lab/          (interactive: sampling rate + bit depth sliders)
            ├── dac/
            ├── pwm/
            └── encoder/
```

**Notes for building this out:**
- The single highest-value interactive element for this chapter is an **"Analog-to-Digital Lab"**: let the reader adjust sampling frequency and ADC bit depth independently on the same input waveform, and watch the reconstructed digital signal go from smooth, to visibly stepped, to outright aliased in real time.
- Keep this chapter focused on the analog/digital distinction itself and the conversion process between them. Specific communication protocols (fieldbus, industrial Ethernet variants) and detailed actuator/drive behavior belong in their own later chapters — Chapter 09 (Actuators) picks up immediately where Section 17's VFD example leaves off.
