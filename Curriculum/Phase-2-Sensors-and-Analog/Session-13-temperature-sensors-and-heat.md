# Week 7, Session 13 — Temperature Sensors & Heat (DHT11/LM35)

**Phase:** Phase 2 — Sensors & Analog Signals
**Session Number:** 13 of 36
**Week:** 7

---

## Learning Objectives

By the end of this session, students will be able to:
- Explain the difference between heat and temperature using a scientific definition.
- Connect energy transfer (thermal conduction) to a measurable sensor reading.
- Wire and program a DHT11 sensor using the DHT library to read temperature and humidity.
- Print labeled temperature and humidity data to the Serial Monitor.

---

## Science Curriculum Link

**Grade 8 Concept: Heat, Thermodynamics & Energy Transfer**

Heat is a form of energy that flows from hotter objects to cooler ones (thermal conduction). **Temperature** is a measure of the average kinetic energy of molecules in a substance — it tells us *how hot* something is but not *how much energy* it contains. Students will measure both temperature and relative humidity using a DHT11, connecting the abstract idea of "molecular motion" to a number on a screen. The session also introduces the Celsius ↔ Fahrenheit conversion, reinforcing proportional reasoning from math.

---

## Materials Checklist (per pair)

- 1 × Arduino Mega 2560
- 1 × USB-A to USB-B cable
- 1 × Solderless breadboard
- 1 × DHT11 temperature & humidity sensor module (3-pin module preferred; if bare 4-pin IC, add a 10 kΩ pull-up on the data line)
- 1 × LED (any color)
- 1 × 220 Ω resistor
- Jumper wires: red × 2, black × 2, white or blue × 1 (data), orange × 1
- Computer with Arduino IDE and **DHT sensor library** installed
- (Backup) 1 × LM35 temperature sensor (3-pin TO-92) — for teacher demo or alternative

**Library required:** "DHT sensor library" by Adafruit (install via Arduino IDE → Tools → Manage Libraries → search "DHT sensor library").

---

## 2. Teacher Guide (45-Minute Breakdown)

### 0–5 min — Hook / Warm-Up

Hold a cup of hot water (carefully, in a safe container) and a cup of cold water.

*"I have two cups of water. One is hot, one is cold. If I pour them together, what temperature will the mixture be?"*

Expected answer: somewhere in between (warm). *"Why? The hot water transferred some of its heat energy to the cold water until they equalized. This is called reaching **thermal equilibrium**. The temperature we measure is how much average energy the molecules have."*

*"Today we're going to measure that energy using a sensor that reads temperature right out of the air. It's also going to measure **humidity** — the amount of water vapor in the air — because heat and water vapor are closely linked in our atmosphere."*

---

### 5–15 min — Direct Instruction

**Temperature vs Heat:**

> "Temperature and heat are NOT the same thing — this is one of the most common confusions in science."

Write on the board:
```
Temperature = average kinetic energy of molecules (measured in °C or °F or K)
Heat        = total thermal energy that flows between objects (measured in joules)
```

> "A bathtub full of warm water (30°C) contains MORE heat energy than a small cup of boiling water (100°C), even though the cup's temperature is higher — because the bathtub has vastly more molecules."

**Temperature scales:**

Write on the board:
```
°C  to  °F:   F = C × (9/5) + 32
°F  to  °C:   C = (F − 32) × (5/9)
°C  to  K:    K = C + 273.15
```

Practice: *"What is 20°C in Fahrenheit?"* → 68°F. *"What is 37°C (body temperature) in Fahrenheit?"* → 98.6°F.

**The DHT11 sensor:**

> "The DHT11 has a humidity sensor (a capacitor whose capacitance changes with moisture) and a temperature sensor (a thermistor — a resistor that changes with temperature) built into one chip. It communicates digitally using a single data wire — it sends a burst of pulses that encode the values. The Arduino library decodes those pulses for us."

> "This is a good moment to note: the LM35 is an alternative — it outputs an analog voltage directly (10 mV per °C). We'll see how they differ. The DHT11 is recommended because it also gives humidity and is easier to wire safely."

**Library installation (teacher does this before class if possible):**

> "The DHT library is not built into the Arduino IDE — it's an add-on. Go to Tools → Manage Libraries, search 'DHT sensor library', install the one by Adafruit. You only need to do this once per computer."

---

### 15–35 min — Hands-On Build + Code

**Remind students:** *"Build → Check → Power on. Unplug USB before wiring."*

1. Unplug the Arduino.
2. If using a **3-pin DHT11 module** (common breakout board): it has pins labeled VCC (+), DATA (S or OUT), GND (–). Wire directly.
3. If using a **bare 4-pin DHT11 IC**: pin 1 = VCC, pin 2 = DATA, pin 3 = NC (no connect), pin 4 = GND. Also add a 10 kΩ resistor between pin 1 (VCC) and pin 2 (DATA) as a pull-up.
4. **VCC** → red wire → 5V rail.
5. **GND** → black wire → GND rail.
6. **DATA** → white or blue wire → **Arduino digital pin 2**.
7. Wire LED + 220 Ω resistor from **Arduino pin 9** to GND (long leg to pin 9, short leg to GND).
8. Partner check all wires. Plug in USB.
9. Open Arduino IDE. Confirm DHT library is installed (File → Examples — should see DHT11 folder).
10. Enter and upload the code from Section 4.
11. Open Serial Monitor (9600 baud). Temperature and humidity readings should appear.
12. **Warm the sensor:** Hold it gently between your fingers. Does the temperature rise?
13. **Breathe on the sensor:** Does the humidity rise?

**LM35 alternative (if no DHT11):**
- LM35: pin 1 = VCC (5V), pin 2 = OUTPUT (to A0), pin 3 = GND.
- `float temp = (analogRead(A0) / 1023.0) * 5000.0 / 10.0;` — gives temperature in °C.
- No library needed. Teacher note: LM35 wired backwards gets very hot instantly — stress correct polarity before students touch it.

---

### 35–42 min — Testing & Debugging

**What success looks like:**
- Serial Monitor prints temperature (roughly 18–28°C for most classrooms) and humidity (20–70% RH) every 2 seconds.
- Temperature rises slightly when sensor is held between fingers.
- Humidity rises slightly when you breathe on it.
- LED blinks twice when a successful reading is obtained (visual confirmation in code).

**Troubleshooting Table:**

| Symptom | Fix |
|---------|-----|
| "Failed to read from DHT sensor!" | Check DATA wire is connected to pin 2 (not another pin). Check VCC is 5V (not 3.3V). |
| Serial Monitor shows nothing | Check baud rate = 9600. Check `Serial.begin(9600)` in setup. |
| DHT library not found ("no such file") | Go to Tools → Manage Libraries → install "DHT sensor library" by Adafruit. |
| Temperature reads 0.00°C every time | Wrong pin number in `DHT dht(pin, DHT11)` — check it matches your DATA wire. |
| Values are NaN (Not a Number) | Data wire may need a 10 kΩ pull-up resistor (if using bare IC, not module). |
| Temperature seems way too high | Check the sensor is DHT11 not DHT22 (they have different calibrations) — change `DHT11` to `DHT22` in code if needed. |

---

### 42–45 min — Reflection / Exit Ticket

**Exit ticket questions:**

1. *"What is the difference between heat and temperature? Use the cup and bathtub example."*
2. *"Convert 25°C (room temperature) to Fahrenheit and to Kelvin. Show your work."*
3. *"Why do we need a library to use the DHT11, but not for `analogRead()`?"*

---

## 3. Circuit Diagram

### ASCII Wiring Diagram

```
   Arduino Mega 2560
   ┌──────────────────────────┐
   │                          │
   │  5V ─────────────────────┼──── Red rail (+)
   │  GND ────────────────────┼──── Black rail (–)
   │                          │
   │  Digital Pin 2 ──────────┼──── DHT11 DATA pin (white wire)
   │                          │
   │  Pin 9 (~) ──────────────┼──── [220 Ω] ──── LED (+) ──── LED (–) ──── GND
   └──────────────────────────┘
   
   DHT11 Module (3-pin):
   
   [ + / VCC ]── Red ──── 5V rail
   [ S / DATA ]── White ── Arduino Pin 2
   [ – / GND  ]── Black ── GND rail
   
   DHT11 bare IC (4-pin, left to right facing front):
   Pin 1 (VCC) ── Red ──── 5V rail
                           │
                          [10 kΩ pull-up]
                           │
   Pin 2 (DATA) ── White ── Arduino Pin 2
   Pin 3 (NC)  ── Not connected
   Pin 4 (GND) ── Black ── GND rail
```

### Fritzing-Style Build Description

| Step | From | To | Wire Color | Notes |
|------|------|----|------------|-------|
| 1 | Arduino **5V** | Breadboard **+ rail** | Red | Power |
| 2 | Arduino **GND** | Breadboard **– rail** | Black | Ground |
| 3 | DHT11 **VCC (+)** | Breadboard **+ rail** | Red | Sensor power |
| 4 | DHT11 **GND (–)** | Breadboard **– rail** | Black | Sensor ground |
| 5 | DHT11 **DATA (S)** | Arduino **Digital Pin 2** | White | Data signal |
| 6 | *(bare IC only)* 10 kΩ pull-up between **VCC** and **DATA** | — | — | Pull-up for bare IC |
| 7 | Arduino **Pin 9** | 220 Ω resistor (row 20) | Orange | LED signal |
| 8 | 220 Ω resistor leg 2 | LED **long leg** (row 22) | — | |
| 9 | LED **short leg** | Breadboard **– rail** | Black | |

**Component values:** DHT11 (module or bare IC), 220 Ω (red-red-brown-gold), 10 kΩ (brown-black-orange-gold, bare IC only).

[Google image search: "DHT11 sensor Arduino Mega wiring breadboard"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 * SESSION 13: Temperature Sensors & Heat (DHT11)
 * Arduino Mega 2560 — Grade 8 STEM Curriculum
 * ============================================================
 *
 * SCIENCE EXPLANATION:
 * ------------------------------------------------------------------
 * TEMPERATURE vs HEAT:
 *   Temperature = average kinetic energy of molecules (°C / °F / K)
 *   Heat        = thermal energy flowing between objects (Joules)
 *
 * The DHT11 sensor contains:
 *   1. A THERMISTOR — a resistor that changes resistance with temp.
 *   2. A HUMIDITY CAPACITOR — a capacitor whose capacitance changes
 *      with water vapor in the air.
 *
 * The chip converts these readings to digital data and sends them
 * in a timed pulse sequence on a single wire. The Adafruit DHT
 * library decodes that sequence for us.
 *
 * Temperature scales:
 *   Celsius (°C) ← most of the world, science
 *   Fahrenheit (°F) = C × 9/5 + 32
 *   Kelvin (K) = C + 273.15  ← used in science (absolute zero = 0 K)
 *
 * Humidity: % relative humidity (how full the air is with water
 * vapor vs its maximum capacity at that temperature).
 * ------------------------------------------------------------------
 *
 * LIBRARY REQUIRED: "DHT sensor library" by Adafruit
 *   Install: Arduino IDE → Tools → Manage Libraries → search "DHT sensor library"
 *
 * Circuit:  DHT11 VCC → 5V, GND → GND, DATA → Pin 2
 *           LED + 220 Ω on pin 9 (blinks on successful read)
 * ============================================================
 */

#include <DHT.h>   // Include the DHT sensor library (must be installed first!)

// ---- Pin and Sensor Definitions ----
const int DHT_PIN  = 2;      // DHT11 DATA wire connected to digital pin 2
const int LED_PIN  = 9;      // LED for visual confirmation of reading

// Tell the library which pin and which sensor type we're using
// Change DHT11 to DHT22 if you have the DHT22 sensor instead
DHT dht(DHT_PIN, DHT11);    // Create a DHT sensor object

void setup() {
  Serial.begin(9600);         // Open serial communication at 9600 baud
  dht.begin();                // Initialize the DHT sensor
  pinMode(LED_PIN, OUTPUT);   // LED as output

  Serial.println("=== Session 13: DHT11 Temperature & Humidity ===");
  Serial.println("Reading every 2 seconds...");
  Serial.println("-------------------------------------------------");
}

void loop() {
  // --- Wait 2 seconds between readings ---
  // DHT11 needs at least 1 second between reads — 2 seconds is safe
  delay(2000);

  // --- Read humidity and temperature ---
  float humidity    = dht.readHumidity();      // Returns % relative humidity
  float tempCelsius = dht.readTemperature();   // Returns degrees Celsius

  // --- Check if reading was successful ---
  // If the sensor fails, dht.read functions return NaN (Not a Number)
  if (isnan(humidity) || isnan(tempCelsius)) {
    Serial.println("ERROR: Failed to read from DHT sensor!");
    Serial.println("  → Check wiring: VCC=5V, DATA=pin 2, GND=GND");
    return;   // Skip the rest of this loop cycle, try again next time
  }

  // --- Convert Celsius to Fahrenheit ---
  float tempFahrenheit = tempCelsius * 9.0 / 5.0 + 32.0;

  // --- Convert Celsius to Kelvin ---
  float tempKelvin = tempCelsius + 273.15;

  // --- Calculate heat index (feels-like temperature) ---
  // The library can calculate this — it accounts for humidity making it "feel" hotter
  float heatIndex = dht.computeHeatIndex(tempCelsius, humidity, false);
  // (false = return Celsius; use true for Fahrenheit)

  // --- Blink LED twice to show successful reading ---
  for (int i = 0; i < 2; i++) {
    digitalWrite(LED_PIN, HIGH);
    delay(50);
    digitalWrite(LED_PIN, LOW);
    delay(50);
  }

  // --- Print all data to Serial Monitor ---
  Serial.println("---------- New Reading ----------");
  Serial.print("Temperature: ");
  Serial.print(tempCelsius, 1);      // 1 decimal place
  Serial.print(" °C  |  ");
  Serial.print(tempFahrenheit, 1);
  Serial.print(" °F  |  ");
  Serial.print(tempKelvin, 1);
  Serial.println(" K");

  Serial.print("Humidity:    ");
  Serial.print(humidity, 1);
  Serial.println(" %RH");

  Serial.print("Heat Index:  ");
  Serial.print(heatIndex, 1);
  Serial.println(" °C  (feels like)");
  Serial.println();

  // >>> TRY CHANGING THIS: Change the delay from 2000 to 5000 (every 5 seconds)
  // or to 1000 (every second, the minimum the DHT11 supports)
}

// ===== CHALLENGE =====
// 1. TEMPERATURE ALERT: Add an if statement that prints "WARNING: HOT!"
//    if the temperature reads above 30°C, or "COLD!" below 15°C.
//
// 2. THERMOMETER LED: Wire 3 LEDs (green, yellow, red) on pins 9, 10, 11.
//    Green lights up below 20°C. Yellow from 20–28°C. Red above 28°C.
//
// 3. UNIT CONVERTER: Print all three scales (°C, °F, K) as a labeled table
//    with columns aligned using Serial.print("  ") for spacing.
//
// 4. LM35 ALTERNATIVE (no library needed):
//    Replace DHT code with:
//    int raw = analogRead(A0);
//    float volts = (raw / 1023.0) * 5000.0;  // millivolts
//    float tempC = volts / 10.0;             // LM35: 10mV per °C
//    Serial.println(tempC);
```

---

## 5. Student Worksheet

---

### Session 13 Worksheet — Temperature Sensors & Heat

**Name(s):** _________________________________ **Date:** _____________ **Kit #:** _____

**Objectives:**
- Explain the difference between temperature and heat.
- Wire and read a DHT11 sensor.
- Convert temperature between Celsius, Fahrenheit, and Kelvin.

---

#### What I Already Know (Warm-Up)

1. In everyday language, people say "the sun gives off a lot of heat." In science, what word do we use to describe the type of energy that transfers from the sun to Earth?

   ___________________________________________________________________________

2. Water freezes at _______ °C. Your body temperature is about _______ °C.

3. If `analogRead()` gives us a number from 0 to 1023, do you think a temperature sensor works the same way? Why or why not?

   ___________________________________________________________________________

---

#### Build It — Step by Step

Unplug USB before wiring.

1. Check that the DHT library is installed: **Tools → Manage Libraries → search "DHT sensor library" → install by Adafruit**.
2. Place the DHT11 module in the breadboard.
3. **VCC (+)** → red wire → **5V rail**.
4. **GND (–)** → black wire → **GND rail**.
5. **DATA (S)** → white wire → **Arduino digital pin 2**.
6. LED (long leg) → via 220 Ω resistor → **Arduino pin 9**. LED (short leg) → **GND rail**.
7. Partner checks wiring. Plug in USB.
8. Type and upload the code. Open **Serial Monitor** at **9600 baud**.
9. Readings should appear every 2 seconds.

---

#### Predict!

Before experimenting:

| Experiment | Prediction: temperature goes UP / DOWN / NO CHANGE? | Humidity goes UP / DOWN / NO CHANGE? |
|------------|-----------------------------------------------------|--------------------------------------|
| Hold sensor between your fingers (body heat) | | |
| Breathe gently on the sensor | | |
| Hold sensor near a window on a cold day | | |
| Nothing (just sitting on the desk) | | |

---

#### Observe / Data Table

Record readings during each experiment. Each cell needs temperature (°C) and humidity (%):

| Time / Condition | Temperature (°C) | Humidity (%RH) | Notes |
|------------------|------------------|----------------|-------|
| Start (baseline) | | | |
| After 2 min (just sitting) | | | |
| Holding sensor (30 sec) | | | |
| After breathing on sensor | | | |
| After cooling / fanning | | | |

---

#### Temperature Conversion Practice

Fill in the blanks using the formulas:
- **°F = °C × (9/5) + 32**
- **K = °C + 273.15**

| °C | °F | K |
|----|----|---|
| 0 (water freezes) | | |
| 100 (water boils) | | |
| 37 (body temp) | | |
| Your classroom reading | | |
| −40 (same in °C and °F!) | | |

---

#### What Did You Notice?

1. Did the temperature increase when you held the sensor? By how many degrees?

   ___________________________________________________________________________

2. Did the humidity increase when you breathed on the sensor? By how many percent?

   ___________________________________________________________________________

3. What is "relative humidity"? In your own words, what does 50% RH mean?

   ___________________________________________________________________________

4. The DHT11 requires a library. What does the library do that we couldn't easily do ourselves?

   ___________________________________________________________________________

---

#### Science Connection

Heat flows from hotter objects to cooler objects (Second Law of Thermodynamics — energy spreads out). When you held the sensor, heat flowed from your fingers (warmer) to the sensor (cooler) until they were close to the same temperature.

**Question:** If you held the sensor in a freezer for 1 minute and then immediately brought it into the warm room, which direction would heat flow? How long do you think it would take the sensor to read room temperature again?

___________________________________________________________________________

**Kelvin Scale:** Scientists use Kelvin because 0 K (absolute zero, −273.15°C) is the point where molecules stop moving entirely. There is no such thing as a negative Kelvin temperature. Why is an absolute scale like Kelvin useful for science calculations?

___________________________________________________________________________

---

#### Challenge Extension

Try the **Thermometer LED** challenge from the code:
1. Wire 3 LEDs: green on pin 9, yellow on pin 10, red on pin 11. (Each with its own 220 Ω resistor.)
2. Modify the code so:
   - Green lights up when temp < 20°C
   - Yellow lights up when temp is 20–28°C
   - Red lights up when temp > 28°C
3. Test by holding the sensor to warm/cool it.

Sketch your circuit here: (Draw the LED + resistor connections)

---

## 6. Safety Notes

| Hazard | Precaution |
|--------|------------|
| DHT11 polarity | CRITICAL: Connect VCC, DATA, and GND to the correct pins. Module boards are labeled. For bare ICs, check the datasheet pinout. Mis-wired power can damage the chip. |
| LM35 backwards wiring | If using the LM35, connecting it backwards causes it to get **very hot very fast**. Power down immediately if any component feels warm. |
| LED polarity | Long leg = anode → toward pin 9. Short leg = cathode → toward GND. |
| Sensor handling | Handle by the edges. The sensing element on top of the DHT11 is fragile — do not poke it. |

**Build → Check → Power on.**

**If something goes wrong:**
- Sensor is warm to touch → wrong polarity! Unplug USB immediately. Wait 30 seconds before touching.
- Code prints "Failed to read" → check DATA pin and VCC connection.
- Values are stuck / NaN → try unplugging and replugging USB.

---

## 7. Assessment Rubric

### Formative Check (not graded)

| Look-For | Not Yet | Got It |
|----------|---------|--------|
| Student correctly installs and includes the DHT library | | |
| Serial Monitor shows valid temperature and humidity readings (not NaN) | | |
| Student can convert a temperature from °C to °F without a calculator error | | |
| Student can distinguish between "heat" and "temperature" using an example | | |
| Temperature changes when sensor is warmed by hand | | |

---

## 8. Differentiation

### Support
- Provide a printed DHT11 pinout diagram at every desk. Students often confuse module (3-pin) vs bare IC (4-pin) pinouts.
- Pre-install the DHT library on all classroom computers before the session — library installation can consume 10+ minutes if done during class.
- Provide a temperature conversion calculator app or a pre-filled conversion table — the goal is sensor reading, not arithmetic.
- Give a sentence frame: *"The DHT11 measures ________ and ________, which are related because ________."*

### Extension
- Implement the full 3-LED thermometer (Challenge 2 in code).
- Write a function `void printAllTemps(float c)` that accepts Celsius and prints all three scales — practice with functions from Phase 1.
- Research: *"What is the 'heat index' and why does high humidity make the same temperature feel hotter?"* (The DHT library can calculate it — it's already in the code!)
- Compare DHT11 vs LM35: wire the LM35 alternative and calculate temperature using `analogRead()` and the 10 mV/°C formula. Compare accuracy with the DHT11.

### Visual / Kinesthetic Accommodations
- Use the Serial Plotter (Tools → Serial Plotter) to graph temperature in real time while holding the sensor — the rising curve is a powerful visual.
- For the heat/temperature concept: use the cup demo physically — have students hold one hand in warm water and one hand in cold water, then put both in room-temperature water. Which feels warm? Which feels cool? This is exactly why temperature is relative.
- Print a Celsius/Fahrenheit/Kelvin comparison poster. Color-code common temperatures (body temp, room temp, freezing, boiling) on all three scales.
