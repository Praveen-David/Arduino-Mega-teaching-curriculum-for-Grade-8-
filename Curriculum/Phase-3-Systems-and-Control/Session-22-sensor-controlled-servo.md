# Week 11, Session 22 — Sensor-Controlled Servo

**Phase:** Phase 3 — Systems & Control
**Week:** 11 | **Session:** 22 of 36

---

## Learning Objectives

By the end of this session, students will be able to:
- Wire a potentiometer (or LDR) and SG90 servo together on the Arduino Mega.
- Use the `map()` function to scale a sensor range (0–1023) to a servo angle range (0–180).
- Explain the concept of **input→output mapping** as an energy conversion chain.
- Identify this circuit as an open-loop control system and explain what is missing compared to a closed-loop system.

---

**Science Curriculum Link:** Energy conversion and input → output mapping (Grade 8 Physics — Energy Transformations; Systems & Technology).
When a sensor controls an actuator through a mapping function, the system is performing an **energy cascade**: light or mechanical energy (turning the pot) → electrical signal (voltage) → digital number → mathematical mapping → PWM signal → mechanical rotation. This is the foundation of all analogue control systems, from guitar volume knobs to car steering systems.

---

## Materials Checklist (per pair)

- [ ] 1 × Arduino Mega 2560 + USB cable
- [ ] 1 × Solderless breadboard
- [ ] 1 × SG90 servo motor (from Session 21)
- [ ] **Option A — Potentiometer control:**
  - 1 × 10 kΩ potentiometer
  - 3 × M-M jumper wires for pot (red, black, signal)
- [ ] **Option B — LDR control:**
  - 1 × LDR (photoresistor)
  - 1 × 10 kΩ resistor (for voltage divider)
  - Breadboard rows for voltage divider
  - 3 × M-M jumper wires
- [ ] 3 × M-F jumper wires for servo (red, black, signal)
- [ ] Computer with Arduino IDE

> **Teacher note:** Potentiometer is recommended for first-timers — the response is immediate and very clear. LDR adds a science dimension (light → angle). Both use identical code with only the pin number changing.

---

## 2. Teacher Guide (45-minute breakdown)

### 0–5 min — Hook / Warm-Up

**Hold up the servo from Session 21. Ask:** "In Session 21 we told the servo exactly what to do with a for-loop — it had no choice. Today, YOU become the input. If I turn this knob, what do you think should happen to the servo?"

*Turn the potentiometer slowly while the demo servo (if available) is hooked up.*

"The servo follows your hand — it becomes your mechanical arm. This is how a remote-controlled plane works: your joystick sends a number, and the plane's servo moves a flap by exactly that many degrees."

**Ask:** "How does the Arduino know how far you've turned the pot? It reads a number between 0 and 1023 from analogRead(). But the servo only understands 0 to 180. How do we convert 0–1023 into 0–180?"

---

### 5–15 min — Direct Instruction

**Concept: The `map()` function as a mathematical scaling tool**

*Words to say:*
"When you turn a potentiometer from one end to the other, `analogRead()` gives us a number between 0 (0V) and 1023 (5V). The servo understands angles from 0° to 180°. These are different ranges — we need to *rescale* the pot number to fit the servo's range.

The Arduino has a built-in function called `map()` that does this rescaling for us:

```
map(value, fromLow, fromHigh, toLow, toHigh)
```

So: `map(potValue, 0, 1023, 0, 180)` 

Mathematically this is: `angle = (potValue / 1023.0) × 180`

But `map()` is cleaner to read and handles the maths for us.

This is called **proportional mapping** — the output is always proportional to the input. If the pot is 50% of the way around, the servo goes to 50% of its range (90°).

**Science connection:** This same mapping happens in your body. Your eye sends a signal about light intensity; your brain maps that to how wide to open your pupil. A stronger signal → larger output. That's proportional control."

Draw on the board:
```
potValue (0–1023)  →  map()  →  angle (0–180)  →  myServo.write()  →  rotation
     [INPUT]           [PROCESS]                                         [OUTPUT]
```

**LDR version:** The LDR + 10 kΩ voltage divider works identically to the pot — it gives 0–1023. Darker = higher resistance = lower voltage at A0 = smaller angle. Light controls the servo position.

---

### 15–35 min — Hands-On Build + Code

**Remind students:** Build → Check → Power on. Keep fingers clear of servo horn.

#### Option A — Potentiometer Control

**Wiring steps:**

1. Insert the potentiometer into the breadboard (3 pins span across the centre gap).
2. Connect potentiometer **left pin** → Arduino **GND** (black wire).
3. Connect potentiometer **right pin** → Arduino **5V** (red wire).
4. Connect potentiometer **middle pin (wiper)** → Arduino **A0** (signal wire, e.g. green).
5. Connect servo **GND** (brown/black) → Arduino **GND** (black M-F wire).
6. Connect servo **VCC** (red) → Arduino **5V** (red M-F wire).
7. Connect servo **Signal** (orange/yellow) → Arduino **pin 9** (~PWM, signal M-F wire).
8. Partner check. Clear fingers from servo horn. Upload code.

#### Option B — LDR Control

**Wiring steps:**

1. Insert LDR into breadboard columns a/b (one leg each side of the gap).
2. Connect **top leg** of LDR → breadboard row X.
3. Connect row X → Arduino **5V** (red wire).
4. Connect **bottom leg** of LDR → breadboard row Y.
5. Connect row Y → Arduino **A0** (signal wire, green).
6. Connect a **10 kΩ resistor** from row Y → Arduino **GND** (this forms the voltage divider).
7. Wire servo as in Option A (steps 5–7 above).
8. Partner check. Upload the same code (change A0 in the code if needed — it's the same pin).

---

### 35–42 min — Testing & Debugging

**What success looks like (Option A):** Turning the pot from one end to the other sweeps the servo smoothly from 0° to 180°. The servo position tracks the pot position in real time.

**What success looks like (Option B):** Covering the LDR (dark) moves the servo toward one end; shining a light on it moves the servo toward the other.

**Troubleshooting table:**

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| Servo doesn't move | Signal wire not on PWM pin / servo not attached in code | Check `myServo.attach(9)` and pin 9 wire |
| Servo only goes to partial range | pot wired with power/GND swapped | Swap the 5V and GND pot wires |
| Servo jitters constantly | `map()` output fluctuating due to noise | Add `constrain()` call; see code comments |
| pot value reads 0 always | Signal wire at wrong pin | Confirm middle pot pin → A0 |
| LDR reads same value in light and dark | Voltage divider not wired | Check 10 kΩ resistor from signal node to GND |
| Servo moves wrong direction | pot orientation reversed | Swap `0` and `180` in `map()` call |

---

### 42–45 min — Reflection / Exit Ticket

Students write on a sticky note:

1. "Write the `map()` function call that converts a 0–1023 pot reading to a 0–180 angle."
2. "This system is called 'open-loop control.' What does that mean? What would make it 'closed-loop'?"
3. "If you wanted the servo to only move between 45° and 135° (not the full range), how would you change the `map()` call?"

---

## 3. Circuit Diagram

### Option A — Potentiometer + Servo

```
  ARDUINO MEGA 2560
  ┌──────────────────────────────────────────────┐
  │  A0 (analog input) ──────────────────────────┼──► [green]  ──► Pot WIPER (middle pin)
  │  5V ─────────────────────────────────────────┼──► [red]    ──► Pot RIGHT pin
  │  GND ────────────────────────────────────────┼──► [black]  ──► Pot LEFT pin
  │                                              │
  │  pin 9 (~PWM) ───────────────────────────────┼──► [orange] ──► Servo SIGNAL
  │  5V ─────────────────────────────────────────┼──► [red]    ──► Servo VCC
  │  GND ────────────────────────────────────────┼──► [black]  ──► Servo GND
  └──────────────────────────────────────────────┘

  POTENTIOMETER (10 kΩ)
  ┌────────────────────────────────────────┐
  │  LEFT  ──►  GND                        │
  │  WIPER ──►  A0  (0–5V, varies with turn│
  │  RIGHT ──►  5V                         │
  └────────────────────────────────────────┘
```

### Option B — LDR Voltage Divider + Servo

```
  5V ──────────────────┬──────────────────────► [red]
                       │
                    [LDR]  (resistance varies with light)
                       │
                       ├──────────────────────► A0  [green]
                       │
                    [10 kΩ]  (fixed resistor)
                       │
  GND ─────────────────┴──────────────────────► [black]

  Servo wiring identical to Option A
```

**Fritzing-style build description (Option A):**

| # | From | To | Wire Color | Notes |
|---|------|----|-----------|-------|
| 1 | Arduino **GND** | Pot **left pin** | Black | Voltage divider low end |
| 2 | Arduino **5V** | Pot **right pin** | Red | Voltage divider high end |
| 3 | Arduino **A0** | Pot **middle/wiper pin** | Green | Reads 0–5V |
| 4 | Arduino **GND** | Servo **GND** (brown) | Black | M-F jumper |
| 5 | Arduino **5V** | Servo **VCC** (red) | Red | M-F jumper |
| 6 | Arduino **pin 9** | Servo **Signal** (orange) | Orange | M-F jumper; ~ PWM pin |

[Google image search: "Arduino potentiometer servo control analogRead map function wiring"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 *  Session 22 — Sensor-Controlled Servo
 *  Arduino Mega Teaching Curriculum — Phase 3
 * ============================================================
 *
 *  SCIENCE EXPLANATION:
 *  ─────────────────────────────────────────────────────────
 *  This sketch demonstrates PROPORTIONAL OPEN-LOOP CONTROL:
 *
 *  1. The potentiometer (or LDR) creates a voltage divider.
 *     Turning the pot changes the wiper voltage from 0V to 5V.
 *  2. analogRead() converts this voltage to 0–1023 (10-bit ADC).
 *  3. map() rescales 0–1023 to 0–180.
 *  4. myServo.write() sets the servo angle.
 *
 *  This is an OPEN-LOOP system: the Arduino sends a command
 *  but does NOT check if the servo actually reached the angle.
 *  (Inside the servo is a closed loop — but the Arduino doesn't
 *  see that feedback.) Compare to a CLOSED-LOOP system where
 *  the Arduino would measure the result and correct errors.
 *
 *  The map() function uses LINEAR INTERPOLATION:
 *    output = (value - fromLow) * (toHigh - toLow)
 *                              / (fromHigh - fromLow)
 *             + toLow
 *
 *  Energy cascade in this system:
 *    Hand movement → pot voltage change → digital number →
 *    map() calculation → PWM pulse → servo rotation
 * ============================================================
 *
 *  HARDWARE (Option A — Potentiometer):
 *    Pot left   → GND
 *    Pot right  → 5V
 *    Pot wiper  → A0
 *    Servo GND  → GND
 *    Servo VCC  → 5V
 *    Servo Sig  → pin 9 (~PWM)
 *
 *  HARDWARE (Option B — LDR):
 *    Same as A but replace pot with LDR + 10kΩ voltage divider
 * ============================================================
 */

#include <Servo.h>            // Built-in Servo library

// Create a Servo object
Servo myServo;

// Pin for the sensor (pot wiper or LDR voltage divider output)
// >>> TRY CHANGING THIS: any analog pin A0–A15 works
const int SENSOR_PIN = A0;

// Pin for the servo signal
// >>> TRY CHANGING THIS: any ~ PWM pin (2–13, 44–46)
const int SERVO_PIN = 9;

// Smoothing: remember last angle to reduce jitter
int lastAngle = 0;

void setup() {
  Serial.begin(9600);
  Serial.println("Sensor-Controlled Servo — Session 22");

  // Attach servo to pin 9
  myServo.attach(SERVO_PIN);

  // Move servo to centre at startup
  myServo.write(90);
  delay(500);
}

void loop() {
  // Step 1: Read the sensor (0–1023)
  int sensorValue = analogRead(SENSOR_PIN);

  // Step 2: Map sensor range to servo angle range
  // map(value, fromLow, fromHigh, toLow, toHigh)
  // >>> TRY CHANGING THIS: change 0,180 to 45,135 for limited range
  int angle = map(sensorValue, 0, 1023, 0, 180);

  // Step 3: Constrain angle to safe servo limits
  // This prevents sending values outside 0–180 due to noise
  angle = constrain(angle, 0, 180);

  // Step 4: Only update servo if angle has changed significantly
  // This reduces jitter from tiny ADC noise variations
  // >>> TRY CHANGING THIS: reduce 2 to 1 (more responsive) or increase to 5 (less jitter)
  if (abs(angle - lastAngle) > 2) {
    myServo.write(angle);     // Command the servo to the new angle
    lastAngle = angle;        // Remember this angle
  }

  // Step 5: Print values to Serial Monitor for debugging
  Serial.print("Sensor: ");
  Serial.print(sensorValue);
  Serial.print("  →  Angle: ");
  Serial.print(angle);
  Serial.println(" degrees");

  // Short delay before next reading
  // >>> TRY CHANGING THIS: reduce to 20 for faster response
  delay(50);
}


// ===== CHALLENGE =====
// 1. REVERSED DIRECTION: Make the servo go to 180° when the pot
//    is turned fully left, and 0° when fully right. Hint: swap
//    the 0 and 180 values in map():
//    map(sensorValue, 0, 1023, 180, 0)
//
// 2. LIMITED ZONE: Change the map() to only use the middle half
//    of the servo range: map(sensorValue, 0, 1023, 45, 135)
//    This keeps the servo away from its mechanical end-stops.
//
// 3. DISPLAY ON LCD: Add the LCD from Sessions 19-20. Show the
//    sensor value on row 0 and the servo angle on row 1.
//    Update the LCD every 200 ms to avoid flickering.
//
// 4. TWO SENSORS, ONE SERVO: Read both a pot (A0) and an LDR (A1).
//    Average the two values: int combined = (potVal + ldrVal) / 2;
//    Then map combined to angle. Does blending inputs smooth the motion?
//
// 5. DEAD ZONE: Add a "dead zone" around 90° (centre). If the sensor
//    reads between 480 and 540, keep the servo at 90° exactly.
//    This prevents hunting around the centre position.
```

---

## 5. Student Worksheet

### Session 22 — Sensor-Controlled Servo
**Name(s):** _________________________ **Date:** _________ **Kit #:** ______
**Option chosen:** ☐ A (Potentiometer)   ☐ B (LDR)

**Objective:** Use a sensor to control servo position in real time using the `map()` function.

---

#### What I Already Know (Warm-Up)

1. In Session 10 you learned that a potentiometer is a variable resistor. When you turn it fully left, what voltage does the wiper output? When fully right?
   > Fully left: _______ V     Fully right: _______ V

2. What range of numbers does `analogRead()` return? What physical quantity does the maximum value correspond to?
   > Range: _______ to _______     Maximum value = _______ V

3. Fill in the blanks: the servo understands angles from _______ to _______ degrees.

---

#### Build It — Step by Step

- [ ] Servo wired: GND → GND, VCC → 5V, Signal → pin 9
- [ ] **Option A:** Pot wired: Left → GND, Right → 5V, Wiper → A0
- [ ] **Option B:** LDR + 10 kΩ voltage divider wired: LDR top → 5V, junction → A0, 10 kΩ → GND
- [ ] Partner check complete
- [ ] Code uploaded
- [ ] Servo moves when sensor changes

---

#### Predict!

Before uploading:

1. If `sensorValue = 512` (middle of range), what will the mapped angle be? Show your working.
   > angle = map(512, 0, 1023, 0, 180) = _______ degrees

2. If you change `map(sensorValue, 0, 1023, 0, 180)` to `map(sensorValue, 0, 1023, 45, 135)`, what will be the minimum and maximum angles the servo reaches?
   > Min: _______ °     Max: _______ °

3. (Option B) If you cover the LDR completely (maximum resistance), will the servo move toward 0° or 180°? Explain your reasoning.
   > ___________________________________________________________________

---

#### Observe / Data Table

##### Option A — Potentiometer Position vs Servo Angle

| Pot Position | Sensor Value (Serial Monitor) | Servo Angle (observed) | Expected Angle |
|-------------|------------------------------|----------------------|---------------|
| Fully left | | | ~0° |
| 1/4 turn | | | ~45° |
| Half turn (centre) | | | ~90° |
| 3/4 turn | | | ~135° |
| Fully right | | | ~180° |

##### Option B — Light Level vs Servo Angle

| Lighting Condition | Sensor Value | Servo Angle |
|-------------------|-------------|------------|
| Very bright (torch/phone light) | | |
| Normal room light | | |
| Hand partially covering LDR | | |
| LDR fully covered (dark) | | |

---

#### What Did You Notice?

1. Was the mapping between sensor and servo angle perfectly proportional? Describe any non-linearity or "jumping" you noticed.
   > ___________________________________________________________________

2. What happened when you changed the threshold `if (abs(angle - lastAngle) > 2)` to `> 0`? Did the servo become more stable or less stable?
   > ___________________________________________________________________

3. At the extreme ends (0° and 180°), did the servo make any unusual sounds? What does this tell you about mechanical limits?
   > ___________________________________________________________________

4. (Option B) Did the LDR servo respond differently to a slow vs fast change in light? Why might a very fast change (like switching a light on/off) cause a sudden servo jerk?
   > ___________________________________________________________________

---

#### Science Connection

1. This is an **open-loop** control system. The Arduino sends a command to the servo but never checks if it actually got there. Describe a real-world situation where open-loop control fails (where checking the result would be important).
   > ___________________________________________________________________

2. The `map()` function performs linear scaling. In mathematics, what type of function produces a straight line when graphed? Sketch what a graph of "Angle vs Sensor Value" would look like.

   ```
   Angle (°)
   180 |                              /
   135 |                         /
    90 |                    /
    45 |               /
     0 |_______/_______________
       0    256   512   768  1023  → Sensor Value
   ```
   Is your observed data close to this ideal line? Describe any differences.
   > ___________________________________________________________________

3. How is this potentiometer-servo system similar to turning a car's steering wheel? Which part is the "potentiometer" and which is the "servo" in a car's power steering system?
   > ___________________________________________________________________

---

#### Challenge Extension

1. **Reverse the direction:** Modify `map()` so that turning the pot clockwise moves the servo counter-clockwise.

2. **Add an LCD:** Display both the sensor value and the servo angle on the 16×2 LCD in real time.

3. **Dead zone:** Add code so the servo stays at exactly 90° whenever the sensor reads between 480 and 540. This prevents jitter at centre.

---

## 6. Safety Notes

**This session's components and hazards:**

- **Servo horn:** The servo moves in response to ANY change in sensor value, including when you are still wiring or adjusting. Before touching any wires with the circuit powered, **set the pot to centre (half-way) first** to minimize unexpected servo movement.
- **Potentiometer:** If the pot is wired with 5V and GND swapped, the reading will be reversed but no damage occurs. If the wiper wire is accidentally shorted to 5V or GND while the circuit is live, the Arduino's analog input may be briefly over-driven — power down and correct the wiring.
- **LDR + resistor:** No hazards beyond standard wire-polarity checks. Ensure the 10 kΩ resistor goes to GND (not 5V) or readings will be backwards.
- **Current limits:** Servo + potentiometer + Mega = well within USB power limits (500 mA). No external supply needed for a single servo.

**Build → Check → Power on:** Complete ALL wiring before plugging in USB. Do a partner check.

**If something goes wrong:**
- Servo moves wildly / unexpectedly → power down, check sensor wiring and the `constrain()` call.
- Any component gets hot → unplug USB, call teacher.

---

## 7. Assessment Rubric

### Formative Check (not graded)

| Look-For | Not Yet | Got It |
|----------|---------|--------|
| Sensor correctly wired as voltage divider (A0 reads varying values) | Sensor reads only 0 or 1023 (stuck at one end) | Serial Monitor shows full range 0–1023 as sensor changes |
| `map()` function used correctly with appropriate parameters | `map()` missing, wrong parameters, or servo doesn't track sensor | Servo smoothly tracks sensor; angle proportional to input |
| Can explain "open-loop" vs "closed-loop" and identify this system | Cannot distinguish between open and closed loop | Correctly identifies this as open-loop and explains the missing feedback |

---

## 8. Differentiation

### Support

- **Pot-only version:** Use only Option A (potentiometer). The direct tactile feedback makes the mapping very easy to feel and understand.
- **Code scaffold:** Provide code with the `map()` call blank: `int angle = map(sensorValue, ___, ___, ___, ___);` — student fills in the four numbers.
- **Printed map() reference card:** A card showing the formula and a labelled example: "map(512, 0, 1023, 0, 180) = 90".
- **Sentence starters for reflection:** "When the sensor value increases, the servo angle ____. This is because map() ____."

### Extension

- **Two-axis control:** Add a second pot on A1 and a second servo on pin 10. Create a 2-axis "joystick" that controls both axes independently — a simple robot arm wrist/elbow.
- **Non-linear mapping:** Research how to use `pow()` or a lookup table to create exponential or logarithmic mapping (small input → tiny output; large input → big output). This is how game controller triggers work.
- **Physical build:** Construct a cardboard "gauge" or pointer that mounts on the servo horn and reads against a protractor scale printed on paper.
- **Jitter analysis:** Record 50 sensor readings when the pot is held still. Calculate the standard deviation. How much angle jitter does this cause? Can the dead-zone fix it?

### Visual / Kinesthetic Accommodations

- **Graph the mapping:** Have students plot 5 data points from the table on graph paper (Sensor Value vs Servo Angle). Seeing the straight line reinforces the linear mapping concept visually.
- **Kinesthetic analogy:** Students stand in a line; one student (the "sensor") holds up 0–10 fingers; another student (the "servo") raises their arms proportionally. Change the input → see the output change. This makes proportional control physical.
- **Color-coded wiring guide:** Red dashed arrows = power, black = GND, green = sensor signal, orange = servo signal on a printed diagram.
- **Glossary card:** map(), constrain(), proportional, open-loop, closed-loop, linear, wiper, voltage divider.
