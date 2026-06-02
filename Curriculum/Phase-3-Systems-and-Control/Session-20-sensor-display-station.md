# Week 10, Session 20 — Building a Sensor Display Station

**Phase:** Phase 3 — Systems & Control
**Week:** 10 | **Session:** 20 of 36

---

## Learning Objectives

By the end of this session, students will be able to:
- Combine a sensor (DHT11 temperature or HC-SR04 ultrasonic) with a 16×2 I2C LCD to build a live-reading display station.
- Describe the complete **input → process → output** flow of an integrated system.
- Update the LCD display in real time inside `loop()` and use `lcd.clear()` or cursor positioning to avoid display artifacts.
- Interpret live sensor data shown on the LCD and explain why formatting matters for human readability.

---

**Science Curriculum Link:** Integrating systems — input, process, output (Grade 8 Systems & Technology / Science of Technology).
A real instrument (thermometer, radar gun, weather station) always has three stages: a **sensor** that detects a physical quantity (input), a **processor** that converts the raw signal to a meaningful value (process), and a **display** that communicates the result to a user (output). Today students build all three stages in one project.

---

## Materials Checklist (per pair)

- [ ] 1 × Arduino Mega 2560 + USB cable
- [ ] 1 × Solderless breadboard
- [ ] 1 × 16×2 LCD with I2C backpack (already tested in Session 19)
- [ ] 4 × M-F jumper wires (for LCD: red, black, blue, yellow)
- [ ] **Option A — Temperature station:**
  - 1 × DHT11 sensor module (3-pin module with built-in resistor) OR bare DHT11 + 10 kΩ pull-up resistor
  - 3 × M-M jumper wires for DHT11
  - `DHT sensor library` installed in Arduino IDE
- [ ] **Option B — Distance station:**
  - 1 × HC-SR04 ultrasonic sensor
  - 4 × M-F jumper wires for HC-SR04
  - (no extra library needed)
- [ ] Computer with Arduino IDE

> **Teacher note:** Choose one option for the whole class OR let pairs choose. Both produce the same pedagogical result. The temperature station is slightly simpler to wire. Instructions below cover both; code files are separate.

---

## 2. Teacher Guide (45-minute breakdown)

### 0–5 min — Hook / Warm-Up

**Display on the projector (or draw on the board) two versions of a thermometer readout:**

```
Version A (raw):  724
Version B (nice): Temp: 23.5 C
```

**Ask:** "Which display is more useful? Why? What extra work did the Arduino have to do to produce Version B?"

*Expected answers:* Version B tells you the unit, what it is measuring, and is formatted for a human. The Arduino had to read the sensor, do a calculation, format the string, and send it to the display — that's input → process → output.

**Bridge:** "Today you are building a real instrument — something that reads a sensor every second and tells you what it measured on an LCD screen, just like a lab thermometer or a parking sensor display."

---

### 5–15 min — Direct Instruction

**Concept: Input → Process → Output as a system**

*Words to say:*
"Every measuring instrument has the same three-stage structure. Let's map it out:

- **INPUT:** The DHT11 senses temperature and humidity. The HC-SR04 sends ultrasound and listens for the echo. Either way, a physical quantity enters the system.
- **PROCESS:** The Arduino reads the raw sensor value, applies a formula (or calls a library function), and converts it into a meaningful number — degrees Celsius, centimetres, etc.
- **OUTPUT:** The LCD shows that number in a readable format, including the unit and a label.

The challenge in output design is **formatting**. If we just call `lcd.print(distance)` each loop, leftover digits from a larger previous number will stay on screen. For example, if the reading was '100 cm' and then '9 cm', the display might show '900 cm' — the '9' overwrote the '1', but '00' stayed behind.

We fix this in two ways:
1. `lcd.clear()` — erases everything and starts fresh. (Drawback: causes a brief flicker.)
2. `lcd.setCursor()` + printing spaces — overwrites specific positions without clearing the whole screen. (Better for fast updates.)

Today we will use `lcd.clear()` with a 500 ms delay, which is fine for once-per-second updates."

**Optional analogy:** "Think of the LCD like a whiteboard. `lcd.clear()` is like erasing the whole board before writing again. `lcd.setCursor()` + spaces is like erasing just one word."

---

### 15–35 min — Hands-On Build + Code

**Remind students:** Build → Check → Power on. Unplug USB before wiring.

#### Option A — Temperature Display Station

**Wiring steps:**

1. LCD is already wired from Session 19 (check: SDA → pin 20, SCL → pin 21, VCC → 5V, GND → GND).
2. Place DHT11 module on breadboard, or insert the bare DHT11 component into breadboard (flat face forward).
   - **DHT11 module (3-pin):** has built-in resistor. Pins labeled GND / DATA / VCC (or – / out / +).
   - **Bare DHT11 (4-pin):** pins are VCC / DATA / NC / GND left to right (flat face forward). Add a 10 kΩ resistor between DATA and VCC.
3. Connect DHT11 **VCC** → Arduino **5V** (red wire).
4. Connect DHT11 **GND** → Arduino **GND** (black wire).
5. Connect DHT11 **DATA** → Arduino **digital pin 2** (green wire).
6. Partner check all wires. Then plug in USB and upload Option A code.

#### Option B — Distance Display Station

**Wiring steps:**

1. LCD wired from Session 19 (same checks as above).
2. Place HC-SR04 on breadboard or hold with M-F wires directly.
3. Connect HC-SR04 **VCC** → Arduino **5V** (red wire).
4. Connect HC-SR04 **GND** → Arduino **GND** (black wire).
5. Connect HC-SR04 **TRIG** → Arduino **digital pin 9** (green wire).
6. Connect HC-SR04 **ECHO** → Arduino **digital pin 10** (orange wire).
7. Partner check all wires. Upload Option B code.

---

### 35–42 min — Testing & Debugging

**What success looks like (Option A):** LCD row 1 shows "Temp: XX.X C" and row 2 shows "Humidity: XX %" updating approximately every second.

**What success looks like (Option B):** LCD row 1 shows "Distance:" and row 2 shows "XX cm" updating in real time as a hand is moved toward/away from the sensor.

**Troubleshooting table:**

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| LCD blank (no text, backlight on) | Wrong I2C address | Change `0x27` to `0x3F` or vice versa; adjust contrast pot |
| "nan" or "-999" on LCD | DHT11 not wired correctly / wrong pin | Check DATA → pin 2; check power polarity |
| Distance reads 0 or 999 | TRIG/ECHO swapped, or HC-SR04 not powered | Check TRIG → 9, ECHO → 10; check 5V and GND |
| Display flickers badly | `lcd.clear()` too fast | Increase delay between updates to 500 ms |
| Old digits stay on screen | Missing `lcd.clear()` or not enough spaces | Add `lcd.clear()` or print trailing spaces |
| DHT library error | Library not installed | Sketch → Include Library → Manage → DHT sensor library |

---

### 42–45 min — Reflection / Exit Ticket

Students write on a sticky note:

1. "Draw a simple three-box diagram with arrows labeling INPUT, PROCESS, and OUTPUT for your station."
2. "Why might a scientist prefer a display that updates every second rather than one that updates every millisecond? (Think: what problems could too-fast updates cause?)"

---

## 3. Circuit Diagram

### Option A — Temperature Display Station

```
  ARDUINO MEGA 2560
  ┌──────────────────────────────────────────────┐
  │  pin 20 (SDA) ────────────────────────────────┼──► [blue]   ──► LCD SDA
  │  pin 21 (SCL) ────────────────────────────────┼──► [yellow] ──► LCD SCL
  │  5V ──────────────────────────────────────────┼──► [red]    ──► LCD VCC
  │  GND ─────────────────────────────────────────┼──► [black]  ──► LCD GND
  │                                               │
  │  5V ──────────────────────────────────────────┼──► [red]    ──► DHT11 VCC
  │  GND ─────────────────────────────────────────┼──► [black]  ──► DHT11 GND
  │  pin 2 (digital) ─────────────────────────────┼──► [green]  ──► DHT11 DATA
  └──────────────────────────────────────────────┘

  DHT11 MODULE       LCD I2C
  ┌──────────┐       ┌──────────────────────┐
  │ VCC DATA │       │  Row 1: Temp: 23.5 C │
  │  GND     │       │  Row 2: Humidity:55% │
  └──────────┘       └──────────────────────┘
```

### Option B — Distance Display Station

```
  ARDUINO MEGA 2560
  ┌──────────────────────────────────────────────┐
  │  pin 20 (SDA) ────────────────────────────────┼──► [blue]   ──► LCD SDA
  │  pin 21 (SCL) ────────────────────────────────┼──► [yellow] ──► LCD SCL
  │  5V ──────────────────────────────────────────┼──► [red]    ──► LCD VCC
  │  GND ─────────────────────────────────────────┼──► [black]  ──► LCD GND
  │                                               │
  │  5V ──────────────────────────────────────────┼──► [red]    ──► HC-SR04 VCC
  │  GND ─────────────────────────────────────────┼──► [black]  ──► HC-SR04 GND
  │  pin 9  (digital) ────────────────────────────┼──► [green]  ──► HC-SR04 TRIG
  │  pin 10 (digital) ────────────────────────────┼──► [orange] ──► HC-SR04 ECHO
  └──────────────────────────────────────────────┘
```

**Fritzing-style build description:**

| # | From | To | Wire Color | Notes |
|---|------|----|-----------|-------|
| 1 | Arduino GND | LCD backpack GND | Black | |
| 2 | Arduino 5V | LCD backpack VCC | Red | |
| 3 | Arduino pin 20 | LCD backpack SDA | Blue | Mega I2C data |
| 4 | Arduino pin 21 | LCD backpack SCL | Yellow | Mega I2C clock |
| 5A | Arduino GND | DHT11 GND | Black | Option A only |
| 6A | Arduino 5V | DHT11 VCC | Red | Option A only |
| 7A | Arduino pin 2 | DHT11 DATA | Green | Option A only |
| 5B | Arduino GND | HC-SR04 GND | Black | Option B only |
| 6B | Arduino 5V | HC-SR04 VCC | Red | Option B only |
| 7B | Arduino pin 9 | HC-SR04 TRIG | Green | Option B only |
| 8B | Arduino pin 10 | HC-SR04 ECHO | Orange | Option B only |

[Google image search: "Arduino Mega DHT11 LCD I2C display temperature humidity wiring"]

---

## 4. Arduino Code

### Option A — Temperature & Humidity Display

```cpp
/*
 * ============================================================
 *  Session 20 — Sensor Display Station (Option A: Temperature)
 *  Arduino Mega Teaching Curriculum — Phase 3
 * ============================================================
 *
 *  SCIENCE EXPLANATION:
 *  ─────────────────────────────────────────────────────────
 *  This sketch completes an INPUT → PROCESS → OUTPUT chain:
 *
 *  INPUT:   DHT11 sensor detects temperature and humidity.
 *           It uses a single-wire protocol to send a digital
 *           value — no analog conversion needed.
 *
 *  PROCESS: The DHT library decodes the serial data from the
 *           sensor and returns temperature in °C and relative
 *           humidity as a percentage.
 *
 *  OUTPUT:  The LiquidCrystal_I2C library sends the formatted
 *           values to the LCD, where liquid crystals rotate
 *           to show characters.
 *
 *  Relative humidity = the amount of water vapor in air as a
 *  percentage of the maximum it COULD hold at that temperature.
 *  100% = saturated (fog/rain); 30–60% = comfortable indoors.
 * ============================================================
 *
 *  HARDWARE:
 *    DHT11 DATA → pin 2
 *    LCD SDA    → pin 20  (Mega I2C)
 *    LCD SCL    → pin 21  (Mega I2C)
 * ============================================================
 */

#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <DHT.h>              // Install: "DHT sensor library" by Adafruit

// --- Pin & sensor type definitions ---
#define DHT_PIN   2           // DHT11 DATA wire connected to digital pin 2
#define DHT_TYPE  DHT11       // We are using the DHT11 model

// Create the LCD object: address 0x27, 16 columns, 2 rows
// >>> TRY CHANGING THIS: if screen is blank, change 0x27 to 0x3F
LiquidCrystal_I2C lcd(0x27, 16, 2);

// Create the DHT sensor object
DHT dht(DHT_PIN, DHT_TYPE);

void setup() {
  // Initialize Serial Monitor for backup debugging
  Serial.begin(9600);

  // Start communicating with the DHT11 sensor
  dht.begin();

  // Initialize the LCD
  lcd.init();
  lcd.backlight();

  // Show a startup message for 2 seconds
  lcd.setCursor(0, 0);
  lcd.print("Sensor Station");
  lcd.setCursor(0, 1);
  lcd.print("Warming up...");
  delay(2000);                // Give DHT11 time to stabilize
  lcd.clear();
}

void loop() {
  // Read temperature in Celsius from the DHT11
  float tempC = dht.readTemperature();

  // Read relative humidity as a percentage
  float humidity = dht.readHumidity();

  // Check if the reading failed (returns NaN = "Not a Number")
  if (isnan(tempC) || isnan(humidity)) {
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Sensor error!");
    lcd.setCursor(0, 1);
    lcd.print("Check wiring");
    delay(2000);
    return;                   // Skip the rest of this loop and try again
  }

  // Also print to Serial Monitor for debugging
  Serial.print("Temp: ");
  Serial.print(tempC);
  Serial.print(" C  |  Humidity: ");
  Serial.print(humidity);
  Serial.println(" %");

  // --- Update the LCD ---
  // Clear the screen first to avoid leftover characters
  lcd.clear();

  // Row 0: Temperature
  lcd.setCursor(0, 0);        // Column 0, Row 0
  lcd.print("Temp: ");
  lcd.print(tempC, 1);        // Print with 1 decimal place
  lcd.print(" C");            // >>> TRY CHANGING THIS: add the degree symbol with char(223)

  // Row 1: Humidity
  lcd.setCursor(0, 1);        // Column 0, Row 1
  lcd.print("Humidity:");
  lcd.print(humidity, 0);     // Print with 0 decimal places (whole number)
  lcd.print("%");

  // Wait before the next reading
  // >>> TRY CHANGING THIS: change 1000 to 500 (faster) or 3000 (slower)
  delay(1000);
}


// ===== CHALLENGE =====
// 1. HEAT INDEX: Calculate the "feels like" temperature using the
//    dht.computeHeatIndex(tempC, humidity, false) function and
//    display it on a third line (you'll need to scroll or alternate).
//
// 2. TEMPERATURE ALERT: Add an if-statement: if tempC > 30, print
//    "TOO HOT!" on row 1 instead of the humidity reading.
//
// 3. FAHRENHEIT OPTION: Add a button on pin 4. When pressed, switch
//    the display between Celsius and Fahrenheit.
//    Formula: float tempF = (tempC * 9.0 / 5.0) + 32.0;
//
// 4. AVERAGE: Keep a rolling average of the last 5 readings using
//    an array and display the average instead of the raw reading.
```

---

### Option B — Distance Display Station

```cpp
/*
 * ============================================================
 *  Session 20 — Sensor Display Station (Option B: Distance)
 *  Arduino Mega Teaching Curriculum — Phase 3
 * ============================================================
 *
 *  SCIENCE EXPLANATION:
 *  ─────────────────────────────────────────────────────────
 *  INPUT:   HC-SR04 emits a 40 kHz ultrasound pulse and times
 *           the echo. Speed of sound ≈ 343 m/s at 20°C.
 *           Distance = (echo time × speed of sound) / 2
 *
 *  PROCESS: The Arduino measures the echo pulse width in
 *           microseconds, then applies the formula above.
 *
 *  OUTPUT:  The distance in cm is formatted and displayed on
 *           the LCD, updating every 500 ms.
 *
 *  This is exactly how a car parking sensor works! The beep
 *  rate gets faster as the object gets closer.
 * ============================================================
 *
 *  HARDWARE:
 *    HC-SR04 TRIG → pin 9
 *    HC-SR04 ECHO → pin 10
 *    LCD SDA      → pin 20
 *    LCD SCL      → pin 21
 * ============================================================
 */

#include <Wire.h>
#include <LiquidCrystal_I2C.h>

// --- Pin definitions ---
const int TRIG_PIN = 9;       // Trigger: sends ultrasound pulse
const int ECHO_PIN = 10;      // Echo: receives the reflected pulse

// Create LCD object
// >>> TRY CHANGING THIS: change 0x27 to 0x3F if screen stays blank
LiquidCrystal_I2C lcd(0x27, 16, 2);

void setup() {
  Serial.begin(9600);

  // Set up HC-SR04 pins
  pinMode(TRIG_PIN, OUTPUT);  // TRIG sends a pulse out
  pinMode(ECHO_PIN, INPUT);   // ECHO listens for the return pulse

  // Initialize LCD
  lcd.init();
  lcd.backlight();

  // Startup message
  lcd.setCursor(0, 0);
  lcd.print("Distance Sensor");
  lcd.setCursor(0, 1);
  lcd.print("Ready!");
  delay(1500);
  lcd.clear();
}

long measureDistanceCm() {
  // Step 1: Make sure TRIG is LOW first (clean start)
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);

  // Step 2: Send a 10-microsecond HIGH pulse to trigger the sensor
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  // Step 3: Measure how long the ECHO pin stays HIGH
  // This duration is the round-trip time of the sound pulse
  long duration = pulseIn(ECHO_PIN, HIGH);

  // Step 4: Calculate distance
  // Speed of sound = 343 m/s = 0.0343 cm/µs
  // Distance = (duration * 0.0343) / 2  (divide by 2: round trip)
  long distance = duration * 0.0343 / 2;

  return distance;
}

void loop() {
  // Measure the distance
  long distanceCm = measureDistanceCm();

  // Print to Serial Monitor for debugging
  Serial.print("Distance: ");
  Serial.print(distanceCm);
  Serial.println(" cm");

  // --- Update the LCD ---
  lcd.clear();

  // Row 0: Label
  lcd.setCursor(0, 0);
  lcd.print("Distance:");

  // Row 1: Value
  lcd.setCursor(0, 1);

  // Handle out-of-range readings
  if (distanceCm < 2 || distanceCm > 400) {
    lcd.print("Out of range");
  } else {
    lcd.print(distanceCm);
    lcd.print(" cm");
    // >>> TRY CHANGING THIS: also print " (");
    // and add a bar-graph of asterisks showing relative distance
  }

  // Update twice per second
  // >>> TRY CHANGING THIS: change 500 to 100 for faster updates
  delay(500);
}


// ===== CHALLENGE =====
// 1. PROGRESS BAR: Use a for-loop to print 0–16 asterisks (*) on
//    row 1 proportional to the distance. This is a visual bar graph!
//    int bars = map(distanceCm, 2, 100, 0, 16);
//    for (int i = 0; i < bars; i++) { lcd.print("*"); }
//
// 2. CATEGORIES: Add if/else to print "CLOSE", "MEDIUM", or "FAR"
//    depending on the distance range.
//
// 3. ADD BUZZER: Connect a buzzer to pin 8. Make it beep faster
//    as the distance decreases (like a parking sensor).
//    This is a preview of Session 25!
```

---

## 5. Student Worksheet

### Session 20 — Building a Sensor Display Station
**Name(s):** _________________________ **Date:** _________ **Kit #:** ______
**Option chosen:** ☐ A (Temperature)   ☐ B (Distance)

**Objective:** Build a complete input → process → output system that reads a sensor and displays live data on an LCD.

---

#### What I Already Know (Warm-Up)

1. What are the three stages of an information system? Fill in the blanks:
   > _____________ → _____________ → _____________

2. In Session 17 (HC-SR04), you learned the formula for distance using ultrasound. Write it here from memory:
   > Distance = _________________________________________________

3. In Session 19 you learned the two I2C pin numbers on the Mega. What are they?
   > SDA = pin ____    SCL = pin ____

---

#### Build It — Step by Step

- [ ] LCD wiring verified from Session 19 (SDA → 20, SCL → 21, 5V, GND)
- [ ] **Option A:** DHT11 wired: VCC → 5V, GND → GND, DATA → pin 2
- [ ] **Option B:** HC-SR04 wired: VCC → 5V, GND → GND, TRIG → pin 9, ECHO → pin 10
- [ ] Partner check complete
- [ ] Code uploaded successfully
- [ ] LCD shows live readings

---

#### Predict!

Before uploading, answer these:

1. (Option A) If the room temperature is about 22°C, what do you expect to see on the LCD?
   > ___________________________________________________________________

2. (Option B) If you hold your hand 15 cm from the sensor, what reading do you expect?
   > ___________________________________________________________________

3. What do you think will happen if you change `delay(1000)` to `delay(50)` (very fast updates)?
   > ___________________________________________________________________

---

#### Observe / Data Table

##### Option A — Temperature Readings

| Measurement | Temperature (°C) | Humidity (%) | Notes (where / conditions) |
|------------|-----------------|-------------|--------------------------|
| 1 — Room air | | | |
| 2 — Breathe on sensor | | | |
| 3 — Near a window | | | |
| 4 — In your hands | | | |

##### Option B — Distance Readings

| Object Placed | Estimated Distance | Measured Distance | Difference |
|--------------|-------------------|------------------|-----------|
| Your hand | _______ cm | | |
| A book | _______ cm | | |
| The wall | _______ cm | | |
| Your partner's hand | _______ cm | | |

---

#### What Did You Notice?

1. Did the LCD display "flicker" when updating? What caused it and how could you reduce it?
   > ___________________________________________________________________

2. (Option A) What happened to the humidity reading when you breathed on the sensor? Why?
   > ___________________________________________________________________
   (Option B) Was the LCD reading accurate compared to your estimate? What could cause small errors?
   > ___________________________________________________________________

3. What happened when you changed the `delay()` value to something very small? Was the display more or less readable?
   > ___________________________________________________________________

4. Draw the input → process → output diagram for YOUR station. Label each box with the specific component doing that job.

   ```
   [          ] → [          ] → [          ]
     INPUT           PROCESS        OUTPUT
   ```

---

#### Science Connection

1. A weather station at an airport reads temperature, pressure, and humidity every 10 seconds and displays them on a screen for air traffic controllers. Which part of that station is like your sensor? Which part is like your Arduino? Which part is like your LCD?
   > ___________________________________________________________________

2. Why is it important that a display updates regularly rather than just once? Give a real-world example where an outdated reading could be dangerous.
   > ___________________________________________________________________

3. The `lcd.clear()` command flickers the screen briefly. A professional instrument like a hospital monitor never flickers. How do you think engineers solve this problem? (Hint: think about rewriting only the part of the display that changed.)
   > ___________________________________________________________________

---

#### Challenge Extension

1. **Dual sensor:** If you built Option A (temperature), add the HC-SR04 as well and alternate between showing temperature and distance every 3 seconds.

2. **Min/Max tracking:** Keep track of the highest and lowest reading you have seen since reset. Display "Hi: XX Lo: XX" on row 2.

3. **Unit conversion:** (Option A) Add a button; when held, show the temperature in Fahrenheit. (Option B) Add a button; when held, show the distance in inches (1 cm ≈ 0.394 in).

---

## 6. Safety Notes

**Components in this session:**

- **DHT11 (Option A):** The DHT11 module has a built-in resistor; the bare sensor needs a 10 kΩ pull-up. Wiring the bare DHT11 with VCC and GND reversed will make it warm — power down immediately and re-check polarity.
- **HC-SR04 (Option B):** The sensor emits 40 kHz ultrasound — harmless to humans, but do not stare into the sensor expecting to see anything. Ensure TRIG and ECHO are on the correct pins; reversed wires will give garbage readings (not a safety hazard, but confusing).
- **LCD Display:** Handle the glass panel gently. Do not press the screen surface. Always adjust contrast with the power on but keyboard/wrist kept away from other pins.
- **Multiple 5V connections:** Both the sensor and the LCD draw from the Mega's 5V pin. The Mega can supply approximately 400 mA from 5V — adding these two components is well within limit, but avoid connecting many more 5V devices without an external supply.

**Build → Check → Power on:** Complete all wiring before plugging in USB. Have your partner verify every connection before powering on.

**If something goes wrong:**
- "Sensor error!" on LCD → unplug USB, check DATA/ECHO/TRIG wiring and polarity.
- Any component gets warm → unplug USB immediately, call the teacher.

---

## 7. Assessment Rubric

### Formative Check (not graded)

| Look-For | Not Yet | Got It |
|----------|---------|--------|
| Circuit wired correctly with sensor and LCD both connected and functional | One or both components wired wrong or not displaying | Both sensor and LCD work; readings appear on screen |
| Code modified from template (changed at least delay or label) | Unchanged starter code | At least one variable, label, or delay customized |
| Can verbally explain input → process → output for their station | Describes only one stage or uses incorrect terms | Names the correct component for each stage and explains what "process" does |

---

## 8. Differentiation

### Support

- **Pre-wired option:** Provide a partially built circuit with LCD already connected. Student only needs to add sensor (3 or 4 wires).
- **Starter code scaffold:** Provide the code with `// FILL IN` blanks at the `lcd.print()` lines so students choose their own labels and units.
- **Flowchart scaffold:** Give a printed blank flowchart template labeled INPUT, PROCESS, OUTPUT, DECISION, OUTPUT — students fill in the component names.
- **Sentence starters:** "My INPUT component is ___. It measures ___. The Arduino PROCESSES this by ___. The OUTPUT is shown by ___."

### Extension

- **Dual sensor display:** Integrate both a DHT11 and HC-SR04 simultaneously and alternate between screens.
- **Graphical bar:** Replace the numerical distance with a bar made of custom LCD block characters to create a visual meter.
- **Alert thresholds:** Add a red LED that turns on when temperature exceeds a set value OR distance drops below 10 cm — a sneak preview of feedback control.
- **Data logging:** Simultaneously send readings to the Serial Monitor in CSV format (value, time) so they can be copied into a spreadsheet and graphed.

### Visual / Kinesthetic Accommodations

- **Flowchart poster:** A large laminated INPUT → PROCESS → OUTPUT poster on the wall that students can point to while explaining their system.
- **Hands-on analogy:** Pass a physical thermometer around and have students read it, then compare the experience to reading the LCD — both are output stages.
- **Large-print wiring card:** A printed color photo of the complete correct circuit for each option.
- **Glossary card:** transducer, sensor, actuator, I2C, input, process, output, real-time, update rate.
