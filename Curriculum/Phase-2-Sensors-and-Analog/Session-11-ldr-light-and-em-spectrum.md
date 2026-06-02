# Week 6, Session 11 — Light Sensors (LDR) & the EM Spectrum

**Phase:** Phase 2 — Sensors & Analog Signals
**Session Number:** 11 of 36
**Week:** 6

---

## Learning Objectives

By the end of this session, students will be able to:
- Describe the electromagnetic spectrum and locate visible light within it.
- Explain how an LDR (Light-Dependent Resistor) changes resistance based on light intensity.
- Build a voltage divider circuit using an LDR and a 10 kΩ fixed resistor.
- Read light level values using `analogRead()` on A0 and observe how they change with light/dark conditions.

---

## Science Curriculum Link

**Grade 8 Concept: Electromagnetic Spectrum & Light**

The electromagnetic spectrum is the range of all electromagnetic radiation, from radio waves to gamma rays. Visible light is a narrow band of this spectrum with wavelengths roughly 380–700 nm. **Light intensity** — the amount of light energy hitting a surface — affects certain materials. An LDR (photoresistor) is made of a semiconductor material whose resistance decreases as more photons hit it, freeing charge carriers. This session gives students a tangible, measurable experience of light intensity and connects the abstract concept of the EM spectrum to a working sensor.

---

## Materials Checklist (per pair)

- 1 × Arduino Mega 2560
- 1 × USB-A to USB-B cable
- 1 × Solderless breadboard
- 1 × LDR (photoresistor)
- 1 × 10 kΩ resistor (brown-black-orange-gold)
- 1 × LED (any color)
- 1 × 220 Ω resistor (red-red-brown-gold)
- Jumper wires: red × 2, black × 2, yellow × 1, orange × 1
- Computer with Arduino IDE installed
- (Optional) a phone flashlight or a piece of cardboard to create shade

---

## 2. Teacher Guide (45-Minute Breakdown)

### 0–5 min — Hook / Warm-Up

Turn off the classroom lights for 15 seconds (or block the windows). Ask:

*"What just changed in this room when I turned off the lights? Think about how your eyes adjusted. What do you think happened to the electricity running through a light-sensing circuit right now?"*

Turn the lights back on. Expected answers: less light, pupils dilated, can't see as well.

*"A device called an LDR — a Light-Dependent Resistor — changes its electrical resistance depending on how much light hits it. In the dark, its resistance is very high (maybe 1 million ohms!). In bright light, its resistance drops to just a few hundred ohms. Today we're going to measure that change and connect it to something amazing: the electromagnetic spectrum."*

---

### 5–15 min — Direct Instruction

**The Electromagnetic Spectrum:**

Draw a spectrum on the board (or project an image):

```
← longer wavelength / lower frequency / lower energy ───────────────────────────── shorter wavelength / higher frequency / higher energy →

[ Radio | Microwave | Infrared | VISIBLE | UV | X-ray | Gamma ]
                               (380–700 nm)
```

> "The electromagnetic spectrum is the full family of light — from radio waves (used by your phone) all the way to gamma rays (emitted by nuclear reactions). We can only see a tiny slice: wavelengths from about 380 nanometers (violet) to 700 nanometers (red). A nanometer is one billionth of a meter — incredibly small."

> "Our LDR is sensitive mostly to visible light. It's made of a semiconductor material called cadmium sulfide (CdS). When photons — particles of light — hit the material, they knock electrons loose, which reduces the resistance. More light = more free electrons = LOWER resistance."

**Write on the board:**
```
Bright light  →  LDR resistance LOW  →  more current flows  →  higher voltage at wiper
Dark          →  LDR resistance HIGH →  less current flows  →  lower voltage at wiper
```

**The voltage divider with the LDR:**

Draw on the board:
```
5V ──── [LDR] ──── A0 ──── [10 kΩ] ──── GND
```

> "We put a fixed 10 kΩ resistor in series with the LDR. As the LDR's resistance changes, the voltage at A0 (the junction between them) changes too. This is the same voltage divider we learned last session — but now one of the resistors is changing based on light!"

> "This is why we need the fixed 10 kΩ resistor: if we just connected the LDR directly to 5V and A0 with nothing else, we'd have no voltage division — the reading would either saturate at 1023 or be unstable."

---

### 15–35 min — Hands-On Build + Code

**Remind students:** *"Build → Check → Power on. Unplug USB before wiring."*

1. Unplug the Arduino.
2. Place the **LDR** in the breadboard (it has no polarity — either leg direction works).
3. Connect one leg of the LDR to the **5V rail** (red wire).
4. Connect the other leg of the LDR to a breadboard row — this is the junction point (A0).
5. Place the **10 kΩ resistor** between the junction row and the **GND rail**.
6. Connect the junction row (between LDR and resistor) to **Arduino A0** (yellow wire).
7. Place the **LED** in the breadboard: long leg (anode) toward pin 9, short leg (cathode) toward GND.
8. Place the **220 Ω resistor** between **Arduino pin 9** and the LED's long leg.
9. Connect LED's short leg to **GND rail** (black wire).
10. Partner double-check: Is 5V on one LDR leg? 10 kΩ to GND from the other? Junction to A0?
11. Plug in USB. Enter and upload the code from Section 4.
12. Open the Serial Monitor (9600 baud).
13. **Experiment:** Cover the LDR with your hand. Watch the values drop. Shine a phone flashlight. Watch values rise.
14. Record values in your worksheet data table.

---

### 35–42 min — Testing & Debugging

**What success looks like:**
- Serial Monitor shows values changing clearly when the LDR is covered vs exposed to light.
- Values in bright light: typically 600–950 (varies by room brightness and sensor).
- Values in darkness (hand covering LDR): typically 50–300.
- LED responds to light level (dimmer in dark, brighter in light, or vice versa per code).

**Troubleshooting Table:**

| Symptom | Fix |
|---------|-----|
| Values don't change when covering the LDR | Check that the junction wire is in A0 (not 5V or GND). Check LDR legs are in separate rows. |
| Values stuck at 0 | The 10 kΩ resistor may be connected to 5V instead of GND (swap it). |
| Values stuck at ~1023 | The LDR may be connected directly to GND instead of 5V. Check the top connection. |
| Values are very low even in bright light | LDR and 10 kΩ may be swapped — 5V should go through the LDR first, then the fixed resistor to GND. |
| No response from LED | Check pin 9 wiring and LED polarity. Verify code uses `analogWrite`. |

---

### 42–45 min — Reflection / Exit Ticket

**Exit ticket questions:**

1. *"Why does covering the LDR with your hand make the `analogRead()` value go DOWN? Trace through the circuit logic."*
2. *"Name two types of electromagnetic radiation that are NOT visible light."*
3. *"What would happen to the circuit if you replaced the 10 kΩ fixed resistor with a 100 Ω resistor? Would the readings be the same in light and dark?"*

---

## 3. Circuit Diagram

### ASCII Wiring Diagram

```
   Arduino Mega 2560
   ┌──────────────────────┐
   │                      │
   │  5V ─────────────────┼──── Red rail (+)
   │  GND ────────────────┼──── Black rail (–)
   │                      │
   │  A0 ─────────────────┼──── Junction point J1
   │                      │
   │  Pin 9 (~) ──────────┼──── [220 Ω] ──── LED (+) ──── LED (–) ──── GND
   └──────────────────────┘
   
   Voltage Divider (LDR + 10 kΩ):
   
   Red rail (5V) ──── LDR (any orientation) ──── J1 ──── 10 kΩ ──── Black rail (GND)
                                                  │
                                                  └──── Arduino A0 (yellow wire)
```

### Fritzing-Style Build Description

| Step | From | To | Wire Color | Notes |
|------|------|----|------------|-------|
| 1 | Arduino **5V** | Breadboard **positive (+) rail** | Red | Power |
| 2 | Arduino **GND** | Breadboard **negative (–) rail** | Black | Ground |
| 3 | Breadboard **+rail** | LDR **leg 1** (row 8) | Red | 5V through LDR |
| 4 | LDR **leg 2** (row 10) | 10 kΩ resistor **leg 1** (same row 10) | — | Junction point |
| 5 | 10 kΩ resistor **leg 2** | Breadboard **–rail** | — | Fixed R to GND |
| 6 | Row 10 (junction) | Arduino **A0** | Yellow | Signal to ADC |
| 7 | Arduino **Pin 9** | 220 Ω resistor leg 1 (row 20) | Orange | LED signal |
| 8 | 220 Ω resistor leg 2 | LED **long leg (anode)** row 22 | — | |
| 9 | LED **short leg (cathode)** | Breadboard **–rail** | Black | |

**Component values:** LDR (any CdS photoresistor), 10 kΩ resistor (brown-black-orange-gold), 220 Ω resistor (red-red-brown-gold).

[Google image search: "Arduino LDR photoresistor voltage divider circuit"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 * SESSION 11: Light Sensors (LDR) & the EM Spectrum
 * Arduino Mega 2560 — Grade 8 STEM Curriculum
 * ============================================================
 *
 * SCIENCE EXPLANATION:
 * ------------------------------------------------------------------
 * An LDR (Light-Dependent Resistor / photoresistor) is made of a
 * semiconductor (usually cadmium sulfide, CdS) whose resistance
 * DECREASES when light shines on it.
 *
 * Physics: photons (particles of light) knock electrons free in
 * the material → more free electrons → lower resistance → more
 * current can flow.
 *
 *   Bright light  →  LDR resistance LOW  (~100–1,000 Ω)
 *   Dark          →  LDR resistance HIGH (~100,000–1,000,000 Ω)
 *
 * We connect the LDR + 10 kΩ resistor as a VOLTAGE DIVIDER:
 *   5V → LDR → A0 → 10 kΩ → GND
 *
 * As light increases, LDR resistance drops → more voltage falls
 * across the fixed 10 kΩ → A0 voltage rises → ADC reading rises.
 *
 * This directly demonstrates: LIGHT INTENSITY affects resistance.
 * ------------------------------------------------------------------
 *
 * Circuit:  LDR + 10 kΩ voltage divider on A0
 *           LED + 220 Ω on pin 9 (brightness follows light level)
 * ============================================================
 */

// ---- Pin Definitions ----
const int LDR_PIN = A0;   // LDR voltage divider output connected here
const int LED_PIN = 9;    // PWM-capable pin for LED (optional visual output)

void setup() {
  Serial.begin(9600);        // Open serial communication
  pinMode(LED_PIN, OUTPUT);  // LED pin as output

  Serial.println("=== Session 11: LDR Light Sensor ===");
  Serial.println("Cover the LDR with your hand, then shine a light on it.");
  Serial.println("-------------------------------------------");
}

void loop() {
  // --- Read the light sensor ---
  int lightLevel = analogRead(LDR_PIN);  // 0 = dark, 1023 = bright
                                          // (with our 5V-LDR-A0-10k-GND divider)

  // --- Map to LED brightness ---
  // When it's brighter, the LED gets brighter too (mirror effect)
  int brightness = map(lightLevel, 0, 1023, 0, 255);
  brightness = constrain(brightness, 0, 255);  // Safety clamp
  analogWrite(LED_PIN, brightness);             // Set LED brightness

  // --- Calculate approximate voltage ---
  float voltage = (lightLevel / 1023.0) * 5.0;

  // --- Describe light level in words ---
  String description;
  if (lightLevel > 700) {
    description = "BRIGHT";       // Strong light (flashlight, sunlight)
  } else if (lightLevel > 400) {
    description = "MEDIUM";       // Normal room lighting
  } else if (lightLevel > 150) {
    description = "DIM";          // Dim room, shade
  } else {
    description = "DARK";         // Hand covering sensor, night
  }

  // --- Print all data to Serial Monitor ---
  Serial.print("Light: ");
  Serial.print(lightLevel);         // Raw 0–1023 value
  Serial.print("  | ");
  Serial.print(voltage, 2);
  Serial.print(" V  | ");
  Serial.println(description);      // Text description of light level

  // >>> TRY CHANGING THIS: adjust the thresholds (700, 400, 150) to match
  // your actual classroom lighting. Cover the LDR, note the "dark" value,
  // and set the bottom threshold just above it.

  delay(200);  // Read every 200 ms
}

// ===== CHALLENGE =====
// 1. CALIBRATION: Find your actual min (hand over LDR) and max
//    (flashlight directly on LDR) values. Use them in map() instead
//    of 0 and 1023 for a more accurate percentage reading:
//    int pct = map(lightLevel, YOUR_MIN, YOUR_MAX, 0, 100);
//    Serial.print(pct); Serial.println("%");
//
// 2. INVERSE MODE: Make the LED get BRIGHTER when it is DARK (like a
//    night light!). Change the map() line to:
//    int brightness = map(lightLevel, 0, 1023, 255, 0);
//
// 3. SERIAL PLOTTER: Open Tools → Serial Plotter instead of Serial Monitor.
//    Slowly move your hand toward and away from the LDR. Describe
//    the shape of the graph in your worksheet.
//
// 4. EM SPECTRUM EXPERIMENT: Use colored cellophane (or colored markers
//    over the sensor). Does the LDR respond differently to different
//    colors? What does that tell you about light energy and wavelength?
```

---

## 5. Student Worksheet

---

### Session 11 Worksheet — Light Sensors (LDR) & the EM Spectrum

**Name(s):** _________________________________ **Date:** _____________ **Kit #:** _____

**Objectives:**
- Understand the electromagnetic spectrum and locate visible light within it.
- Explain how an LDR changes resistance with light.
- Read and interpret light level data from the Serial Monitor.

---

#### What I Already Know (Warm-Up)

1. From Session 10, how does a voltage divider circuit work? What is connected between 5V and GND?

   ___________________________________________________________________________

2. Name two types of electromagnetic radiation that humans cannot see:

   ___________________________________________________________________________

3. If the LDR's resistance goes DOWN in bright light, what do you think happens to the voltage at A0 in our circuit (does it go up or down)?

   ___________________________________________________________________________

---

#### Build It — Step by Step

Unplug USB before wiring.

1. Place the **LDR** in the breadboard spanning the center gap.
2. One LDR leg → **positive (+) rail** (red wire from 5V).
3. Other LDR leg → a breadboard row (this will be the junction). Connect a **yellow wire** from this row to **Arduino A0**.
4. From the same junction row, place the **10 kΩ resistor** to the **GND (–) rail**.
5. Place the **LED** with long leg toward **pin 9**, short leg toward **GND**.
6. **220 Ω resistor** between **pin 9** and the LED's long leg.
7. LED's short leg → **GND rail** (black wire).
8. Partner checks all wiring. Plug in USB.
9. Type and upload the code. Open Serial Monitor at **9600 baud**.

---

#### Predict!

Before you start experimenting, write your predictions:

| Condition | Predicted raw value (higher or lower?) | Actual value |
|-----------|----------------------------------------|--------------|
| Normal room light (baseline) | | |
| Cover LDR completely with both hands | | |
| Shine phone flashlight directly on LDR | | |
| Hold LDR next to a window on a cloudy day | | |

---

#### Observe / Data Table

Try each condition for 10 seconds. Record the highest and lowest values you see, then average them.

| Light Condition | Low reading | High reading | Average | Voltage (V) | Description |
|-----------------|-------------|--------------|---------|-------------|-------------|
| Room light (normal) | | | | | |
| Hand covering LDR | | | | | |
| Phone flashlight on LDR | | | | | |
| Colored cellophane (if available) | | | | | |

Voltage formula: Voltage = (Average ÷ 1023) × 5.0

---

#### What Did You Notice?

1. As light increased, did the raw `analogRead()` value go UP or DOWN? Does this match your prediction?

   ___________________________________________________________________________

2. Looking at your data, what is the approximate range of values in your classroom's normal lighting? ________ to ________

3. Why do we use a fixed 10 kΩ resistor alongside the LDR? What would happen without it?

   ___________________________________________________________________________

4. The LED followed the light level. Describe what happened when you covered the LDR:

   ___________________________________________________________________________

---

#### Science Connection

The electromagnetic spectrum arranges all radiation by wavelength. Label this diagram with the types of radiation in the correct order:

```
Long wavelength                                              Short wavelength
Low energy                                                    High energy
─────────────────────────────────────────────────────────────────────────────
[ _____ ]  [ _____ ]  [ _____ ]  [ VISIBLE ]  [ _____ ]  [ _____ ]  [ _____ ]
```

*(Choose from: Radio, Microwave, Infrared, Ultraviolet, X-ray, Gamma)*

**Question:** The LDR is sensitive to visible light (roughly 400–700 nm). Infrared light has wavelengths longer than 700 nm. Do you think the LDR could detect a TV remote (which uses infrared) in the same way? Why or why not?

___________________________________________________________________________

---

#### Challenge Extension

**Color experiment:** Find three pieces of colored cellophane (or colored transparent folders) — red, green, and blue. Hold each one over the LDR and record the value. Does the LDR respond equally to all colors?

| Filter Color | Raw Value | Compared to no filter |
|-------------|-----------|----------------------|
| No filter | | |
| Red filter | | |
| Green filter | | |
| Blue filter | | |

What does this tell you about how LDRs respond to different wavelengths of light?

___________________________________________________________________________

---

## 6. Safety Notes

| Hazard | Precaution |
|--------|------------|
| LDR polarity | LDRs have NO polarity — either leg orientation works. No risk of damage from reversing. |
| Phone flashlight | Shining a flashlight at the LDR is fine. Do not shine it directly into anyone's eyes. |
| Short circuit | Double-check: 5V on one LDR leg, 10 kΩ to GND from the other. Never connect 5V and GND to the same point. |
| CdS material | The LDR contains cadmium sulfide — it is sealed inside. Do not break it. Wash hands after handling. |

**Build → Check → Power on.**

**If something goes wrong:**
- Values don't respond to light → re-check that the junction (between LDR and 10 kΩ) is connected to A0.
- Board feels warm → check for short circuits; unplug immediately.

---

## 7. Assessment Rubric

### Formative Check (not graded)

| Look-For | Not Yet | Got It |
|----------|---------|--------|
| Student can correctly name the type of circuit (voltage divider) | | |
| Values on Serial Monitor change noticeably between bright and dark conditions | | |
| Student can explain in words why covering the LDR changes the reading | | |
| Student can locate visible light on the EM spectrum | | |
| Student correctly calculates voltage from a raw reading | | |

---

## 8. Differentiation

### Support
- Provide a printed EM spectrum poster at each workstation so students can reference it during the worksheet.
- Pre-build the voltage divider section; let supported students focus on the code and observation.
- Provide a sentence frame for the science connection: *"When light shines on the LDR, the resistance ________, which means more/less voltage reaches A0, so the reading goes ______."*
- Use Tinkercad to simulate the LDR circuit before handling real components.

### Extension
- Challenge: build a "light-controlled servo" preview — log min and max light values, then map them to a 0°–180° servo angle (code only, no servo hardware yet).
- Research: *"How does a digital camera sensor work? Is it more like an LDR or more like an ADC?"*
- Code extension: write a function `float lightPercent()` that returns a calibrated 0–100% value and call it from `loop()`.
- Add a second LDR on A1. Print both readings side by side. Shine light on just one. What happens?

### Visual / Kinesthetic Accommodations
- The Serial Plotter (Tools → Serial Plotter) provides a real-time graph — highly effective for visual learners who find numbers less meaningful.
- Physical demonstration: have students slowly move a flashlight from 2 meters away toward the LDR while watching the Serial Plotter trace a curve upward.
- Post a spectrum diagram (printed in color) on the wall. When discussing wavelength, physically point to where visible light sits.
- Key vocabulary cards (with pictures) at each desk: *LDR, photon, resistance, voltage divider, electromagnetic spectrum.*
