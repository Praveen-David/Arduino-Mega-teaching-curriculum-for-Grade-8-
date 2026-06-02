# Week 13, Session 26 — Smart Automatic Lighting System

**Phase:** Phase 3 — Systems & Control
**Week:** 13 | **Session:** 26 of 36

---

## Learning Objectives

By the end of this session, students will be able to:
- Wire an LDR (light sensor) and a pushbutton together with an LED output on the Arduino Mega.
- Use `&&` (AND) and `||` (OR) logic operators in code to combine two sensor inputs into multi-condition control.
- Explain AND/OR logic using truth tables and connect this to real smart-home systems.
- Design a lighting control rule that combines light level AND button state to produce a nuanced output.

---

**Science Curriculum Link:** Multi-input systems and Boolean logic (Grade 8 Systems & Technology; Grade 8 Mathematics — Logic).
Modern smart-home lighting systems combine multiple inputs: ambient light sensors, motion detectors, timers, and manual overrides. Each input is a logical condition; the system uses AND/OR logic to decide when to act. This mirrors the logic of the human nervous system, which also combines multiple sensory inputs before deciding on a response.

---

## Materials Checklist (per pair)

- [ ] 1 × Arduino Mega 2560 + USB cable
- [ ] 1 × Solderless breadboard
- [ ] 1 × LDR (photoresistor)
- [ ] 1 × 10 kΩ resistor (LDR voltage divider pull-down)
- [ ] 1 × Pushbutton (tactile, 4-pin)
- [ ] 1 × 10 kΩ resistor (button pull-down)
- [ ] 1 × White or yellow LED (main "light")
- [ ] 1 × 220 Ω resistor (LED current limiting)
- [ ] Several M-M jumper wires (red, black, green, yellow, and others)
- [ ] Computer with Arduino IDE

---

## 2. Teacher Guide (45-minute breakdown)

### 0–5 min — Hook / Warm-Up

**Ask (while showing a photo of a smart street light or bathroom light):** "When should an automatic light turn on? Think of as many rules as you can."

*Expected answers:*
- When it gets dark
- When someone walks in the room (motion)
- When someone presses a button (override)
- At a specific time of day

**Write their answers on the board and underline the word "AND" and "OR" in natural rules:**
- "Turn ON if it's dark **AND** someone is present."
- "Turn ON if it's dark **OR** if the manual switch is pressed."

**Bridge:** "Notice those words AND and OR? In programming and in electronics, AND and OR are called **logical operators**. Today we are going to build a two-input smart light that uses both. You will discover that just two inputs — a light sensor and a button — can create a surprisingly smart system."

---

### 5–15 min — Direct Instruction

**Concept: Multi-input systems and Boolean (AND/OR) logic**

*Words to say:*
"Boolean logic is named after mathematician George Boole. It operates on true/false (or 1/0) values.

**AND logic:** Both conditions must be true.
- 'Light turns ON when it is dark AND the button is pressed.'
- You want lighting ONLY in dark occupied rooms.

**OR logic:** At least one condition must be true.
- 'Light turns ON when it is dark OR the button is pressed.'
- Pressing the button always forces the light on, even in daylight.

**Truth table — AND (`&&`):**

| isDark | buttonPressed | Light ON? |
|--------|--------------|-----------|
| False  | False        | No |
| False  | True         | No |
| True   | False        | No |
| True   | True         | **Yes** |

**Truth table — OR (`||`):**

| isDark | buttonPressed | Light ON? |
|--------|--------------|-----------|
| False  | False        | No |
| False  | True         | **Yes** |
| True   | False        | **Yes** |
| True   | True         | **Yes** |

In our code we will implement a more nuanced rule:
- If **dark AND button NOT pressed** → LED ON (dark room, automated)
- If **NOT dark AND button pressed** → LED ON (daylight override)
- Otherwise → LED OFF

This combines AND, NOT, and OR — a real multi-condition logic system.

**LDR review:** LDR + 10 kΩ voltage divider on A0. High light → high voltage → high analogRead value. 'isDark' = `(analogRead(A0) < threshold)`.

**Button review:** Pushbutton with 10 kΩ pull-down on digital pin 4. `digitalRead(4) == HIGH` when pressed.

**Hysteresis for LDR:** To prevent flickering as the light level hovers at the threshold, we use a small hysteresis band (just as in Session 23)."

Draw the truth table on the board and have students copy it into their worksheets.

---

### 15–35 min — Hands-On Build + Code

**Remind students:** Build → Check → Power on.

**Building the circuit (numbered steps):**

1. **LDR voltage divider** on breadboard:
   - Insert LDR — one leg in row A, one leg in row B (across the centre gap).
   - Connect LDR **top leg** → Arduino **5V** (red wire).
   - Connect LDR **bottom leg** → Arduino **A0** (green wire).
   - Connect a **10 kΩ resistor** from the LDR bottom leg → Arduino **GND** (black wire).
2. **Pushbutton** on breadboard:
   - Insert the button so its two operating legs bridge the centre gap.
   - Connect one leg of the button → Arduino **5V** (red wire).
   - Connect the opposite leg → Arduino **pin 4** (blue wire) AND to a **10 kΩ pull-down resistor** → Arduino **GND** (black wire).
3. **LED output**:
   - Connect Arduino **pin 13** (or any digital pin) → **220 Ω resistor** → LED anode (long leg) → yellow wire.
   - Connect LED cathode (short leg) → Arduino **GND** (black wire).
4. Partner check. Upload code.

---

### 35–42 min — Testing & Debugging

**What success looks like:**
- In a dark environment (cover LDR), LED turns ON automatically.
- In normal light, LED is OFF.
- Press the button in normal light → LED turns ON (override).
- Cover LDR AND press button → LED still ON.

**Troubleshooting table:**

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| LED always ON | LDR pull-down missing; analog threshold too high | Check voltage divider; reduce DARK_THRESHOLD in code |
| LED never ON even in dark | LDR pull-down on wrong side; threshold too low | Check 10 kΩ goes from A0 to GND; raise DARK_THRESHOLD |
| Button press doesn't override | Button pull-down missing; button pin wrong | Check 10 kΩ from button output to GND; verify pin 4 |
| Bouncy/flickery button | Button bouncing | Add `delay(50)` debounce or use the `buttonState == HIGH` check |
| LDR threshold wrong for room | Room is brighter/darker than expected | Read Serial Monitor, note LDR value in room light, set DARK_THRESHOLD just below it |

---

### 42–45 min — Reflection / Exit Ticket

Students write on a sticky note:

1. "Write the AND condition in C++ code that checks 'isDark' is true AND 'buttonPressed' is false."
2. "Give one real-world smart lighting scenario that would use AND logic and one that would use OR logic."
3. "How many rows are in a truth table for THREE inputs? Draw the first two rows."

---

## 3. Circuit Diagram

```
  ARDUINO MEGA 2560
  ┌──────────────────────────────────────────────────────────────┐
  │  5V ──────────────────────────────────────────────────────── ┼──► [red]    LDR top leg
  │  A0 (analog in) ──────────────────────────────────────────── ┼──► [green]  LDR bottom leg
  │  GND ─────────────────────────────────────────────────────── ┼──► [black]  10 kΩ (LDR pull-down)
  │                                                              │
  │  5V ──────────────────────────────────────────────────────── ┼──► [red]    Button leg 1
  │  pin 4 (digital in) ──────────────────────────────────────── ┼──► [blue]   Button leg 2
  │  GND ─────────────────────────────────────────────────────── ┼──► [black]  10 kΩ (button pull-down)
  │                                                              │
  │  pin 13 (digital out) ─────────────────────────────────────── ┼──► [yellow] 220 Ω ──► LED(+)
  │  GND ─────────────────────────────────────────────────────── ┼──► [black]  LED(−)
  └──────────────────────────────────────────────────────────────┘

  LDR VOLTAGE DIVIDER:
  5V ──[LDR]──┬──[10 kΩ]── GND
              └──► A0

  BUTTON PULL-DOWN:
  5V ──[BUTTON]──┬──[10 kΩ]── GND
                 └──► pin 4
```

**Fritzing-style build description:**

| # | From | To | Wire Color | Notes |
|---|------|----|-----------|-------|
| 1 | Arduino 5V | LDR top leg | Red | Top of voltage divider |
| 2 | LDR bottom leg / 10 kΩ junction | Arduino A0 | Green | Sense node |
| 3 | 10 kΩ resistor bottom | Arduino GND | Black | LDR pull-down |
| 4 | Arduino 5V | Button leg 1 | Red | Button power |
| 5 | Button leg 2 | Arduino pin 4 | Blue | Button signal |
| 6 | Button leg 2 / 10 kΩ junction | 10 kΩ resistor top | Blue | Same node as pin 4 |
| 7 | 10 kΩ resistor bottom | Arduino GND | Black | Button pull-down |
| 8 | Arduino pin 13 | 220 Ω resistor top | Yellow | LED signal |
| 9 | 220 Ω resistor bottom | LED anode (+, long leg) | Yellow | |
| 10 | LED cathode (−, short leg) | Arduino GND | Black | |

[Google image search: "Arduino LDR pushbutton LED multi-input automatic lighting circuit breadboard"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 *  Session 26 — Smart Automatic Lighting System
 *  Arduino Mega Teaching Curriculum — Phase 3
 * ============================================================
 *
 *  SCIENCE EXPLANATION:
 *  ─────────────────────────────────────────────────────────
 *  This sketch implements a MULTI-INPUT CONTROL SYSTEM using
 *  BOOLEAN (AND/OR) LOGIC — the same logic used in:
 *    - Smart home lighting (dark + motion + timer)
 *    - Traffic light controllers (multiple sensor inputs)
 *    - Computer processors (billions of AND/OR/NOT gates)
 *    - Neuroscience (neurons "fire" only when enough
 *      excitatory inputs overcome inhibitory inputs — AND logic)
 *
 *  BOOLEAN LOGIC REFRESHER:
 *  AND (&&): output is TRUE only when ALL inputs are TRUE
 *  OR  (||): output is TRUE when ANY input is TRUE
 *  NOT (!):  flips TRUE to FALSE and vice versa
 *
 *  LIGHTING RULES:
 *  Rule 1 (Automatic): Light ON if isDark AND NOT buttonOverride
 *                      (automatic dark-room lighting, overrideable)
 *  Rule 2 (Override):  Light ON if buttonOverride
 *                      (manual override regardless of light level)
 *  Combined:           lightOn = isDark || buttonOverride
 *                      (simplifies to: light on if dark OR button)
 *
 *  HYSTERESIS prevents flickering when the ambient light is
 *  hovering right at the threshold. We use two thresholds:
 *  DARK_ON  (lower value) and DARK_OFF (higher value).
 * ============================================================
 *
 *  HARDWARE:
 *    LDR + 10 kΩ pull-down → A0
 *    Pushbutton + 10 kΩ pull-down → pin 4
 *    LED + 220 Ω resistor → pin 13
 * ============================================================
 */

// --- Pin definitions ---
const int LDR_PIN    = A0;    // LDR voltage divider
const int BUTTON_PIN = 4;     // Pushbutton (active HIGH with pull-down)
const int LED_PIN    = 13;    // Output: "smart light" LED

// --- Light threshold with hysteresis ---
// These values depend on your room brightness — read them from
// Serial Monitor and adjust. Lower number = darker condition.
// >>> TRY CHANGING THIS: adjust both values for your room
const int DARK_THRESHOLD_ON  = 400;  // Below this → considered "dark"
const int DARK_THRESHOLD_OFF = 500;  // Above this → considered "bright"
// (Hysteresis band = 400 to 500)

// Track the current darkness state (hysteresis needs memory)
bool isDark = false;

void setup() {
  Serial.begin(9600);

  // Button pin as input (pull-down resistor on breadboard keeps it LOW)
  pinMode(BUTTON_PIN, INPUT);

  // LED pin as output
  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, LOW);   // Start with light off

  Serial.println("Smart Lighting System — Session 26");
  Serial.println("LDR | Button | isDark | lightOn");
  Serial.println("------------------------------------");
}

void loop() {
  // --- READ inputs ---
  int ldrValue = analogRead(LDR_PIN);
  bool buttonPressed = (digitalRead(BUTTON_PIN) == HIGH);

  // --- UPDATE darkness state with hysteresis ---
  // Only change isDark when clearly below ON threshold or above OFF threshold
  if (ldrValue < DARK_THRESHOLD_ON) {
    isDark = true;
  } else if (ldrValue > DARK_THRESHOLD_OFF) {
    isDark = false;
  }
  // Between thresholds: isDark keeps its previous state (hysteresis)

  // --- APPLY LOGIC RULES ---
  // >>> TRY CHANGING THIS: experiment with different logic combinations
  
  // RULE: Light ON if it is dark, OR if the button is pressed
  // (button = manual override — always forces light on)
  bool lightOn = isDark || buttonPressed;

  // ALTERNATIVE RULE (comment out above and try this):
  // Light ON ONLY if dark AND button is pressed (both conditions needed)
  // bool lightOn = isDark && buttonPressed;

  // --- CONTROL output ---
  digitalWrite(LED_PIN, lightOn ? HIGH : LOW);

  // --- SERIAL MONITOR output (for debugging and data collection) ---
  Serial.print("LDR: ");
  Serial.print(ldrValue);
  Serial.print("  |  Button: ");
  Serial.print(buttonPressed ? "PRESSED" : "open   ");
  Serial.print("  |  Dark: ");
  Serial.print(isDark ? "YES" : "NO ");
  Serial.print("  |  LED: ");
  Serial.println(lightOn ? "ON " : "OFF");

  // Short delay
  // >>> TRY CHANGING THIS: reduce to 50 for faster response
  delay(200);
}


// ===== CHALLENGE =====
// 1. THREE-STATE OUTPUT: Add a second LED (pin 12).
//    - Dark + no button: BOTH LEDs on (full automatic lighting)
//    - Bright + button: ONE LED on (manual dim light)
//    - Dark + button: ONE LED on (manual override in dark)
//    - Bright + no button: BOTH LEDs off
//
// 2. ADD TIMER: Use millis() to add a "timer" feature — the button
//    turns the light on for 30 seconds then automatically turns it off.
//    (Like a bathroom fan timer!)
//    unsigned long onTime = 0;
//    if (buttonPressed) onTime = millis();
//    if (millis() - onTime < 30000) timerActive = true;
//
// 3. PIR SIMULATION: Connect a second button on pin 5 to simulate
//    a PIR motion sensor. Rule: light ON if (dark AND motion) OR
//    (manual button). Now you have a three-input system!
//
// 4. LCD STATUS DISPLAY: Add the I2C LCD.
//    Row 0: show ldrValue and "DARK"/"BRIGHT"
//    Row 1: show "LED: ON " or "LED: OFF" with button state
//
// 5. TRUTH TABLE EXPLORER: Write code that cycles through all 4
//    combinations of isDark/buttonPressed by toggling the variables,
//    and prints the full truth table to Serial Monitor with results.
```

---

## 5. Student Worksheet

### Session 26 — Smart Automatic Lighting System
**Name(s):** _________________________ **Date:** _________ **Kit #:** ______

**Objective:** Build a two-input smart lighting system using AND/OR logic, and connect Boolean logic to real-world multi-input systems.

---

#### What I Already Know (Warm-Up)

1. What does `&&` mean in C++? Give an example of when you would use it.
   > ___________________________________________________________________

2. What does `||` mean in C++? Give an example.
   > ___________________________________________________________________

3. A smart street light turns on when it gets dark AND when a car drives past. If it's dark but no car passes, should the light be on? If a car passes but it's daytime, should it be on?
   > ___________________________________________________________________

---

#### Build It — Step by Step

- [ ] LDR voltage divider: 5V → LDR top, LDR bottom → A0 + 10 kΩ → GND
- [ ] Button pull-down: 5V → button leg 1, button leg 2 → pin 4 + 10 kΩ → GND
- [ ] LED: pin 13 → 220 Ω → LED(+), LED(−) → GND
- [ ] Partner check complete
- [ ] Code uploaded
- [ ] Test: cover LDR (LED on), then uncover (LED off), then press button (LED on in any light)

---

#### Predict!

Fill in the truth tables BEFORE testing:

**Predict: `isDark || buttonPressed` (OR rule)**

| isDark | buttonPressed | Light ON? (prediction) | Light ON? (observed) |
|--------|--------------|----------------------|---------------------|
| false | false | | |
| false | true | | |
| true | false | | |
| true | true | | |

**Predict: `isDark && buttonPressed` (AND rule)**

| isDark | buttonPressed | Light ON? (prediction) | Light ON? (observed) |
|--------|--------------|----------------------|---------------------|
| false | false | | |
| false | true | | |
| true | false | | |
| true | true | | |

---

#### Observe / Data Table

Record your observations for both logic rules:

| Test Condition | LDR Reading | Button State | OR rule: LED | AND rule: LED |
|---------------|------------|-------------|-------------|--------------|
| Normal room light, no button | | Released | | |
| Cover LDR (dark), no button | | Released | | |
| Normal room light, button held | | Pressed | | |
| Cover LDR (dark), button held | | Pressed | | |

---

#### What Did You Notice?

1. For the OR rule: which single condition was most "powerful" — dark alone, button alone, or both needed?
   > ___________________________________________________________________

2. For the AND rule: how many conditions were needed to turn the LED on? Does this seem more or less "permissive" than the OR rule?
   > ___________________________________________________________________

3. Think about a motion-activated light in a hallway. Which logic rule (AND or OR) would be most appropriate? Why?
   > ___________________________________________________________________

4. What happened to the LED when the LDR reading was right at the threshold? Did the hysteresis (two thresholds: DARK_THRESHOLD_ON and DARK_THRESHOLD_OFF) help? Explain.
   > ___________________________________________________________________

---

#### Science Connection

1. In your nervous system, a neuron "fires" (sends a signal) only when the sum of all incoming signals passes a threshold. Is this more like AND logic, OR logic, or something in between? Explain.
   > ___________________________________________________________________

2. Research "logic gate" online. Draw the symbol for an AND gate and an OR gate. Label the inputs and output.

   *(Draw here)*

3. A home security alarm triggers when: (door is open OR window is broken) AND (alarm is armed). Write this as a single Boolean expression using `||` and `&&`.
   > lightOn = ( _____________ || _____________ ) && _____________

4. How many different output combinations are possible for a system with 3 binary inputs (true/false)? Show your working. (Hint: 2² = 4 for 2 inputs...)
   > ___________________________________________________________________

---

#### Challenge Extension

1. **Three-input system:** Add a second button on pin 5 to simulate a motion detector. Program: light ON if `(isDark && motionDetected) || manualOverride`.

2. **Timer override:** Press the button to turn on the light for 30 seconds, then auto-off, even in daylight (use `millis()`).

3. **LCD display:** Add the LCD to show the ldrValue and logic state on screen.

---

## 6. Safety Notes

**This session's components and hazards:**

- **LDR:** Fragile glass bead — handle carefully. No electrical hazards with correct wiring.
- **Pushbutton:** 4-pin tactile buttons can be inserted in two orientations. The two pins on the same side are internally connected. If the button doesn't work, try rotating it 90°.
- **Pull-down resistors:** Both the LDR and button require pull-down resistors to GND. Without them, floating pin readings will cause erratic behaviour. Always use 10 kΩ for both.
- **LED current limiting:** Confirm the 220 Ω resistor is in place before uploading. Check that the LED long leg (anode) connects to the resistor, not directly to the pin.

**Build → Check → Power on.** This circuit has two sensors and one output — verify each section separately before powering on.

**If something goes wrong:**
- LED continuously on or off regardless of inputs → open Serial Monitor, read actual LDR values, adjust thresholds.
- Button always reads as pressed → check pull-down resistor connection to GND.

---

## 7. Assessment Rubric

### Formative Check (not graded)

| Look-For | Not Yet | Got It |
|----------|---------|--------|
| Circuit functional: both inputs read correctly on Serial Monitor | One or both inputs give fixed/wrong values | LDR reads varying values; button reads HIGH when pressed and LOW when released |
| Both OR and AND rules tested and truth table filled in correctly | Only one logic rule tested, or truth table predictions all wrong | Both logic rules tested; at least 3 of 4 truth table rows correctly filled in |
| Can explain why the same hardware gives different behaviour with different logic operators | "It's just code" | Explains that AND requires more conditions to be true than OR, and connects this to a real-world example |

---

## 8. Differentiation

### Support

- **Pre-drawn truth tables:** Provide a printed truth table with the input columns filled in — student only fills in the output column from observation.
- **Step-by-step logic test card:** "Test 1: Cover LDR, don't press button → what happens? Test 2: Uncover LDR, press button → what happens?" — a guided test sequence.
- **Simplified rule only:** Start with OR rule only (simpler — any one input triggers output). Add AND as extension once OR is understood.
- **Sentence starters:** "The AND rule is different from the OR rule because ____. I would use AND when ____ and OR when ____."

### Extension

- **Full truth table analysis:** With 3 inputs (LDR, button1, button2) there are 8 rows. Have the student enumerate all 8 in a table and define a rule that produces their desired output for each combination.
- **NAND and NOR:** Look up NAND and NOR gates. What is the output of `!(isDark && buttonPressed)` (NAND)? How does NAND relate to AND?
- **Microcontroller logic gates:** Research how physical logic gates (74HC04, 74HC08, 74HC32 chips) implement NOT, AND, and OR in hardware. How does the Arduino achieve the same result in software?
- **Smart home research project:** Research the "rules engine" in a smart home system (e.g., Home Assistant, Apple HomeKit). How do professional smart home systems define multi-condition rules? Present a diagram of one rule from a real smart home product.

### Visual / Kinesthetic Accommodations

- **Human logic gate game:** Three students stand up. Student A = LDR (raises hand if covering LDR). Student B = button (raises hand if pressed). Student C = AND gate (holds LED prop aloft only when BOTH A and B raise hands). Then switch to OR gate.
- **Truth table physical demonstration:** Make a 4-row truth table on the floor using tape. Students stand in each cell and hold "LED on" / "LED off" cards.
- **Light-level colour indicator:** Run the circuit in a darkened part of the room and a bright part. The physical difference in LED response is more visible than reading numbers on a screen.
- **Glossary card:** Boolean logic, AND, OR, NOT, truth table, multi-input system, pull-down resistor, threshold, hysteresis, logic gate.
