# Week 11, Session 21 — Servo Motors: Mechanical Motion

**Phase:** Phase 3 — Systems & Control
**Week:** 11 | **Session:** 21 of 36

---

## Learning Objectives

By the end of this session, students will be able to:
- Wire an SG90 servo motor safely to the Arduino Mega on a PWM-capable pin.
- Use the `Servo.h` library to make the servo sweep from 0° to 180° and back.
- Explain how electrical energy is converted to mechanical motion (rotation), and define torque and angular position.
- Describe the difference between a servo motor and a regular DC motor.

---

**Science Curriculum Link:** Mechanical motion and energy conversion (Grade 8 Physics — Forces, Motion & Energy).
A servo motor is an **electromechanical transducer** — it converts electrical energy into precise rotational (mechanical) energy. This connects directly to the Grade 8 study of energy transformation and mechanical systems. Torque (rotational force) and angular position (measured in degrees) are core mechanical concepts students encounter here in a hands-on context.

---

## Materials Checklist (per pair)

- [ ] 1 × Arduino Mega 2560 + USB cable
- [ ] 1 × Solderless breadboard
- [ ] 1 × SG90 servo motor (9g micro servo)
- [ ] 3 × M-F jumper wires (red, black, and 1 signal color — often orange/yellow)
- [ ] Computer with Arduino IDE (`Servo.h` is built into Arduino IDE — no extra install needed)

> **Teacher note:** The SG90 comes with a small bag of plastic servo horns (the white attachments). Snap one onto the shaft so students can see the rotation angle clearly. Do NOT use a full 180° horn arm near fingers while in motion — brief teacher demo with the arm installed.

---

## 2. Teacher Guide (45-minute breakdown)

### 0–5 min — Hook / Warm-Up

**Bring in (or show a photo of) three different motors:**
1. A regular DC motor (from a toy car or fan) — spins continuously.
2. A stepper motor (if available, or show a photo) — moves in fixed steps.
3. A servo motor (SG90) — moves to a specific angle and holds it.

**Ask:** "These are all motors — they all convert electricity into motion. But they behave very differently. What do you think makes each one suited for a different job? Where would you use each one?"

*Expected answers:* DC motor for a fan/wheel (continuous spin); servo for a robot arm/rudder/antenna (precise position); stepper for a printer head (precise steps).

**Bridge:** "Today we are using a servo — the kind found in model aeroplanes, robot arms, and camera gimbals. It moves to any angle from 0° to 180° and stays there. Let's find out how."

---

### 5–15 min — Direct Instruction

**Concept: Servo motors, PWM position control, energy conversion**

*Words to say:*
"A servo motor is actually a small package containing three things: a DC motor, a set of gears (to increase torque and slow down the speed), and a position sensor (a potentiometer) that tells the control circuit exactly what angle the output shaft is at. The control circuit inside the servo compares the *desired angle* (the signal you send) with the *actual angle* (from the internal pot) and drives the motor until they match. This is a **mini feedback loop** built right inside the servo!

We talk to the servo using a **PWM signal** — a pulse that repeats every 20 ms. The *width* of the pulse sets the angle:
- 1 ms pulse → 0°
- 1.5 ms pulse → 90° (centre)
- 2 ms pulse → 180°

The Arduino's `Servo.h` library handles all this timing for us. We just call `myServo.write(angle)` with a number 0–180.

**The SG90 has three wires:**
- Brown / Black = GND
- Red = 5V (power)
- Orange / Yellow / White = Signal (PWM)

**IMPORTANT safety note about current:** Servos can draw 100–250 mA when moving under load. That is within the Mega's 5V pin limit for ONE servo, but keep the servo horn free and do not stall it (force it against a stop) because a stalled servo draws peak current continuously and can get hot.

**Energy conversion:** Electrical energy (5V × ~100 mA = ~0.5 W) → mechanical energy (rotation × torque). The SG90 produces about 1.8 kg·cm of torque — enough to flick a switch or move a lightweight lever."

Draw on the board:
```
Arduino           Servo Motor
pin ~9 ──PWM──►  [Signal wire]
                       │
               ┌───────▼──────────────────┐
               │  PWM → Angle decoder     │
               │  DC motor + gearbox      │
               │  Internal pot (feedback) │
               └──────────────────────────┘
                    Output shaft rotates 0–180°
```

---

### 15–35 min — Hands-On Build + Code

**Remind students:** Build → Check → Power on. Keep fingers clear of the servo horn while powered. Do not force the horn by hand while powered.

**Building the circuit (numbered steps):**

1. The servo has a 3-pin connector on a short cable. You do NOT need the breadboard for the servo itself — use M-F jumper wires to connect directly to the Mega's header pins.
2. Identify the servo wire colors:
   - **Brown or Black** = GND
   - **Red** = 5V (VCC)
   - **Orange or Yellow or White** = Signal (PWM)
3. Connect servo **GND** (brown/black) → Arduino **GND** (black wire).
4. Connect servo **VCC** (red) → Arduino **5V** (red wire).
5. Connect servo **Signal** (orange/yellow) → Arduino **pin 9** (signal wire). Pin 9 is PWM-capable (marked ~ on the Mega).
6. Partner check: brown/black → GND, red → 5V, orange → pin 9. ✓
7. Make sure the servo horn is attached to the shaft (snap one of the white plastic pieces on).
8. Plug in USB, upload code, and observe the sweep.

> **Servo horn safety:** Before uploading, make sure no one's fingers are near the servo horn. The horn sweeps the full 180° and can pinch fingers near the pivot. Clear the area before powering on.

---

### 35–42 min — Testing & Debugging

**What success looks like:** The servo sweeps smoothly from 0° to 180°, then back to 0°, on a 15-second cycle. The horn moves in a clear arc.

**Troubleshooting table:**

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| Servo twitches but doesn't sweep | Signal wire on wrong pin / non-PWM pin | Move signal to a PWM pin (2–13, 44–46 on Mega) |
| Servo hums/buzzes, doesn't move | Mechanical obstruction or stalled | Check nothing blocks the horn; reduce load |
| Servo only goes to 90°, not 180° | Code uses 90 as max | Change max angle to 180 in the for-loop |
| Servo jitters at 0° or 180° | Hitting mechanical end-stop | Limit range to 5°–175° to stay off the stops |
| Board resets when servo moves | Current spike causes brownout | This is rare on USB power; add a 100 µF cap across 5V/GND if needed |
| No movement at all | Power wires wrong | Check red → 5V, brown/black → GND |

---

### 42–45 min — Reflection / Exit Ticket

Students write on a sticky note:

1. "What energy transformation happens inside the servo motor?"
2. "Why does a servo know what angle it is at? What component inside it measures position?"
3. "What is the difference between calling `myServo.write(0)` and `myServo.write(180)`?"

---

## 3. Circuit Diagram

```
  ARDUINO MEGA 2560
  ┌──────────────────────────────────────────────┐
  │                                              │
  │  pin 9 (~PWM) ───────────────────────────────┼──► [orange/yellow] ──► Servo SIGNAL
  │  5V ─────────────────────────────────────────┼──► [red]           ──► Servo VCC
  │  GND ────────────────────────────────────────┼──► [black]         ──► Servo GND
  │                                              │
  └──────────────────────────────────────────────┘

               SG90 SERVO
         ┌─────────────────┐
         │  ┌────────────┐ │
         │  │  DC motor  │ │
         │  │  Gearbox   │ │  ← reduces speed, increases torque
         │  │  Int. pot  │ │  ← measures actual angle (feedback)
         │  └──────┬─────┘ │
         │         │        │
         │    Output shaft  │  ← horn attaches here; 0–180° range
         └─────────────────┘
           (GND)(5V)(Signal)
           brown  red  orange
```

**Fritzing-style build description:**

| # | From | To | Wire Color | Notes |
|---|------|----|-----------|-------|
| 1 | Arduino **GND** | Servo connector **GND** (brown/black wire) | Black | Use M-F jumper |
| 2 | Arduino **5V** | Servo connector **VCC** (red wire) | Red | Use M-F jumper |
| 3 | Arduino **pin 9** (~PWM) | Servo connector **Signal** (orange/yellow) | Orange | Use M-F jumper; MUST be a ~ pin |

> No breadboard is required. The 3-wire servo connector plugs directly onto M-F jumpers going to the Mega.

> **PWM pins on Mega:** 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 44, 45, 46 (all marked ~ in the IDE and on most pinout cards).

[Google image search: "SG90 servo motor Arduino Mega wiring diagram sweep"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 *  Session 21 — Servo Motors: Mechanical Motion
 *  Arduino Mega Teaching Curriculum — Phase 3
 * ============================================================
 *
 *  SCIENCE EXPLANATION:
 *  ─────────────────────────────────────────────────────────
 *  A servo motor converts ELECTRICAL energy into MECHANICAL
 *  energy (rotational motion). Inside the SG90 servo:
 *
 *  1. A tiny DC motor receives power and spins fast.
 *  2. A plastic gearbox (reduction gears) slows the spin
 *     down and multiplies the TORQUE (rotational force).
 *     The SG90 produces about 1.8 kg·cm of torque — enough
 *     to lift a 180g weight at 1 cm from the shaft.
 *  3. A small potentiometer mounted on the output shaft
 *     measures the ACTUAL angle continuously.
 *  4. A control IC compares DESIRED angle (from PWM signal)
 *     with ACTUAL angle (from pot) and drives the motor
 *     until they match. This is a closed-loop feedback system!
 *
 *  The Servo library sends PWM pulses automatically:
 *    0°  → 1.0 ms pulse width
 *    90° → 1.5 ms pulse width (centre)
 *    180°→ 2.0 ms pulse width
 *  These pulses repeat every 20 ms (50 Hz).
 *
 *  ENERGY TRANSFORMATION:
 *    Electrical energy (5V × ~100 mA) 
 *      → Heat (in motor coils, small loss)
 *      → Mechanical energy (rotation)
 * ============================================================
 *
 *  HARDWARE:
 *    Servo GND    → Arduino GND
 *    Servo VCC    → Arduino 5V
 *    Servo Signal → Arduino pin 9 (~PWM)
 *
 *  SAFETY: Keep fingers clear of the servo horn while powered.
 *          Do NOT hold the horn to stop it — this stalls the
 *          motor and causes high current draw and heat.
 * ============================================================
 */

// Include the Servo library — this is BUILT INTO Arduino IDE
// No installation needed!
#include <Servo.h>

// Create a Servo object to control our SG90
Servo myServo;

// Define which pin the servo signal wire is connected to
// >>> TRY CHANGING THIS: try pin 3, 5, or 6 — all are PWM pins
const int SERVO_PIN = 9;

// Define the sweep angles
// >>> TRY CHANGING THIS: change START_ANGLE and END_ANGLE
const int START_ANGLE = 0;    // Starting position in degrees
const int END_ANGLE   = 180;  // Ending position in degrees

// Define how fast to sweep (milliseconds delay per degree)
// >>> TRY CHANGING THIS: change 15 to 5 (fast) or 30 (slow)
const int SWEEP_DELAY = 15;

void setup() {
  Serial.begin(9600);
  Serial.println("Servo Sweep — Session 21");

  // Attach the servo to pin 9
  // This tells the library which PWM pin to use
  myServo.attach(SERVO_PIN);

  // Move to the starting position first
  myServo.write(START_ANGLE);
  delay(1000);                // Wait 1 second for servo to reach start
  Serial.println("Starting sweep...");
}

void loop() {
  // ── Sweep forward: from START_ANGLE to END_ANGLE ──
  // This for-loop increases the angle one degree at a time
  for (int angle = START_ANGLE; angle <= END_ANGLE; angle++) {
    myServo.write(angle);     // Tell servo to go to this angle
    delay(SWEEP_DELAY);       // Wait a moment so the servo can move

    // Print angle to Serial Monitor every 10 degrees
    if (angle % 10 == 0) {
      Serial.print("Angle: ");
      Serial.print(angle);
      Serial.println(" degrees");
    }
  }

  // Brief pause at the end position
  delay(500);

  // ── Sweep backward: from END_ANGLE back to START_ANGLE ──
  for (int angle = END_ANGLE; angle >= START_ANGLE; angle--) {
    myServo.write(angle);     // Tell servo to go to this angle
    delay(SWEEP_DELAY);       // Wait for servo to move
  }

  // Brief pause at the start position before next sweep
  delay(500);
}


// ===== CHALLENGE =====
// 1. SPECIFIC POSITIONS: Instead of sweeping, write a sketch that
//    moves the servo to 0°, waits 1 second, goes to 90°, waits,
//    then goes to 180°, waits — and repeats. Use myServo.write()
//    and delay() in sequence.
//
// 2. VARIABLE SPEED: Make the servo sweep FAST one way (delay 5)
//    and SLOW the other way (delay 30). Notice the difference.
//
// 3. BUTTON CONTROL (preview of Session 22): Add a pushbutton to
//    pin 4. Each button press advances the servo by 10 degrees.
//    When it reaches 180, it resets to 0.
//
// 4. WAVE MOTION: Use the sin() function to generate a smooth
//    wave sweep:
//    int angle = 90 + 85 * sin(millis() / 500.0);
//    myServo.write(angle);
//    (Smooth and continuous — like a wing flapping!)
```

---

## 5. Student Worksheet

### Session 21 — Servo Motors: Mechanical Motion
**Name(s):** _________________________ **Date:** _________ **Kit #:** ______

**Objective:** Wire an SG90 servo and program it to sweep from 0° to 180°, and explain the energy transformation from electrical to mechanical.

---

#### What I Already Know (Warm-Up)

1. What is "energy transformation"? Give one example from everyday life.
   > ___________________________________________________________________

2. You have used PWM (Pulse Width Modulation) before to fade LEDs. How do you think PWM could be used to control the *position* of a motor instead of the *brightness* of an LED?
   > ___________________________________________________________________

3. What is "torque"? (Try to remember from science class, or give your best guess.)
   > ___________________________________________________________________

---

#### Build It — Step by Step

- [ ] **Step 1:** Identify the 3 servo wires: Brown/Black = GND, Red = VCC, Orange/Yellow = Signal
- [ ] **Step 2:** Connect **black/brown** servo wire → Arduino **GND**
- [ ] **Step 3:** Connect **red** servo wire → Arduino **5V**
- [ ] **Step 4:** Connect **orange/yellow** servo wire → Arduino **pin 9** (~PWM)
- [ ] **Step 5:** Attach a servo horn (white plastic piece) to the shaft
- [ ] **Step 6:** Partner check — 3 wires correct before powering on
- [ ] **Step 7:** Clear fingers away from the servo horn
- [ ] **Step 8:** Upload code and observe the sweep

---

#### Predict!

Answer BEFORE uploading:

1. What do you think the servo arm will do when the code runs?
   > ___________________________________________________________________

2. The for-loop increases the angle from 0 to 180 one degree at a time, with a 15 ms delay between each degree. How long will the full 0→180 sweep take? (Show your calculation.)
   > 180 degrees × 15 ms = _______ ms = _______ seconds

3. What do you think will happen if you connect the signal wire to a non-PWM pin (e.g., pin 22)?
   > ___________________________________________________________________

---

#### Observe / Data Table

| Experiment | Code Change | Observed Behaviour |
|-----------|------------|-------------------|
| 1 — Basic sweep | No change (baseline) | |
| 2 — Fast sweep | SWEEP_DELAY = 5 | |
| 3 — Slow sweep | SWEEP_DELAY = 50 | |
| 4 — Limited range | START_ANGLE=45, END_ANGLE=135 | |
| 5 — Write(90) only | Move to 90° and stop | |

---

#### What Did You Notice?

1. At very small `SWEEP_DELAY` values, did the servo skip steps or vibrate? What does this tell you about how fast the servo can mechanically move?
   > ___________________________________________________________________

2. What happened near 0° and 180°? Did the servo make a different sound? Why might it be a good idea to limit the range to 5°–175°?
   > ___________________________________________________________________

3. When the servo was moving, could you feel (or hear) the motor working harder against resistance? What does that tell you about where the electrical energy is going?
   > ___________________________________________________________________

4. Fill in the energy transformation chain for this servo:
   > Electrical energy → _________________ energy → _________________ energy

---

#### Science Connection

1. Inside the servo is a tiny potentiometer that measures the actual angle. The servo compares "desired angle" vs "actual angle" and corrects itself. What is the scientific word for a system that compares desired vs actual and corrects the difference?
   > ___________________________________________________________________

2. The SG90 has a torque of 1.8 kg·cm. This means it can lift 1.8 kg at a distance of 1 cm from the shaft, or 0.18 kg at 10 cm. In your own words, explain the trade-off between distance from the shaft and the weight the servo can lift.
   > ___________________________________________________________________

3. A servo moves only 0–180°. A DC motor spins continuously (360° and beyond). Name one real-world application that needs a servo (precise angle) and one that needs a DC motor (continuous spin). Explain why each choice is appropriate.
   > Servo application: ___________________________________________________
   > DC motor application: ________________________________________________

---

#### Challenge Extension

1. **Specific positions:** Rewrite `loop()` to move the servo to 0°, 45°, 90°, 135°, 180°, then back — with a 1-second pause at each position.

2. **Freehand control:** Connect a potentiometer to A0. Use `analogRead()` and `map()` to set the servo angle based on the pot position (preview of Session 22!):
   ```cpp
   int potVal = analogRead(A0);
   int angle = map(potVal, 0, 1023, 0, 180);
   myServo.write(angle);
   ```

3. **Research:** Find out what a "gimbal" is. How many servo motors does a 3-axis camera gimbal use? What angle does each servo control?

---

## 6. Safety Notes

**This session's components and hazards:**

- **Servo motor horn:** The servo arm sweeps through 180°. **Keep fingers, hair, and loose clothing away from the horn while the servo is powered.** The SG90 is small but produces enough torque to give a sharp pinch.
- **Do not force the horn by hand while powered:** Forcing the horn to move or stop against the motor's direction stalls the motor. A stalled servo draws maximum current continuously, heats up quickly, and can damage the motor or the Arduino's 5V pin. If the servo jams, **unplug USB first**.
- **Current draw:** The SG90 draws ~100–250 mA when moving. One servo is fine on the Mega's USB power. Do NOT connect more than 2 servos from the Mega's 5V pin without an external power supply.
- **PWM pin requirement:** The servo signal wire MUST go to a PWM pin (marked ~). Connecting to a non-PWM digital pin will cause jitter or no movement.
- **Wire polarity:** Red → 5V, brown/black → GND. Reversed power wires can damage the servo's internal circuit.

**Build → Check → Power on:** Wire all three connections before plugging USB. Confirm wire colors and pin numbers with your partner before powering on.

**If something goes wrong:**
- Servo vibrates/buzzes without moving → power down, check for mechanical obstruction.
- Servo gets hot → unplug USB immediately, check the code is not stalling the servo at an end stop.
- Board brownout (resets) → reduce servo load; add a capacitor or use external 5V.

---

## 7. Assessment Rubric

### Formative Check (not graded)

| Look-For | Not Yet | Got It |
|----------|---------|--------|
| Correct 3-wire connection: GND → GND, 5V → 5V, Signal → PWM pin | Any wire incorrect, or signal on non-PWM pin | All three wires correctly placed on pin 9 (or another ~ pin) |
| Servo sweeps the full 0°–180° range | Servo twitches, only moves partially, or doesn't move | Smooth continuous sweep visible |
| Can explain the energy transformation in the servo | "It just moves" with no scientific vocabulary | Uses "electrical energy," "mechanical energy," and "rotation/torque" correctly |

---

## 8. Differentiation

### Support

- **Pre-labelled servo connector:** Place a small sticker on each wire (G, V, S) so students match letters to the Mega pins.
- **Code scaffold:** Provide the code with `myServo.write( __ )` blanks in the for-loop — student just fills in the variable name.
- **Limited sweep:** Reduce the sweep to 0°–90° to make observation easier and reduce the risk of hitting end stops.
- **Sentence starters:** "Inside the servo, electrical energy is converted to ____. The gears ____. The potentiometer ____."

### Extension

- **PWM waveform investigation:** Use an oscilloscope app (like an Arduino-based oscilloscope) to visualize the 1–2 ms PWM pulses as the angle changes.
- **Torque experiment:** Attach a lightweight arm to the horn and measure the maximum weight (using coins) the servo can lift at 2 cm vs. 4 cm from the shaft. Calculate torque.
- **Multi-servo choreography:** Wire a second servo on pin 10 and write a choreographed sequence where both servos move in patterns (mirrored, sequential, synchronized).
- **Physical model:** Have the student design a paper/cardboard "gate" or "flag" that mounts to the servo horn, then use the sweep code to open and close it.

### Visual / Kinesthetic Accommodations

- **Physical analogy for torque:** Use a spanner (wrench) — turning a bolt near the tip is easy (long lever = high torque), near the bolt head is hard (short lever). Relate to servo distance from shaft.
- **Gear analogy:** Bring in a bicycle wheel/gear to show how a smaller gear turning a larger one trades speed for torque.
- **Slow-motion run:** Set SWEEP_DELAY to 100 ms and watch the angle counter in the Serial Monitor climb one degree at a time.
- **Glossary card:** servo, torque, angular position, PWM, gearbox, feedback, energy transformation, degrees.
