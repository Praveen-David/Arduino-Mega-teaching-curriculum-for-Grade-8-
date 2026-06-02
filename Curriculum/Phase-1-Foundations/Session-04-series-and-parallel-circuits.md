# Week 2, Session 4 — Series & Parallel Circuits (Multiple LEDs)

**Phase:** Phase 1 — Foundations

**Learning Objectives:**
- Distinguish between series and parallel circuits by their topology and behavior
- Build both a series LED circuit and a parallel LED circuit on a breadboard
- Write code that drives multiple LEDs on separate pins with different blinking patterns
- Predict and explain what happens to other LEDs when one LED in a series circuit is removed, versus in a parallel circuit

**Science Curriculum Link:** Grade 8 Electricity — Series and Parallel Circuit Topology and Current Paths. In a series circuit, current has only one path; in a parallel circuit, current splits into multiple paths. This explains why removing one bulb in old-style holiday lights turns off the whole string (series), while modern lights stay on when one is removed (parallel). Students see both behaviors physically and connect them to real-world applications.

**Materials Checklist (per pair):**
- 1 × Arduino Mega 2560 + USB cable
- 1 × Solderless breadboard
- 3 × Red LEDs (5mm) — or use red, yellow, green for visual clarity
- 3 × 220 Ω resistors (red-red-brown)
- 6 × Jumper wires (assorted colors, M-M)
- 1 × Laptop with Arduino IDE 2.x
- 1 × Printed Mega pinout diagram

---

## 2. Teacher Guide

### 0–5 min — Hook / Warm-Up

**Ask the class:** "Has anyone ever had a string of holiday lights where ONE bulb went out and the ENTIRE string went dark? Why do you think that happened?"

Take 2–3 responses. Then ask: "Has anyone seen a string of lights where you could pull out one bulb and the rest stayed on?"

"Both types of strings are real. The difference is how the bulbs are wired: in a series or in a parallel circuit. By the end of today, you will know exactly why one fails completely and the other keeps going — and you will build both."

---

### 5–15 min — Direct Instruction

**Concept 1: Series circuits**

"In a series circuit, all components are wired in a single chain — one path. Current flows through EVERY component one after another. Think of it like a one-lane road: every car must pass through every traffic light in order."

Draw on board:

```
  Pin 8 → Resistor → LED1 → LED2 → LED3 → GND

  ONE PATH — if any component breaks (or is removed), the whole chain breaks.
  Current is the SAME everywhere in the series circuit.
  Voltage is SHARED across all components.
```

"Problem with series for multiple LEDs: each LED 'uses up' about 2V. With 3 LEDs in series: 3 × 2V = 6V needed, but we only have 5V. The LEDs will not work properly (or at all). This is why series LEDs are problematic with low-voltage Arduino circuits."

**Concept 2: Parallel circuits**

"In a parallel circuit, components are wired side-by-side — each one gets its own path from power to ground. Think of it like a highway with multiple lanes: current can travel through any lane independently."

Draw on board:

```
           ┌──→ Resistor → LED1 ──┐
  Pin 8 ──→├──→ Resistor → LED2 ──┤──→ GND
           └──→ Resistor → LED3 ──┘

  MULTIPLE PATHS — removing one LED does not affect the others.
  Voltage is the SAME across each branch (all get the full supply voltage).
  Current SPLITS — each branch carries its own share.
```

"In our Arduino circuits, we do NOT put multiple LEDs on one pin in parallel (too much current for one pin). Instead, each LED gets its OWN pin. This is even better — each LED can be controlled independently by code."

**Concept 3: Multiple pin output in code**

"With the Mega's 54 digital pins, we can control each LED with its own pin. This gives us full independent control — we can turn any LED on or off in any pattern, at any time, with code."

---

### 15–35 min — Hands-On Build + Code

**SAFETY: USB unplugged. Build → Check → Power on.**

**Part A — Independent Parallel LED Build (15–27 min)**

Each LED gets its own pin and its own resistor. This is the standard design for the rest of the course.

Build steps:

1. **USB UNPLUGGED.**
2. **LED 1 (Red):** Insert into rows 5–6 (anode in row 5/E, cathode in row 5/F). Insert 220Ω resistor (row 3/A to row 5/A). Red jumper wire from pin 8 → row 3/A. Black jumper from GND → row 5/J.
3. **LED 2 (Yellow/Red):** Insert into rows 12–13. Insert 220Ω resistor (row 10/A to row 12/A). Green jumper wire from pin 9 → row 10/A. Black jumper from GND → row 12/J.
4. **LED 3 (Green/Red):** Insert into rows 19–20. Insert 220Ω resistor (row 17/A to row 19/A). Blue jumper wire from pin 10 → row 17/A. Black jumper from GND → row 19/J.
5. **Partner check:** Each LED-resistor-wire combination is independent. Verify all three GND wires are connected.
6. Upload the multi-LED blink pattern code (Section 4).
7. Observe the sequential blinking pattern.

**Part B — Series Demonstration (27–35 min)**

Teacher-led demo (or advanced pairs). Build one series chain:

1. Connect pin 8 → resistor → LED1 anode, LED1 cathode → LED2 anode, LED2 cathode → GND. (No separate resistors per LED — one 220Ω at the front of the chain.)
2. Power on, observe: the LEDs are very dim because they share the voltage.
3. **Remove one LED from the chain** while observing — the other goes out too.
4. Compare this to the parallel circuit — removing one LED does not affect others.

**Discussion:** "Which is better for our projects — and why? What are the tradeoffs?"

---

### 35–42 min — Testing & Debugging

**Success looks like:** Three LEDs blink in a sequential pattern (1 on, 2 on, 3 on — like a chase pattern), each independently controllable by code.

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| One LED does not light | That LED may be backwards, or its resistor/wire is not in the same row | Power down; check polarity and row connections for that LED only |
| All three LEDs on or off together | Code error — all pins set the same state | Check the code for each pin's HIGH/LOW sequence |
| One LED much dimmer than others | Different resistor value, or LED in a slightly different row causing partial contact | Check color bands; re-seat in adjacent hole |
| Code won't upload | Board/port settings | Tools → Mega 2560, correct port |
| Series LEDs very dim | Correct — voltage is shared across all | Explain this is Ohm's Law from Session 3 in action |

---

### 42–45 min — Reflection / Exit Ticket

**Students answer:**

1. "You have a series circuit with 3 LEDs. You remove one LED. What happens to the other two and WHY?"
   *(Expected: they go off — the single current path is broken)*

2. "You have a parallel circuit with 3 LEDs on pins 8, 9, 10. You remove the LED on pin 9. What happens to the LEDs on pins 8 and 10 and WHY?"
   *(Expected: they stay on — each has an independent path)*

3. "Give one real-world example where you would want a series circuit, and one where you would want parallel. Explain your thinking."

---

## 3. Circuit Diagram

**Parallel build (3 independent LEDs on pins 8, 9, 10):**

```
  Arduino Mega 2560               Breadboard
  ┌──────────────┐       ┌────────────────────────────────────┐
  │              │       │                                    │
  │  Pin 8   ●───┼──red──→ Row 3/A ─[220Ω]─ Row 5/A          │
  │              │       │                  Row 5/E  [LED1+] │
  │              │       │                  Row 5/F  [LED1-] │
  │              │       │                  Row 5/J ←──────┐ │
  │              │       │                                  │ │
  │  Pin 9   ●───┼──grn──→ Row 10/A─[220Ω]─ Row 12/A        │ │
  │              │       │                  Row 12/E [LED2+]│ │
  │              │       │                  Row 12/F [LED2-]│ │
  │              │       │                  Row 12/J ←────┐ │ │
  │              │       │                                │ │ │
  │  Pin 10  ●───┼──blu──→ Row 17/A─[220Ω]─ Row 19/A      │ │ │
  │              │       │                  Row 19/E [LED3+]│ │
  │              │       │                  Row 19/F [LED3-]│ │
  │              │       │                  Row 19/J ──→GND │ │
  │              │       │                              ↑   │ │
  │  GND     ●───┼──blk──→─────────────────────────────┴───┴─┘
  └──────────────┘       └────────────────────────────────────┘

  Each LED has its own independent current path (parallel topology).
```

**Fritzing-style Build Description:**

1. **Arduino Mega, Pin 8** → red jumper wire → breadboard **row 3, column A**
2. **220 Ω resistor #1**: one leg in **row 3, column A**; other leg in **row 5, column A**
3. **LED 1**: long leg (anode +) in **row 5, column E**; short leg (cathode –) in **row 5, column F**
4. **Black jumper wire** → **row 5, column J** → Arduino Mega **GND**
5. **Arduino Mega, Pin 9** → green jumper wire → breadboard **row 10, column A**
6. **220 Ω resistor #2**: one leg in **row 10, column A**; other leg in **row 12, column A**
7. **LED 2**: long leg in **row 12, column E**; short leg in **row 12, column F**
8. **Black jumper wire** → **row 12, column J** → Arduino Mega **GND**
9. **Arduino Mega, Pin 10** → blue jumper wire → breadboard **row 17, column A**
10. **220 Ω resistor #3**: one leg in **row 17, column A**; other leg in **row 19, column A**
11. **LED 3**: long leg in **row 19, column E**; short leg in **row 19, column F**
12. **Black jumper wire** → **row 19, column J** → Arduino Mega **GND**

Wire colors: Red = pin 8, Green = pin 9, Blue = pin 10, Black = all GND connections.

[Google image search: "Arduino multiple LED circuit breadboard parallel pins 8 9 10 tutorial"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 *  Session 4 — Series & Parallel Circuits: Multiple LEDs
 *
 *  SCIENCE EXPLANATION:
 *  In a PARALLEL circuit, each component has its own independent
 *  path for current to flow from the power source back to ground.
 *
 *  Here, each LED is connected to its own Arduino pin.
 *  That means each has its own complete circuit (closed loop).
 *  Turning one LED off (LOW) does NOT affect the others —
 *  their circuits are independent.
 *
 *  Compare this to SERIES: in a series circuit there is only
 *  ONE current path. If any part breaks, all go off.
 *
 *  With 3 independent pins we can program any blink pattern
 *  we like — sequential, alternating, random, etc.
 * ============================================================
 */

// ---- PIN DEFINITIONS ----
// Using constants makes the code easier to read and change later
const int LED1 = 8;    // First LED  — connected to digital pin 8
const int LED2 = 9;    // Second LED — connected to digital pin 9
const int LED3 = 10;   // Third LED  — connected to digital pin 10

// ============================================================
// setup() — Runs once when the Arduino powers on or resets
// ============================================================
void setup() {
  // Set all three LED pins as outputs
  // (They will send voltage OUT to control the LEDs)
  pinMode(LED1, OUTPUT);
  pinMode(LED2, OUTPUT);
  pinMode(LED3, OUTPUT);
}

// ============================================================
// loop() — Runs forever
// ============================================================
void loop() {

  // --- PATTERN 1: Sequential (one at a time) ---
  // LED 1 on, others off
  digitalWrite(LED1, HIGH);
  digitalWrite(LED2, LOW);
  digitalWrite(LED3, LOW);
  delay(500);  // >>> TRY CHANGING THIS: faster = 200, slower = 1000

  // LED 2 on, others off
  digitalWrite(LED1, LOW);
  digitalWrite(LED2, HIGH);
  digitalWrite(LED3, LOW);
  delay(500);

  // LED 3 on, others off
  digitalWrite(LED1, LOW);
  digitalWrite(LED2, LOW);
  digitalWrite(LED3, HIGH);
  delay(500);

  // All off briefly
  digitalWrite(LED1, LOW);
  digitalWrite(LED2, LOW);
  digitalWrite(LED3, LOW);
  delay(250);  // Short pause before repeating

  // The pattern then repeats from the top of loop()
}

// ===== CHALLENGE =====
// Challenge 1: ALL ON — add code to turn all three LEDs on at the same time.
//              Then blink them all together (all HIGH, delay, all LOW, delay).
//
// Challenge 2: ALTERNATING — make LED1 and LED3 turn on while LED2 is off,
//              then swap (LED2 on, LED1 and LED3 off). Repeat.
//              Hint: you will need two blocks of digitalWrite + delay.
//
// Challenge 3: COUNT IN BINARY — three LEDs can represent binary numbers 0–7.
//              LED3 = 4s place, LED2 = 2s place, LED1 = 1s place.
//              Program the sequence: 0 (000), 1 (001), 2 (010), 3 (011),
//              4 (100), 5 (101), 6 (110), 7 (111).
//              You will need 8 steps in your loop().
//
// Challenge 4: SERIES EXPERIMENT — disconnect two of the three LED circuits
//              and wire the remaining LED + a new LED in SERIES (one after another,
//              no separate pin or resistor for the second LED). What happens to
//              brightness? What happens when you remove one LED from the chain?
```

---

## 5. Student Worksheet

### Session 4 — Series & Parallel Circuits (Multiple LEDs)
**Name(s):** _________________________________ **Date:** _____________ **Kit #:** _____

**Objectives:** By the end of this session I will be able to:
- Build a parallel 3-LED circuit on a breadboard
- Write code to control multiple LEDs with different timing patterns
- Explain the difference between series and parallel circuits using evidence from my experiment

---

#### What I Already Know — Warm-Up Questions

1. Think of a string of old holiday lights where ONE broken bulb turns off the ENTIRE string. What does this tell you about how those lights are wired?

   _____________________________________________________________________________

2. If one lane of a three-lane highway is closed, can cars still travel on the other two lanes? How is this similar to electrical circuits?

   _____________________________________________________________________________

3. What was the problem we saw in Session 3 when using a 10kΩ resistor with a single LED? What would happen if you put three LEDs in a series chain?

   _____________________________________________________________________________

---

#### Build It — Step by Step

Follow the numbered build steps in the Teacher Guide. Here is a checklist:

**For each LED (do steps 1–4 three times, for pins 8, 9, and 10):**

LED 1 (Pin 8):
- [ ] 220Ω resistor from row 3/A to row 5/A
- [ ] LED: long leg in row 5/E, short leg in row 5/F
- [ ] Red jumper: pin 8 → row 3/A
- [ ] Black jumper: row 5/J → GND

LED 2 (Pin 9):
- [ ] 220Ω resistor from row 10/A to row 12/A
- [ ] LED: long leg in row 12/E, short leg in row 12/F
- [ ] Green jumper: pin 9 → row 10/A
- [ ] Black jumper: row 12/J → GND

LED 3 (Pin 10):
- [ ] 220Ω resistor from row 17/A to row 19/A
- [ ] LED: long leg in row 19/E, short leg in row 19/F
- [ ] Blue jumper: pin 10 → row 17/A
- [ ] Black jumper: row 19/J → GND

**Partner Check:** ☐ Done — partner has verified all connections.

---

#### Predict!

1. What pattern do you predict the LEDs will show when the code runs?

   _____________________________________________________________________________

2. If you physically pull one LED out of its socket while the code is running, what do you predict will happen to the other two? Why?

   _____________________________________________________________________________

3. In the series demo: if you connect two LEDs end-to-end on one pin (with one 220Ω resistor), you are giving both LEDs the same ~13mA of current, but sharing 5V across both. Each LED only gets about 1.5V (less than its normal 2V). What do you predict the brightness will look like?

   _____________________________________________________________________________

---

#### Observe / Data Table

**Parallel circuit observations:**

| Test | What the code does | What I observed | Did it match prediction? |
|------|--------------------|-----------------|--------------------------|
| Sequential pattern | LED1 on, then LED2, then LED3 | | |
| Remove LED2 while code runs | LED2 is out | Did LED1 and LED3 keep blinking? | |
| Change delay to 100ms | All delays = 100 | How does it look different? | |

**Series demo observations (if done):**

| Number of LEDs in series | Brightness compared to single LED | What happened when one LED removed? |
|--------------------------|----------------------------------|-------------------------------------|
| 1 LED (baseline) | Bright | N/A |
| 2 LEDs in series | | |

---

#### What Did You Notice?

1. When you removed one LED from the parallel circuit, what happened to the others? Explain why using the term "current path."

   _____________________________________________________________________________

   _____________________________________________________________________________

2. In the series demo, why were the LEDs dimmer than in the parallel circuit? Use the words voltage and Ohm's Law in your answer.

   _____________________________________________________________________________

   _____________________________________________________________________________

3. Could you make the LEDs blink in different patterns by changing the code WITHOUT touching the wiring? What does this tell you about what controls the behavior of a circuit?

   _____________________________________________________________________________

4. In the code, what does `pinMode(LED1, OUTPUT)` do, and why do we need to do it three times?

   _____________________________________________________________________________

---

#### Science Connection

1. Old holiday lights used series wiring. Modern LED holiday lights use parallel wiring. Based on what you learned today, what advantage do modern parallel lights have?

   _____________________________________________________________________________

2. The wiring in a house is a parallel circuit — each outlet is on its own branch. Why would it be terrible if your house was wired in series?

   _____________________________________________________________________________

3. When all three LEDs are on at the same time (parallel), each draws about 13mA. The total current drawn from the Arduino is approximately 3 × 13mA = 39mA. The Arduino Mega can safely supply about 200mA total. How many LEDs like this could you run at once before approaching that limit?

   _____________________________________________________________________________

---

#### Challenge Extension

1. **Binary counting:** Three LEDs can represent the numbers 0–7 in binary. Program the sequence 0, 1, 2, 3, 4, 5, 6, 7 with a 500ms delay between each step. Use the table below to plan your code:

   | Number | LED3 (4s) | LED2 (2s) | LED1 (1s) |
   |--------|-----------|-----------|-----------|
   | 0 | OFF | OFF | OFF |
   | 1 | OFF | OFF | ON |
   | 2 | OFF | ON | OFF |
   | 3 | OFF | ON | ON |
   | 4 | ON | OFF | OFF |
   | 5 | ON | OFF | ON |
   | 6 | ON | ON | OFF |
   | 7 | ON | ON | ON |

2. After building the binary counter, how could you modify the code to count from 7 back DOWN to 0? Write the plan.

---

## 6. Safety Notes

**Session 4 Hazards — Multiple components, more wiring.**

**General reminders:**
- With more wires comes more chance of accidental short circuits. Double-check every connection before powering on.
- The Build → Check → Power on sequence is even more important with 3 LEDs — there are 12 connections to verify.

**Specific hazards this session:**
- **Do NOT connect two pins together** with a jumper wire (e.g., pin 8 and pin 9 directly connected). This creates a short between two output pins and can damage the Mega.
- **Do NOT share a resistor between two LEDs** unless intentionally wiring series (demo only). Each LED must have its own 220Ω resistor.
- With multiple GND wires, confirm all black wires go to a GND pin, not to a signal pin or 5V. Plugging a GND wire into 5V and 5V into GND creates a short circuit.

**During the series demo:**
- If the teacher demonstrates a series circuit with no resistor for shock effect, limit the test to 1 second and replace the LED immediately. This is a teaching moment, not standard lab practice.

**If something goes wrong:**
| Symptom | Action |
|---------|--------|
| One LED is much brighter than the others | A wire may be bypassing its resistor — power down, check routing |
| Board feels warm | Too many components with too-low resistance, or a short circuit — power down, check wiring |
| LEDs light in a different order than code says | Check which physical LED is on which pin number — wires may be swapped |
| All LEDs turn on/off at once instead of sequentially | Check code — may have inadvertently set all pins to the same state |

---

## 7. Assessment Rubric

### Formative Check — Session 4 (Not Graded)

| Look-For | What to Observe |
|----------|----------------|
| **Circuit construction** | Does the student build all three LED branches correctly with independent resistors, independent signal wires (correct pins), and all GND connections? Do all three LEDs work? |
| **Code control** | Does the student understand that changing `HIGH`/`LOW` for each pin independently controls each LED independently? Can they write a new pattern (e.g., alternating) by modifying the code? |
| **Series vs. parallel** | Can the student explain in words: (a) why removing one LED in a parallel circuit doesn't affect others, and (b) why LEDs are dimmer in series? Do they use the terms current path or voltage in their explanation? |

---

## 8. Differentiation

### Support
- Provide a pre-wired breadboard with the first LED branch already installed. Students add LED2 and LED3 by following the same pattern.
- Provide partially-completed code with the pin definitions and `setup()` filled in, with blanks for the `digitalWrite()` calls in `loop()`.
- Color-code the worksheet: highlight each LED's connections in the same color as the jumper wire used for that branch (red section for LED1, green for LED2, blue for LED3).
- Focus on the parallel circuit only — skip the series demo if time is short, referring back to the holiday lights discussion as sufficient for the conceptual understanding.

### Extension
- Binary counter challenge (described in worksheet): introduces the concept of binary representation in a hands-on way. Ask them: "How many different states can you represent with 4 LEDs? 8 LEDs?"
- Have students design a "traffic light" pattern in code as a preview for Session 6. Red on (3s) → yellow on (1s) → green on (3s) → repeat.
- Research: where else do parallel circuits appear in everyday life? (House wiring, USB hubs, battery packs.) Write a 3-sentence explanation.
- Challenge: can you make an "LED chaser" that goes back and forth (1, 2, 3, 2, 1, 2, 3...)? How does the code need to change?

### Visual / Kinesthetic Accommodations
- Physical series/parallel demo with students as "components": Students form a chain holding hands (series) — one person letting go breaks the chain. Then split into parallel lines — one line dropping does not affect the other.
- Large-print circuit diagrams with color-coded wire paths.
- For students who struggle with the breadboard row system, use pre-labeled breadboard stickers (Row 5, Row 10, Row 17) to mark the insertion points before class.
- Tinkercad: practice the 3-LED parallel and series circuits in simulation before building in hardware.
