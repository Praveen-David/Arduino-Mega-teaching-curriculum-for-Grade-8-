# Week 6, Session 12 — Automatic Night Light (LDR + Threshold)

**Phase:** Phase 2 — Sensors & Analog Signals
**Session Number:** 12 of 36
**Week:** 6

---

## Learning Objectives

By the end of this session, students will be able to:
- Use an `if` statement with a threshold value to make an automatic decision based on sensor input.
- Calibrate a sensor system by measuring real-world minimum and maximum values.
- Explain the concept of a control threshold and relate it to biological and engineered systems.
- Build a working automatic night-light that turns an LED on when it gets dark.

---

## Science Curriculum Link

**Grade 8 Concept: Light Intensity, Thresholds & Automatic Control**

In science and engineering, a **threshold** is a critical level at which a system changes its behavior. This appears throughout biology (e.g., the threshold light level at which plants start photosynthesizing, or the light level at which your pupils dilate), engineering (street lights turning on at dusk), and electronics. Today's session bridges the analog world of Session 11 (continuous light readings) with a discrete decision (LED on or off). This is the first time students build a fully automatic control system — a foundational concept in systems thinking.

---

## Materials Checklist (per pair)

- 1 × Arduino Mega 2560
- 1 × USB-A to USB-B cable
- 1 × Solderless breadboard
- 1 × LDR (photoresistor) — same as Session 11
- 1 × 10 kΩ resistor (brown-black-orange-gold)
- 1 × LED (any color — white or yellow recommended for "night light" effect)
- 1 × 220 Ω resistor (red-red-brown-gold)
- Jumper wires: red × 2, black × 2, yellow × 1, orange × 1
- Computer with Arduino IDE installed
- (Optional) cardboard or a box to create a "dark enclosure" for testing

---

## 2. Teacher Guide (45-Minute Breakdown)

### 0–5 min — Hook / Warm-Up

Ask the class:

*"Has anyone noticed that streetlights turn on automatically at dusk? Or that your phone screen gets dimmer when you're in the dark? How do those systems 'know' it's dark?"*

Let students brainstorm. Expected answers: there's a sensor; it measures the light; when it's below a certain level it switches on.

*"Exactly — there's a threshold. Below a certain level of light, a switch triggers. Above it, the switch turns off. Today you're going to build exactly that — your own automatic night light — using the LDR circuit from last session and one very important line of code: an if statement."*

Quick review question: *"Last session, what value did your LDR circuit give in the dark? In bright light?"* Get a rough range from the class — this will help set the threshold.

---

### 5–15 min — Direct Instruction

**Threshold concept:**

Draw on the board:

```
Light level (raw value)
1023 ──────────────────────────────────────────────────
                     ← BRIGHT (LED OFF)
                                        
  ─────────────────── THRESHOLD LINE ───────────────────  ← e.g., 400
                     
                     ← DARK (LED ON)
   0 ──────────────────────────────────────────────────
```

> "A threshold is just a number we choose. If the light reading is BELOW the threshold → dark → LED turns ON. If the reading is ABOVE the threshold → bright → LED turns OFF. One line of code makes this decision every loop — hundreds of times per second."

**`if` statement review (students learned this in Phase 1):**

> "You already know `if` statements from Session 6. Here's the pattern for our night light:"

Write on the board:
```cpp
if (lightLevel < threshold) {
  digitalWrite(LED_PIN, HIGH);  // It's dark — turn LED on
} else {
  digitalWrite(LED_PIN, LOW);   // It's bright — turn LED off
}
```

> "The key question is: what should `threshold` be? If we set it too low, the LED will only turn on in nearly complete darkness. Too high, and it turns on in a normally lit room. We need to **calibrate** our sensor."

**Calibration:**

> "Calibration means measuring the actual range our sensor gives in real conditions, then choosing a threshold that makes sense for those conditions. We'll do a quick calibration at the start: measure the 'dark' value (hand over LDR) and the 'bright' value (normal room light), then pick a number roughly halfway between."

Write the formula on the board:
```
threshold = (dark_value + bright_value) / 2
```

---

### 15–35 min — Hands-On Build + Code

**Note:** The circuit is the **same as Session 11** — students can keep their existing build if they haven't disassembled it, or rebuild it from scratch.

1. Verify (or rebuild) the LDR voltage divider circuit from Session 11:
   - LDR between **5V** and junction.
   - 10 kΩ resistor between junction and **GND**.
   - Junction to **Arduino A0** (yellow wire).
2. LED (long leg to **pin 9** via 220 Ω resistor, short leg to GND) — same as before.
3. Partner checks connections. Plug in USB.
4. **Step 1 — Calibration run:** Upload the "calibration sketch" (first code block in Section 4).
5. Open Serial Monitor (9600 baud).
6. Cover the LDR completely with both hands for 5 seconds. Note the average reading — record as `darkValue`.
7. Uncover LDR under normal room light. Note the average reading — record as `brightValue`.
8. Calculate threshold: `(darkValue + brightValue) / 2`.
9. Write your threshold value in your worksheet.
10. **Step 2 — Night light sketch:** Upload the main code (Section 4), replacing the `THRESHOLD` constant with your calibrated value.
11. Test: cover the LDR → LED should turn ON. Uncover → LED should turn OFF.
12. Fine-tune the threshold if needed: if the LED turns on too easily, raise the threshold. If it never turns on, lower it.

---

### 35–42 min — Testing & Debugging

**What success looks like:**
- LED turns ON when the LDR is covered / in darkness.
- LED turns OFF when the LDR is uncovered in room light.
- Transition happens at a consistent light level (no flickering unless light level is right at the threshold).

**Troubleshooting Table:**

| Symptom | Fix |
|---------|-----|
| LED is always ON | Threshold is too high — reduce it. Or check LDR circuit: values may be inverted (see Session 11 notes). |
| LED is always OFF | Threshold is too low — increase it. Or LED is wired to output pin incorrectly. |
| LED flickers rapidly at the threshold | The light is hovering right at the threshold. Add hysteresis (see Challenge section) or choose a clearer light/dark test. |
| Circuit seems wired correctly but values don't change | Same fixes as Session 11: check LDR is in correct orientation (5V side), junction wired to A0. |
| Light level reads 0 all the time | Check the 10 kΩ resistor connection to GND. May be missing. |

---

### 42–45 min — Reflection / Exit Ticket

**Exit ticket questions:**

1. *"What is a threshold in a control system? Give one example from nature and one from engineering."*
2. *"Your night light threshold is set to 350. The light reading is 380. Is the LED on or off? Show your if-statement logic."*
3. *"Why is calibration important? What would happen if you used a threshold value measured in a different classroom?"*

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
   │  A0 ─────────────────┼──── Junction J1 (between LDR and 10 kΩ)
   │                      │
   │  Pin 9 (~) ──────────┼──── [220 Ω] ──── LED (+, long leg) ──── LED (–, short leg) ──── GND
   └──────────────────────┘
   
   Voltage Divider:
   
   Red rail (5V) ──── [LDR] ──── J1 ──── [10 kΩ] ──── Black rail (GND)
                                  │
                                  └── Arduino A0 (yellow)
```

*(This circuit is identical to Session 11. If students kept their builds, no rewiring needed.)*

### Fritzing-Style Build Description

| Step | From | To | Wire Color | Notes |
|------|------|----|------------|-------|
| 1 | Arduino **5V** | Breadboard **+ rail** | Red | Power |
| 2 | Arduino **GND** | Breadboard **– rail** | Black | Ground |
| 3 | Breadboard **+ rail** | LDR **leg 1** (row 8) | Red | 5V to LDR |
| 4 | LDR **leg 2** (row 10) | 10 kΩ resistor **leg 1** | — | LDR to junction |
| 5 | 10 kΩ resistor **leg 2** | Breadboard **– rail** | — | Fixed R to GND |
| 6 | Row 10 (junction) | Arduino **A0** | Yellow | Signal |
| 7 | Arduino **Pin 9** | 220 Ω resistor leg 1 (row 20) | Orange | LED drive |
| 8 | 220 Ω resistor leg 2 | LED **long leg** (row 22) | — | |
| 9 | LED **short leg** | Breadboard **– rail** | Black | |

**Component values:** LDR (CdS photoresistor), 10 kΩ (brown-black-orange-gold), 220 Ω (red-red-brown-gold).

[Google image search: "Arduino LDR automatic night light if statement threshold circuit"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 * SESSION 12: Automatic Night Light (LDR + Threshold)
 * Arduino Mega 2560 — Grade 8 STEM Curriculum
 * ============================================================
 *
 * SCIENCE EXPLANATION:
 * ------------------------------------------------------------------
 * A THRESHOLD is a critical level at which a system changes state.
 * This is seen in nature (light level triggers sleep in animals,
 * temperature triggers sweating) and in engineering (streetlights,
 * thermostats, automatic doors).
 *
 * Here we convert a continuous analog reading (0–1023) into a
 * binary decision: dark or light. When the light level falls
 * BELOW our threshold → we call it "dark" → LED turns ON.
 *
 * CALIBRATION: Because different rooms, sensors, and component
 * tolerances give different ranges, we measure our specific
 * sensor's range and pick a threshold that makes sense for it.
 * This is what real scientists and engineers do!
 * ------------------------------------------------------------------
 *
 * Circuit:  LDR + 10 kΩ voltage divider on A0 (same as Session 11)
 *           LED + 220 Ω on pin 9
 * ============================================================
 */

// ---- STEP 1: CALIBRATION SKETCH ----
// Run this FIRST to measure your sensor's dark and bright values.
// Then move to Step 2 (the main night-light sketch) below.
//
// Uncomment this block and upload to calibrate:
/*
void setup() {
  Serial.begin(9600);
  Serial.println("CALIBRATION MODE");
  Serial.println("Cover LDR → note value (dark)");
  Serial.println("Normal light → note value (bright)");
}
void loop() {
  int val = analogRead(A0);
  Serial.println(val);
  delay(200);
}
*/

// ============================================================
// ---- STEP 2: MAIN NIGHT-LIGHT SKETCH ----
// After calibrating, enter your measured values below.
// ============================================================

// ---- Pin Definitions ----
const int LDR_PIN = A0;   // LDR voltage divider output
const int LED_PIN = 9;    // LED (with 220 Ω resistor)

// ---- Calibration Values — CHANGE THESE after Step 1 ----
// >>> TRY CHANGING THIS: replace 400 with your calibrated threshold
const int THRESHOLD = 400;  // Below this = DARK → turn LED on

// Optional: add HYSTERESIS to prevent flickering
// The LED turns ON below THRESHOLD_LOW, turns OFF above THRESHOLD_HIGH
// (If you don't want hysteresis, set both to the same value = THRESHOLD)
const int THRESHOLD_LOW  = THRESHOLD - 30;   // LED turns ON below this
const int THRESHOLD_HIGH = THRESHOLD + 30;   // LED turns OFF above this

// Track LED state for hysteresis
bool ledIsOn = false;

void setup() {
  Serial.begin(9600);
  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, LOW);   // Start with LED off

  Serial.println("=== Session 12: Automatic Night Light ===");
  Serial.print("Threshold: ");
  Serial.println(THRESHOLD);
  Serial.println("Cover the LDR to turn on the LED!");
  Serial.println("-----------------------------------------");
}

void loop() {
  // --- Read current light level ---
  int lightLevel = analogRead(LDR_PIN);   // 0 = dark, ~1023 = bright

  // --- Simple threshold decision ---
  // (Comment this out and use the hysteresis version below if you get flickering)
  if (lightLevel < THRESHOLD) {
    digitalWrite(LED_PIN, HIGH);   // Dark → LED ON (night light!)
  } else {
    digitalWrite(LED_PIN, LOW);    // Bright → LED OFF
  }

  // --- Hysteresis version (un-comment this, comment out the simple version above) ---
  // Hysteresis uses TWO thresholds to prevent rapid flickering at the boundary.
  // if (lightLevel < THRESHOLD_LOW) {
  //   ledIsOn = true;              // Goes dark → turn ON
  // } else if (lightLevel > THRESHOLD_HIGH) {
  //   ledIsOn = false;             // Goes bright → turn OFF
  // }
  // // (If between the two thresholds, keep whatever state we were in)
  // digitalWrite(LED_PIN, ledIsOn ? HIGH : LOW);

  // --- Print status to Serial Monitor ---
  Serial.print("Light: ");
  Serial.print(lightLevel);
  Serial.print("  | Threshold: ");
  Serial.print(THRESHOLD);
  Serial.print("  | LED: ");
  Serial.println((lightLevel < THRESHOLD) ? "ON " : "OFF");

  // >>> TRY CHANGING THIS: try 50 or 500 and see how responsiveness changes
  delay(100);  // Check light level every 100 ms
}

// ===== CHALLENGE =====
// 1. HYSTERESIS: Uncomment the hysteresis block above. What threshold gap
//    (THRESHOLD_HIGH - THRESHOLD_LOW) removes the flickering? Record it.
//
// 2. FADE TRANSITION: Instead of ON/OFF, use analogWrite() to fade
//    the LED in as it gets darker. Use map(lightLevel, 0, THRESHOLD, 255, 0)
//    for the brightness. (Only works below THRESHOLD.)
//
// 3. ALARM MODE: When it gets dark, also blink the LED rapidly 3 times
//    before leaving it on steadily — like an "alert" notification.
//
// 4. SECOND LED: Wire a second LED (white or blue) on pin 10.
//    Make it turn ON only when the first LED is ON AND the reading
//    is below THRESHOLD/2 (very dark). This gives a two-stage night light.
```

---

## 5. Student Worksheet

---

### Session 12 Worksheet — Automatic Night Light

**Name(s):** _________________________________ **Date:** _____________ **Kit #:** _____

**Objectives:**
- Calibrate your LDR sensor to your actual environment.
- Use an `if` statement threshold to create an automatic control system.
- Connect the concept of thresholds to real-world biology and engineering.

---

#### What I Already Know (Warm-Up)

1. From Session 11: When the LDR is in bright light, does the `analogRead()` value go UP or DOWN?

   ___________________________________________________________________________

2. From Session 6 (Traffic Light): Complete this `if` statement:

   ```cpp
   if (buttonPressed == HIGH) {
     __________________________________________
   }
   ```

3. A thermostat turns on the heater when the temperature drops below 20°C. What is the "threshold" in this system?

   ___________________________________________________________________________

---

#### Build It — Step by Step

**FIRST: Calibration (no code changes needed — use the calibration sketch)**

1. Build or verify the LDR circuit (same as Session 11): LDR + 10 kΩ divider on A0, LED + 220 Ω on pin 9.
2. Upload the **calibration sketch** (uncomment the calibration block in the code).
3. Open Serial Monitor at 9600 baud.
4. **Dark test:** Cover the LDR completely with both hands for 5 seconds. Record readings: _______, _______, _______. Average: _______.
5. **Bright test:** Uncover LDR in normal room light. Record readings: _______, _______, _______. Average: _______.
6. Calculate your threshold: (dark average + bright average) ÷ 2 = _______

**THEN: Night Light Sketch**

7. Comment out the calibration code. Uncomment the main sketch.
8. Change `const int THRESHOLD = 400;` to `const int THRESHOLD = ______;` (your calibrated value).
9. Upload. Open Serial Monitor (9600 baud).
10. Cover the LDR. The LED should turn ON.
11. Uncover the LDR. The LED should turn OFF.
12. If it doesn't work as expected, adjust your threshold value and re-upload.

---

#### Predict!

| Scenario | Your Prediction: LED ON or OFF? | Actual result |
|----------|--------------------------------|---------------|
| Normal room light | | |
| Hand completely covering LDR | | |
| Dimmed room lights | | |
| Flashlight shining directly on LDR | | |
| LDR inside a paper cup (dim) | | |

---

#### Observe / Data Table

Record 5 readings at each condition and whether the LED was ON or OFF:

| Condition | Reading 1 | Reading 2 | Reading 3 | LED state | Above/below threshold? |
|-----------|-----------|-----------|-----------|-----------|------------------------|
| Full room light | | | | | |
| Lamp off (dimmer) | | | | | |
| Hand cover | | | | | |
| Your threshold value | | | | | (right at boundary) |

---

#### What Did You Notice?

1. What threshold value did you calculate? Did it work well, or did you have to adjust it?

   ___________________________________________________________________________

2. Did the LED ever flicker (turn on and off rapidly)? When? Why might this happen?

   ___________________________________________________________________________

3. What would happen if your threshold was set to 1? Would the night light ever turn on?

   ___________________________________________________________________________

4. Street lights use a similar circuit. They turn on around **dusk**. What challenges would a real street light engineer face in choosing a threshold value?

   ___________________________________________________________________________

---

#### Science Connection

A **threshold** appears in many natural systems:
- Plants begin photosynthesis when light intensity reaches a minimum level.
- Your eyes' rods (night vision cells) activate when light drops below a threshold.
- Animals (like fireflies and nocturnal animals) use light level thresholds to control behavior.

**Question:** The light level that triggers a firefly to start glowing is different in summer vs winter (because of different background light levels). How is this similar to the calibration problem you encountered today?

___________________________________________________________________________

The engineering solution in real streetlights is a device called a **photocell relay**. It uses the same LDR + threshold principle you built today, but handles much higher voltages for actual street lights safely.

---

#### Challenge Extension

**Try the hysteresis version** from the code's Challenge section:
1. Uncomment the hysteresis block.
2. Set THRESHOLD_LOW = your threshold − 30, THRESHOLD_HIGH = your threshold + 30.
3. Does the flickering stop? What is the "gap" between the two thresholds?

**Explain in your own words:** Why does using TWO threshold values (one for turning on, one for turning off) reduce flickering?

___________________________________________________________________________

---

## 6. Safety Notes

| Hazard | Precaution |
|--------|------------|
| Short circuit (same circuit as Session 11) | Double-check: LDR between 5V and junction; 10 kΩ between junction and GND. Never connect both resistors to the same rail. |
| Rapid LED switching | Rapid LED flashing at the threshold is harmless but can be annoying. Adjust threshold or use hysteresis to prevent it. |
| Flashlight eye safety | When testing with a phone flashlight, aim it at the LDR only — not at eyes. |

**Build → Check → Power on.**

**If something goes wrong:**
- LED never turns on → threshold too high, or circuit inverted (check LDR orientation in divider).
- LED always on → threshold too low, or LDR isn't connected to 5V.
- Constant flickering → add hysteresis (two-threshold method) or choose a clearer dark/light environment.

---

## 7. Assessment Rubric

### Formative Check (not graded)

| Look-For | Not Yet | Got It |
|----------|---------|--------|
| Student correctly calculates calibration threshold from measured data | | |
| Night light turns on when LDR is covered and off when uncovered | | |
| Student can explain what would happen if threshold is set to 0 or 1023 | | |
| Student connects threshold concept to at least one real-world or biological example | | |
| Student attempts the hysteresis challenge or explains flickering | | |

---

## 8. Differentiation

### Support
- Provide a printed "calibration record sheet" with blanks for dark value, bright value, and calculated threshold — reduces cognitive load during the calibration step.
- If calibration is too complex, provide a pre-set threshold value appropriate for your classroom lighting (typically 300–500) and skip the calculation.
- Offer a sentence frame: *"The LED turns on when the light reading is [below / above] the threshold because ____________."*
- Pre-built circuit (same as Session 11) with the wiring already correct; student's job is the code edit only.

### Extension
- Implement **hysteresis** (two-threshold method from the Challenge section) and document the minimum gap needed to stop flickering.
- Research: *"What is 'noise' in a sensor signal and how do engineers use filtering to reduce it? Look up 'moving average filter' for Arduino."*
- Code challenge: add a 10-second **time delay** after the LED turns on, so it can't turn off again for 10 seconds (prevents rapid cycling). Use `millis()` instead of `delay()` for non-blocking timing.
- Design challenge: *"How would you build a night light that DIMS gradually as the room gets darker, rather than switching abruptly? Write pseudocode."*

### Visual / Kinesthetic Accommodations
- Use a physical analogy: a thermostat is something students can visualize. Draw a thermometer with a red line (threshold). Ask: *"If the temperature bar is below the red line, what does the heater do?"*
- Print a number line 0–1023 and have students physically mark their dark value, bright value, and threshold with colored markers.
- For students with light sensitivity, allow them to use a small cardboard box over the LDR instead of a flashlight for the bright test.
- Annotate the if/else structure with physical arrows on a printed code snippet before students type it: "This is the question → this is the YES answer → this is the NO answer."
