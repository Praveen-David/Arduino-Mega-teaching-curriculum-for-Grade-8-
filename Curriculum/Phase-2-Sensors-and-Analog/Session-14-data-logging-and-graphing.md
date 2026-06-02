# Week 7, Session 14 — Data Logging & Graphing Temperature

**Phase:** Phase 2 — Sensors & Analog Signals
**Session Number:** 14 of 36
**Week:** 7

---

## Learning Objectives

By the end of this session, students will be able to:
- Collect time-stamped temperature data systematically using the Serial Monitor.
- Use the Arduino IDE Serial Plotter to visualize a live sensor data graph.
- Record data in a table and create a hand-drawn graph with correctly labeled axes.
- Identify trends, patterns, and anomalies in their data and connect them to real events.

---

## Science Curriculum Link

**Grade 8 Concept: Data Collection, Graphing & the Scientific Method**

Science is not just about collecting data — it is about recording, displaying, and interpreting data to draw conclusions. This session puts students through a complete cycle of the **scientific method**: question → hypothesis → experiment → data table → graph → analysis → conclusion. The skill of graphing a time-series (data vs time) is a cornerstone of both science and mathematics at this level, and using a real sensor grounds the graph in authentic measurements rather than hypothetical numbers.

---

## Materials Checklist (per pair)

- 1 × Arduino Mega 2560
- 1 × USB-A to USB-B cable
- 1 × Solderless breadboard
- 1 × DHT11 sensor module (same as Session 13)
- 1 × LED (any color)
- 1 × 220 Ω resistor
- Jumper wires: red × 2, black × 2, white × 1
- Computer with Arduino IDE and DHT library installed
- Printed graph paper (or draw axes on regular paper) — 1 sheet per student
- Ruler and pencil

---

## 2. Teacher Guide (45-Minute Breakdown)

### 0–5 min — Hook / Warm-Up

Display a photo of a weather station chart or a temperature graph over 24 hours (find one via a weather website or the Board). Ask:

*"Scientists at a weather station record the temperature every 10 minutes — all day, every day, for years. Why? What can they learn from that data that a single temperature reading can't tell them?"*

Expected answers: trends, patterns (hot in afternoon, cold at night), changes over seasons, predicting weather, climate data.

*"Today we're going to become weather scientists. You'll log temperature data every 15 seconds for 5 minutes, record it in a table, draw a graph, and find the story your data tells. Let's see if anything interesting happens in your classroom microclimate."*

---

### 5–15 min — Direct Instruction

**What is data logging?**

> "Data logging means recording sensor readings over time so you can analyze patterns. Every data point needs: a **time stamp** (when it was measured), and the **value** (what was measured). Without the time stamp, your data is just a list of numbers — you can't see how things change."

**Parts of a graph (quick review):**

Draw a blank graph on the board:

```
  °C
   |
28 |
27 |
26 |
25 |
24 |
   |___________________________
   0   1   2   3   4   5   min
```

> "Every graph needs: a **title**, labeled **axes** (with units!), an **appropriate scale** that fits your data without wasting space, and **data points** connected by a smooth line or dots. The x-axis (horizontal) = time. The y-axis (vertical) = temperature."

**Reading the Serial Plotter:**

> "The Arduino IDE has a powerful tool called the Serial Plotter. Instead of showing numbers, it automatically graphs anything your Serial.println() sends. It's like having a live graph — but it only keeps the last few hundred points and you can't save it. That's why we'll also record data by hand."

**Setting up the timed data log:**

> "We'll use a timer based on `millis()` — the number of milliseconds since the Arduino turned on. This way, the Arduino prints a reading every 15 seconds automatically. You write down each value in your data table."

---

### 15–35 min — Hands-On Build + Code

**The circuit is the same as Session 13.** Students should keep their DHT11 build unchanged.

1. Verify the DHT11 is still wired: VCC → 5V, GND → GND, DATA → pin 2.
2. Enter and upload the data-logging code from Section 4.
3. Open the **Serial Plotter** (Tools → Serial Plotter). Set the Serial Plotter baud to 9600 if prompted.
4. Watch the graph build for 1–2 minutes to get a feel for the baseline.
5. Close the Serial Plotter. Open the **Serial Monitor** (9600 baud) instead.
6. **Start the experiment.** One student reads the data from the Serial Monitor every 15 seconds (or copy the auto-printed lines). The other student records each reading in the data table on the worksheet.
7. During the 5-minute logging window, perform the following experiments at marked times:
   - **Minute 1–2:** Baseline (no change, just sitting on desk).
   - **Minute 2.5:** One student holds the sensor between their fingers.
   - **Minute 3.5:** Release the sensor and fan it gently with a piece of paper.
   - **Minute 4.5:** Breathe gently on the sensor.
8. After 5 minutes (~20 data points), stop recording.
9. Use the recorded data to draw a graph by hand on graph paper.
10. Answer the reflection questions on the worksheet.

---

### 35–42 min — Testing & Debugging

**What success looks like:**
- Serial Monitor prints a reading every 15 seconds with a time stamp in seconds.
- Data shows a clear rise when sensor is held and a slight drop when fanned.
- Hand-drawn graph has title, labeled axes with units, correct scale, and plotted data points.

**Troubleshooting Table:**

| Symptom | Fix |
|---------|-----|
| Serial Plotter shows flat line at 0 | Sensor may be reading NaN — check wiring. Serial Plotter interprets NaN as 0. |
| Multiple lines on Serial Plotter | Each `Serial.println()` call creates a new line. Use only one `Serial.println(tempCelsius)` for the plotter. |
| Readings seem wrong (too high/low) | Check DHT11 type — change `DHT11` to `DHT22` in code if using DHT22 sensor. |
| Time stamp is not in seconds | The code divides `millis()` by 1000 — check this line. |
| Graph looks strange | Check that students recorded the data in order (time increases left to right). |

---

### 42–45 min — Reflection / Exit Ticket

**Exit ticket questions:**

1. *"Look at your graph. Identify one moment where the temperature clearly changed. What caused it? How can you tell from the graph?"*
2. *"Why is a graph more useful than just a list of numbers?"*
3. *"What is one source of error in your temperature data today? How would you reduce it?"*

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
   │  Digital Pin 2 ──────────┼──── DHT11 DATA (white)
   │                          │
   │  Pin 9 (~) ──────────────┼──── [220 Ω] ──── LED (+) ──── LED (–) ──── GND
   └──────────────────────────┘
   
   DHT11 Module:
   
   [ VCC + ] ── Red ──── 5V rail
   [ DATA S ] ── White ── Arduino Pin 2
   [ GND  – ] ── Black ── GND rail
```

*(Identical circuit to Session 13 — no rewiring needed.)*

### Fritzing-Style Build Description

Same as Session 13 — no changes to the circuit.

| Step | From | To | Wire Color | Notes |
|------|------|----|------------|-------|
| 1 | Arduino **5V** | Breadboard **+ rail** | Red | Power |
| 2 | Arduino **GND** | Breadboard **– rail** | Black | Ground |
| 3 | DHT11 **VCC** | Breadboard **+ rail** | Red | Sensor power |
| 4 | DHT11 **GND** | Breadboard **– rail** | Black | Sensor ground |
| 5 | DHT11 **DATA** | Arduino **Pin 2** | White | Data |
| 6 | Arduino **Pin 9** | 220 Ω → LED **long leg** | Orange | LED confirmation |
| 7 | LED **short leg** | Breadboard **– rail** | Black | |

[Google image search: "Arduino Serial Plotter temperature data logging DHT11"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 * SESSION 14: Data Logging & Graphing Temperature
 * Arduino Mega 2560 — Grade 8 STEM Curriculum
 * ============================================================
 *
 * SCIENCE EXPLANATION:
 * ------------------------------------------------------------------
 * DATA LOGGING = systematically recording measurements over time.
 *
 * Every real scientific instrument logs data over time:
 *   - Weather stations log temperature every 10 minutes.
 *   - Seismographs record ground vibration continuously.
 *   - Medical devices log heart rate every second.
 *
 * KEY CONCEPT: A time-series graph (value vs time) shows TRENDS
 * and PATTERNS that a single reading cannot reveal.
 *
 * This sketch uses millis() — the Arduino's built-in timer.
 * millis() returns how many milliseconds have passed since
 * the Arduino was powered on. We use it to print a reading
 * at regular intervals WITHOUT using delay() — so the Arduino
 * keeps running while waiting for the next reading time.
 *
 * For the Serial Plotter to draw a clean graph, we print
 * ONLY the numeric value (no labels) when in plotter mode.
 * Switch between modes using the PLOTTER_MODE constant.
 * ------------------------------------------------------------------
 *
 * LIBRARY REQUIRED: "DHT sensor library" by Adafruit
 *
 * Circuit:  DHT11 → Pin 2 (same as Session 13)
 *           LED + 220 Ω on pin 9
 * ============================================================
 */

#include <DHT.h>   // Adafruit DHT sensor library

// ---- Pin and Sensor Setup ----
const int DHT_PIN = 2;         // DHT11 DATA pin
const int LED_PIN = 9;         // Confirmation LED

DHT dht(DHT_PIN, DHT11);       // Create DHT sensor object

// ---- Timing Settings ----
// >>> TRY CHANGING THIS: change 15000 to 5000 for faster logging (every 5 sec)
const unsigned long LOG_INTERVAL = 15000;  // Log every 15 seconds (15,000 ms)

unsigned long lastLogTime = 0;   // Tracks when we last printed a reading

// ---- Mode Selection ----
// Set to true to use Serial Plotter (clean numbers only)
// Set to false for Serial Monitor (labeled, human-readable)
// >>> TRY CHANGING THIS: swap between true and false to switch modes
const bool PLOTTER_MODE = false;

// ---- Reading counter ----
int readingNumber = 0;

void setup() {
  Serial.begin(9600);          // Open serial communication
  dht.begin();                 // Initialize DHT sensor
  pinMode(LED_PIN, OUTPUT);

  if (!PLOTTER_MODE) {
    Serial.println("=== Session 14: Temperature Data Logger ===");
    Serial.println("Recording every 15 seconds. Fill in the data table!");
    Serial.println("--------------------------------------------");
    Serial.println("Reading# | Time(s) | Temp(C) | Humidity(%) | Event");
    Serial.println("--------------------------------------------");
  }
}

void loop() {
  unsigned long currentTime = millis();   // Get current time in milliseconds

  // --- Check if it's time for a new reading ---
  // This is a NON-BLOCKING timer: the Arduino is not paused, just checking
  if (currentTime - lastLogTime >= LOG_INTERVAL) {
    lastLogTime = currentTime;    // Reset the timer

    // --- Take readings ---
    float humidity    = dht.readHumidity();
    float tempCelsius = dht.readTemperature();

    // --- Check for sensor error ---
    if (isnan(humidity) || isnan(tempCelsius)) {
      Serial.println("ERROR: Sensor read failed. Check wiring.");
      return;
    }

    readingNumber++;   // Count up each successful reading

    // --- Calculate time in seconds since start ---
    unsigned long timeSeconds = currentTime / 1000;

    // --- Blink LED to show a reading was taken ---
    digitalWrite(LED_PIN, HIGH);
    delay(100);
    digitalWrite(LED_PIN, LOW);

    if (PLOTTER_MODE) {
      // --- Serial Plotter mode: print ONLY the number ---
      // (Serial Plotter graphs each println as a data point)
      Serial.println(tempCelsius);

    } else {
      // --- Serial Monitor mode: print labeled, formatted data ---
      Serial.print(readingNumber);
      Serial.print("       | ");
      Serial.print(timeSeconds);
      Serial.print("      | ");
      Serial.print(tempCelsius, 1);
      Serial.print("    | ");
      Serial.print(humidity, 1);
      Serial.println("          | <-- write event here on your worksheet");
    }
  }

  // Note: no delay() here! The Arduino keeps looping as fast as possible.
  // The millis() check above makes it log only at the right interval.
}

// ===== CHALLENGE =====
// 1. DUAL VARIABLE PLOTTER: In PLOTTER_MODE, print BOTH temperature
//    and humidity on the same graph using:
//    Serial.print(tempCelsius);
//    Serial.print(",");
//    Serial.println(humidity);
//    The Serial Plotter will show two lines!
//
// 2. LOG COUNT ALERT: Print an extra message after every 10 readings.
//    Use: if (readingNumber % 10 == 0) { Serial.println("--- 10 readings logged! ---"); }
//
// 3. FASTER LOGGING: Change LOG_INTERVAL to 1000 (1 second) and
//    calculate the heat index at each step. Log temp, humidity, AND
//    heat index as three columns. What is the typical difference between
//    actual temp and heat index in your classroom?
//
// 4. STATISTICS: After 20 readings, compute and print the average temperature.
//    You'll need to add up all readings in an array:
//    float readings[20]; — store each reading; after 20, use a for loop to sum.
```

---

## 5. Student Worksheet

---

### Session 14 Worksheet — Data Logging & Graphing Temperature

**Name(s):** _________________________________ **Date:** _____________ **Kit #:** _____

**Objectives:**
- Collect temperature data over time using a DHT11 and the Serial Monitor.
- Record data in a table and draw a correctly labeled time-series graph.
- Identify patterns and anomalies in your data.

---

#### What I Already Know (Warm-Up)

1. From Session 13: What does the DHT11 sensor measure?

   ___________________________________________________________________________

2. You are reading a temperature graph and the line goes sharply up for 1 minute, then comes back down. What might have caused that "spike"?

   ___________________________________________________________________________

3. A scientist wants to graph "Temperature over 24 hours." What goes on the x-axis? What goes on the y-axis?

   ___________________________________________________________________________

---

#### Build It — Step by Step

The circuit is the same as Session 13. Verify it is still working:

1. Check: DHT11 VCC → 5V, DATA → pin 2, GND → GND.
2. Upload the Session 14 code (make sure `PLOTTER_MODE = false` for table recording).
3. Open **Serial Monitor** at **9600 baud**.
4. Wait for the header row to appear.
5. **START THE EXPERIMENT.** Readings appear every 15 seconds.
6. Record each reading in the data table below. Also write down what was happening at that moment (Event column).

---

#### Observe / Data Table

Write down each reading as it appears on the Serial Monitor:

| Reading # | Time (s) | Temperature (°C) | Humidity (%RH) | Event / What you were doing |
|-----------|----------|------------------|----------------|------------------------------|
| 1 | | | | Baseline — just sitting |
| 2 | | | | Baseline |
| 3 | | | | Baseline |
| 4 | | | | Hold sensor with fingers |
| 5 | | | | Hold sensor with fingers |
| 6 | | | | Hold sensor with fingers |
| 7 | | | | Release — fan gently |
| 8 | | | | Fan gently |
| 9 | | | | Breathe gently on sensor |
| 10 | | | | Breathe gently on sensor |
| 11 | | | | Cool / fan |
| 12 | | | | Returning to baseline |
| 13 | | | | |
| 14 | | | | |
| 15 | | | | |
| 16 | | | | |
| 17 | | | | |
| 18 | | | | |
| 19 | | | | |
| 20 | | | | |

---

#### Draw Your Graph

Use the graph paper provided (or the grid below). Plot **Temperature (°C) vs Time (seconds)**.

Requirements for full credit:
- [ ] Title: "Temperature Log — [Date] — [Your name(s)]"
- [ ] X-axis label: "Time (seconds)"
- [ ] Y-axis label: "Temperature (°C)"
- [ ] Appropriate scale (does NOT need to start at 0 — pick a range that shows detail, e.g., 18°C to 32°C)
- [ ] Each data point marked with a dot (•)
- [ ] Data points connected by a smooth line
- [ ] Events annotated with small arrows where the experiment changed (e.g., "grabbed sensor here")

*(Grid placeholder — students use separate graph paper)*

```
  °C
  34 |
  32 |
  30 |
  28 |
  26 |
  24 |
  22 |
  20 |
     |_____|_____|_____|_____|_____|_____|_____|_____|_____|_____|___
     0    30    60    90   120   150   180   210   240   270   300  sec
```

---

#### Predict!

Before Step 4 (hold sensor), predict:

- What temperature do you think the sensor will reach after 45 seconds of being held? ______ °C
- How long will it take to cool back to baseline? ______ seconds

---

#### What Did You Notice?

1. What was your baseline temperature (average of first 3 readings)? ______ °C

2. What was the highest temperature the sensor reached when held? ______ °C. Change from baseline: ______ °C

3. Did the humidity change when you breathed on the sensor? By how much?

   ___________________________________________________________________________

4. Look at your graph. Is the line smooth or jagged? What might cause jagged temperature data?

   ___________________________________________________________________________

---

#### Science Connection

**Trends** are gradual changes over time (e.g., temperature slowly rising). **Anomalies** are unexpected sudden changes. In real climate science, distinguishing trends from anomalies is critical.

**Question:** If you logged temperature every 15 seconds for an entire school day (6 hours = 1440 readings), what trend might you see? What anomalies might appear?

___________________________________________________________________________

**Graphing choice:** Why did scientists choose time for the x-axis (horizontal) and not the y-axis? What convention makes data easier to read?

___________________________________________________________________________

---

#### Challenge Extension

1. Switch to `PLOTTER_MODE = true` in the code. Take a screenshot of the Serial Plotter graph. Paste it in your lab notebook or describe its shape here.

2. Use the **dual variable plotter** from the Challenge code section to plot both temperature and humidity on the same graph. What pattern do you notice between the two variables?

3. **Prediction test:** Use your first 5 data points to predict what the 10th data point will be. Were you right? What does this say about how predictable temperature change is?

---

## 6. Safety Notes

| Hazard | Precaution |
|--------|------------|
| Same circuit as Session 13 | All Session 13 safety rules apply. Check DHT11 polarity before powering on. |
| Long USB sessions | The Arduino may be plugged in for the full 5-minute logging window. This is fine — USB power is safe. Ensure cables are not a trip hazard. |
| Breathing on sensors | Breathing gently on the sensor is safe. Do not spray water or hold a drink near the sensor. |

**Build → Check → Power on.**

---

## 7. Assessment Rubric

### Formative Check (not graded)

| Look-For | Not Yet | Got It |
|----------|---------|--------|
| Data table has at least 15 readings with time stamps and event notes | | |
| Graph has title, both axes labeled with units, correct scale | | |
| Data points are plotted correctly and connected | | |
| Student can identify at least one trend and one anomaly in their graph | | |
| Student connects the graphing exercise to the scientific method (hypothesis/observation/conclusion) | | |

---

## 8. Differentiation

### Support
- Provide a pre-formatted data table (printed) so students only need to fill in numbers — reduces cognitive load during fast-paced logging.
- Pre-draw the graph axes at an appropriate scale and label the y-axis in 2°C increments — students only need to plot points.
- Provide a checklist for the graph requirements (title, axes labels, units, scale, dots, line, annotations).
- Assign one student as "recorder" and one as "reader" during logging — clear role division helps pairs work efficiently.

### Extension
- Implement Challenge 4 (statistics): students calculate the mean and range of their temperature data and add these to the Serial Monitor output.
- Introduce the concept of **outlier detection**: if any single reading differs by more than 3°C from the previous one, print "POSSIBLE OUTLIER" on the Serial Monitor.
- Ask students to design and run their own 5-minute experiment: *"What variable will you change? What do you predict? How will you control other variables?"* — a full mini scientific method exercise.
- Compare their hand-drawn graph with the Serial Plotter screenshot. Are they the same shape? What are the advantages of each method of data presentation?

### Visual / Kinesthetic Accommodations
- The Serial Plotter is the primary visual accommodation — it shows a live graph without any manual plotting.
- For students who struggle with graphing scales, provide pre-marked graph paper with a y-axis already scaled from 18°C to 35°C.
- Kinesthetic: have students physically stand up and hold the sensor at arm's length, then close to the body — a physical representation of the experiment's variable (proximity = heat source).
- Allow students to annotate directly on a printed screenshot of their Serial Plotter graph as an alternative to a hand-drawn graph.
