# Week 1, Session 2 — Your First Circuit: Blink an LED

**Phase:** Phase 1 — Foundations

**Learning Objectives:**
- Build a working LED circuit on a breadboard using correct polarity and a current-limiting resistor
- Write and upload an Arduino sketch using `pinMode()`, `digitalWrite()`, and `delay()`
- Explain why a resistor is required in an LED circuit using the concept of a closed circuit
- Demonstrate the Build → Check → Power on safety sequence independently

**Science Curriculum Link:** Grade 8 Electricity — Closed Circuits and Digital Signals. A digital signal has exactly two states: HIGH (5V, circuit closed, current flows) or LOW (0V, circuit open, no current). This session makes that concept physical: students build the circuit by hand and see the direct relationship between a code command and a physical event (the LED turning on).

**Materials Checklist (per pair):**
- 1 × Arduino Mega 2560 + USB cable
- 1 × Solderless breadboard
- 1 × Red LED (5mm)
- 1 × 220 Ω resistor (red-red-brown color bands)
- 2 × Jumper wires — red and black (M-M)
- 1 × Laptop with Arduino IDE 2.x
- 1 × Printed Mega pinout diagram

---

## 2. Teacher Guide

### 0–5 min — Hook / Warm-Up

**Show the class:** Hold up an LED and a 9V battery. Touch the LED leads directly to the battery terminals. The LED will flash very briefly (if at all) and may make a soft pop sound — it is likely destroyed.

**Say:** "That LED just burned out in less than a second. Why? Too much current — like trying to push a fire hose's worth of water through a garden straw. Today you will learn how to protect your LED using a resistor, and build your first working circuit. This time, everything will survive."

If you prefer not to destroy a component: show a photo of a burned-out LED instead, or describe what happens.

**Ask:** "What do you remember from last session about what needs to happen for an LED to light up?" *(Expected: closed circuit, voltage, current flows through a complete loop)*

---

### 5–15 min — Direct Instruction

**Concept 1: How a breadboard works**

"A breadboard looks like a grid of holes, but inside, the holes are connected in a specific pattern." Diagram on the board:

```
  Top power rail:  (+) row — all holes connected horizontally  ← 5V (red wire)
                   (–) row — all holes connected horizontally  ← GND (black wire)

  Main area:  columns A-E connected vertically in groups of 5
              columns F-J connected vertically in groups of 5
              (the center gap BREAKS the connection — important!)
```

"When you push a component's leg into a hole, it makes contact with every other hole in that same 5-hole group. That is how we connect components without soldering."

**Concept 2: The LED — polarity matters**

"An LED (Light Emitting Diode) is a one-way device. It only lets current flow in one direction. It has a longer leg called the **anode** (positive, +) and a shorter leg called the **cathode** (negative, –). If you put it in backwards, no current flows and it will not light up — but at least it will not burn out. When you see it is not working, the first fix is always: flip the LED."

**Concept 3: The 220 Ω resistor — why every LED needs one**

"At 5V with a typical LED that wants about 2V across it, if there were no resistor the current would spike to a dangerous level in microseconds. The resistor limits current to a safe amount — about 14 milliamps — which is enough to make the LED bright without burning it out."

Use the water analogy: "The LED is like a fragile piece of equipment that can only handle a gentle stream. The resistor is a flow restrictor on the pipe. Even though it reduces the flow, it *protects* the equipment."

**Concept 4: Digital pin as a switchable power source**

"Pin 8 on the Mega behaves like a tiny power outlet that our code can switch on (5V) or off (0V). When we call `digitalWrite(8, HIGH)`, pin 8 becomes the + end of our circuit. GND is always the – end. Current flows from pin 8, through the resistor, through the LED, to GND. That is a closed circuit. `digitalWrite(8, LOW)` turns off the voltage — open circuit — LED off."

---

### 15–35 min — Hands-On Build + Code

**SAFETY REMINDER before any building:** "USB cable is UNPLUGGED. We Build, then Check, then Power on. Say it with me: Build → Check → Power on."

**Circuit Build Steps (numbered — do these in order with the class):**

1. Place the Arduino Mega next to the breadboard on the desk (not on top of each other).
2. **CONFIRM the USB cable is not connected.**
3. Insert the red LED into the breadboard. Place the **longer leg (anode, +)** into row 10, column E. Place the **shorter leg (cathode, –)** into row 10, column F. (The center gap separates them — this is correct.)
4. Insert the 220 Ω resistor. One leg in row 10, column A (same row as the LED anode, left side). Other leg in row 7, column A.
5. Take a **red jumper wire**. Connect one end to the Arduino's **digital pin 8**. Connect the other end to row 7, column A (the top of the resistor).
6. Take a **black jumper wire**. Connect one end to the Arduino's **GND** pin (any of the three). Connect the other end to row 10, column J (same row as the LED cathode, right side).

**Partner Check:** Before plugging in, partner confirms:
- LED legs in correct rows (longer on left/anode side)
- Resistor connecting pin 8 wire to LED anode
- Black wire going from LED cathode to GND
- No wires touching each other except at intended connection points

**Coding Steps:**

7. Plug in the USB cable.
8. Open the Arduino IDE.
9. Open a new sketch (File → New).
10. **Delete the default empty code** and type (or copy from the worksheet) the code shown in Section 4.
11. Confirm board = Arduino Mega 2560, correct port selected.
12. Click Upload (right arrow).
13. Watch the LED on the breadboard blink!

---

### 35–42 min — Testing & Debugging

**Success looks like:** The red LED on the breadboard blinks on for 1 second, off for 1 second, in a steady repeating pattern.

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| LED does not light at all | LED in backwards | Power down, flip the LED 180°, power on |
| LED does not light at all | Resistor not in correct row | Power down, check that resistor bridges pin-8 wire to LED anode row |
| LED does not light at all | GND wire not connected | Power down, confirm black wire goes to GND pin and same row as LED cathode |
| LED stays on constantly, no blinking | Code uses `HIGH` but no `LOW` / delays | Check code — confirm both HIGH and LOW lines, and both delays |
| LED very dim | Resistor value too high | Check color bands: red-red-brown = 220 Ω. If you have the wrong resistor, swap it |
| LED very bright for a split second then off | LED inserted wrong polarity initially | Power down, flip LED, retry |
| Upload error in IDE | Wrong port or board | Tools → Board (Mega 2560), Tools → Port (check which port is listed) |

**Circulate and ask each pair:** "Explain to me why you need the resistor." A correct answer indicates conceptual understanding, not just following steps.

---

### 42–45 min — Reflection / Exit Ticket

**Students answer on exit ticket (index card or worksheet bottom):**

1. Draw a simple diagram of your circuit. Label: pin 8, resistor, LED anode (+), LED cathode (–), GND.
2. "What would happen if you removed the resistor from your circuit right now?" *(Expected: LED may burn out due to too much current)*
3. "In the code, what does `HIGH` mean in terms of voltage? What does `LOW` mean?" *(Expected: HIGH = 5V, LOW = 0V)*

---

## 3. Circuit Diagram

```
  Arduino Mega 2560               Breadboard
  ┌──────────────┐                ┌─────────────────────────────┐
  │              │                │  Row 7:  [A]──Resistor──[E]  │
  │   Pin 8  ●───┼── red wire ───→│  Row 7:  [A] (top resistor) │
  │              │                │                              │
  │              │                │  Row 10: [A]──(bot resist)  │
  │              │                │                              │
  │              │                │  Row 10: [E]  LED anode (+) │
  │              │                │           gap (center)       │
  │              │                │  Row 10: [F]  LED cathode(-) │
  │              │                │                              │
  │   GND    ●───┼── blk wire ───→│  Row 10: [J] (GND side)     │
  └──────────────┘                └─────────────────────────────┘

  Current path (when pin 8 = HIGH):
  Pin 8 → red wire → Resistor (220Ω) → LED anode (+) → LED cathode (-) → black wire → GND
```

**Fritzing-style Build Description:**

1. **Arduino Mega, Pin 8** → red jumper wire → breadboard **row 7, column A**
2. **220 Ω resistor** (red-red-brown): one leg in **row 7, column A**; other leg in **row 10, column A**
3. **Red LED**: long leg (anode +) in **row 10, column E**; short leg (cathode –) in **row 10, column F**
4. **Arduino Mega, any GND pin** → black jumper wire → breadboard **row 10, column J**

Wire colors: Red = signal from pin 8 (5V when HIGH), Black = GND (0V, return path).

[Google image search: "Arduino LED breadboard circuit 220 ohm resistor pin 13 tutorial"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 *  Session 2 — Your First Circuit: Blink an LED
 *
 *  SCIENCE EXPLANATION:
 *  A digital output pin on the Arduino Mega can be in one of
 *  two states: HIGH (5 volts) or LOW (0 volts). This is called
 *  a "digital signal" — it is either fully ON or fully OFF,
 *  with nothing in between.
 *
 *  When pin 8 is HIGH:
 *    - 5V is applied to the circuit
 *    - Current flows from pin 8 → 220Ω resistor → LED → GND
 *    - This is a CLOSED CIRCUIT → LED lights up
 *
 *  When pin 8 is LOW:
 *    - 0V (no voltage difference) across the circuit
 *    - No current flows
 *    - This is an OPEN CIRCUIT → LED off
 *
 *  The 220Ω resistor limits current to a safe level (~14 mA),
 *  protecting the LED from burning out. Without it, the LED
 *  would receive too much current and fail instantly.
 * ============================================================
 */

// ---- PIN DEFINITION ----
// Using a constant makes it easy to change the pin number later
// without hunting through every line of code.
const int LED_PIN = 8;  // Red LED connected to digital pin 8

// ============================================================
// setup() — Runs ONCE when the Arduino powers on or resets
// ============================================================
void setup() {

  // Tell the Arduino: "Pin 8 is an OUTPUT — it will send voltage out"
  // This must be done before we can use digitalWrite() on that pin
  pinMode(LED_PIN, OUTPUT);

}

// ============================================================
// loop() — Runs OVER AND OVER FOREVER after setup() finishes
// ============================================================
void loop() {

  // Turn the LED ON: set pin 8 to 5V (HIGH)
  // This closes the circuit → current flows → LED lights up
  digitalWrite(LED_PIN, HIGH);

  // >>> TRY CHANGING THIS: make the ON time longer (e.g. 2000) or shorter (e.g. 200)
  delay(1000);   // Wait 1000 milliseconds = 1 second while LED is ON

  // Turn the LED OFF: set pin 8 to 0V (LOW)
  // This opens the circuit → no current flows → LED turns off
  digitalWrite(LED_PIN, LOW);

  // >>> TRY CHANGING THIS: make the OFF time different from the ON time
  delay(1000);   // Wait 1000 milliseconds = 1 second while LED is OFF

  // After this, loop() starts again from the top — LED blinks forever

}

// ===== CHALLENGE =====
// Challenge 1: Change the delay values to make the LED blink FAST (try delay(100))
//              Then SLOW (try delay(3000)). What is the difference?
//
// Challenge 2: Make the LED blink in the SOS pattern:
//              Short-Short-Short (S) then Long-Long-Long (O) then Short-Short-Short (S)
//              Hint: short = 200ms, long = 600ms, gap between letters = 600ms pause
//
// Challenge 3: Add a SECOND LED on pin 7 (you will need another resistor and LED)
//              and make them blink alternately (one on while the other is off).
//              Hint: you will need TWO sets of digitalWrite and delay lines.
```

---

## 5. Student Worksheet

### Session 2 — Your First Circuit: Blink an LED
**Name(s):** _________________________________ **Date:** _____________ **Kit #:** _____

**Objectives:** By the end of this session I will be able to:
- Build a working LED + resistor circuit on a breadboard
- Write an Arduino sketch with `pinMode`, `digitalWrite`, and `delay`
- Explain what a closed circuit is and why the LED needs a resistor

---

#### What I Already Know — Warm-Up Questions

1. What is a closed circuit? Draw and label a simple diagram.

   _____________________________________________________________________________

   _____________________________________________________________________________

2. Why do you think an LED might burn out if connected directly to 5V without a resistor?

   _____________________________________________________________________________

3. Look at an LED. Which leg is longer? What do you think the longer leg is called?

   _____________________________________________________________________________

---

#### Build It — Step by Step

Follow these steps IN ORDER. Do not plug in the USB cable until Step 6 says to.

**Step 1:** Place the red LED into the breadboard.
- Longer leg (anode +) → row 10, column E
- Shorter leg (cathode –) → row 10, column F
- The center gap of the breadboard separates the two sides. This is correct.

**Step 2:** Insert the 220 Ω resistor.
- One leg → row 7, column A
- Other leg → row 10, column A
- The resistor bridges from the "pin 8 side" to the LED's anode.

**Step 3:** Connect a RED jumper wire.
- One end → Arduino Mega, digital pin 8
- Other end → breadboard row 7, column A (top of resistor)

**Step 4:** Connect a BLACK jumper wire.
- One end → Arduino Mega, any GND pin
- Other end → breadboard row 10, column J (same row as LED cathode)

**Step 5 — Partner Check:** Your partner checks every connection against this list:
- [ ] LED legs: longer in row 10/E, shorter in row 10/F
- [ ] Resistor: row 7/A to row 10/A
- [ ] Red wire: pin 8 to row 7/A
- [ ] Black wire: GND to row 10/J
- [ ] USB cable is NOT plugged in yet

**Step 6:** When BOTH partners have checked and agree — plug in the USB cable, upload the code, and observe.

---

#### Predict!

Answer BEFORE you power on:

1. What do you predict will happen when you upload and run the code?

   _____________________________________________________________________________

2. If you flipped the LED around (swapped which leg goes in which hole), what do you predict would happen?

   _____________________________________________________________________________

3. In the code, there are two `delay(1000)` lines. If you changed the first one to `delay(2000)` but kept the second at `delay(1000)`, what would the blink pattern look like?

   _____________________________________________________________________________

---

#### Observe / Data Table

After uploading the code, observe and record:

| Observation | Your Answer |
|-------------|-------------|
| Does the LED blink? (yes/no) | |
| ON time: count how many times it blinks in 20 seconds | |
| Does the ON time and OFF time look equal? | |
| What does the built-in LED on the Arduino board do? | |
| After you change the delay to 200ms: what changes? | |

---

#### What Did You Notice?

1. Did anything surprise you? What worked differently from what you predicted?

   _____________________________________________________________________________

   _____________________________________________________________________________

2. You changed the delay values — describe in your own words what `delay()` controls.

   _____________________________________________________________________________

3. If you flipped the LED and tested it, what happened? What does this tell you about LEDs?

   _____________________________________________________________________________

4. The code uses `const int LED_PIN = 8`. Why do you think the programmer used a constant instead of just writing the number 8 everywhere?

   _____________________________________________________________________________

   _____________________________________________________________________________

---

#### Science Connection

1. When `digitalWrite(LED_PIN, HIGH)` runs, the pin outputs 5V. Trace the path of current through the circuit (write it out as a chain: pin 8 → ??? → ??? → GND).

   _____________________________________________________________________________

2. Is your circuit "open" or "closed" when the LED is ON? What about when it is OFF?

   _____________________________________________________________________________

3. In Session 1 we said voltage is like water pressure. What is the resistor like in that analogy, and why is it important?

   _____________________________________________________________________________

   _____________________________________________________________________________

---

#### Challenge Extension

1. Change the code to make the LED blink in an SOS pattern (short-short-short, long-long-long, short-short-short). Write the delay values you used:

   Short blink ON time: _______ ms | Short gap: _______ ms
   Long blink ON time:  _______ ms | Long gap:  _______ ms

2. If you have a second LED and resistor: add it to pin 7 and make both LEDs alternate (one on while the other is off). Sketch your new circuit diagram here:

   _(space for drawing)_

---

## 6. Safety Notes

**Session 2 Hazards — First time building and powering a circuit.**

**Before building:**
- Confirm the USB cable is UNPLUGGED before placing any components. Say aloud: "Build → Check → Power on."
- Inspect every jumper wire for bare or frayed ends before use. Damaged wires go in the scrap bin.

**LED handling:**
- LED leads are sharp — do not press them against bare skin or poke others.
- If you push an LED lead into a breadboard hole and it bends or breaks, do not force it. Pull it out and use a replacement from the spare parts box.
- An LED with the wrong polarity will simply not light — it will not be harmed by a short test. But do NOT leave a reversed LED running for extended periods at high currents.

**Resistor handling:**
- Resistors are not polarized (direction does not matter — either leg can go in either hole).
- Confirm color bands before inserting: 220 Ω = red-red-brown (or red-red-black-black with 4-band code).

**If something goes wrong:**
| Symptom | Action |
|---------|--------|
| LED gets hot or makes a smell | Immediately unplug USB. Tell teacher. Do not touch the LED. |
| LED flashes once then stops | May be inserted backwards AND getting a very tiny reverse current — power down, flip, retry |
| Smoke from breadboard area | Unplug USB immediately. Step back. Tell teacher. |
| Breadboard rail feels warm | A short circuit may exist. Unplug, inspect for wires connecting 5V rail directly to GND rail. |

**Reminder:** You are working with 5V — this is too low to give you an electric shock. But improper wiring can damage components. When in doubt: power down first.

---

## 7. Assessment Rubric

### Formative Check — Session 2 (Not Graded)

| Look-For | What to Observe |
|----------|----------------|
| **Circuit construction** | Did the student correctly identify LED polarity (longer leg = anode)? Is the 220Ω resistor in place? Are red/black wires on the correct pins? Does the circuit work? |
| **Code understanding** | Can the student explain what `pinMode(OUTPUT)`, `digitalWrite(HIGH)`, and `delay()` each do in plain English? Can they predict what changing a delay value will do before trying it? |
| **Science connection** | When asked "why does the LED need a resistor?", can the student use the words current, voltage, or limit in their answer (even imperfectly)? |

**Note:** If a pair cannot get their LED to light after 10 minutes, intervene with the troubleshooting table (Section 2). This session is about success and confidence — ensure every pair leaves with a working blinking LED.

---

## 8. Differentiation

### Support
- Provide a pre-printed photo of the completed circuit (taken from overhead) with color-coded arrows showing each connection. Students can match their circuit to the photo.
- Provide a partially completed code template with blanks: `pinMode(___, OUTPUT);` and `digitalWrite(___, ___);` — students fill in the pin number and HIGH/LOW.
- Sentence starter for exit ticket: "The LED needs a resistor because without it, the current would..."
- If a student struggles with breadboard row/column notation, use sticky labels (A, B, C...) on the breadboard columns during the build.

### Extension
- Challenge 1 (SOS pattern): requires understanding that delay controls time, and that a pattern is just repeated groups of digitalWrite/delay calls.
- Challenge 2 (two alternating LEDs): requires a second LED+resistor and understanding that while one pin is HIGH, the other should be LOW.
- Ask them: "What is the minimum delay value before the LED looks like it is always ON? Try values and record the threshold." (Answer: around 20–30ms — connects to the persistence of vision concept.)
- Have them calculate the current in the circuit using Ohm's Law: I = V/R = (5V – 2V) / 220Ω ≈ 13.6 mA. (Preview of Session 3.)

### Visual / Kinesthetic Accommodations
- Use the "rope loop" analogy from Session 1 physically — have a student hold the LED cardboard symbol in the loop while another holds the resistor symbol. Breaking the loop = open circuit.
- Provide a large-print version of the wiring diagram with step numbers highlighted.
- For students who find fine motor work with breadboard difficult: pre-insert the LED and resistor into the board before class, so they only need to add the jumper wires.
- Tinkercad simulation: students can build and test the circuit digitally first, then replicate in hardware — reduces anxiety about "breaking things."
