# Week 5, Session 10 — Potentiometers & Voltage Dividers

**Phase:** Phase 2 — Sensors & Analog Signals
**Session Number:** 10 of 36
**Week:** 5

---

## Learning Objectives

By the end of this session, students will be able to:
- Explain how a potentiometer works as a variable resistor and as a voltage divider.
- Read a potentiometer value using `analogRead()` on pin A0 (introduced in Session 9).
- Use `map()` and `constrain()` to scale sensor data to a useful output range.
- Control LED brightness and blink rate using a potentiometer.

---

## Science Curriculum Link

**Grade 8 Concept: Variable Resistance & Voltage Division**

Students already know from Phase 1 (Session 3) that resistance controls how much current flows in a circuit (Ohm's Law: V = IR). A potentiometer is a **variable resistor** — it lets you physically adjust resistance from 0 Ω to its rated maximum (10 kΩ here) by turning a dial. When connected between 5V, a wiper, and GND, it forms a **voltage divider**: the output voltage at the wiper depends on where the wiper is positioned, splitting the 5 V across the two halves of the resistive track. This is one of the most important and widely used circuits in electronics.

---

## Materials Checklist (per pair)

- 1 × Arduino Mega 2560
- 1 × USB-A to USB-B cable
- 1 × Solderless breadboard
- 1 × 10 kΩ potentiometer (3-pin)
- 1 × LED (any color)
- 1 × 220 Ω resistor
- Jumper wires: red × 2, black × 2, yellow × 1, orange × 1
- Computer with Arduino IDE installed

---

## 2. Teacher Guide (45-Minute Breakdown)

### 0–5 min — Hook / Warm-Up

**Hold up a potentiometer (or show one on the document camera).** Ask:

*"This little dial is inside every speaker volume knob, every dimmer switch, and every analog joystick in a video game controller. Can anyone guess what it does?"*

Let a few students guess. Then ask: *"Last session, we learned that `analogRead()` gives us 0 to 1023. What if WE could control that number with our fingers — dial it up, dial it down? What could we do with that?"*

Expected answers: control brightness, control speed, control volume.

*"That's exactly what a potentiometer does. Let's build one."*

---

### 5–15 min — Direct Instruction

**Draw on the board:**

```
5V ──────[  R1  ]──────[wiper]──────[  R2  ]────── GND
                           │
                          A0
```

> "A potentiometer is just a long resistive track with a sliding contact called a **wiper**. When you turn the knob, the wiper moves. R1 is the resistance above the wiper; R2 is below it. Their total always equals 10 kΩ. The voltage at the wiper — which is what A0 reads — equals:"

Write on the board:
```
V_out = 5V × R2 / (R1 + R2)
```

> "When the wiper is all the way down: R2 = 0 Ω, so V_out = 0 V → ADC reads 0.
> When the wiper is all the way up: R2 = 10 kΩ, so V_out = 5 V → ADC reads 1023.
> This is called a **voltage divider** — two resistors splitting a voltage."

**Introduce `map()`:**

> "The raw `analogRead()` value is 0–1023. But `analogWrite()` for LED brightness needs 0–255. They're different ranges! The `map()` function rescales a number from one range to another. Think of it like a math proportion:"

Write on the board:
```
map(value, fromLow, fromHigh, toLow, toHigh)
map(rawValue, 0, 1023, 0, 255)  // Converts ADC range to PWM range
```

**Introduce `constrain()`:**

> "`constrain()` is a safety guard. It clips a value so it never goes outside a minimum and maximum. We use it to prevent glitches from pushing our LED beyond 255 or below 0."

```
constrain(value, min, max)
constrain(brightness, 0, 255)   // Will never let brightness go below 0 or above 255
```

---

### 15–35 min — Hands-On Build + Code

**Remind students:** *"Build → Check → Power on. Unplug USB first."*

1. Unplug the Arduino USB cable.
2. Place the potentiometer across the center of the breadboard so its three pins sit in three different rows.
3. Connect the **left pin** of the pot to the **5V rail** (red wire).
4. Connect the **right pin** of the pot to the **GND rail** (black wire).
5. Connect the **middle pin** (wiper) of the pot to **Arduino A0** (yellow wire).
6. Place the LED in the breadboard: **long leg (anode)** in one row, **short leg (cathode)** in the next row down.
7. Place the **220 Ω resistor** between the LED's long leg and **Arduino pin 9**.
8. Connect the LED's short leg to the **GND rail** (black wire).
9. Have your partner check: Is 5V on the left pot pin? GND on the right? Middle to A0?
10. Plug in the USB cable. Open the Arduino IDE.
11. Enter and upload the code from Section 4.
12. Open the Serial Monitor (9600 baud). Turn the potentiometer knob slowly.
13. Observe: the number on the Serial Monitor should change from 0 to 1023 smoothly.
14. Look at the LED: it should get brighter as you turn up the pot.

---

### 35–42 min — Testing & Debugging

**What success looks like:**
- Serial Monitor shows a steadily changing number (0–1023) as the knob turns.
- LED visibly gets brighter and dimmer as the pot turns.
- Values near 0 at one extreme; near 1023 at the other.
- No random jumping (unlike the floating pin from Session 9).

**Troubleshooting Table:**

| Symptom | Fix |
|---------|-----|
| Value stuck at 0 | Check that the wiper (middle pin) is connected to A0, not to GND. |
| Value stuck at 1023 | Check that the wiper is connected to A0, not to 5V. |
| Values jump erratically | Check for a loose wire at the wiper. Press the pot firmly into the breadboard. |
| LED doesn't change brightness | Check pin 9 is connected correctly; ensure code uses `analogWrite`, not `digitalWrite`. |
| LED doesn't light at all | Check LED polarity (long leg = anode = toward pin 9). Check 220 Ω resistor. |
| Serial Monitor shows nothing | Check baud rate = 9600. Close and reopen Serial Monitor. |

---

### 42–45 min — Reflection / Exit Ticket

**Exit ticket questions:**

1. *"Draw the voltage divider circuit for a potentiometer. Label 5V, GND, the wiper, and A0."*
2. *"If `analogRead()` returns 512, what does `map(512, 0, 1023, 0, 255)` return? Show your work."* (Answer: ≈ 127)
3. *"Name one real device that uses a potentiometer."*

---

## 3. Circuit Diagram

### ASCII Wiring Diagram

```
   Arduino Mega 2560
   ┌─────────────────────┐
   │                     │
   │   5V ───────────────┼──── Red rail (+) ──── Pot LEFT pin
   │                     │
   │   GND ──────────────┼──── Black rail (–) ── Pot RIGHT pin
   │                     │
   │   A0 ───────────────┼──── Pot MIDDLE pin (wiper)
   │                     │
   │   Pin 9 (~) ────────┼──── [220 Ω] ──── LED (+, long leg) ──── LED (–, short leg) ──── GND
   │                     │
   └─────────────────────┘

   Potentiometer pins (viewed from front, knob facing you):
   [ LEFT | MIDDLE(wiper) | RIGHT ]
     5V        A0           GND
```

### Fritzing-Style Build Description

| Step | From | To | Wire Color | Notes |
|------|------|----|------------|-------|
| 1 | Arduino **5V** | Breadboard **positive (+) rail** | Red | Power rail |
| 2 | Arduino **GND** | Breadboard **negative (–) rail** | Black | Ground rail |
| 3 | Breadboard **+rail** | Pot **left pin** (row 5) | Red | 5V to pot |
| 4 | Breadboard **–rail** | Pot **right pin** (row 7) | Black | GND to pot |
| 5 | Pot **middle pin** (row 6) | Arduino **A0** | Yellow | Wiper = signal |
| 6 | Arduino **Pin 9** | Breadboard row 15 | Orange | LED signal |
| 7 | Row 15 | 220 Ω resistor leg 1 | — | Inline resistor |
| 8 | 220 Ω resistor leg 2 | LED **long leg (anode, +)** row 17 | — | |
| 9 | LED **short leg (cathode, –)** | Breadboard **GND (–) rail** | Black | |

**Component values:** 10 kΩ potentiometer, 220 Ω resistor (red-red-brown-gold), any color LED.

[Google image search: "Arduino potentiometer LED brightness circuit breadboard"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 * SESSION 10: Potentiometers & Voltage Dividers
 * Arduino Mega 2560 — Grade 8 STEM Curriculum
 * ============================================================
 *
 * SCIENCE EXPLANATION:
 * ------------------------------------------------------------------
 * A potentiometer is a VARIABLE RESISTOR built with a long
 * resistive strip and a movable wiper contact. It forms a
 * VOLTAGE DIVIDER circuit:
 *
 *   5V ──[R1]──[wiper]──[R2]── GND
 *                   │
 *                  A0
 *
 * The voltage at the wiper (A0) = 5V × R2 / (R1 + R2)
 *
 * When wiper is at the bottom:  V_out ≈ 0 V  →  ADC = 0
 * When wiper is at the top:     V_out ≈ 5 V  →  ADC = 1023
 *
 * map() rescales a number proportionally from one range to another.
 * constrain() prevents a value from going outside safe limits.
 * ------------------------------------------------------------------
 *
 * Circuit:  10 kΩ pot (left=5V, middle=A0, right=GND)
 *           LED + 220 Ω on pin 9
 * ============================================================
 */

// ---- Pin Definitions ----
const int POT_PIN    = A0;  // Potentiometer wiper connected here
const int LED_PIN    = 9;   // PWM-capable pin (~) for LED brightness

void setup() {
  Serial.begin(9600);          // Open serial channel at 9600 baud
  pinMode(LED_PIN, OUTPUT);    // Set LED pin as output

  Serial.println("=== Session 10: Potentiometer ===");
  Serial.println("Turn the pot knob. Watch the values change!");
  Serial.println("--------------------------------------");
}

void loop() {
  // --- Read the potentiometer ---
  int rawValue = analogRead(POT_PIN);  // Returns 0–1023

  // --- Scale for LED brightness ---
  // map() converts the range 0–1023 to the range 0–255 (for analogWrite)
  int brightness = map(rawValue, 0, 1023, 0, 255);

  // constrain() ensures brightness never accidentally goes outside 0–255
  brightness = constrain(brightness, 0, 255);

  // --- Scale for blink rate (optional experiment) ---
  // Convert 0–1023 to a delay of 1000 ms down to 50 ms
  // >>> TRY CHANGING THIS: swap to this line to make the pot control blink rate
  // int blinkDelay = map(rawValue, 0, 1023, 1000, 50);

  // --- Control the LED ---
  analogWrite(LED_PIN, brightness);  // Set LED brightness via PWM

  // --- Calculate voltage for display ---
  float voltage = (rawValue / 1023.0) * 5.0;  // Convert raw reading to volts

  // --- Print to Serial Monitor ---
  Serial.print("Raw: ");
  Serial.print(rawValue);           // Print raw 0–1023 value
  Serial.print("  | Brightness: ");
  Serial.print(brightness);         // Print mapped 0–255 value
  Serial.print("  | Voltage: ");
  Serial.print(voltage, 2);         // Print voltage with 2 decimal places
  Serial.println(" V");

  // >>> TRY CHANGING THIS: Change 100 to 500 or 10 to see effect on smoothness
  delay(100);  // Short delay to prevent serial flooding
}

// ===== CHALLENGE =====
// 1. BLINK RATE CONTROL: Comment out the analogWrite line above.
//    Uncomment the blinkDelay line. Then add:
//      digitalWrite(LED_PIN, HIGH);
//      delay(blinkDelay);
//      digitalWrite(LED_PIN, LOW);
//      delay(blinkDelay);
//    Now the pot controls how fast the LED blinks!
//
// 2. THREE ZONES: Use if/else to print "SLOW", "MEDIUM", or "FAST"
//    depending on whether rawValue is <341, 341–682, or >682.
//
// 3. SECOND LED: Wire a second LED on pin 10. Make it get DIMMER
//    as the first LED gets brighter. Hint: use map(rawValue, 0, 1023, 255, 0)
//    for the second LED.
//
// 4. MATH CHECK: Calculate the voltage using the divider formula:
//    If R2 = rawValue * (10000.0 / 1023), what is V_out = 5.0 * R2 / 10000?
//    Print this and compare to the simpler calculation above — are they equal?
```

---

## 5. Student Worksheet

---

### Session 10 Worksheet — Potentiometers & Voltage Dividers

**Name(s):** _________________________________ **Date:** _____________ **Kit #:** _____

**Objectives:**
- Understand how a potentiometer works as a variable voltage divider.
- Use `map()` to scale a sensor reading to a useful output range.
- Control LED brightness with a physical knob.

---

#### What I Already Know (Warm-Up)

1. From Session 3 (Ohm's Law), what happens to current when resistance increases?

   ___________________________________________________________________________

2. From Session 9, what does `analogRead()` return when A0 is connected to 5V? To GND?

   ___________________________________________________________________________

3. What is the range of values that `analogWrite()` accepts for LED brightness (from Session 7)?

   ___________________________________________________________________________

---

#### Build It — Step by Step

Wire with USB **unplugged**.

1. Insert the **potentiometer** across the breadboard center gap (one pin in each of 3 rows).
2. **Left pin** of pot → breadboard **positive (+) rail** (red wire).
3. **Right pin** of pot → breadboard **negative (–) rail** (black wire).
4. **Middle pin** (wiper) → **Arduino A0** (yellow wire).
5. Insert the **LED**: long leg (anode) in row 17, short leg (cathode) in row 18.
6. **220 Ω resistor** between **Arduino pin 9** and LED's long leg.
7. LED's short leg → **GND rail** (black wire).
8. Partner checks all connections. Plug in USB.
9. Enter and upload the code. Open Serial Monitor at **9600 baud**.
10. Slowly turn the potentiometer knob. Record what you see.

---

#### Predict!

Before turning the pot:

| Knob Position | Predicted `analogRead()` value | Predicted LED brightness | Actual reading |
|---------------|-------------------------------|--------------------------|----------------|
| All the way to the left (minimum) | | | |
| In the middle | | | |
| All the way to the right (maximum) | | | |

---

#### Observe / Data Table

Turn the pot to each position and record the Serial Monitor readings:

| Knob Position | Raw Value (0–1023) | Voltage (V) | LED Brightness |
|---------------|-------------------|-------------|----------------|
| Minimum (left) | | | |
| Quarter turn | | | |
| Halfway | | | |
| Three-quarter turn | | | |
| Maximum (right) | | | |

**Calculate:** When the raw value is 512, what does `map(512, 0, 1023, 0, 255)` give you?

Show your working: ___________________________________________________________________________

---

#### What Did You Notice?

1. Did the LED brightness change smoothly or in jumps? What does that tell you about the type of signal the pot produces?

   ___________________________________________________________________________

2. When the pot was at half-turn (roughly 512), was the LED at exactly half brightness? Why or why not?

   ___________________________________________________________________________

3. What would happen if you forgot to use `constrain()` and your `rawValue` somehow glitched to 1100? What would `map(1100, 0, 1023, 0, 255)` return? Is that a problem?

   ___________________________________________________________________________

4. Name a device in your everyday life that uses a potentiometer (a physical knob controlling a continuous range). Describe what it controls.

   ___________________________________________________________________________

---

#### Science Connection

A voltage divider is one of the most fundamental circuits in electronics. The formula is:

**V_out = V_in × R2 / (R1 + R2)**

**Calculate:** If the pot knob is at the 75% position, R2 = 7,500 Ω and R1 = 2,500 Ω. What is V_out when V_in = 5 V?

V_out = 5 × 7500 / (2500 + 7500) = ________ V

What `analogRead()` value would you expect? ________ (Remember: 1023 ÷ 5 × V_out)

---

#### Challenge Extension

Try the "BLINK RATE CONTROL" challenge from the code:
1. Modify the code so the pot controls **how fast** the LED blinks (not brightness).
2. At minimum pot position: LED blinks slowly (once per second).
3. At maximum pot position: LED blinks as fast as possible.
4. Record the delay value at each pot position in the table above (replace LED Brightness column with Blink Delay).

**Think about it:** Would a fast-blinking LED look like a solid dim glow at very high speeds? Try it! This is actually how movie projectors and old TVs work — a phenomenon called **persistence of vision**.

---

## 6. Safety Notes

| Hazard | Precaution |
|--------|------------|
| Short circuit from mis-wired pot | Always verify: left pin = 5V, right pin = GND. NEVER connect both outer pins to the same rail. |
| LED without resistor | The 220 Ω resistor is essential. Removing it while powered can burn out the LED instantly. |
| Pot mechanical damage | Turn the knob gently — forcing the knob past the hard stop will damage the component. |

**Build → Check → Power on.** Unplug USB before changing any wiring.

**If something goes wrong:**
- Values jump or are wrong → check the three pot connections (left=5V, middle=A0, right=GND).
- LED very dim or not working → check resistor value (should be 220 Ω, not 10 kΩ).
- Board gets warm → short circuit likely. Unplug immediately and re-check wiring.

---

## 7. Assessment Rubric

### Formative Check (not graded)

| Look-For | Not Yet | Got It |
|----------|---------|--------|
| Student can describe the voltage divider circuit (what's connected to each pin) | | |
| Serial Monitor shows smooth 0–1023 range as pot is turned | | |
| LED visibly changes brightness when pot is turned | | |
| Student can calculate `map(512, 0, 1023, 0, 255)` correctly | | |
| Student links the pot to the concept of variable resistance | | |

---

## 8. Differentiation

### Support
- Provide a labeled photo of the potentiometer with arrows indicating "left = 5V," "middle = wiper/A0," "right = GND."
- Give a partially completed worksheet with the voltage divider formula already filled in; students only substitute values.
- If the concept of map() is confusing, use a proportion analogy: *"If 1023 maps to 255, then 512 maps to ___. It's like a unit conversion."*
- Pre-build the LED + resistor part so students only need to wire the potentiometer.

### Extension
- Research challenge: *"What is a 'trim potentiometer' (trimpot)? How is it used in professional electronics?"*
- Have students wire a **second potentiometer on A1** to control the blink duration separately from brightness — two independent controls.
- Introduce the concept of **non-linearity**: swap the 10 kΩ pot for a 1 kΩ fixed resistor in series with the wiper to see how the divider formula changes.
- Write code to print the voltage divider formula calculation alongside the simpler ADC formula — are the results equal?

### Visual / Kinesthetic Accommodations
- Before wiring, physically hold the potentiometer. Turn the knob. Explain that inside, a wiper is sliding along a resistive strip — you can even draw this.
- Label each breadboard row with a small sticky label: "5V," "WIPER/A0," "GND" before the student inserts the pot.
- For students who finish early, draw a graph of Raw Value (x-axis) vs Voltage (y-axis) by hand using their data table. What shape is the line? (Linear — it should be straight.)
