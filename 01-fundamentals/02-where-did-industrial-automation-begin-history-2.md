# Where Did Industrial Automation Begin? — A History

[📄👾 Open Interactive Article 02](https://qurvon.github.io/Industrial-Automation-from-First-Principles/01-fundamentals/02-where-did-industrial-automation-begin-history-2.html)

> Industrial automation did not begin with PLCs, robots, or computers.
> It evolved step by step — through mechanical machines, feedback mechanisms, electrical control, electronics, and finally computing and networked intelligence.

This chapter tells that story as a chain of **problems and solutions**. Every technology on this timeline exists because the technology before it hit a hard limit.

| Era | Years | Core Shift |
|---|---|---|
| Mechanical | 1760s–1800s | Muscle → machine power |
| Electrical | 1870s–1940s | Central power → distributed electrical control |
| Electronic | 1947–1960s | Mechanical/relay logic → solid-state logic |
| Digital | 1968–1990s | Hard-wired logic → programmable, networked control |
| Smart / Connected | 2000s–today | Isolated systems → connected, data-driven, intelligent systems |

---

## Table of Contents

1. [Before Industrial Automation — Human Control](#1-before-industrial-automation--human-control)
2. [The First Industrial Revolution — Mechanization](#2-the-first-industrial-revolution--mechanization)
3. [The Beginning of Automatic Control — Watt's Governor](#3-the-beginning-of-automatic-control--watts-governor)
4. [The Jacquard Loom — Machines Begin Following Instructions](#4-the-jacquard-loom--machines-begin-following-instructions)
5. [The Second Industrial Revolution — Electrification](#5-the-second-industrial-revolution--electrification)
6. [From Mechanical/Pneumatic Control to Electronic Control](#6-from-mechanicalpneumatic-control-to-electronic-control)
7. [The Rise of Process Control Computers](#7-the-rise-of-process-control-computers)
8. [The Birth of the PLC (1960s–1970s)](#8-the-birth-of-the-plc-1960s1970s)
9. [The Third Industrial Revolution — Computerized Automation](#9-the-third-industrial-revolution--computerized-automation)
10. [The Birth of the DCS](#10-the-birth-of-the-dcs)
11. [HMI, SCADA and Industrial Networking](#11-hmi-scada-and-industrial-networking)
12. [Fieldbus and Open Communication](#12-fieldbus-and-open-communication)
13. [Ethernet and Industrial Ethernet](#13-ethernet-and-industrial-ethernet)
14. [Industry 4.0](#14-industry-40)
15. [Automation Today](#15-automation-today)
16. [What Actually Changed at Every Stage?](#16-what-actually-changed-at-every-stage)
17. [The First-Principles Mental Model](#17-the-first-principles-mental-model)
18. [Evolution Timeline (Year Scale)](#18-evolution-timeline-year-scale)
19. [References](#19-references)

---

## 1. Before Industrial Automation — Human Control

Before any machine could regulate itself, a **human being was the controller**: observing, deciding, and acting, over and over.

![Ford Highland Park assembly line workers, 1913](https://upload.wikimedia.org/wikipedia/commons/2/29/Ford_assembly_line_-_1913.jpg)
*Workers performing the sensing, deciding, and acting that automation would later take over. Ford Highland Park, 1913. (Source: [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Ford_assembly_line_-_1913.jpg), public domain)*

**Key points:**
- Manual work and human effort powered and controlled every process.
- Human observation → decision → action was the entire control loop.
- Limitations: fatigue, inconsistency, slow response, human error.

**How to picture it:** a worker checks a gauge, thinks, walks to a valve, adjusts it — slow, and error-prone under repetition. That single four-step loop (observe → decide → act → check) is the "control loop" every later technology in this document tries to speed up, tighten, and eventually remove the human from.

---

## 2. The First Industrial Revolution — Mechanization

Beginning around the 1760s, water and steam power began replacing human and animal muscle in production — this is **Industry 1.0**.

![Boulton and Watt steam engine](https://upload.wikimedia.org/wikipedia/commons/3/35/Boulton_and_Watt_Steam_Engine_(7494783616).jpg)
*A Boulton and Watt steam engine — the power source that made mechanized factories possible. (Source: [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Boulton_and_Watt_Steam_Engine_(7494783616).jpg), CC BY-SA 2.0)*

**Key points:**
- Water and steam power replaced muscle as the primary energy source.
- Textile machinery mechanized spinning and weaving for the first time.
- Factories concentrated production around a single shared power source.
- Industry wanted **speed and repeatability** — but control was still 100% manual.

**Important distinction:** mechanization replaced *muscle*. A human was still the controller — pulling levers, watching gauges, making every correction by hand. Automation, which replaces *judgment*, comes later. Confusing these two is the most common mistake in popular explanations of this history.

---

## 3. The Beginning of Automatic Control — Watt's Governor

This is the single most important idea in the entire history of automation: **feedback**.

![Centrifugal flyball governor](https://upload.wikimedia.org/wikipedia/commons/2/2b/Centrifugal_flyball_governor.jpg)
*A centrifugal (flyball) governor, Science Museum, London. (Source: [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Centrifugal_flyball_governor.jpg), CC BY 3.0)*

**Key points:**
- What is feedback? A correction based on a measurement of the system's own output.
- James Watt adapted the centrifugal (flyball) governor to his steam engine in 1788.
- Why it was revolutionary: for the first time, a machine corrected itself with no person in the loop.

**How it works, in words:** as engine speed increases, the spinning flyballs swing outward under centrifugal force. That mechanical motion pulls a linkage that closes the steam valve slightly. Steam input drops, and the engine slows back down. When speed drops too far, the flyballs fall inward, the valve opens again, and steam input increases. The engine continuously "hunts" around its target speed — measuring, comparing, and correcting, forever, without a human hand.

> The same fundamental idea lives inside every modern control loop: **measure → compare → correct → measure again.** Every PID loop, every PLC control block, and every SCADA alarm limit in later chapters of this repository is a direct descendant of this single 1788 mechanism.

---

## 4. The Jacquard Loom — Machines Begin Following Instructions

![Jacquard loom with punched cards, National Museum of Scotland](https://upload.wikimedia.org/wikipedia/commons/5/55/A_Jacquard_loom_showing_information_punchcards%2C_National_Museum_of_Scotland.jpg)
*A Jacquard loom showing its punched-card control mechanism. (Source: [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:A_Jacquard_loom_showing_information_punchcards,_National_Museum_of_Scotland.jpg))*

**Key points:**
- Joseph Marie Jacquard demonstrated his loom in Lyon, France, in 1801.
- Chained **punched cards** told the loom which threads to raise for each pass of the shuttle.
- The loom followed a **stored, repeatable sequence of instructions** instead of a weaver's constant judgment — unskilled workers could now weave complex, intricate patterns.

**Why it matters:** this is the conceptual bridge to *programmability*. Program → controller → output. Punched cards later inspired Charles Babbage's Analytical Engine and Herman Hollerith's tabulating machines — a direct line runs from this 1801 loom to the punched paper tape that programmed early PLCs 170 years later.

---

## 5. The Second Industrial Revolution — Electrification

From roughly the 1870s onward, **electricity** replaced steam and line-shaft power as the dominant means of driving machinery — this is **Industry 2.0**.

![Three-phase industrial electric motor](https://upload.wikimedia.org/wikipedia/commons/a/a5/Electric_motor_3-phase.JPG)
*A three-phase industrial electric motor — the component that let power be placed exactly where a process needed it. (Source: [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Electric_motor_3-phase.JPG), CC BY 2.5)*

**Key points:**
- Electric motors let power be placed wherever a process needed it, instead of everything running off one central steam engine via belts and shafts.
- Electrified factories enabled machine layout to follow process logic, not proximity to a boiler.
- Assembly-line production became achievable at real industrial scale.
- Electrical signals — unlike mechanical linkages — could travel instantly over wires and be **switched**, opening the door to electrical control.

This is the point where control starts to become *electrical* rather than purely mechanical: switches, relays, contactors, and motors enter the factory floor as standard equipment.

---

## 6. From Mechanical/Pneumatic Control to Electronic Control

Before digital electronics, industrial instruments were largely **pneumatic** (compressed-air signals) or simple analog-electrical devices. The invention of the **transistor** in 1947 changed that trajectory permanently.

![Replica of the first transistor, Bell Labs 1947](https://upload.wikimedia.org/wikipedia/commons/b/bf/Replica-of-first-transistor.jpg)
*A replica of the first point-contact transistor, invented at Bell Labs in December 1947 by Bardeen, Brattain, and Shockley. (Source: [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Replica-of-first-transistor.jpg))*

**Key points:**
- Pneumatic controllers used air pressure signals to represent process values — reliable, but slow and mechanically limited.
- Electrical instruments (analog meters, potentiometric recorders) added electrical sensing.
- The transistor, introduced in 1947, replaced bulky, fragile vacuum tubes with a small, reliable, low-power solid-state switch.
- Electronics began replacing many functions that mechanical and pneumatic devices used to perform — amplification, switching, and eventually logic.

The transistor is the single component that made everything from Section 7 onward — computers, PLCs, microprocessors — physically possible. Without a small, reliable, mass-producible switch, none of the digital control systems later in this document could exist.

---

## 7. The Rise of Process Control Computers

Computers began entering industrial control directly in the late 1950s, enabling digital measurement and centralized supervision of a process for the first time.

**Key points:**
- Computers took on digital measurement and centralized control roles previously split across many separate instruments.
- An early landmark: a computer directly supervised a live industrial process at the **Texaco refinery in Port Arthur, Texas, in 1959** — one of the first uses of a general-purpose computer for real-time industrial control.
- Why computers were needed: as processes grew larger and more interconnected, no single pneumatic or relay-based system could coordinate them all from one place.

This is the first appearance of the pattern that defines the rest of modern automation: **a computer sits above the physical control hardware, coordinating and supervising it**, rather than a person walking between separate local panels.

---

## 8. The Birth of the PLC (1960s–1970s)

This is one of the defining milestones in the entire history of industrial automation.

![Electromechanical relay control panel](https://upload.wikimedia.org/wikipedia/commons/5/56/Panel_Sender_Relays.jpg)
*A large electromechanical relay panel of the kind used to control complex systems before the PLC. (Source: [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Panel_Sender_Relays.jpg))*

**Key points — the exact sequence of events:**

1. **The problem:** relay control panels for complex machines had grown enormous — a wall of wired relays, expensive to build, difficult to modify, and hard to troubleshoot. Every logic change meant physically rewiring the panel.
2. **1968:** General Motors' Hydra-Matic (automatic transmission) division issued a request for an electronic replacement for hard-wired relay systems, based on a white paper by engineer Edward R. Clark.
3. **Bedford Associates**, a Massachusetts engineering firm led by **Dick Morley**, won the proposal.
4. The result, the **Modicon 084**, got its name simply because it was Bedford Associates' 84th project. "Modicon" stood for **Mo**dular **Di**gital **Con**troller.
5. A working prototype existed by March 1968; the first commercial unit was delivered to GM around 1969–1970.
6. The 084 was programmed in **Ladder Logic** — a language deliberately designed to *look like* the relay ladder diagrams electricians already knew how to read, which eased adoption on the factory floor.
7. Dick Morley is widely regarded as the **father of the PLC**. The 084's 1973 successor, the **Modicon 184**, became the real commercial breakthrough that displaced relay panels across industry at scale.

**What changed, in one sentence:** logic that used to require physically rewiring a cabinet full of relays could now be changed by editing and downloading a program — in minutes, with no rewiring at all.

---

## 9. The Third Industrial Revolution — Computerized Automation

The arrival of microprocessors, PLCs, CNC machines, and electronic instrumentation together defines **Industry 3.0** — automation built on electronics and computing rather than purely electromechanical hardware.

![CNC milling machine](https://upload.wikimedia.org/wikipedia/commons/f/f0/CNC_Mill_1.jpg)
*A modern CNC (Computer Numerical Control) machine — a direct descendant of the digital, program-driven control introduced with the PLC. (Source: [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:CNC_Mill_1.jpg), CC BY-SA 4.0)*

**Key points:**
- Microprocessors packed the logic of an entire computer onto a single chip, making digital controllers small and affordable.
- PLCs spread from automotive plants into nearly every discrete manufacturing industry.
- CNC (Computer Numerical Control) applied the same programmable-logic idea to precision machining — a program, not a human hand, now guides the cutting tool.
- Electronic instrumentation replaced most remaining pneumatic and purely mechanical measurement devices.

---

## 10. The Birth of the DCS

The PLC excelled at discrete, sequential control (start/stop, step sequences, interlocks). Large **continuous** processes — refineries, chemical plants, power generation — needed something architecturally different.

**Key points:**
- Why PLCs weren't the whole answer: continuous processes (temperature, flow, pressure control across hundreds of loops) needed control **distributed** across many process areas, with one unified operator view — not one central box.
- **1975:** Honeywell introduced the **TDC 2000**, widely credited as the first commercial **Distributed Control System (DCS)**.
- **Yokogawa's** early **CENTUM** system appeared around the same period.
- A DCS spreads many controllers across the plant — each handling a portion of the process — all tied together through shared communication and a common operator display.

**PLC vs. DCS, in one line:** PLCs grew up dominating discrete manufacturing (automotive, packaging, machining); DCS grew up dominating continuous process industries (oil & gas, chemicals, power) — largely in parallel, solving different problems. Modern systems increasingly blur this line, but understanding *why* each was invented explains a great deal about plant architecture today.

---

## 11. HMI, SCADA and Industrial Networking

As plants filled with PLCs and DCS nodes, a new problem appeared: these "islands of automation" needed a way for a human to see and supervise all of them at once.

![SCADA operator at a control console, US Navy](https://upload.wikimedia.org/wikipedia/commons/0/0f/US_Navy_110328-N-OJ170-009_Hideji_Kawasaki_operates_the_supervisory_control_and_data_acquisition_(SCADA)_system_to_balance_an_electrical_load_insid.jpg)
*An operator using a supervisory control and data acquisition (SCADA) console. (Source: [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:US_Navy_110328-N-OJ170-009_Hideji_Kawasaki_operates_the_supervisory_control_and_data_acquisition_(SCADA)_system_to_balance_an_electrical_load_insid.jpg), public domain, US Navy)*

**Key points:**
- **HMI (Human-Machine Interface)** gave an operator a visual window into a machine or process — replacing physical gauges and indicator lamps with screens.
- **SCADA (Supervisory Control and Data Acquisition)** extended this across an entire site or region, monitoring and issuing commands to many remote controllers from one control room.
- The transition ran from local, panel-by-panel control, to centralized computer control, to distributed control, to fully networked supervisory control — each stage removing the need for a person to physically travel between separate panels.

---

## 12. Fieldbus and Open Communication

![DeviceNet fieldbus components](https://upload.wikimedia.org/wikipedia/commons/e/ee/DeviceNet_Components_067.jpg)
*DeviceNet fieldbus components — devices sharing one digital line instead of individual point-to-point wiring. (Source: [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:DeviceNet_Components_067.jpg))*

Wiring every single sensor and actuator back to a controller with its own dedicated pair of wires does not scale once a plant has thousands of I/O points.

**Key points:**
- The problem: point-to-point wiring became enormously expensive and physically impractical at scale.
- **Fieldbus** technologies let many devices share a single digital communication line instead of each needing its own wires.
- Device-to-device digital communication meant a sensor could report far more than a single raw value — it could report diagnostics, status, and configuration data too.
- This is the direct ancestor of the layered industrial network architectures used throughout modern plants.

---

## 13. Ethernet and Industrial Ethernet

![Industrial Ethernet switch](https://upload.wikimedia.org/wikipedia/commons/c/c4/Siemens_ESM_TP80.JPG)
*A ruggedized industrial Ethernet switch — standard networking hardware adapted for the factory floor. (Source: [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Siemens_ESM_TP80.JPG))*

Standard **Ethernet** and **TCP/IP** — already dominant in office computing — moved onto the factory floor in the 2000s, ruggedized and adapted for real-time determinism, but fundamentally the same networking technology used everywhere else.

**Key points:**
- Industrial Ethernet let controllers, drives, and I/O share one common physical network instead of dozens of incompatible proprietary fieldbuses.
- Protocols such as **EtherNet/IP**, **PROFINET**, and **Modbus TCP** became the industrial standard.
- This created the now-familiar layered architecture: Sensor → I/O → PLC → Industrial Ethernet → HMI/SCADA → Historian → MES → ERP — connecting the factory floor all the way up to business systems.

---

## 14. Industry 4.0

![Industrial robots palletizing in a smart factory](https://upload.wikimedia.org/wikipedia/commons/2/22/Factory_Automation_Robotics_Palettizing_Bread.jpg)
*Articulated robots performing coordinated, sensor-driven palletizing — a modern smart-factory cell. (Source: [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Factory_Automation_Robotics_Palettizing_Bread.jpg))*

The term **Industry 4.0** emerged around 2011, describing a shift from automation that simply *executes* instructions to automation that **senses, connects, analyzes, and decides.**

**Key points:**
- **Cyber-physical systems** — physical machines tightly coupled with their digital models.
- **IIoT (Industrial Internet of Things)** — sensors and devices connected far beyond the traditional control network.
- **Edge and cloud computing** — processing data close to the source (edge) or centrally at scale (cloud).
- **Big data and machine learning** — using historical and real-time data to predict, optimize, and increasingly support or automate decisions.
- **Smart, connected factories** — where OT (operational technology) and IT (information technology) converge.

---

## 15. Automation Today

![Modern automated manufacturing line](https://upload.wikimedia.org/wikipedia/commons/e/eb/Manufacturing_line.jpg)
*A modern, highly automated manufacturing line — the accumulated result of every stage in this chapter, layered together. (Source: [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Manufacturing_line.jpg), CC BY-SA 4.0)*

Every layer described in this chapter is still in active use somewhere in industry today. Relay logic still runs simple machines. PLCs still run most discrete manufacturing. DCS still runs most continuous process plants. All of it is increasingly wrapped in IIoT connectivity, edge analytics, and AI-assisted decision support — layered **on top of**, not replacing, the control fundamentals built over the last 250 years.

**Key points:**
- Integration of OT and IT is now the central engineering challenge in most plants.
- Smart, connected, data-driven operation is the baseline expectation for a modern factory.
- Focus has shifted toward flexibility, sustainability, and efficiency — not just raw automation.

---

## 16. What Actually Changed at Every Stage?

The most important question in this chapter is not "what was invented?" but **"what problem did it solve, and what changed as a result?"**

| Era | Main Technology | Human Role | Major Limitation Solved |
|---|---|---|---|
| Manual | Human operation | High | — |
| Mechanization | Steam / water power | High | Physical effort |
| Feedback | Watt's governor | Medium | Manual speed/process correction |
| Electrification | Electric power, motors | Medium | Production limitations, fixed layout |
| Relay Control | Electromechanical logic | Lower | Repetitive sequential control |
| PLC | Programmable electronics | Lower | Hard-wired logic, slow changes |
| DCS / SCADA | Distributed computing | Supervisory | Large-scale continuous control |
| Industrial Networks | Digital communication | Supervisory | Point-to-point wiring cost |
| Industry 4.0 | Cyber-physical / IIoT | Data-driven | Integration & intelligence |

---

## 17. The First-Principles Mental Model

Every major evolution in the history of industrial automation was an attempt to improve one or more of the following:

```
Human effort → Repeatability → Speed → Precision → Safety → Flexibility → Information → Connectivity → Intelligence
```

> **Important correction to keep in mind:** don't treat "Industry 1.0 → 2.0 → 3.0 → 4.0" as if those four labels *are* the entire history. They're a useful coarse framework, but automation also evolved through feedback control, instrumentation, electronics, PLCs, DCS, networking, and SCADA — technologies that developed across overlapping periods, not a single straight line.

That progression is the single idea worth carrying into every later chapter of this repository: automation is not a list of inventions — it is a continuous, 250-year effort to remove one specific limitation at a time.

---

## 18. Evolution Timeline (Year Scale)

| Year | Milestone | Era |
|---|---|---|
| 1760s–1770s | Steam power (1st Industrial Revolution) | Mechanical |
| 1788 | Watt's governor — feedback control begins | Mechanical |
| 1801 | Jacquard loom — punched-card programmability | Mechanical |
| 1870s | Electricity & electric motors (2nd Industrial Revolution) | Electrical |
| 1913 | Ford moving assembly line | Electrical |
| 1947 | Transistor invented (Bell Labs) | Electronic |
| 1959 | First process control computer (Texaco Port Arthur) | Electronic |
| 1968 | Modicon 084 — the first PLC developed | Digital |
| 1970 | First commercial PLC delivered to GM | Digital |
| 1975 | DCS commercialized (Honeywell TDC 2000) | Digital |
| 1980s–1990s | Fieldbus & industrial networking | Digital |
| 2000s | Industrial Ethernet & plant-wide connectivity | Digital |
| 2011 | Industry 4.0 | Smart / Connected |
| 2010s–today | IIoT, edge computing, AI-assisted automation | Smart / Connected |

**One diagram worth keeping, because the whole chapter compresses into it:**

```mermaid
flowchart LR
    A[Human Control] --> B[Mechanization]
    B --> C[Feedback / Watt's Governor]
    C --> D[Electrification]
    D --> E[Relay Logic]
    E --> F[PLC]
    F --> G[DCS / SCADA]
    G --> H[Industrial Networking]
    H --> I[IIoT / Industry 4.0]
```

---

## 19. References

- ISA (International Society of Automation) — timeline of automation milestones and the Industrial Revolution framework.
- Schneider Electric — official history of the Modicon PLC and Dick Morley.
- Computer History Museum, *The Storage Engine* — punched cards and the Jacquard loom.
- *Programmable Logic Controller*, Wikipedia — Modicon 084 origin and the GM Hydra-Matic request for proposals.
- *Centrifugal Governor*, Wikipedia — James Watt's 1788 governor.
- *History of the Transistor*, Wikipedia — Bell Labs, 1947.
- AutomationDirect Library — "History of the PLC" and "What is a Relay?"
- Wikimedia Commons — public-domain and Creative-Commons-licensed historical photographs (credited individually above each image).

---

**Previous:** [01 — What Is Industrial Automation](01-what-is-industrial-automation.md)  
**Next:** [03 — Why Industrial Automation Exists](03-why-industrial-automation-exists.md)

