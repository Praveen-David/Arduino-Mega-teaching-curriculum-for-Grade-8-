# Week 5, Session 9 — Analog vs Digital Signals & the Serial Monitor

**Phase:** Phase 2 — Sensors & Analog Signals
**Session Number:** 9 of 36
**Week:** 5

---

## Learning Objectives

By the end of this session, students will be able to:
- Explain the difference between analog and digital signals using real-world examples.
- Use `analogRead()` to read a voltage at an analog pin and receive a value from 0 to 1023.
- Use `Serial.begin()` and `Serial.println()` to send data from the Arduino to the Serial Monitor.
- Observe and interpret live sensor data on the Serial Monitor window.

---

## Science Curriculum Link

**Grade 8 Concept: Analog vs Digital Signals (Waves & Information Transfer)**

In science, we study how information travels as signals. An **analog signal** is continuous — it can take any value between two extremes (like a volume knob that slides smoothly). A **digital signal** is discrete — it is always one of a fixed set of values (like a light switch: ON or OFF). This session makes that abstract concept tangible: students see their own finger pressure (or a floating wire) produce a continuously changing number from 0 to 1023 — that's an analog signal being digitized by the Arduino's ADC (Analog-to-Digital Converter).

---

## Materials Checklist (per pair)

- 1 × Arduino Mega 2560
- 1 × USB-A to USB-B cable
- 1 × Solderless breadboard
- 1 × Jumper wire, red (5V)
- 1 × Jumper wire, black (GND)
- 1 × Jumper wire, any color (signal to A0)
- 1 × LED (any color)
- 1 × 220 Ω resistor
- Computer with Arduino IDE installed

---

## 2. Teacher Guide (45-Minute Breakdown)

### 0–5 min — Hook / Warm-Up

**Ask the class:** *"Hold up one finger if you've ever seen a record player or a cassette tape. Now hold up two fingers if you've used Spotify or YouTube Music. What's different about how those two store sound?"*

Expected answers: Records have grooves that match the sound wave exactly (analog). Digital music is stored as millions of 1s and 0s (digital).

**Follow-up:** *"If I showed you a number — say, 637 — and told you it was light measured by a sensor, what would you want to know? How do I turn that number back into something meaningful?"* Let students wonder. Tell them that's exactly what they'll figure out today.

---

### 5–15 min — Direct Instruction

**Say to the class:**

> "You already know about digital signals from Phase 1. Every time we used `digitalWrite(pin, HIGH)` or `LOW`, we were working in the digital world — only two choices: 5 V or 0 V. But the real world doesn't work that way. Temperature doesn't jump from cold to hot. Light doesn't jump from dark to bright. Real-world signals are **analog** — they slide through every possible value in between."

**Water-pipe analogy:** Draw a simple diagram on the board.
- Digital = a light switch on a pipe: either fully open (5 V) or fully closed (0 V). No middle ground.
- Analog = a faucet handle: you can turn it to any position between fully off and fully on. Infinite positions.

> "The Arduino Mega has 16 analog input pins labeled A0 through A15. Each one contains a tiny converter called an ADC — Analog-to-Digital Converter. It reads the voltage at that pin (anywhere between 0 V and 5 V) and converts it into a number between **0 and 1023**. Why 1023? Because the ADC has 10 bits — and 2 to the power of 10 is 1024 possible values (0 through 1023)."

Write on the board:
```
0 V  →  0
2.5 V  →  511  (roughly halfway)
5 V  →  1023
```

> "We also need a way to see those numbers on the computer screen. That's what the **Serial Monitor** does. Think of it like a text-message channel between the Arduino and your laptop. We use `Serial.begin(9600)` to open the channel at 9600 baud (characters per second), and `Serial.println()` to print a line of text or a number."

---

### 15–35 min — Hands-On Build + Code

**Remind students:** *"Build → Check → Power on. Wire with USB unplugged."*

**Part 1 — Floating Pin Experiment (no extra wiring needed)**

1. Leave pin A0 **completely unconnected** (floating) for the first experiment.
2. Open the Arduino IDE. Create a new sketch.
3. Type in (or copy from the worksheet) the code in Section 4.
4. Upload the code.
5. Open the Serial Monitor: **Tools → Serial Monitor** (or Ctrl+Shift+M). Set baud rate to **9600**.
6. Watch the numbers scroll. A floating pin will show random, jumping values. *Ask: "Why does it jump around? Because a floating pin picks up electrical noise from the environment — like an antenna."*

**Part 2 — Connect 5V and GND**

7. Power down (unplug USB).
8. Use a red wire to connect **5V** rail to **A0**.
9. Re-upload (re-plug USB) and observe: the Serial Monitor should now show values near **1023**.
10. Power down, move the red wire to connect **GND** to **A0** instead. What do you predict? (*Answer: values near 0.*)

**Part 3 — Touch the wire (body voltage)**

11. Power down, remove the 5V/GND wire from A0. Leave A0 floating again.
12. Power back up. Touch the end of the wire connected to A0 with your finger.
13. Observe: the value changes based on static electricity from your body. *This is a real analog signal!*

**Part 4 — LED brightness indicator (optional, if time allows)**

14. Wire the LED + 220 Ω resistor between digital pin 9 and GND.
15. Upload the extended code that maps the analog reading to `analogWrite`.

---

### 35–42 min — Testing & Debugging

**What success looks like:**
- Serial Monitor is open, set to 9600 baud, and numbers are printing every ~200 ms.
- Connecting A0 to 5V gives values 1015–1023. Connecting to GND gives values 0–5.
- Floating A0 gives jumping, unpredictable values.

**Troubleshooting Table:**

| Symptom | Fix |
|---------|-----|
| Serial Monitor shows nothing | Check that baud rate dropdown at bottom of Serial Monitor is set to **9600**. |
| Numbers don't appear / gibberish | Close and reopen Serial Monitor. Check `Serial.begin(9600)` is in `setup()`. |
| Sketch won't upload | Verify **Tools → Board → Arduino Mega 2560** and correct port are selected. |
| All values are exactly 0 | Check that A0 wire is actually in the A0 hole, not A1 or a digital pin. |
| LED doesn't change brightness | Check LED polarity (long leg = anode = toward pin 9), check resistor is 220 Ω. |

---

### 42–45 min — Reflection / Exit Ticket

**Exit ticket questions (students write on a sticky note or in their worksheet):**

1. *"In your own words, what is the difference between an analog and a digital signal? Give one example of each from real life."*
2. *"If `analogRead()` returns 512, what voltage is at pin A0? Show your calculation."* (Answer: 512/1023 × 5 V ≈ 2.5 V)
3. *"Why do we write `Serial.begin(9600)` in `setup()` instead of `loop()`?"*

Collect sticky notes or call on 2–3 pairs to share.

---

## 3. Circuit Diagram

### ASCII Wiring Diagram

```
   Arduino Mega 2560
   ┌─────────────────┐
   │                 │
   │   A0 ───────────┼──── [Signal wire — leave floating OR connect to 5V/GND for test]
   │                 │
   │   Pin 9 (~) ────┼──── [220 Ω] ──── [LED long leg (+)] ──── [LED short leg (–)] ──── GND
   │                 │
   │   5V ───────────┼──── Red power rail on breadboard
   │   GND ──────────┼──── Black ground rail on breadboard
   └─────────────────┘
```

### Fritzing-Style Build Description

| Step | From | To | Wire Color | Notes |
|------|------|----|------------|-------|
| 1 | Arduino **5V** pin | Breadboard **positive (+) rail** | Red | Power rail |
| 2 | Arduino **GND** pin | Breadboard **negative (–) rail** | Black | Ground rail |
| 3 | Arduino **A0** | Breadboard row 10, column e | Yellow | Signal wire — leave free end floating for Part 1 |
| 4 | Arduino **Pin 9** | Breadboard row 20, column a | Orange | LED signal |
| 5 | Breadboard row 20, column b | 220 Ω resistor leg 1 | — | Resistor inline |
| 6 | 220 Ω resistor leg 2 | LED **long leg (anode, +)** in row 22 | — | |
| 7 | LED **short leg (cathode, –)** | Breadboard **GND rail** | Black | |

**Component values:** 220 Ω resistor (red-red-brown-gold), any color LED.

[Google image search: "Arduino analogRead serial monitor tutorial floating pin"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 * SESSION 9: Analog vs Digital Signals & the Serial Monitor
 * Arduino Mega 2560 — Grade 8 STEM Curriculum
 * ============================================================
 *
 * SCIENCE EXPLANATION:
 * ------------------------------------------------------------------
 * Analog signals are CONTINUOUS — they can take ANY value between
 * two limits (like 0 V to 5 V). Digital signals are DISCRETE —
 * they are always one of a fixed number of values (like HIGH/LOW).
 *
 * The Arduino Mega has a built-in ADC (Analog-to-Digital Converter)
 * on each analog pin (A0–A15). The ADC is "10-bit," which means it
 * divides the 0–5 V range into 1024 steps (2^10 = 1024).
 *
 *   0 V   →  ADC reads:   0
 *   2.5 V →  ADC reads: ~511
 *   5 V   →  ADC reads: 1023
 *
 * Formula:  Voltage = (reading / 1023.0) × 5.0
 *
 * The Serial Monitor is a text window in the Arduino IDE that lets
 * the Arduino send messages to the computer over the USB cable.
 * We call Serial.println() to print a new line of text.
 * ------------------------------------------------------------------
 *
 * Circuit: A0 = floating / connected to 5V or GND for tests
 *          Pin 9 = LED with 220 Ω resistor to GND (optional visual)
 * ============================================================
 */

// ---- Pin Definitions ----
const int ANALOG_PIN = A0;   // We will read voltage from this pin
const int LED_PIN    = 9;    // PWM-capable pin for LED brightness (optional)

void setup() {
  // Start the Serial communication channel at 9600 baud
  // (baud = bits per second; 9600 is the standard beginner speed)
  Serial.begin(9600);

  // Tell the user the program has started
  Serial.println("=== Session 9: Analog Signals ===");
  Serial.println("Reading A0 every 200 ms. Open Serial Monitor at 9600 baud.");
  Serial.println("-----------------------------------------");

  // Set LED pin as output
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  // --- Step 1: Read the analog value ---
  // analogRead() returns an integer from 0 to 1023
  int rawValue = analogRead(ANALOG_PIN);  // Read voltage at A0

  // --- Step 2: Convert to voltage ---
  // Multiply by 5.0 and divide by 1023 to get real volts
  float voltage = (rawValue / 1023.0) * 5.0;  // Calculate actual voltage

  // --- Step 3: Send data to the Serial Monitor ---
  Serial.print("Raw ADC value: ");   // Print label (no new line)
  Serial.print(rawValue);            // Print the number
  Serial.print("  |  Voltage: ");    // Print separator
  Serial.print(voltage, 2);          // Print voltage with 2 decimal places
  Serial.println(" V");              // Print units and move to next line

  // --- Step 4: Use the reading to control LED brightness ---
  // map() converts a number from one range to another range
  // Here: convert 0–1023  →  0–255  (the range for analogWrite)
  int brightness = map(rawValue, 0, 1023, 0, 255);  // Scale the value
  analogWrite(LED_PIN, brightness);  // Set LED brightness

  // >>> TRY CHANGING THIS: Change 200 to 500 or 50. What happens to the printout speed?
  delay(200);  // Wait 200 milliseconds before the next reading
}

// ===== CHALLENGE =====
// 1. Print a "bar graph" in the Serial Monitor using a loop that
//    prints a '#' character for every 50 units of rawValue.
//    (Hint: use a for loop from 0 to rawValue/50 and Serial.print("#"))
//
// 2. Calculate and print the percentage: (rawValue / 1023.0) * 100
//    Add the label "  | Percent: " and "%" to your output line.
//
// 3. Add a second label line above each reading showing whether
//    the value is "HIGH (>512)", "MID (256–512)", or "LOW (<256)".
```

---

## 5. Student Worksheet

---

### Session 9 Worksheet — Analog vs Digital Signals & the Serial Monitor

**Name(s):** _________________________________ **Date:** _____________ **Kit #:** _____

**Objectives:**
- Tell the difference between analog and digital signals.
- Read a voltage using `analogRead()` and see the number on the Serial Monitor.
- Connect what you see to the science of signal types.

---

#### What I Already Know (Warm-Up)

Answer these before the lesson starts:

1. In Phase 1, we used `digitalRead()` — what values could it return? __________

2. A light dimmer switch on a wall can be turned to ANY brightness level. Is that more like an analog or a digital signal? __________

3. What do you think the number **511** might mean if the Arduino is reading a pin between 0 V and 5 V? __________

---

#### Build It — Step by Step

Follow these instructions with USB **unplugged** until told to plug in.

1. Connect **5V** to the breadboard positive rail (red wire).
2. Connect **GND** to the breadboard negative rail (black wire).
3. Place a yellow wire from **Arduino A0** into the breadboard (leave the other end free — do not connect it to anything yet).
4. Place the LED in the breadboard with the **long leg (anode, +)** in row 22.
5. Place the **220 Ω resistor** between the LED's long leg and **pin 9** on the Arduino.
6. Connect the LED's **short leg (cathode, –)** to the **GND rail** with a black wire.
7. Have your partner double-check all wires. Now plug in the USB.
8. Open Arduino IDE. Enter the code from Section 4. Click **Upload** (arrow button).
9. After upload, go to **Tools → Serial Monitor** and set the baud rate to **9600**.

---

#### Predict!

Before you connect A0 to anything, make your predictions:

| Situation | Your Prediction (what value will show?) | What you actually see |
|-----------|----------------------------------------|----------------------|
| A0 is floating (connected to nothing) | | |
| A0 is connected to 5V with a red wire | | |
| A0 is connected to GND with a black wire | | |
| You touch the wire at A0 with your finger | | |

---

#### Observe / Data Table

Fill in the table as you do each test. Record 3 readings for each test.

| Test | Reading 1 | Reading 2 | Reading 3 | Average | Voltage (calculate) |
|------|-----------|-----------|-----------|---------|---------------------|
| A0 floating | | | | | |
| A0 to 5V | | | | | |
| A0 to GND | | | | | |
| Finger touching A0 | | | | | |

**Voltage formula:** Voltage = (Average ÷ 1023) × 5.0 V

---

#### What Did You Notice?

1. Why did the floating pin give unpredictable, jumping values?

   ___________________________________________________________________________

2. When A0 was connected to 5V, you got ~1023. When connected to GND, you got ~0. What does this tell you about the range of `analogRead()`?

   ___________________________________________________________________________

3. Look at the LED while you change the connections. How did the LED brightness change as the raw value changed?

   ___________________________________________________________________________

4. The Arduino's ADC is "10-bit." What does that mean for the number of steps between 0 V and 5 V?

   ___________________________________________________________________________

---

#### Science Connection

The ADC (Analog-to-Digital Converter) inside the Arduino converts a continuous analog voltage into a discrete digital number. This is exactly how a microphone in a smartphone converts continuous sound waves into digital audio files — millions of times per second!

**Question:** A music file on a CD is sampled at 44,100 times per second with 16-bit resolution. How many possible volume levels can a 16-bit ADC represent? (Hint: 2^16 = ?)

Answer: ___________________________________________________________________________

How does that compare to the Arduino's 10-bit ADC (1024 steps)?

___________________________________________________________________________

---

#### Challenge Extension

Open the Serial **Plotter** instead of the Serial Monitor (Tools → Serial Plotter). Wave your hand near the floating wire. You should see a live graph of your readings.

- Sketch what the graph looks like when A0 is floating:

  (Draw here)

- Sketch what the graph looks like when A0 is connected to 5V:

  (Draw here)

- What do you think the Serial Plotter could be useful for in a real science experiment?

  ___________________________________________________________________________

---

## 6. Safety Notes

| Hazard | Precaution |
|--------|------------|
| Short circuit via floating wire | Keep loose signal wires away from the 5V and GND rails. A floating wire accidentally landing on both rails at once can short the board. |
| LED polarity | Always connect the long leg (anode) toward the signal pin and short leg (cathode) toward GND. Reversed LED = no light, no damage, but confusing. |
| USB power only | This session uses USB power only (no extra power supply). Maximum current draw is well within safe limits. |

**Build → Check → Power on.** Never change wiring while the USB is plugged in.

**If something goes wrong:**
- Numbers look strange or freeze → unplug USB, re-check A0 wire, replug.
- Board feels warm → unplug immediately and tell the teacher.
- Serial Monitor shows garbage characters → close it, change baud to 9600, reopen.

---

## 7. Assessment Rubric

### Formative Check (not graded)

This is a practice / exploration session. The teacher observes for the following look-fors:

| Look-For | Not Yet | Got It |
|----------|---------|--------|
| Student can explain in their own words why a floating pin gives random values | | |
| Student correctly calculates voltage from a raw ADC reading (e.g., 512 → ~2.5 V) | | |
| Serial Monitor is open, baud matches code, and readings are scrolling | | |
| Student can connect the analog/digital concept to at least one real-world example | | |

**Exit Ticket Collection:** Collect sticky notes. Students who cannot convert a raw reading to volts need a brief re-teach at the start of Session 10.

---

## 8. Differentiation

### Support
- Provide a printed "Serial Monitor Cheat Sheet" showing where the baud rate dropdown is and how to open the monitor.
- Pre-write the code in a file and give it to students who struggle with typing — their focus should be on the *observation* and *science connection*.
- Offer a sentence starter for the science connection: *"An analog signal is like _______ because it can take _______ values."*
- Use Tinkercad Circuits (free, browser-based) to simulate the floating pin behavior before using real hardware.

### Extension
- Challenge students to write code that **prints a warning message** ("VOLTAGE TOO HIGH!") when rawValue exceeds 900.
- Ask: *"The Arduino's ADC reads 0–5 V. What if our sensor only outputs 0–3.3 V? What would the maximum raw value be?"* (Answer: ~676)
- Introduce the concept of **sampling rate**: change the `delay()` to 10 ms. How fast can the Arduino sample? Calculate: 1000 ms ÷ 10 ms = 100 readings per second.

### Visual / Kinesthetic Accommodations
- Use the Serial Plotter (Tools → Serial Plotter) for a visual graph — powerful for visual learners.
- Have students physically slide a finger along the desk while holding the wire and describe the "feeling" as they watch numbers change — connects tactile sensation to the concept of continuous (analog) change.
- Post a number line on the wall from 0 to 1023. Mark 0 V, 2.5 V, and 5 V. Have students come up and point to where their reading falls.
