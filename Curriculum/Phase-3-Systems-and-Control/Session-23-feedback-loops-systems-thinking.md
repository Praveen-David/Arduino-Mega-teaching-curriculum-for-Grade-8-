# Week 12, Session 23 — Feedback Loops & Systems Thinking

**Phase:** Phase 3 — Systems & Control
**Week:** 12 | **Session:** 23 of 36

---

## Learning Objectives

By the end of this session, students will be able to:
- Define a **feedback loop** and distinguish between **negative (corrective)** and **positive (amplifying)** feedback.
- Draw a block diagram of a closed-loop control system with labelled components: sensor, comparator, controller, actuator, and plant.
- Connect the concept of a feedback loop to biological homeostasis (body temperature regulation).
- Build and observe a simple closed-loop demo where the Arduino reads a sensor, makes a decision, acts on an output, and re-reads the sensor.

---

**Science Curriculum Link:** Feedback loops, homeostasis, and control systems (Grade 8 Biology — Maintaining a Healthy Body; Grade 8 Systems & Technology).
The human body maintains a core temperature of ~37°C using exactly the same control structure as a thermostat: a sensor (thermoreceptors in skin and hypothalamus), a comparator and controller (the hypothalamus), an actuator (sweat glands, blood vessels, muscles/shivering), and a feedback signal (the new temperature). This session makes that biological concept tangible through electronics.

---

## Materials Checklist (per pair)

- [ ] 1 × Arduino Mega 2560 + USB cable
- [ ] 1 × Solderless breadboard
- [ ] 1 × LDR (photoresistor)
- [ ] 1 × 10 kΩ resistor
- [ ] 1 × LED (any colour, e.g. red)
- [ ] 1 × 220 Ω resistor (for LED)
- [ ] Several M-M jumper wires (red, black, green, others)
- [ ] Computer with Arduino IDE
- [ ] **Worksheets printed** (this session is concept-heavy; the worksheet diagrams are essential)
- [ ] Optional: a small torch/flashlight per pair for testing the feedback demo

> **Teacher note:** This is the most concept-rich session of Phase 3. The build is intentionally simple (LDR + LED already familiar) so that most of the time is spent on understanding the feedback concept and drawing diagrams. Allocate at least 10 minutes for whole-class diagram discussion.

---

## 2. Teacher Guide (45-minute breakdown)

### 0–5 min — Hook / Warm-Up

**Demo (or describe):** "Stand up and try to stand on one foot with your eyes open. Now close your eyes and try again."

*Students will wobble much more with eyes closed.*

**Ask:** "Why is it harder with your eyes closed? What information are you losing? What does your brain do with that visual information?"

*Expected answers:* With eyes open, students see themselves tilting and correct immediately. Eyes closed = no feedback = no correction = fall over.

**Bridge:** "What you just experienced was a **feedback loop**. Your eyes sensed where you were (sensor), your brain compared it to where you should be (comparator), your muscles adjusted (actuator), and then your eyes checked again. This cycle repeats dozens of times per second. Today we are going to build this same idea in electronics — and connect it to how your body stays at 37°C."

---

### 5–15 min — Direct Instruction

**Concept: Feedback loops, systems thinking, homeostasis**

*Words to say:*
"Let me draw two types of systems on the board:

**Open-loop system** (no feedback):
```
INPUT → CONTROLLER → ACTUATOR → OUTPUT
                                  (no feedback — controller is blind)
```
Example: a simple light switch. You flip it, the light turns on. The switch doesn't know if the room is bright enough — it just does what you told it.

**Closed-loop system** (with feedback):
```
SETPOINT ──►  [COMPARATOR] ──► CONTROLLER ──► ACTUATOR ──► PLANT/PROCESS
                   ▲                                            │
                   └─────────────[SENSOR]◄──────────────────────┘
                                  (FEEDBACK PATH)
```
The loop is *closed* because the output is measured and fed back to the comparator, which keeps correcting the error.

The KEY concept is **error = setpoint − actual value**. The controller works to reduce this error toward zero.

**Negative feedback = stabilising:** When the output goes too high, the feedback *reduces* it. Example: your body temperature rises → you sweat → evaporative cooling → temperature falls back to 37°C. The response is *negative* (opposite direction) to the disturbance.

**Positive feedback = amplifying:** When the output goes high, the feedback *increases* it further. Example: a microphone pointing at a speaker — sound → microphone amplifies → speaker louder → microphone louder → runaway SCREECH. Usually undesirable in control, but useful in biology (childbirth contractions, blood clotting, electrical signals in neurons firing).

**Body temperature homeostasis (negative feedback):**
```
Setpoint: 37°C
Sensor: thermoreceptors in skin & hypothalamus
Comparator/Controller: hypothalamus
Error Too High (+): sweating, vasodilation (blood vessels near skin open up)
Error Too Low (−): shivering, vasoconstriction, raised hair
```

Today's demo: a photoresistor (LDR) detects light level. The Arduino compares it to a setpoint. If it's too dark, an LED turns on (adding light). If it's bright enough, the LED turns off. This is a simple closed-loop light controller — a tiny version of automatic street lights."

**Draw the block diagram on the board and have students copy it onto their worksheet:**

```
SETPOINT (target brightness)
     │
     ▼
[COMPARATOR] ──error──► [CONTROLLER (Arduino)] ──► [LED (actuator)]
     ▲                                                     │
     │                                               (light output)
     │                                                     │
[LDR sensor] ◄─────────────────────────────────────────────┘
(measures current brightness)
     ↑ FEEDBACK PATH
```

---

### 15–35 min — Hands-On Build + Code

**Remind students:** Build → Check → Power on.

**Building the circuit:**

1. Insert the LDR into the breadboard (legs span the centre gap).
2. Connect the **top leg** of the LDR → Arduino **5V** (red wire).
3. Connect the **bottom leg** of the LDR → Arduino **A0** (signal wire, green) AND to one end of the **10 kΩ resistor**.
4. Connect the other end of the 10 kΩ resistor → Arduino **GND** (black wire).
   *(This creates the voltage divider: LDR on top, 10 kΩ on bottom. A0 reads the voltage between them.)*
5. Insert the LED into the breadboard (longer leg = anode/+, shorter leg = cathode/−).
6. Connect the **LED anode (long leg)** through a **220 Ω resistor** → Arduino **pin 13** (signal wire, yellow).
7. Connect the **LED cathode (short leg)** → Arduino **GND** (black wire).
8. Partner check. Upload the feedback loop code.

**Test the feedback loop:**
- In normal light: LED should be OFF (room is bright enough).
- Cover the LDR with a hand: the LED should turn ON (it got dark, controller compensates).
- Shine a torch on the LDR: LED stays solidly OFF.

---

### 35–42 min — Testing & Debugging

**What success looks like:** Covering the LDR turns the LED on; uncovering it turns the LED off. The Serial Monitor shows the sensor reading, setpoint, error, and current LED state.

**Troubleshooting table:**

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| LED always ON | LDR voltage divider inverted or setpoint too high | Check LDR on top of divider; lower SETPOINT value |
| LED always OFF | Setpoint too low for room conditions | Read the Serial Monitor — raise SETPOINT to just above room reading |
| LED flickers rapidly | Sensor reading hovering right at the setpoint (hysteresis needed) | Add a hysteresis band: ON below (setpoint-20), OFF above (setpoint+20) |
| No response to covering LDR | LDR and 10 kΩ resistor swapped | Swap — LDR must be on the 5V side |
| Serial Monitor shows nothing | Wrong baud rate | Set Serial Monitor to 9600 |

---

### 42–45 min — Reflection / Exit Ticket

Students complete diagram and write on a sticky note:

1. "In your own words, what is the difference between open-loop and closed-loop control? Give one example of each from everyday life."
2. "Name the four components of a closed-loop control system and say what each one does."
3. "Is the body's temperature regulation positive or negative feedback? How do you know?"

---

## 3. Circuit Diagram

```
  ARDUINO MEGA 2560
  ┌──────────────────────────────────────────────────────┐
  │  5V ──────────────────────────────────────────────────┼──► [red]    ──► LDR top leg
  │                                                       │
  │  A0 (analog in) ──────────────────────────────────────┼──► [green]  ──► LDR bottom leg
  │                                                       │                 AND 10 kΩ top
  │  GND ─────────────────────────────────────────────────┼──► [black]  ──► 10 kΩ bottom
  │                                                       │
  │  pin 13 (digital out) ─────────────────────────────── ┼──► [yellow] ──► 220Ω ──► LED (+)
  │  GND ─────────────────────────────────────────────────┼──► [black]  ──► LED (−)
  └──────────────────────────────────────────────────────┘

  LDR VOLTAGE DIVIDER:
  5V ──[LDR]──┬──[10 kΩ]── GND
              │
              └──► A0  (voltage here varies with light)
              
  When LDR is lit (low resistance): voltage at A0 is HIGH (close to 5V → high analogRead)
  When LDR is dark (high resistance): voltage at A0 is LOW (close to 0V → low analogRead)
```

**Fritzing-style build description:**

| # | From | To | Wire Color | Notes |
|---|------|----|-----------|-------|
| 1 | Arduino **5V** | LDR **top leg** | Red | Top of voltage divider |
| 2 | Arduino **A0** | LDR **bottom leg** / resistor junction | Green | Sense point |
| 3 | LDR **bottom leg** / resistor junction | 10 kΩ resistor top | Green | Same node as A0 |
| 4 | 10 kΩ resistor bottom | Arduino **GND** | Black | Bottom of divider |
| 5 | Arduino **pin 13** | 220 Ω resistor top | Yellow | LED current limit |
| 6 | 220 Ω resistor bottom | LED **anode** (+, long leg) | Yellow | |
| 7 | LED **cathode** (−, short leg) | Arduino **GND** | Black | |

[Google image search: "Arduino LDR LED feedback loop dark sensor automatic light circuit"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 *  Session 23 — Feedback Loops & Systems Thinking
 *  Arduino Mega Teaching Curriculum — Phase 3
 * ============================================================
 *
 *  SCIENCE EXPLANATION:
 *  ─────────────────────────────────────────────────────────
 *  This sketch implements a CLOSED-LOOP NEGATIVE FEEDBACK
 *  control system, the same type used by living organisms for
 *  HOMEOSTASIS.
 *
 *  System map:
 *    SETPOINT:   target light level (a number we choose)
 *    SENSOR:     LDR voltage divider → A0 → analogRead → 0–1023
 *    ERROR:      setpoint − actual
 *    CONTROLLER: if error > 0 (too dark) → turn LED ON
 *                if error < 0 (too bright) → turn LED OFF
 *    ACTUATOR:   LED (adds light to the environment)
 *    FEEDBACK:   the LED's light falls on the LDR, changing its reading
 *
 *  HYSTERESIS:
 *    Without hysteresis, the LED flickers rapidly when the
 *    sensor value is right at the setpoint. Hysteresis adds a
 *    "band of tolerance" — the LED stays ON until the reading
 *    is clearly above the setpoint, and stays OFF until clearly
 *    below. This is how your home thermostat avoids turning the
 *    heater on/off every second — it heats to 21°C, then waits
 *    until the room cools to 19°C before heating again.
 *
 *  NEGATIVE FEEDBACK (stabilising):
 *    Too dark  (+error) → LED turns ON  → brighter → error reduces ✓
 *    Too bright (-error) → LED turns OFF → darker  → error reduces ✓
 *    The response is always opposite in direction to the disturbance.
 * ============================================================
 *
 *  HARDWARE:
 *    LDR + 10 kΩ voltage divider → A0
 *    LED + 220 Ω resistor       → pin 13
 * ============================================================
 */

// --- Pin definitions ---
const int LDR_PIN = A0;        // LDR voltage divider connected to A0
const int LED_PIN = 13;        // LED connected to pin 13

// --- Control parameters ---
// SETPOINT: the target light level we want to maintain.
// Range: 0 (very dark) to 1023 (maximum brightness).
// >>> TRY CHANGING THIS: read the normal room value from Serial Monitor,
// then set the setpoint just below it. E.g., if room reads 700, set to 600.
const int SETPOINT = 500;

// HYSTERESIS: how far above/below the setpoint before we change state.
// This prevents rapid on/off flickering near the setpoint.
// >>> TRY CHANGING THIS: set to 0 to see flickering; set to 100 for sluggish response
const int HYSTERESIS = 30;

// Track current LED state (needed for hysteresis logic)
bool ledIsOn = false;

void setup() {
  Serial.begin(9600);

  // Set LED pin as output
  pinMode(LED_PIN, OUTPUT);

  // Start with LED off
  digitalWrite(LED_PIN, LOW);

  // Print column headers to Serial Monitor
  Serial.println("LightValue | Setpoint | Error | LED State");
  Serial.println("------------------------------------------");
}

void loop() {
  // Step 1: READ the sensor (the "feedback" signal)
  int lightValue = analogRead(LDR_PIN);

  // Step 2: COMPARE — calculate the error
  int error = SETPOINT - lightValue;
  // Positive error = too dark (light value is below setpoint)
  // Negative error = too bright (light value is above setpoint)

  // Step 3: CONTROL — make a decision with hysteresis
  // Turn LED ON if it's too dark AND the LED is currently off
  if (!ledIsOn && lightValue < (SETPOINT - HYSTERESIS)) {
    ledIsOn = true;
    digitalWrite(LED_PIN, HIGH);
  }
  // Turn LED OFF if it's bright enough AND the LED is currently on
  if (ledIsOn && lightValue > (SETPOINT + HYSTERESIS)) {
    ledIsOn = false;
    digitalWrite(LED_PIN, LOW);
  }

  // Step 4: REPORT to Serial Monitor (shows the feedback loop in action)
  Serial.print("Light: ");
  Serial.print(lightValue);
  Serial.print("  |  Setpoint: ");
  Serial.print(SETPOINT);
  Serial.print("  |  Error: ");
  Serial.print(error);
  Serial.print("  |  LED: ");
  Serial.println(ledIsOn ? "ON " : "OFF");

  // Short delay before next measurement cycle
  delay(200);
}


// ===== CHALLENGE =====
// 1. HYSTERESIS EXPERIMENT: Change HYSTERESIS to 0, then to 5, 20, 50.
//    In each case: does the LED flicker? How quickly does it respond?
//    Write down your observations — this is real engineering data!
//
// 2. MULTIPLE LEVELS: Add a second LED on pin 12. Create three zones:
//    - Very dark  (lightValue < 300): BOTH LEDs ON (full compensation)
//    - Medium dark (300–500):         ONE LED ON (partial compensation)
//    - Bright (> 500):                BOTH LEDs OFF
//    This is called BANG-BANG CONTROL with two levels.
//
// 3. ADD LCD: Display the light level, setpoint, error, and LED state
//    on the 16×2 LCD. Update it every 500 ms.
//
// 4. ADJUSTABLE SETPOINT: Connect a potentiometer to A1. Map its value
//    (0–1023) to a setpoint range (100–900). The user can now turn the
//    pot to change the threshold — a real-world user-adjustable setpoint!
```

---

## 5. Student Worksheet

### Session 23 — Feedback Loops & Systems Thinking
**Name(s):** _________________________ **Date:** _________ **Kit #:** ______

**Objectives:** Understand feedback loops, build a simple closed-loop demo, and connect the concept to biological homeostasis.

---

#### What I Already Know (Warm-Up)

1. Try balancing on one foot with your eyes open for 10 seconds. Then try with eyes closed. What is different?
   > ___________________________________________________________________

2. What is "homeostasis"? (Check your science notes or give your best definition.)
   > ___________________________________________________________________

3. When you are cold, your muscles start shivering. When you are hot, your body sweats. What do both of these responses have in common?
   > ___________________________________________________________________

---

#### Build It — Step by Step

- [ ] LDR wired: top leg → 5V, bottom leg → A0, 10 kΩ from A0 to GND
- [ ] LED wired: pin 13 → 220 Ω → LED anode, LED cathode → GND
- [ ] Partner check complete
- [ ] Code uploaded
- [ ] Test: cover LDR → LED turns ON; uncover → LED turns OFF

---

#### Systems Thinking Diagrams

**Task A:** Draw the OPEN-LOOP version of a light switch controlling a street lamp. Use boxes and arrows. Label: INPUT, CONTROLLER, ACTUATOR, OUTPUT.

*(Draw here)*

---

**Task B:** Draw the CLOSED-LOOP version of your LDR+LED circuit. Use boxes and arrows. Label all 5 components: SENSOR, COMPARATOR, CONTROLLER, ACTUATOR, FEEDBACK PATH. Mark the SETPOINT and ACTUAL VALUE.

*(Draw here)*

---

**Task C:** Draw the closed-loop diagram for your body's temperature control system. Use the same format. Label which body part plays each role.

*(Draw here)*

---

#### Predict!

Before uploading the code:

1. If `SETPOINT = 500` and the room light reading is 700, what is the error?
   > Error = _______ − _______ = _______

2. Is this error positive or negative? What does the controller do in response?
   > ___________________________________________________________________

3. What will happen if you set `HYSTERESIS = 0`? What problem do you predict?
   > ___________________________________________________________________

---

#### Observe / Data Table

| Experiment | SETPOINT | HYSTERESIS | Light Condition | LED Behaviour |
|-----------|---------|-----------|----------------|--------------|
| 1 — Baseline | 500 | 30 | Normal room | |
| 2 — Cover LDR | 500 | 30 | LDR covered (dark) | |
| 3 — Shine torch | 500 | 30 | Torch on LDR | |
| 4 — No hysteresis | 500 | 0 | Hand slowly uncovering | |
| 5 — Large hysteresis | 500 | 100 | Gradually change light | |

---

#### What Did You Notice?

1. What happened when you set `HYSTERESIS = 0` and slowly moved your hand near the LDR? Why?
   > ___________________________________________________________________

2. With `HYSTERESIS = 100`, the LED took longer to respond to changes. Is this a good thing or a bad thing? Can you think of a real situation where a slow, stable response is preferable to a fast, flickery one?
   > ___________________________________________________________________

3. When the LED turned on in response to darkness, did it actually make the LDR reading change? (You may need to observe in a darkened space.) What does this tell you about whether the feedback loop is actually "closing"?
   > ___________________________________________________________________

4. Our feedback system only has two states (LED on or off). A real thermostat can have 100 different heating levels. What is the advantage of having more "levels" of response?
   > ___________________________________________________________________

---

#### Science Connection — Homeostasis Comparison

Fill in the table comparing your electronic system to the human body's temperature control:

| Component | Your Electronic System | Human Body (Temperature) |
|-----------|----------------------|--------------------------|
| Setpoint | SETPOINT constant in code (e.g., 500) | ~37°C |
| Sensor | | |
| Comparator | | |
| Controller | | |
| Actuator (too high) | | |
| Actuator (too low) | (LED only compensates one direction) | |
| Feedback path | | |
| Type of feedback | | Negative |

---

#### Challenge Extension

1. **Positive feedback demo:** Describe (you don't need to build it) what would happen if you wired the system so the LED turned ON when light was HIGH (not low). Why would this be a runaway system?

2. **Adjustable setpoint:** Add a potentiometer to A1 and use its value to dynamically set the SETPOINT. Now the user can adjust the threshold by turning the knob.

3. **Three-way response:** Add two LEDs. Write code that turns on 0, 1, or 2 LEDs depending on how dark it is (three levels of response instead of two).

---

## 6. Safety Notes

**This session's components and hazards:**

- **LDR:** Passive component — no hazards. Handle with care (fragile glass bead at the sensing end).
- **LED:** Polarity matters. Long leg (anode) → resistor → Arduino pin. Short leg (cathode) → GND. Always use the 220 Ω current-limiting resistor.
- **Voltage divider:** Both ends of the divider must be connected (top to 5V, bottom to GND) before the middle wire goes to A0. If only one end is connected, the reading will be fixed at 0 or 1023.
- **Torch/flashlight:** If using a phone torch to test, keep the phone away from the breadboard. Accidental pressure on the phone can disturb wires.

**Build → Check → Power on.** Partner checks are especially important this session because the circuit has more wires than previous sessions.

**If something goes wrong:**
- LED stays off and Serial Monitor shows 0 always: check LDR voltage divider wiring (especially the 10 kΩ resistor connection).
- LED always on even in bright light: setpoint too high, or LDR and resistor swapped.

---

## 7. Assessment Rubric

### Formative Check (not graded)

| Look-For | Not Yet | Got It |
|----------|---------|--------|
| Circuit functional: LED turns ON in dark, OFF in light | No response to light change | Clear LED response to LDR light changes |
| Can draw a complete closed-loop block diagram with all 5 components labelled | Diagram incomplete or components mis-labelled | All 5 components (sensor, comparator, controller, actuator, feedback) labelled correctly |
| Can connect feedback loop concept to body temperature regulation | Cannot link electronics to biology | Names the biological equivalent of at least 3 circuit components |

---

## 8. Differentiation

### Support

- **Pre-drawn block diagram:** Provide a printed block diagram with the boxes and arrows drawn; students fill in the component labels only.
- **Concept word bank:** Provide the labels on sticky notes (sensor, comparator, controller, actuator, feedback path, setpoint, error) that students place on the blank diagram.
- **Simpler demo:** Reduce the circuit to just reading the LDR and printing "DARK" or "BRIGHT" on the Serial Monitor — build the software feedback without the LED output first.
- **Concrete analogy script:** "Your eyes are the sensor. Your brain is the comparator and controller. Your muscles are the actuator. Your new position is the feedback." Give this as a fill-in card.

### Extension

- **PID introduction:** Look up "PID controller" online. Explain to the class what P, I, and D stand for and why simple ON/OFF control is less precise than proportional control.
- **Ecosystem application:** Research how a forest ecosystem shows negative feedback (e.g., more prey → more predators → fewer prey → fewer predators). Present as a block diagram.
- **Cascaded loops:** Research "inner loop" and "outer loop" in control systems (e.g., a fighter jet autopilot). What advantage does having multiple nested feedback loops provide?
- **Positive feedback examples:** Research and present 3 biological examples of positive feedback and explain why each eventually stops (stabilising event).

### Visual / Kinesthetic Accommodations

- **Body-game:** One student is the "thermometer" (holds up fingers: 0=cold, 10=normal, 20=hot). Another is the "hypothalamus" (calls out response: shiver, normal, sweat). The class acts as the muscles/sweat glands. Run 3 rounds changing the "temperature" each time.
- **Giant classroom block diagram:** Tape a large block diagram on the floor with labels. Students stand in each box and physically pass a "signal card" around the loop.
- **Glossary card:** feedback loop, negative feedback, positive feedback, homeostasis, setpoint, error, hysteresis, closed-loop, open-loop, actuator, comparator, controller.
