# Week 13, Session 25 — Distance Alarm / Parking Sensor

**Phase:** Phase 3 — Systems & Control
**Week:** 13 | **Session:** 25 of 36

---

## Learning Objectives

By the end of this session, students will be able to:
- Wire an HC-SR04 ultrasonic sensor, passive buzzer, and 16×2 I2C LCD together on the Arduino Mega.
- Program a parking-sensor system where the buzzer beep rate increases as an object gets closer (threshold-based system).
- Explain how sound waves, distance, and thresholds combine into a useful warning system.
- Define a **threshold** in a control system and explain how multiple thresholds create graduated responses.

---

**Science Curriculum Link:** Sound waves, systems design, and threshold-based control (Grade 8 Physics — Sound Waves; Grade 8 Systems & Technology).
The HC-SR04 uses the same principle as sonar and echolocation: emitting a sound pulse and measuring the time for the echo to return. The parking-sensor application demonstrates threshold-based control: different distance zones trigger different beep rates, giving the driver graduated feedback. This also connects to wave properties (frequency, speed of sound) and how animals like bats navigate.

---

## Materials Checklist (per pair)

- [ ] 1 × Arduino Mega 2560 + USB cable
- [ ] 1 × Solderless breadboard
- [ ] 1 × HC-SR04 ultrasonic sensor
- [ ] 1 × 16×2 LCD with I2C backpack (SDA → pin 20, SCL → pin 21)
- [ ] 4 × M-F jumper wires for LCD
- [ ] 4 × M-F or M-M jumper wires for HC-SR04
- [ ] 1 × Passive buzzer (NOT active — must respond to `tone()`)
- [ ] 2 × M-M jumper wires for buzzer
- [ ] Several M-M jumper wires (various colours)
- [ ] Computer with Arduino IDE

> **Teacher note:** Confirm students have **passive** buzzers. Active buzzers beep at one fixed frequency regardless of `tone()`. If you only have active buzzers, adjust the code to use `digitalWrite()` on/off with `delay()` instead of `tone()`. The parking-sensor behaviour still works.

---

## 2. Teacher Guide (45-minute breakdown)

### 0–5 min — Hook / Warm-Up

**Play a short clip of a car reversing with a parking sensor** (search "car parking sensor sound" on YouTube, or imitate the beeping yourself with increasing frequency). 

**Ask:** "What information does the RATE of the beep give you? Why don't car parking sensors just beep at the same rate all the time?"

*Expected answers:* Faster beep = closer to obstacle. Constant beep means you can't tell how close you are. The beep rate is a clever way to encode distance information in an audio signal — you can hear it without looking.

**Ask:** "What is the sensor in a car parking system? How does it measure distance?" (Recap from Session 17.)

---

### 5–15 min — Direct Instruction

**Concept: Sound waves, thresholds, and graduated response**

*Words to say:*
"In Session 17 you learned how the HC-SR04 works. Quick recap:

- It sends out a 40 kHz ultrasound pulse — too high for humans to hear.
- It times how long the echo takes to return.
- Distance = echo time × speed of sound ÷ 2 (because the sound travels to the object AND back).

Today we're not just measuring distance — we're making the distance **mean something** by mapping it to a warning system.

**Thresholds:** A threshold is a critical value that triggers a state change. A car parking sensor has multiple thresholds:

| Zone | Distance | Beep rate |
|------|----------|-----------|
| Safe | > 100 cm | No beep |
| Caution | 50–100 cm | Slow beep (every 500 ms) |
| Warning | 20–50 cm | Fast beep (every 200 ms) |
| Danger | < 20 cm | Continuous tone |

We implement this with if/else if statements — a 'cascade of thresholds.'

**LCD design:** We'll show the distance in cm on the LCD, and the zone label (SAFE / CAUTION / WARNING / DANGER). This gives the user both precise data AND a quick visual summary.

**Buzz timing trick:** Instead of using `delay()` for the buzz (which would freeze the distance reading), we use `millis()` — the Arduino's built-in timer that counts milliseconds since startup. This is called **non-blocking code**. It lets us do two things at once: update the buzzer AND keep reading the sensor."

Write the zone table on the board and have students copy it onto the worksheet.

---

### 15–35 min — Hands-On Build + Code

**Remind students:** Build → Check → Power on. Buzzer can be loud — use short tones.

**Building the circuit (numbered steps):**

1. **LCD** (from Sessions 19–20): GND → GND, VCC → 5V, SDA → pin 20, SCL → pin 21. Verify.
2. **HC-SR04 sensor**:
   - VCC → Arduino **5V** (red wire)
   - GND → Arduino **GND** (black wire)
   - TRIG → Arduino **pin 9** (green wire)
   - ECHO → Arduino **pin 10** (orange wire)
3. **Passive buzzer**:
   - Insert buzzer into breadboard (or use M-M wires directly).
   - Longer leg (positive/+) → Arduino **pin 8** (via jumper wire — yellow).
   - Shorter leg (negative/−) → Arduino **GND** (black wire).
4. Partner check: HC-SR04 TRIG/ECHO on correct pins, buzzer polarity.
5. Upload code. Hold a hand in front of the HC-SR04 at different distances and listen to the beep rate change.

---

### 35–42 min — Testing & Debugging

**What success looks like:** LCD shows distance in cm and zone label. Buzzer beeps slowly far away, faster when closer, and gives a continuous tone inside 20 cm.

**Troubleshooting table:**

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| Distance always reads 0 or 999 | TRIG/ECHO swapped, or sensor not powered | Check TRIG → 9, ECHO → 10, VCC → 5V |
| Buzzer makes no sound | Active buzzer used instead of passive; or wired backwards | Check buzzer polarity; if active, use `digitalWrite()` instead of `tone()` |
| Buzzer beeps but no distance change | millis() timing works but distance calculation wrong | Check pulseIn logic; open Serial Monitor to see raw values |
| LCD shows wrong/garbled text | Wrong I2C address | Try 0x3F; check contrast |
| Beep rate doesn't change with distance | Zone thresholds not matching actual distances | Read Serial Monitor, adjust `ZONE_*` constants in code |
| Buzzer is extremely loud | Volume too high | Reduce tone frequency to 500 Hz for a softer tone |

---

### 42–45 min — Reflection / Exit Ticket

Students write on a sticky note:

1. "Name the four distance zones in your parking sensor and the beep rate for each."
2. "Why did we use `millis()` for the buzzer timing instead of `delay()`? What problem does `delay()` cause in this system?"
3. "The HC-SR04 uses 40 kHz sound. The speed of sound is ~343 m/s. What is the wavelength of this ultrasound pulse?" (λ = v/f — optional enrichment question.)

---

## 3. Circuit Diagram

```
  ARDUINO MEGA 2560
  ┌────────────────────────────────────────────────────────────┐
  │  pin 20 (SDA) ──────────────────────────────────────────── ┼──► [blue]   ──► LCD SDA
  │  pin 21 (SCL) ──────────────────────────────────────────── ┼──► [yellow] ──► LCD SCL
  │  5V ────────────────────────────────────────────────────── ┼──► [red]    ──► LCD VCC
  │  GND ───────────────────────────────────────────────────── ┼──► [black]  ──► LCD GND
  │                                                            │
  │  5V ────────────────────────────────────────────────────── ┼──► [red]    ──► HC-SR04 VCC
  │  GND ───────────────────────────────────────────────────── ┼──► [black]  ──► HC-SR04 GND
  │  pin 9  (digital out) ─────────────────────────────────── ┼──► [green]  ──► HC-SR04 TRIG
  │  pin 10 (digital in)  ─────────────────────────────────── ┼──► [orange] ──► HC-SR04 ECHO
  │                                                            │
  │  pin 8  (digital ~PWM) ───────────────────────────────── ┼──► [yellow] ──► Buzzer (+)
  │  GND ───────────────────────────────────────────────────── ┼──► [black]  ──► Buzzer (−)
  └────────────────────────────────────────────────────────────┘
  
  HC-SR04 ULTRASOUND PATH:
  
  [TRIG]──► emits 40 kHz pulse ──►  ○ ○ ○ ○ ──► [OBJECT] ──► ○ ○ ○ ○ (echo)
  [ECHO]◄─────────────── echo received ◄──────────────────────────────────
  
  Distance (cm) = pulse duration (µs) × 0.0343 / 2
```

**Fritzing-style build description:**

| # | From | To | Wire Color | Notes |
|---|------|----|-----------|-------|
| 1 | Arduino GND | LCD backpack GND | Black | |
| 2 | Arduino 5V | LCD backpack VCC | Red | |
| 3 | Arduino pin 20 | LCD backpack SDA | Blue | Mega I2C data |
| 4 | Arduino pin 21 | LCD backpack SCL | Yellow | Mega I2C clock |
| 5 | Arduino GND | HC-SR04 GND | Black | |
| 6 | Arduino 5V | HC-SR04 VCC | Red | |
| 7 | Arduino pin 9 | HC-SR04 TRIG | Green | Trigger output |
| 8 | Arduino pin 10 | HC-SR04 ECHO | Orange | Echo input |
| 9 | Arduino pin 8 | Buzzer + (long leg) | Yellow | PWM tone output |
| 10 | Arduino GND | Buzzer − (short leg) | Black | |

[Google image search: "Arduino HC-SR04 ultrasonic parking sensor buzzer LCD distance alarm"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 *  Session 25 — Distance Alarm / Parking Sensor
 *  Arduino Mega Teaching Curriculum — Phase 3
 * ============================================================
 *
 *  SCIENCE EXPLANATION:
 *  ─────────────────────────────────────────────────────────
 *  The HC-SR04 uses ECHOLOCATION (same principle as bat sonar
 *  and submarine SONAR):
 *  1. TRIG pin fires a brief 40 kHz ultrasound burst.
 *  2. ECHO pin times how long it takes the reflection to return.
 *  3. Distance = (echo_time_µs × 0.0343 cm/µs) / 2
 *     (÷2 because the sound makes a round trip)
 *
 *  Speed of sound varies with temperature:
 *    v ≈ 331.4 + 0.6 × T(°C) metres per second
 *  At 20°C: v ≈ 343 m/s = 0.0343 cm/µs (our constant)
 *
 *  THRESHOLDS: We define distance zones. Each zone has a
 *  different beep interval. This maps a continuous measurement
 *  (cm) to a graduated categorical response (beep rate).
 *
 *  NON-BLOCKING BUZZER (millis()):
 *  We cannot use delay() for buzzer timing — it would freeze
 *  the distance measurement. Instead, we check millis() to
 *  see if enough time has passed since the last beep.
 *  This is called "non-blocking" or "cooperative" timing.
 * ============================================================
 *
 *  HARDWARE:
 *    HC-SR04 TRIG → pin 9
 *    HC-SR04 ECHO → pin 10
 *    Passive Buzzer → pin 8
 *    LCD SDA → pin 20, LCD SCL → pin 21
 * ============================================================
 */

#include <Wire.h>
#include <LiquidCrystal_I2C.h>

// --- Pin definitions ---
const int TRIG_PIN   = 9;
const int ECHO_PIN   = 10;
const int BUZZER_PIN = 8;

// --- Distance zone thresholds (in cm) ---
// >>> TRY CHANGING THIS: adjust zones to match your testing space
const int ZONE_SAFE    = 100;   // > 100 cm → safe, no beep
const int ZONE_CAUTION = 50;    // 50–100 cm → slow beep
const int ZONE_WARNING = 20;    // 20–50 cm → fast beep
// < 20 cm = DANGER (continuous tone)

// --- Buzzer tone frequencies ---
const int BEEP_FREQ = 1000;     // Hz — change for different tone pitch
// >>> TRY CHANGING THIS: 500 = lower tone, 2000 = higher pitch

// --- Beep interval durations (milliseconds between beeps) ---
const int BEEP_DURATION    = 80;    // How long each beep lasts (ms)
const int CAUTION_INTERVAL = 600;   // Time between beeps in CAUTION zone
const int WARNING_INTERVAL = 200;   // Time between beeps in WARNING zone

// --- LCD object ---
// >>> TRY CHANGING THIS: change 0x27 to 0x3F if screen blank
LiquidCrystal_I2C lcd(0x27, 16, 2);

// --- Non-blocking timer variables ---
unsigned long lastBeepTime = 0;   // When the last beep started
bool beepActive = false;          // Is a beep currently playing?

// ── Helper function: measure distance ──────────────────────
long measureDistanceCm() {
  // Send trigger pulse
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  // Measure echo duration
  long duration = pulseIn(ECHO_PIN, HIGH, 30000); // 30 ms timeout

  // Convert to cm using speed of sound
  long distance = duration * 0.0343 / 2;

  // Return 0 if out of range or timeout
  if (distance <= 0 || distance > 400) return 0;
  return distance;
}

void setup() {
  Serial.begin(9600);

  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  pinMode(BUZZER_PIN, OUTPUT);

  lcd.init();
  lcd.backlight();

  lcd.setCursor(0, 0);
  lcd.print("Parking Sensor");
  lcd.setCursor(0, 1);
  lcd.print("  Ready!       ");
  delay(1500);
  lcd.clear();
}

void loop() {
  // --- MEASURE distance ---
  long distance = measureDistanceCm();
  unsigned long now = millis();     // Current time in milliseconds

  // --- DETERMINE zone ---
  String zoneName;
  int beepInterval;

  if (distance == 0) {
    zoneName = "No signal";
    beepInterval = 0;               // No beep when no object detected
  } else if (distance > ZONE_SAFE) {
    zoneName = "SAFE      ";
    beepInterval = 0;               // No beep in safe zone
  } else if (distance > ZONE_CAUTION) {
    zoneName = "CAUTION   ";
    beepInterval = CAUTION_INTERVAL;
  } else if (distance > ZONE_WARNING) {
    zoneName = "WARNING!! ";
    beepInterval = WARNING_INTERVAL;
  } else {
    zoneName = "DANGER!!! ";
    beepInterval = -1;              // -1 = continuous tone
  }

  // --- UPDATE LCD ---
  lcd.setCursor(0, 0);
  if (distance == 0) {
    lcd.print("Dist: ------ ");
  } else {
    lcd.print("Dist: ");
    lcd.print(distance);
    lcd.print(" cm    ");           // Trailing spaces erase old characters
  }

  lcd.setCursor(0, 1);
  lcd.print(zoneName);

  // --- NON-BLOCKING BUZZER ---
  // DANGER: continuous tone
  if (beepInterval == -1) {
    tone(BUZZER_PIN, BEEP_FREQ);   // Continuous tone, no noTone() call
  }
  // No beep zone: make sure buzzer is silent
  else if (beepInterval == 0) {
    noTone(BUZZER_PIN);
  }
  // Beep zone: use millis() for non-blocking timing
  else {
    // Check if it's time to start a new beep
    if (!beepActive && (now - lastBeepTime >= (unsigned long)beepInterval)) {
      tone(BUZZER_PIN, BEEP_FREQ); // Start a beep
      lastBeepTime = now;
      beepActive = true;
    }
    // Check if the beep duration has expired
    if (beepActive && (now - lastBeepTime >= BEEP_DURATION)) {
      noTone(BUZZER_PIN);          // Stop the beep
      beepActive = false;
    }
  }

  // --- SERIAL MONITOR ---
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.print(" cm  Zone: ");
  Serial.println(zoneName);

  // Small delay for sensor stability
  // >>> TRY CHANGING THIS: reduce to 50 for faster response
  delay(100);
}


// ===== CHALLENGE =====
// 1. ADD LED COLOURS: Wire three LEDs (green, yellow, red) on
//    pins 4, 5, 6. Light the appropriate one for each zone:
//    green=SAFE, yellow=CAUTION, red=WARNING/DANGER.
//
// 2. DISTANCE BAR: Add a bar graph on the LCD. Use a for-loop
//    to print 0–16 block characters proportional to closeness.
//    (16 blocks = closest, 0 blocks = SAFE zone)
//    Hint: int bars = map(distance, 0, ZONE_SAFE, 16, 0);
//
// 3. TONE PITCH VARIATION: Instead of a fixed BEEP_FREQ, change
//    the frequency based on distance:
//    int freq = map(distance, 0, 100, 2000, 500);
//    tone(BUZZER_PIN, freq);
//    Closer = higher pitch. This is how some scientific sonar
//    displays work (pitch maps to depth/distance).
//
// 4. SPEED CALCULATION: If you take two distance readings
//    100 ms apart, you can estimate the speed of a moving object:
//    speed (cm/s) = (dist1 - dist2) / 0.1
//    Display the speed on the LCD. Positive = moving away,
//    negative = approaching.
```

---

## 5. Student Worksheet

### Session 25 — Distance Alarm / Parking Sensor
**Name(s):** _________________________ **Date:** _________ **Kit #:** ______

**Objective:** Build a parking-sensor system that uses the HC-SR04 and a buzzer to give graduated distance warnings, and connect it to the science of sound waves.

---

#### What I Already Know (Warm-Up)

1. Write the formula for calculating distance from an ultrasonic sensor's echo time (from Session 17):
   > Distance = ___________________________________________________

2. What is the approximate speed of sound in air at 20°C?
   > ≈ _______ m/s = _______ cm/µs

3. "Echolocation" is used by bats and dolphins. In one sentence, describe how it works.
   > ___________________________________________________________________

---

#### Build It — Step by Step

- [ ] LCD wired: GND, VCC, SDA → pin 20, SCL → pin 21
- [ ] HC-SR04: VCC → 5V, GND → GND, TRIG → pin 9, ECHO → pin 10
- [ ] Buzzer: positive → pin 8, negative → GND
- [ ] Partner check complete
- [ ] Code uploaded
- [ ] Test: slowly move hand toward sensor, beep rate increases

---

#### Predict!

1. At what distance will the buzzer first start beeping? (Check the zone constants in the code.)
   > ___________________________________________________________________

2. What will happen when you put your hand closer than 20 cm?
   > ___________________________________________________________________

3. If the speed of sound doubled (hypothetically), would the formula `duration × 0.0343 / 2` give the right distance? How would you need to change it?
   > ___________________________________________________________________

---

#### Observe / Data Table

| Distance (cm) | Zone Label (LCD) | Beep Behaviour | Notes |
|--------------|----------------|----------------|-------|
| >100 | | | |
| 80 | | | |
| 60 | | | |
| 40 | | | |
| 25 | | | |
| 10 | | | |
| Object touching sensor | | | |

---

#### What Did You Notice?

1. At exactly the zone boundary (e.g., 50 cm), did the beep rate jump suddenly or change gradually? What type of control is this called? (Hint: recall Session 23.)
   > ___________________________________________________________________

2. The code uses `millis()` instead of `delay()` for buzzer timing. In your own words, explain why using `delay()` would cause a problem.
   > ___________________________________________________________________

3. The HC-SR04 sometimes gives incorrect readings if the object is at a sharp angle. Why might an angled surface cause an error? (Think about what happens when sound bounces off an angled wall.)
   > ___________________________________________________________________

4. Compare your parking sensor to how a bat uses echolocation. List two similarities and one difference.
   > Similarity 1: ______________________________________________________
   > Similarity 2: ______________________________________________________
   > Difference: ________________________________________________________

---

#### Science Connection — Sound Waves

1. The HC-SR04 emits sound at 40,000 Hz (40 kHz). Humans can hear 20 Hz – 20,000 Hz. Why can't we hear the sensor operating?
   > ___________________________________________________________________

2. The speed of sound increases with temperature: `v ≈ 331.4 + 0.6 × T°C` metres per second.
   - At 0°C: v = _______ m/s
   - At 30°C: v = _______ m/s
   - If you used the sensor on a very cold day without adjusting the formula, would the measured distance be too large or too small? Explain.
   > ___________________________________________________________________

3. Parking sensors in cars are often arranged in a semi-circle across the rear bumper. Why use multiple sensors instead of just one? (Think about what a single sensor cannot detect.)
   > ___________________________________________________________________

---

#### Challenge Extension

1. **Colour LEDs:** Add green, yellow, and red LEDs. Light the correct one for each zone.
2. **Bar graph:** Program the LCD row 2 to show a visual bar (asterisks) that gets longer as you get closer.
3. **Speed estimate:** Take two readings 100 ms apart. Calculate and display approach speed in cm/s.

---

## 6. Safety Notes

**This session's components and hazards:**

- **Buzzer:** Passive buzzers can be **loud at close range** — keep the tone frequency at or below 1500 Hz and beep duration short (80 ms). Students sensitive to loud sounds should be seated away from the buzzer or can use noise-cancelling earbuds. Never place the buzzer directly against the ear.
- **HC-SR04:** The ultrasonic pulses are at 40 kHz — above human hearing and harmless at the power levels used. Do not disassemble the sensor; the transducer elements are fragile.
- **Buzzer polarity:** Most passive buzzers work in both orientations, but respect the marked + and − for consistency. An active buzzer wired backwards may not sound and could warm up — power down if silent and very warm.
- **Wire congestion:** This circuit has the most wires so far. Arrange wires neatly and confirm the LCD, HC-SR04, and buzzer each have their own GND and 5V connections before powering on.

**Build → Check → Power on.** With three separate modules, do a three-step partner check: (1) LCD wires, (2) HC-SR04 wires, (3) buzzer wires.

**If something goes wrong:**
- Buzzer makes continuous loud noise at startup → check if `beepInterval == -1` condition is being triggered at startup; check sensor reading is not returning 0.
- Any component warm to the touch → unplug USB, check polarity of that component.

---

## 7. Assessment Rubric

### Formative Check (not graded)

| Look-For | Not Yet | Got It |
|----------|---------|--------|
| Three components all functional: HC-SR04 reading distance, LCD displaying zone, buzzer responding | One or more components not working | All three working; distance displayed on LCD, buzzer rate changes with distance |
| Correct zone logic: 4 distinct behaviours at 4 distances | Only one or two zones trigger | All four zones (safe/caution/warning/danger) produce correct buzzer response |
| Can explain the `millis()` non-blocking approach | "I don't know why we didn't use delay()" | Explains that delay() would stop distance readings during the wait |

---

## 8. Differentiation

### Support

- **Simplified code version:** Provide an alternative version that uses `delay()` (simpler to understand) — accept that the sensor pauses during beeps. This is pedagogically fine for this session.
- **Pre-set zone constants:** Set `ZONE_SAFE = 30`, `ZONE_CAUTION = 20`, `ZONE_WARNING = 10` so the zones fit on a desk surface and are easy to test with a hand.
- **Step-by-step circuit check card:** Provide a 3-step checklist: "1. LCD working? 2. Distance reading on Serial Monitor? 3. Buzzer responding?" so students can debug module by module.

### Extension

- **millis() deep dive:** Research "Arduino millis() blink without delay" — the standard tutorial. Understand the general pattern and rewrite the buzzer timing as a reusable function.
- **Multiple sensor fusion:** Position two HC-SR04 sensors side by side (TRIG 9/ECHO 10 and TRIG 11/ECHO 12). Show the minimum of both distances — this covers a wider detection angle.
- **Doppler effect research:** Research the Doppler effect (why a passing ambulance siren changes pitch). How do police speed guns and bat sonar use the Doppler effect to measure speed as well as distance?
- **Custom alarm melody:** When the DANGER zone is entered, play a short melody (defined as an array of frequencies) using `tone()` — use the Session 16 button piano code as a reference.

### Visual / Kinesthetic Accommodations

- **Physical zone demonstration:** Mark four distances on the floor with tape (100 cm, 50 cm, 20 cm, 0 cm). Have a student hold the sensor and walk toward the teacher — the class calls out the zone as each tape line is crossed.
- **Sound wave analogy:** Use a slinky spring to demonstrate sound wave compression: compress and release one end to show how the wave travels and reflects.
- **Large LCD font:** For visually impaired students, use the `LiquidCrystal_I2C` `bigNum` library extension to display the distance as a 2×3-character large digit on the LCD.
- **Glossary card:** echolocation, ultrasonic, threshold, zone, non-blocking, millis(), beep rate, SONAR, graduated response.
