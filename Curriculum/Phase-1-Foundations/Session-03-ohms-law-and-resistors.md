# Week 2, Session 3 — Ohm's Law & Resistors

**Phase:** Phase 1 — Foundations

**Learning Objectives:**
- State Ohm's Law (V = I × R) and use it to calculate current in a simple LED circuit
- Read a 4-band resistor color code and identify the resistance value of a given resistor
- Predict and observe how changing the resistor value changes LED brightness
- Explain why a current-limiting resistor is essential for protecting an LED

**Science Curriculum Link:** Grade 8 Electricity — Voltage, Current, and Resistance (V = I × R). This is the mathematical heart of all circuit work. By measuring LED brightness as a proxy for current, students see Ohm's Law in action: higher resistance → lower current → dimmer LED. The formula they apply here will reappear throughout the course wherever sensors, motors, and components are connected.

**Materials Checklist (per pair):**
- 1 × Arduino Mega 2560 + USB cable
- 1 × Solderless breadboard
- 1 × Red LED (5mm)
- 1 × 220 Ω resistor (red-red-brown)
- 1 × 1 kΩ resistor (brown-black-red)
- 1 × 10 kΩ resistor (brown-black-orange)
- 4 × Jumper wires (red + black, M-M)
- 1 × Laptop with Arduino IDE 2.x
- 1 × Resistor color-code reference card (printed)
- 1 × Multimeter (shared — 1 per 4 students)

---

## 2. Teacher Guide

### 0–5 min — Hook / Warm-Up

**Set up in advance:** On the teacher demo board, have the same LED circuit from Session 2 but secretly swap the 220Ω resistor for a 10kΩ one before class. When students come in, the code is already running and the LED is barely visible.

**Ask the class:** "Last session this LED was blinking brightly. Today it looks almost off. I have not changed the code at all. The LED is not broken. What could have changed to make it so dim?"

Take ideas (2–3 responses). Students may guess: "less voltage?", "LED is old?", or "different resistor?"

**Reveal:** Change the resistor back to 220Ω live in front of the class (with USB unplugged, of course). The LED is suddenly bright again.

"The ONLY thing I changed was the resistor. Bigger resistance → less current → dimmer LED. That relationship is Ohm's Law, and you are going to prove it yourself today."

---

### 5–15 min — Direct Instruction

**Concept 1: The three electrical quantities**

"In any circuit, three quantities are always related to each other:

- **Voltage (V)** — measured in volts (V). This is the electrical pressure that pushes current through the circuit. Our Arduino provides 5V.
- **Current (I)** — measured in amperes or amps (A). This is the amount of charge flowing per second. For our LED circuits, we use milliamps (mA). A safe LED current is 10–20 mA.
- **Resistance (R)** — measured in ohms (Ω). This is anything that opposes the flow of current. Resistors are components designed specifically to add resistance."

**Concept 2: Ohm's Law**

"A German physicist named Georg Ohm discovered in 1827 that these three quantities are linked by a beautifully simple formula:

**V = I × R**

Or rearranged: **I = V ÷ R**

Let us use it. Our circuit has 5V from the Arduino pin. An LED 'uses up' about 2V across itself. So the voltage that the resistor sees is 5 − 2 = 3V. The current through the LED is:"

Write on board:

```
  I = V ÷ R
  I = 3V ÷ 220Ω = 0.0136 A = 13.6 mA    ← with 220Ω resistor (bright LED)
  I = 3V ÷ 1000Ω = 0.003 A  = 3 mA      ← with 1kΩ resistor (dimmer)
  I = 3V ÷ 10000Ω = 0.0003 A = 0.3 mA   ← with 10kΩ resistor (very dim / barely visible)
```

"So three times more resistance means three times less current means a much dimmer LED. You will see this with your own eyes today."

**Concept 3: Resistor color codes**

Display a resistor color code poster or draw this on the board:

```
  4-BAND RESISTOR COLOR CODE
  Band 1 (1st digit) | Band 2 (2nd digit) | Band 3 (multiplier) | Band 4 (tolerance)

  Black = 0    Brown = 1    Red = 2    Orange = 3    Yellow = 4
  Green = 5    Blue  = 6    Violet= 7  Gray  = 8    White  = 9

  Multiplier: Black=×1  Brown=×10  Red=×100  Orange=×1000

  Example: Red - Red - Brown - Gold
           2  -  2  - ×10   - ±5%   = 220Ω ± 5%

  Example: Brown - Black - Red - Gold
           1     -  0    - ×100 - ±5%  = 1000Ω = 1kΩ

  Example: Brown - Black - Orange - Gold
           1     -  0    - ×1000  - ±5% = 10000Ω = 10kΩ
```

Have students identify each of their three resistors by color code before the build starts.

---

### 15–35 min — Hands-On Build + Code

**SAFETY: USB unplugged. Build → Check → Power on.**

This session uses the same basic circuit as Session 2. Students will swap the resistor three times during the experiment.

**Build Steps:**

1. Build the same circuit as Session 2 (LED on pin 8, 220Ω resistor, red/black wires).
2. Verify it works: upload the code (constant ON this session, not blinking — see Section 4).
3. **Partner reads aloud:** "LED with 220Ω — observe brightness. Record in data table as a rating 1 (dim) to 5 (bright)."
4. **Swap resistor:** Power down (unplug USB). Remove 220Ω. Insert 1kΩ. Power on. Observe. Record.
5. **Swap again:** Power down. Remove 1kΩ. Insert 10kΩ. Power on. Observe. Record.
6. Optional: remove the resistor entirely. **Ask students to predict** what will happen. Then: **DO NOT do this without teacher supervision** — the LED will likely burn out. Teacher demo only, or simply discuss what would happen theoretically.

**Ohm's Law Calculation Practice:**

While partners take turns swapping resistors, the other partner completes the Ohm's Law calculation table on the worksheet. Teacher circulates to check work.

---

### 35–42 min — Testing & Debugging

**Success looks like:** LED is brightly lit with 220Ω, noticeably dimmer with 1kΩ, barely or not visible with 10kΩ.

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| LED looks the same brightness with all resistors | Circuit has a wiring error bypassing the resistor | Power down; check that the resistor is in the same row as the red wire (not a direct wire to the LED) |
| LED does not light with any resistor | LED backwards, or code not uploaded | Power down, check LED polarity; re-upload code |
| LED is very dim even with 220Ω | Using the 10kΩ resistor by mistake | Read color bands again — confirm red-red-brown for 220Ω |
| LED stays on constantly during swap test | Code written as constant ON — this is correct for this session | Confirm code outputs constant HIGH, not blink |
| Can't tell 220Ω from 1kΩ by color | Color code reading difficulty | Use the multimeter in Ohm mode to measure each resistor |

---

### 42–45 min — Reflection / Exit Ticket

**Students answer:**

1. A circuit has 5V and a 470Ω resistor (pretend the LED uses 0V for simplicity). Use Ohm's Law to calculate the current in milliamps. Show your work.
   *(Expected: I = 5 ÷ 470 = 0.0106 A = 10.6 mA)*

2. "Why is it dangerous to connect an LED directly to 5V with no resistor?" Use the words current and Ohm's Law in your answer.

3. A classmate says "bigger resistance = brighter LED because more resistance means more power." Do you agree? Explain.
   *(Expected: Disagree — bigger R means LESS current, which means LESS brightness)*

---

## 3. Circuit Diagram

```
  Arduino Mega 2560               Breadboard
  ┌──────────────┐                ┌──────────────────────────────┐
  │              │                │  Row 7:  [A]──[Resistor]──[E]│
  │   Pin 8  ●───┼── red wire ───→│  Row 7:  [A] ← swap R here  │
  │              │                │                              │
  │              │                │  Row 10: [A] (bottom of R)  │
  │              │                │  Row 10: [E] LED anode (+)  │
  │              │                │  Row 10: [F] LED cathode(-) │
  │              │                │                              │
  │   GND    ●───┼── blk wire ───→│  Row 10: [J]                │
  └──────────────┘                └──────────────────────────────┘

  Experiment: swap the resistor between tests
  Test 1: 220Ω  (red-red-brown)     → bright
  Test 2: 1kΩ   (brown-black-red)   → dim
  Test 3: 10kΩ  (brown-black-orange) → very dim / barely visible
```

**Fritzing-style Build Description:**

1. **Arduino Mega, Pin 8** → red jumper wire → breadboard **row 7, column A**
2. **Resistor** (swap between tests): one leg in **row 7, column A**; other leg in **row 10, column A**
3. **Red LED**: long leg (anode +) in **row 10, column E**; short leg (cathode –) in **row 10, column F**
4. **Arduino Mega, GND pin** → black jumper wire → breadboard **row 10, column J**

Wire colors: Red = pin 8 signal, Black = GND.

[Google image search: "ohms law LED resistor circuit different resistance values brightness comparison Arduino"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 *  Session 3 — Ohm's Law & Resistors
 *
 *  SCIENCE EXPLANATION:
 *  Ohm's Law: V = I × R  (or rearranged: I = V / R)
 *
 *  In this experiment, we keep the VOLTAGE constant (always 5V
 *  from pin 8) and change the RESISTANCE (swap resistors).
 *  According to Ohm's Law, if V stays the same and R gets
 *  larger, then I (current) must get SMALLER.
 *
 *  Less current through an LED = less brightness.
 *
 *  We use a constant ON (not blinking) so it is easier to
 *  compare brightness between resistor swaps.
 *
 *  Resistor calculations for our circuit:
 *  (We subtract 2V because the LED "uses" ~2V across itself)
 *
 *    With 220Ω:   I = (5V - 2V) / 220Ω  = 3/220  = 13.6 mA (bright)
 *    With 1kΩ:    I = (5V - 2V) / 1000Ω = 3/1000 =  3.0 mA (dim)
 *    With 10kΩ:   I = (5V - 2V) / 10000Ω= 3/10000=  0.3 mA (very dim)
 * ============================================================
 */

// ---- PIN DEFINITION ----
const int LED_PIN = 8;   // Red LED connected to digital pin 8

// ============================================================
// setup() — Runs once on power-on or reset
// ============================================================
void setup() {
  // Set pin 8 as an output so we can send voltage out of it
  pinMode(LED_PIN, OUTPUT);
}

// ============================================================
// loop() — Runs forever
// ============================================================
void loop() {

  // Keep the LED constantly ON so we can compare brightness
  // between resistor swaps without the blinking being confusing
  digitalWrite(LED_PIN, HIGH);  // Pin 8 = 5V → LED on (brightness depends on resistor)

  // >>> TRY CHANGING THIS: After swapping resistors, observe
  // how brightness changes with the SAME line of code.
  // The code does NOT change — only the hardware changes.

  // No delay and no OFF command — just constant HIGH
  // (The loop() will keep re-running this single line forever)
}

// ===== CHALLENGE =====
// Challenge 1: CALCULATE — before testing, use I = V/R to predict
//              the current for each resistor (V across resistor = 3V).
//              Write your predictions on the worksheet, then test.
//
// Challenge 2: BLINK SPEED — change the code to blink again (add delays).
//              Does the blink SPEED change when you swap resistors?
//              (It should not — speed is controlled by delay(), not resistance)
//              Why not? Explain in a sentence.
//
// Challenge 3: SERIES RESISTORS — place the 220Ω AND the 1kΩ resistor
//              end-to-end (in series) in the circuit.
//              Combined resistance = 220 + 1000 = 1220Ω.
//              Predict the brightness, then test it.
//              Calculate expected current: I = 3V / 1220Ω = ?
```

---

## 5. Student Worksheet

### Session 3 — Ohm's Law & Resistors
**Name(s):** _________________________________ **Date:** _____________ **Kit #:** _____

**Objectives:** By the end of this session I will be able to:
- Use V = I × R to calculate current in a circuit
- Read a resistor color code
- Explain how resistance affects LED brightness

---

#### What I Already Know — Warm-Up Questions

1. What are the three components in our LED circuit from Session 2? List them.

   _____________________________________________________________________________

2. In your own words, what does a resistor do to the flow of electricity?

   _____________________________________________________________________________

3. If you wanted to make an LED brighter (more current), would you use a bigger or smaller resistor? Why?

   _____________________________________________________________________________

---

#### Resistor Identification — Color Code Practice

Before building, identify your three resistors using the color code. Circle the bands you see:

| Resistor | Band 1 | Band 2 | Band 3 (×) | Value (Ω) |
|----------|--------|--------|-----------|-----------|
| A | Red | Red | Brown (×10) | 220 Ω |
| B | Brown | Black | Red (×100) | _______ Ω |
| C | Brown | Black | Orange (×1000) | _______ Ω |

---

#### Ohm's Law Calculations — Predict BEFORE Testing

The voltage across the resistor in our circuit = 5V − 2V (used by LED) = **3V**.

Use **I = V ÷ R** to predict the current for each resistor:

| Resistor Value | Formula | Predicted Current (mA) |
|---------------|---------|----------------------|
| 220 Ω | I = 3 ÷ 220 = | _______ mA |
| 1,000 Ω (1kΩ) | I = 3 ÷ 1000 = | _______ mA |
| 10,000 Ω (10kΩ) | I = 3 ÷ 10000 = | _______ mA |

*(Convert to mA by multiplying your answer in amps by 1000)*

---

#### Predict!

1. Based on your calculations, which resistor will produce the **brightest** LED? ___________

2. Which will produce the **dimmest**? ___________

3. If you connected the LED with NO resistor at 5V, the formula gives I = 3V ÷ 0Ω. What mathematical problem does this create? What does this tell you about real circuits?

   _____________________________________________________________________________

   _____________________________________________________________________________

---

#### Build It — Step by Step

1. Build the same circuit as Session 2 (LED on pin 8, start with 220Ω resistor).
2. Upload the code (LED stays ON, not blinking).
3. Rate brightness: 1 = barely visible, 5 = very bright.
4. Power down (unplug USB), swap to 1kΩ, power on, rate brightness.
5. Power down, swap to 10kΩ, power on, rate brightness.

#### Observe / Data Table

| Resistor | Color Bands | Predicted Current (mA) | Brightness Rating (1–5) | Did it match prediction? |
|----------|-------------|----------------------|------------------------|--------------------------|
| 220 Ω | Red-Red-Brown | | | |
| 1 kΩ | Brown-Black-Red | | | |
| 10 kΩ | Brown-Black-Orange | | | |

---

#### What Did You Notice?

1. How did the brightness change as you increased the resistance? Does this match Ohm's Law?

   _____________________________________________________________________________

2. The code did not change at all — only the hardware changed. What does this tell you about what controls the brightness?

   _____________________________________________________________________________

3. At what point (which resistor) did the LED almost disappear? How does this connect to the calculated current value?

   _____________________________________________________________________________

4. If you have access to a multimeter: measure the resistance of each resistor and record the actual value. How close are the measured values to the labeled values?

   | Resistor | Labeled Value | Measured Value | Difference |
   |----------|--------------|----------------|-----------|
   | A | 220 Ω | | |
   | B | 1 kΩ | | |
   | C | 10 kΩ | | |

---

#### Science Connection

1. **Ohm's Law in words:** Finish this sentence — "When voltage is constant, if resistance doubles, the current ____________________."

2. Georg Ohm discovered this relationship in 1827. Why do you think this formula is so important for anyone designing electrical devices?

   _____________________________________________________________________________

   _____________________________________________________________________________

3. The resistor color code was invented so engineers could read resistor values without special tools. Why do you think a visual system like color bands was chosen instead of, say, printing numbers on the resistor?

   _____________________________________________________________________________

---

#### Challenge Extension

1. **Series resistors:** Place the 220Ω and 1kΩ resistors end-to-end in the circuit. Total R = 220 + 1000 = 1220Ω. Calculate the expected current, then test and rate the brightness.
   - Predicted current: I = 3V ÷ 1220Ω = _______ mA
   - Observed brightness rating: _______
   - Does the brightness fall between the 220Ω result and the 1kΩ result? _______

2. **Research:** What is the maximum safe continuous current for a standard 5mm red LED? (Hint: look at the LED datasheet for a "1N4148" or "generic 5mm LED.") How does your 220Ω calculation compare?

---

## 6. Safety Notes

**Session 3 Hazards — Resistor swapping on a live… wait, NO.**

**Critical reminder this session:**
Students will be tempted to swap resistors quickly without unplugging USB. This must NOT happen.

- **Every resistor swap requires the USB cable to be UNPLUGGED first.** Post this on the board.
- Say aloud each time: "Build → Check → Power on" — including for the swap.
- Reason: although 5V is not a shock hazard, touching metal jumper wire ends or resistor leads while the board is powered can cause short circuits that damage the Mega or the LED.

**Component handling:**
- Resistors are not fragile, but leads can poke. Handle from the body (the colored cylinder), not the bare metal legs.
- If a resistor lead bends while inserting, straighten it gently with fingers — do not use tools that could break it.
- Multimeter probe tips are sharp. When measuring resistance, the circuit must be powered OFF and unplugged (Ohmmeter mode requires this anyway, as applying voltage to an ohmmeter can damage it).

**If something goes wrong:**
| Symptom | Action |
|---------|--------|
| LED flashes very brightly then goes dark | LED may have burned out due to no resistor / wrong resistor. Power down. Replace LED from spare parts bin. |
| Multimeter reads "1" or "OL" when measuring resistance | Multimeter is in the wrong range or leads are not on the resistor — check lead placement and range setting |
| Burning smell | Unplug USB immediately. Identify the hot component — likely a wrong resistor value or short circuit. Tell teacher. |

---

## 7. Assessment Rubric

### Formative Check — Session 3 (Not Graded)

This is a concept-building session. Use the following three look-fors:

| Look-For | What to Observe |
|----------|----------------|
| **Ohm's Law application** | Can the student correctly use I = V/R to calculate current for each resistor before testing? Do they understand that subtracting the LED voltage drop (2V) gives the resistor voltage? |
| **Color code reading** | Can the student identify the resistance value of all three resistors by reading the color bands alone (without measuring)? |
| **Observation quality** | In the data table, are brightness ratings consistent with Ohm's Law predictions? Does the student notice that code does not change, but brightness does? Can they explain why? |

**Intervention point:** If a student cannot connect "more resistance → less current → less brightness" in their own words after the experiment, spend 2 minutes revisiting the water analogy (the resistor is a narrower pipe; same water pressure, less flow, less output).

---

## 8. Differentiation

### Support
- Provide a pre-filled "Ohm's Law calculation guide" showing the formula, a worked example, and space for students to complete the three values — reduces cognitive load so attention goes to the experiment.
- Color-print the color-code chart and tape it to the desk during the activity.
- For students struggling with the formula, use a triangle diagram: draw a triangle divided into three parts — V on top, I and R on the bottom. Cover the one you want to find, and the remaining shape shows the formula (V=I×R, I=V/R, R=V/I).
- Reduce the experiment to two resistors (220Ω vs 10kΩ) if time is a constraint — the difference is more dramatic and clearer.

### Extension
- Have students calculate what resistor value they would need if they wanted exactly 20mA through the LED (the maximum safe value). What is R = (5V – 2V) / 0.020A? (Answer: 150Ω — closest standard value.) Does this value exist in standard resistor series?
- Challenge: What would happen in a circuit with two identical 220Ω resistors in series vs. two in parallel? Calculate both cases and build one to test.
- Research: Why do modern LED circuits often use a "constant-current driver" instead of just a resistor? What advantage does it give? (Connects to more advanced electronics.)

### Visual / Kinesthetic Accommodations
- Use the "garden hose with a nozzle" physical demonstration: show a real squirt bottle with a narrow tip (high resistance) vs. a wide opening (low resistance). Same squeeze pressure = different flow rates.
- Large-print color code cards available.
- Students who are color-vision deficient: provide the numerical values of the resistor bands, or use the multimeter for all identification tasks. Note this in your roster.
- ELL students: "voltage" = "electrical pressure," "current" = "flow of electricity," "resistance" = "opposition to flow" — post bilingual glossary on wall.
