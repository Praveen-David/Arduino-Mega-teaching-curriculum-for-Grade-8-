# Week 9, Session 17 — Ultrasonic Distance Sensor (HC-SR04)

**Phase:** Phase 2 — Sensors & Analog Signals
**Session Number:** 17 of 36
**Week:** 9

---

## Learning Objectives

By the end of this session, students will be able to:
- Explain how an ultrasonic sensor measures distance using the principle of echolocation.
- Wire an HC-SR04 sensor to the Arduino Mega (TRIG and ECHO pins).
- Use `pulseIn()` to measure the duration of the returning echo pulse.
- Calculate distance in centimeters using the formula: distance = (duration × speed of sound) / 2.

---

## Science Curriculum Link

**Grade 8 Concept: Sound Waves, Echolocation & Speed of Sound**

The HC-SR04 sends a 40 kHz ultrasonic pulse (above human hearing range) and measures how long it takes for the echo to return. This is **echolocation** — the same technique used by bats, dolphins, and submarine sonar. The science is elegant: since we know the speed of sound (≈ 343 m/s at 20°C), and we can measure the time for the round trip, we can calculate the one-way distance using **distance = speed × time**, divided by 2 for the round trip. This session integrates wave physics, kinematics, and real engineering in one compact experiment.

---

## Materials Checklist (per pair)

- 1 × Arduino Mega 2560
- 1 × USB-A to USB-B cable
- 1 × Solderless breadboard
- 1 × HC-SR04 ultrasonic distance sensor (4 pins: VCC, TRIG, ECHO, GND)
- 1 × LED (red recommended — for distance alarm)
- 1 × 220 Ω resistor
- Jumper wires (M-F preferred for HC-SR04): red × 1, black × 1, white × 1, blue × 1, orange × 1
- Computer with Arduino IDE installed
- (Optional) ruler or tape measure to verify distance readings

---

## 2. Teacher Guide (45-Minute Breakdown)

### 0–5 min — Hook / Warm-Up

*Hold the HC-SR04 sensor up and make an exaggerated bat sound:*

*"Squeak! I'm a bat. I just sent out a super-high-pitched sound. In a fraction of a second that sound bounced off the wall and came back to my ears. I know how fast sound travels, so I can calculate exactly how far the wall is just from how long the echo took. This is echolocation — and this little chip does exactly the same thing, 40 times per second, using sound you can't even hear."*

Ask: *"What information would you need to calculate a distance from a timed echo? What equation would you use?"*

Expected answer: distance = speed × time. But it's a round trip: divide by 2.

---

### 5–15 min — Direct Instruction

**How the HC-SR04 works:**

Draw the timing sequence on the board:

```
Step 1 — Trigger:
   Arduino sets TRIG pin HIGH for 10 microseconds → sensor fires an ultrasonic burst

Step 2 — Echo travel:
   [Sensor] ─────── [pulse outgoing] ─────────► [Object]
                                                    │
   [Sensor] ◄──── [echo returning] ─────────────────┘

Step 3 — Echo measurement:
   ECHO pin stays HIGH from the moment the burst is sent until the echo returns.
   We measure that duration with pulseIn().
```

**The distance formula:**

Write on the board:
```
Speed of sound ≈ 343 m/s = 0.0343 cm/µs   (at 20°C)

Time measured = round-trip time (there AND back)

One-way distance = (speed × time) / 2

In cm:
distance_cm = (duration_µs × 0.0343) / 2

Simplified:
distance_cm = duration_µs / 58.2     ← commonly used approximation
```

> "The `pulseIn(pin, HIGH)` function waits for the ECHO pin to go HIGH, then measures how long (in microseconds) it stays HIGH. That duration is the round-trip travel time."

**Typical measurement range:**

- Minimum: ~2 cm (too close = echo overlaps with transmitted pulse)
- Maximum: ~400 cm
- Best range: 5–200 cm

**TRIG and ECHO pin roles:**

> "TRIG = trigger output. WE send a pulse to the sensor to tell it to fire. ECHO = echo input. The SENSOR sends us a pulse whose duration tells us the travel time. So TRIG is an OUTPUT from the Arduino; ECHO is an INPUT to the Arduino."

---

### 15–35 min — Hands-On Build + Code

**Remind students:** *"Build → Check → Power on. Unplug USB before wiring."*

1. Unplug the Arduino.
2. The HC-SR04 has 4 pins labeled **VCC, TRIG, ECHO, GND** (check the label on the sensor).
3. Connect **VCC** → 5V rail (red wire).
4. Connect **GND** → GND rail (black wire).
5. Connect **TRIG** → Arduino **digital pin 10** (white wire).
6. Connect **ECHO** → Arduino **digital pin 11** (blue wire).
7. Wire the **LED** + **220 Ω resistor** from **Arduino pin 9** to GND (alarm LED).
8. Partner checks: VCC=5V, GND=GND, TRIG=10 (output), ECHO=11 (input). Verify colors.
9. Plug in USB. Enter and upload the code from Section 4.
10. Open Serial Monitor (9600 baud).
11. Hold your hand in front of the sensor. Readings should show distance in cm.
12. Slowly move your hand closer and further away. Watch the values change.
13. Verify with a ruler: hold hand exactly 10 cm, 20 cm, 30 cm from sensor. How accurate are the readings?

---

### 35–42 min — Testing & Debugging

**What success looks like:**
- Serial Monitor shows distance in cm, updating ~5 times per second.
- Readings are stable (±1–2 cm) at a fixed distance.
- Moving hand closer = smaller number; moving further = larger number.
- LED turns on/blinks when an object is closer than the alarm threshold.
- Readings of 0 or 400+ mean no object in range — expected.

**Troubleshooting Table:**

| Symptom | Fix |
|---------|-----|
| Always reads 0 cm | Check TRIG and ECHO are not swapped. TRIG → pin 10 (output), ECHO → pin 11 (input). |
| Always reads 400+ cm or "OUT OF RANGE" | No echo returning — check TRIG sends a pulse: confirm TRIG is wired to OUTPUT pin 10. Make sure something is in front of the sensor (within 2–400 cm). |
| Very unstable readings (jumping ±20 cm) | Add a solid object (hard flat surface, not a hand). Porous surfaces absorb ultrasonic pulses and give poor readings. |
| Reads correctly but LED doesn't respond | Check pin 9 wiring and LED polarity. Check `ALARM_DISTANCE` constant value in code. |
| Sensor gets warm | Check VCC is 5V, not a signal pin accidentally set HIGH. |

---

### 42–45 min — Reflection / Exit Ticket

**Exit ticket questions:**

1. *"A pulse takes 1460 microseconds to return. Calculate the distance in cm. Show your work."* (Answer: 1460 / 58.2 ≈ 25.1 cm)
2. *"Why do we divide by 2 in the distance formula?"*
3. *"Name one real-world device or animal that uses echolocation. Describe how it works."*

---

## 3. Circuit Diagram

### ASCII Wiring Diagram

```
   Arduino Mega 2560
   ┌──────────────────────────────┐
   │                              │
   │  5V ─────────────────────────┼──── Red rail (+)
   │  GND ────────────────────────┼──── Black rail (–)
   │                              │
   │  Digital Pin 10 (OUTPUT) ────┼──── HC-SR04 TRIG (white wire)
   │  Digital Pin 11 (INPUT)  ────┼──── HC-SR04 ECHO (blue wire)
   │                              │
   │  Pin 9 (~) ──────────────────┼──── [220 Ω] ──── LED (+) ──── LED (–) ──── GND
   └──────────────────────────────┘
   
   HC-SR04 Module (left to right, facing front with transducer eyes):
   
   [ VCC ] ── Red ──── 5V rail
   [ TRIG ] ── White ── Arduino Pin 10
   [ ECHO ] ── Blue ──  Arduino Pin 11
   [ GND  ] ── Black ── GND rail
   
   Sensor fires ultrasonic burst:
   
   [○○] ──── ())) ────────────► object ◄──── ))) ──── ECHO received
   TRIG ECHO   40 kHz pulse                  reflected pulse
```

### Fritzing-Style Build Description

| Step | From | To | Wire Color | Notes |
|------|------|----|------------|-------|
| 1 | Arduino **5V** | Breadboard **+ rail** | Red | Power |
| 2 | Arduino **GND** | Breadboard **– rail** | Black | Ground |
| 3 | HC-SR04 **VCC** | Breadboard **+ rail** | Red | Sensor power |
| 4 | HC-SR04 **GND** | Breadboard **– rail** | Black | Sensor ground |
| 5 | HC-SR04 **TRIG** | Arduino **Pin 10** | White | Trigger output |
| 6 | HC-SR04 **ECHO** | Arduino **Pin 11** | Blue | Echo input |
| 7 | Arduino **Pin 9** | 220 Ω resistor leg 1 (row 20) | Orange | LED signal |
| 8 | 220 Ω resistor leg 2 | LED **long leg** (row 22) | — | |
| 9 | LED **short leg** | Breadboard **– rail** | Black | |

**Note:** Male-to-female jumper wires are recommended for the HC-SR04 (its pins are header pins, not breadboard legs). Some HC-SR04 modules sit directly on the breadboard with their 4 pins if they have single-row pin headers.

**Component values:** HC-SR04 (operates at 5V; some clones are 3.3V — check your specific module), 220 Ω (red-red-brown-gold).

[Google image search: "HC-SR04 Arduino Mega wiring diagram"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 * SESSION 17: Ultrasonic Distance Sensor (HC-SR04)
 * Arduino Mega 2560 — Grade 8 STEM Curriculum
 * ============================================================
 *
 * SCIENCE EXPLANATION:
 * ------------------------------------------------------------------
 * ECHOLOCATION: the HC-SR04 emits a 40 kHz ultrasonic burst and
 * measures how long the echo takes to return.
 *
 * PHYSICS:
 *   Speed of sound (air, 20°C) = 343 m/s = 0.0343 cm/µs
 *
 *   Duration = round-trip time (outgoing + return)
 *   One-way distance = (speed × duration) / 2
 *
 *   distance_cm = (0.0343 cm/µs × duration_µs) / 2
 *               = duration_µs / 58.2        ← simplified formula
 *
 * HOW THE PINS WORK:
 *   TRIG (Trigger) — OUTPUT: Arduino sends a 10 µs HIGH pulse
 *                    to tell the sensor to fire a burst.
 *   ECHO          — INPUT:  Sensor holds this pin HIGH for the
 *                    duration of the round-trip travel time.
 *   pulseIn(pin, HIGH) measures how long ECHO stays HIGH (in µs).
 * ------------------------------------------------------------------
 *
 * Circuit:  HC-SR04 TRIG → Pin 10, ECHO → Pin 11
 *           LED + 220 Ω on Pin 9 (alarm when object is close)
 * ============================================================
 */

// ---- Pin Definitions ----
const int TRIG_PIN  = 10;  // OUTPUT: trigger pulse to HC-SR04
const int ECHO_PIN  = 11;  // INPUT: echo pulse from HC-SR04
const int LED_PIN   = 9;   // OUTPUT: alarm LED (blinks when object is close)

// ---- Distance Threshold for Alarm ----
// >>> TRY CHANGING THIS: set to 15 (cm) for a closer alarm trigger
const int ALARM_DISTANCE = 20;   // LED blinks when object is within 20 cm

// ---- Speed of sound constant ----
// 0.0343 cm per microsecond (at room temperature, ~20°C)
const float SOUND_SPEED_CM_PER_US = 0.0343;

void setup() {
  Serial.begin(9600);
  pinMode(TRIG_PIN, OUTPUT);   // Trigger pin: we send pulses OUT
  pinMode(ECHO_PIN, INPUT);    // Echo pin: we read pulses IN
  pinMode(LED_PIN, OUTPUT);

  Serial.println("=== Session 17: HC-SR04 Distance Sensor ===");
  Serial.println("Hold an object in front of the sensor.");
  Serial.println("-------------------------------------------");
  Serial.println("Distance (cm) | Duration (µs) | Status");
  Serial.println("-------------------------------------------");
}

long measureDistance() {
  // --- Step 1: Ensure TRIG is LOW first ---
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);   // Brief LOW before trigger

  // --- Step 2: Send a 10-microsecond HIGH pulse on TRIG ---
  // This tells the HC-SR04 to fire 8 ultrasonic pulses at 40 kHz
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);  // Keep HIGH for exactly 10 µs
  digitalWrite(TRIG_PIN, LOW);   // End the trigger pulse

  // --- Step 3: Measure how long ECHO stays HIGH ---
  // pulseIn() waits for ECHO to go HIGH, then times how long it stays HIGH
  // timeout = 30000 µs = 30 ms → max range = 343*0.03/2 = ~5 m
  long duration = pulseIn(ECHO_PIN, HIGH, 30000);

  return duration;   // Return the raw duration in microseconds
}

void loop() {
  // --- Take a distance measurement ---
  long duration = measureDistance();   // Duration in microseconds

  // --- Calculate distance ---
  // duration is round-trip time → divide by 2 for one-way
  float distance_cm = (duration * SOUND_SPEED_CM_PER_US) / 2.0;

  // --- Handle out-of-range readings ---
  // pulseIn() returns 0 if timeout occurs (nothing in range)
  if (duration == 0 || distance_cm > 400) {
    Serial.println("OUT OF RANGE       |              | No object detected");
    digitalWrite(LED_PIN, LOW);
    delay(200);
    return;   // Skip the rest, take next reading
  }

  // --- Alarm: blink LED if object is too close ---
  if (distance_cm < ALARM_DISTANCE) {
    digitalWrite(LED_PIN, HIGH);
    Serial.print(distance_cm, 1);
    Serial.print(" cm        | ");
    Serial.print(duration);
    Serial.println(" µs       | *** CLOSE! ***");
  } else {
    digitalWrite(LED_PIN, LOW);
    Serial.print(distance_cm, 1);
    Serial.print(" cm        | ");
    Serial.print(duration);
    Serial.println(" µs       | OK");
  }

  // >>> TRY CHANGING THIS: 200 ms between readings. Try 100 or 500.
  delay(200);
}

// ===== CHALLENGE =====
// 1. PARKING SENSOR: Blink the LED faster as the object gets closer.
//    Use map(distance_cm, 5, 50, 50, 1000) to get blink delay,
//    then blink the LED with that delay.
//
// 2. BUZZER ALARM: Wire the passive buzzer on pin 8.
//    When distance < 10 cm: tone(8, 2000, 100)
//    When distance 10–20 cm: tone(8, 1000, 50)
//    When distance > 20 cm: noTone(8)
//
// 3. ACCURACY EXPERIMENT: Use a ruler to set objects at exactly
//    5, 10, 20, 30, 40, 50 cm. Record the measured distance.
//    Calculate the error (measured − actual) and percentage error.
//    Plot error vs distance. What pattern do you see?
//
// 4. SPEED OF SOUND: Rearrange the formula to calculate speed of sound
//    from your measured time and known distance:
//    speed = 2 × distance / duration
//    Does your value match 0.0343 cm/µs?
```

---

## 5. Student Worksheet

---

### Session 17 Worksheet — Ultrasonic Distance Sensor (HC-SR04)

**Name(s):** _________________________________ **Date:** _____________ **Kit #:** _____

**Objectives:**
- Understand echolocation and how sound can measure distance.
- Wire the HC-SR04 and read distance values.
- Apply the distance = speed × time formula.

---

#### What I Already Know (Warm-Up)

1. From Session 15: What is the approximate speed of sound in air at 20°C?

   ___________________________________________________________________________

2. A bat sends out a squeak and hears the echo 0.006 seconds (6,000 µs) later. How far away is the object?
   (Hint: use distance = speed × time ÷ 2. Speed = 34,300 cm/s)

   Show your work: ___________________________________________________________________________

3. What does "ultrasonic" mean? Why can't we hear the HC-SR04's sound pulse?

   ___________________________________________________________________________

---

#### Build It — Step by Step

Unplug USB before wiring.

1. Hold the HC-SR04 with the two transducer "eyes" facing you. Find the 4 pins: VCC, TRIG, ECHO, GND.
2. Connect **VCC** → breadboard **+ rail** (red wire).
3. Connect **GND** → breadboard **– rail** (black wire).
4. Connect **TRIG** → Arduino **Pin 10** (white wire).
5. Connect **ECHO** → Arduino **Pin 11** (blue wire).
6. Wire **LED** (long leg) → 220 Ω resistor → **Pin 9**. LED (short leg) → GND.
7. Partner check: TRIG=10, ECHO=11, VCC=5V, GND=GND.
8. Plug in USB. Upload code. Open Serial Monitor at **9600 baud**.

---

#### Predict!

Use the formula `distance_cm = duration_µs / 58.2` to predict duration for each distance:

| Actual distance (cm) | Predicted duration (µs) | Measured duration (µs) | Measured distance (cm) |
|---------------------|------------------------|------------------------|------------------------|
| 5 cm | | | |
| 10 cm | | | |
| 20 cm | | | |
| 30 cm | | | |
| 50 cm | | | |

Show your prediction calculation for 20 cm: Duration = 20 × 58.2 = _______ µs

---

#### Observe / Accuracy Data Table

Use a ruler. Set an object (book, hand, wall) at the exact distance listed. Record the measured value from the Serial Monitor:

| Actual distance (cm) | Measured distance (cm) | Error (measured − actual) | % Error |
|---------------------|------------------------|--------------------------|---------|
| 5 | | | |
| 10 | | | |
| 20 | | | |
| 30 | | | |
| 50 | | | |
| 100 | | | |

% Error formula: (|Error| ÷ Actual) × 100

---

#### What Did You Notice?

1. Was the sensor more accurate at short distances or long distances? Why might this be?

   ___________________________________________________________________________

2. Try measuring the distance to a soft, padded object (like a backpack or jacket). Is the reading more or less accurate than for a hard flat surface? Why?

   ___________________________________________________________________________

3. The LED turned on when the object was within 20 cm. What real device uses this same principle? (Think: cars reversing.)

   ___________________________________________________________________________

4. What is the fastest you can wave your hand back and forth while the sensor still tracks it correctly? What limits the measurement speed?

   ___________________________________________________________________________

---

#### Science Connection

**Echolocation in nature:**
- Bats can locate insects as small as 1 mm using echolocation at 20,000–100,000 Hz.
- Dolphins use echolocation in water at speeds of ~1,500 m/s.
- Medical ultrasound (sonography) uses exactly this principle to image organs inside the body at frequencies of 2–18 MHz.

**Question:** Medical ultrasound uses 5 MHz (5,000,000 Hz) sound in body tissue (speed ≈ 1,540 m/s). What is the wavelength of this sound?

λ = 1540 ÷ 5,000,000 = ________ m = ________ mm

At this wavelength, what is the smallest feature that can be detected? (Hint: you can't detect anything smaller than one wavelength.)

___________________________________________________________________________

---

#### Challenge Extension

**Accuracy experiment** (from Challenge 3 in the code):
1. Measure distances at 5, 10, 20, 30, 40, 50 cm with a ruler.
2. Record the sensor's reading at each point.
3. On graph paper, plot: x-axis = actual distance, y-axis = measured distance.
4. Draw a diagonal "perfect accuracy" line (where measured = actual).
5. How close are your data points to the ideal line?

Describe what you observe: ___________________________________________________________________________

Does the error increase or decrease with distance? ___________________________________________________________________________

---

## 6. Safety Notes

| Hazard | Precaution |
|--------|------------|
| Ultrasonic sound | The 40 kHz signal is above human hearing range and harmless. Do not stare into the transducer — it emits sound, not light, so there is nothing to see. |
| Sensor facing | Point the sensor at objects, not at people's faces during operation, as a safe practice habit. |
| Voltage | Use 5V for HC-SR04 VCC. A 3.3V HC-SR04 clone (check your specific sensor) connected to 5V may be damaged. If in doubt, check the datasheet. |
| ECHO pin voltage | Standard HC-SR04 returns 5V on the ECHO pin, which is safe for Arduino Mega. Some 3.3V modules need a voltage divider on ECHO — confirm your module. |

**Build → Check → Power on.**

**If something goes wrong:**
- Reading is always 0 → TRIG and ECHO may be swapped. TRIG is output (we send); ECHO is input (we receive).
- Sensor warm/hot → VCC may be wired to a signal pin accidentally. Unplug immediately.

---

## 7. Assessment Rubric

### Formative Check (not graded)

| Look-For | Not Yet | Got It |
|----------|---------|--------|
| Sensor produces changing distance readings as object moves | | |
| Student correctly identifies which pin is TRIG and which is ECHO | | |
| Student can correctly calculate distance from a given duration (e.g., 1460 µs → 25.1 cm) | | |
| Student can explain the "divide by 2" in the formula | | |
| Student connects to echolocation (bats, sonar, medical ultrasound) | | |

---

## 8. Differentiation

### Support
- Provide a pre-filled formula template: `distance_cm = _______ ÷ 58.2` — students only insert the measured duration.
- Assign specific roles: one student holds the ruler, one watches the Serial Monitor, one records — reduces cognitive load.
- If TRIG/ECHO confusion is common, label the wires with masking tape before the session: "TRIG → Pin 10 (we SEND)", "ECHO → Pin 11 (we RECEIVE)."
- Use the `measureDistance()` function in the code as a discussion point: *"What does a function do? Why is it useful to put the measurement code in a function?"* (This previews Phase 3 concepts.)

### Extension
- Use the `NewPing` library (install from Library Manager) as an alternative to the manual `pulseIn()` approach. Compare code complexity — which is cleaner?
- **Speed of sound experiment** (Challenge 4): measure duration at known distances and back-calculate the speed of sound. Does the result change at different room temperatures? (Speed of sound increases ~0.6 m/s per °C — combine with DHT11 readings from Sessions 13–14.)
- Build a **basic parking sensor**: map distance to LED blink rate and buzzer pitch — a preview of Phase 3 (Systems & Control).
- Research: *"What is LIDAR? How do self-driving cars use it? How is it similar to and different from ultrasonic ranging?"*

### Visual / Kinesthetic Accommodations
- Use the Serial Plotter to graph distance in real time — moving objects produce a continuous waving line on screen.
- Physical demonstration: clap hands together and listen for an echo in a hallway or against a hard wall. Time the echo with a stopwatch. Calculate the distance. Compare to sensor accuracy.
- Draw the timing diagram on the board (TRIG pulse, ECHO response) as a physical analogy: teacher says "GO" (TRIG), waits, hears echo (ECHO returned), counts the wait time.
- For the accuracy experiment, have students use masking tape to mark exact distances on the desk before measuring — reduces error from measurement uncertainty.
