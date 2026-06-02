# Week 12, Session 24 — Build a Thermostat

**Phase:** Phase 3 — Systems & Control
**Week:** 12 | **Session:** 24 of 36

---

## Learning Objectives

By the end of this session, students will be able to:
- Wire a DHT11 (or LM35) temperature sensor, 16×2 I2C LCD, and two LEDs together on the Arduino Mega to form a working thermostat.
- Explain the role of **setpoint** and **hysteresis** in preventing rapid on/off switching ("hunting").
- Connect the thermostat's negative-feedback loop to the concept of thermodynamic equilibrium.
- Read, modify, and explain the thermostat code including the conditional logic for heater/cooler LED indicators.

---

**Science Curriculum Link:** Feedback loops and thermodynamics (Grade 8 Physics — Heat & Thermodynamics; Grade 8 Biology — Homeostasis).
A thermostat is the classic real-world example of a negative-feedback control system. It connects thermodynamics (heat flows from hot to cool until equilibrium) with control systems (the thermostat counters natural heat loss or gain to maintain a target temperature). This mirrors the body's temperature regulation covered in Session 23.

---

## Materials Checklist (per pair)

- [ ] 1 × Arduino Mega 2560 + USB cable
- [ ] 1 × Solderless breadboard
- [ ] 1 × 16×2 LCD with I2C backpack (SDA → pin 20, SCL → pin 21)
- [ ] 4 × M-F jumper wires for LCD (red, black, blue, yellow)
- [ ] 1 × DHT11 module (preferred) OR bare DHT11 + 10 kΩ pull-up, OR LM35
- [ ] 3 × M-M jumper wires for temperature sensor
- [ ] 1 × Red LED (represents "heater ON")
- [ ] 1 × Blue LED (represents "cooler ON") — use any colour if blue unavailable
- [ ] 2 × 220 Ω resistors (one per LED)
- [ ] Several M-M jumper wires (various colours)
- [ ] Computer with Arduino IDE (`DHT sensor library` installed)

---

## 2. Teacher Guide (45-minute breakdown)

### 0–5 min — Hook / Warm-Up

**Ask:** "Your home thermostat is set to 20°C. When the room reaches exactly 20°C, does it turn the heater OFF immediately? What do you think would happen if it did?"

*Expected answers:* The heater would turn off and on many times per minute because the room temperature would hover right at 20°C. That would be inefficient and would wear out the heater.

**Follow-up:** "What do you think a real thermostat actually does? Does it turn on at exactly 20°C, or does it give the temperature some 'room to wander'?"

*Bridge to hysteresis:* "Real thermostats use a band — for example, heating turns ON at 19°C and turns OFF at 21°C. That 2°C gap is called 'hysteresis.' Today you are going to program exactly that."

---

### 5–15 min — Direct Instruction

**Concept: Thermostat as a closed-loop feedback system with hysteresis**

*Words to say:*
"A thermostat works like this:

1. **Setpoint** — the temperature you want (e.g., 22°C).
2. **Sensor** — the thermometer reads the actual temperature.
3. **Comparator** — is actual above or below setpoint?
4. **Controller** — if too cold, turn on the 'heater'; if too hot, turn on the 'cooler'.
5. **Actuator** — in real buildings: a boiler or air conditioner. In our model: a red LED ('heater') or blue LED ('cooler').
6. **Feedback** — the new temperature is measured again a few seconds later, and the cycle repeats.

**Hysteresis:** Without it, the heater turns on at 22.01°C and off at 21.99°C — a very unstable situation. With hysteresis of 1°C:
- Heater turns ON when temp drops below **21°C** (setpoint − 1).
- Heater turns OFF when temp rises above **23°C** (setpoint + 1).

This creates a *dead zone* between 21°C and 23°C where nothing changes. The room wanders freely in that zone.

**LCD display plan:**
- Row 0: `Temp: 22.5C  [OK]`   (or `[HEAT]` or `[COOL]`)
- Row 1: `Set: 22C  H:1C`

**Thermodynamics connection:** Second Law of Thermodynamics tells us heat always flows from hot to cold. A room in winter naturally loses heat to the outside. The thermostat counters this natural flow by adding heat when needed. Without the thermostat, the room would eventually reach thermal equilibrium with the outside temperature."

Write the pseudocode on the board:
```
every 2 seconds:
    read temperature from DHT11
    display on LCD
    if temperature < (setpoint - hysteresis):
        turn RED LED on  ("heating")
        turn BLUE LED off
    else if temperature > (setpoint + hysteresis):
        turn RED LED off
        turn BLUE LED on  ("cooling")
    else:
        turn BOTH LEDs off ("OK zone")
```

---

### 15–35 min — Hands-On Build + Code

**Remind students:** Build → Check → Power on.

**Building the circuit (numbered steps):**

1. **LCD** (from Session 19): GND → GND, VCC → 5V, SDA → pin 20, SCL → pin 21. Verify first.
2. **DHT11 module**: VCC → 5V (red), GND → GND (black), DATA → **pin 2** (green).
3. **Red LED (heater indicator)**:
   - Pin 7 → 220 Ω resistor → LED anode (long leg)
   - LED cathode (short leg) → GND (black)
4. **Blue LED (cooler indicator)**:
   - Pin 6 → 220 Ω resistor → LED anode (long leg)
   - LED cathode (short leg) → GND (black)
5. Partner check all wires. Use the wiring table below to verify.
6. Upload code. Observe LCD and LEDs.

**Testing the setpoint:** The DHT11 typically reads room temperature (~18–25°C). To trigger the "HEAT" LED, the student must lower the setpoint below the current room temperature. To trigger the "COOL" LED, they raise the setpoint above room temperature.

---

### 35–42 min — Testing & Debugging

**What success looks like:**
- LCD shows current temperature and "OK", "HEAT", or "COOL" status.
- Red LED ON when temperature is below (setpoint − hysteresis).
- Blue LED ON when temperature is above (setpoint + hysteresis).
- Both LEDs OFF when temperature is in the OK zone.

**Troubleshooting table:**

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| LCD blank | Wrong I2C address or SDA/SCL wires swapped | Try 0x3F; check pins 20 and 21 |
| "nan" or "0.00" on LCD | DHT11 DATA wrong pin or bad pull-up | Check DATA → pin 2; add 10 kΩ pull-up if using bare DHT11 |
| Red LED always on / always off | Setpoint not appropriate for room temperature | Read actual temp from LCD, adjust SETPOINT in code |
| Both LEDs flicker | Hysteresis too small / temp fluctuates | Increase HYSTERESIS from 1 to 2 |
| Blue LED never comes on | Room temp always below setpoint + hysteresis | Set SETPOINT to 5 (below room temp) to test cooler path |

---

### 42–45 min — Reflection / Exit Ticket

Students write on a sticky note:

1. "What is the purpose of hysteresis in a thermostat? What goes wrong without it?"
2. "In your thermostat code, what value is the 'error'? Write the expression."
3. "If you could add a real heater output to this system, what component might you use instead of a red LED to actually heat something? (Hint: something from Phase 2 or everyday life.)"

---

## 3. Circuit Diagram

```
  ARDUINO MEGA 2560
  ┌──────────────────────────────────────────────────────────┐
  │  pin 20 (SDA) ─────────────────────────────────────────── ┼──► [blue]   ──► LCD SDA
  │  pin 21 (SCL) ─────────────────────────────────────────── ┼──► [yellow] ──► LCD SCL
  │  5V ──────────────────────────────────────────────────────┼──► [red]    ──► LCD VCC
  │  GND ─────────────────────────────────────────────────────┼──► [black]  ──► LCD GND
  │                                                           │
  │  pin 2 (digital) ─────────────────────────────────────────┼──► [green]  ──► DHT11 DATA
  │  5V ──────────────────────────────────────────────────────┼──► [red]    ──► DHT11 VCC
  │  GND ─────────────────────────────────────────────────────┼──► [black]  ──► DHT11 GND
  │                                                           │
  │  pin 7 (digital) ─────────────────────────────────────────┼──► [red]    ──► 220Ω ──► RED LED (+)
  │  GND ─────────────────────────────────────────────────────┼──► [black]  ──► RED LED (−)
  │                                                           │
  │  pin 6 (digital) ─────────────────────────────────────────┼──► [blue]   ──► 220Ω ──► BLUE LED (+)
  │  GND ─────────────────────────────────────────────────────┼──► [black]  ──► BLUE LED (−)
  └──────────────────────────────────────────────────────────┘
```

**Fritzing-style build description:**

| # | From | To | Wire Color | Notes |
|---|------|----|-----------|-------|
| 1 | Arduino GND | LCD backpack GND | Black | |
| 2 | Arduino 5V | LCD backpack VCC | Red | |
| 3 | Arduino pin 20 | LCD backpack SDA | Blue | Mega I2C data |
| 4 | Arduino pin 21 | LCD backpack SCL | Yellow | Mega I2C clock |
| 5 | Arduino GND | DHT11 GND | Black | |
| 6 | Arduino 5V | DHT11 VCC | Red | |
| 7 | Arduino pin 2 | DHT11 DATA | Green | |
| 8 | Arduino pin 7 | 220 Ω resistor (end A) | Red | Heater LED signal |
| 9 | 220 Ω resistor (end B) | Red LED anode (+, long leg) | Red | |
| 10 | Red LED cathode (−, short leg) | Arduino GND | Black | |
| 11 | Arduino pin 6 | 220 Ω resistor (end A) | Blue | Cooler LED signal |
| 12 | 220 Ω resistor (end B) | Blue LED anode (+, long leg) | Blue | |
| 13 | Blue LED cathode (−, short leg) | Arduino GND | Black | |

[Google image search: "Arduino thermostat DHT11 LCD LED heater cooler indicator circuit"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 *  Session 24 — Build a Thermostat
 *  Arduino Mega Teaching Curriculum — Phase 3
 * ============================================================
 *
 *  SCIENCE EXPLANATION:
 *  ─────────────────────────────────────────────────────────
 *  This is a complete THERMOSTAT: a negative-feedback control
 *  system that maintains temperature near a setpoint.
 *
 *  THERMODYNAMICS CONNECTION:
 *  - Second Law: heat flows spontaneously from hot → cold.
 *  - In winter, a room loses heat to the outside.
 *  - The thermostat detects this drop and activates a heater
 *    to counteract it — fighting the natural thermodynamic flow.
 *  - This is why "maintaining" temperature requires energy input.
 *
 *  HYSTERESIS (the dead zone):
 *  Without hysteresis, the heater would switch on/off 
 *  thousands of times as temperature hovers at the setpoint.
 *  With hysteresis:
 *    - Heater turns ON  only when temp < setpoint − hysteresis
 *    - Heater turns OFF only when temp > setpoint + hysteresis
 *  This means the temperature is allowed to wander in a band.
 *  Real home thermostats typically use 0.5°C–2°C hysteresis.
 *
 *  COMPONENTS:
 *    DHT11:  temperature/humidity sensor (INPUT)
 *    Arduino: comparator + controller (PROCESS)
 *    LEDs:   heater/cooler indicators (OUTPUT/ACTUATOR)
 *    LCD:    status display (OUTPUT)
 * ============================================================
 *
 *  HARDWARE:
 *    DHT11 DATA → pin 2
 *    Red LED    → pin 7 (via 220 Ω)   "Heater ON"
 *    Blue LED   → pin 6 (via 220 Ω)   "Cooler ON"
 *    LCD SDA    → pin 20 (Mega I2C)
 *    LCD SCL    → pin 21 (Mega I2C)
 * ============================================================
 */

#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <DHT.h>

// --- Hardware definitions ---
#define DHT_PIN    2          // DHT11 data wire
#define DHT_TYPE   DHT11      // Sensor model

const int HEATER_LED = 7;     // Red LED = "heater on"
const int COOLER_LED = 6;     // Blue LED = "cooler on"

// --- Thermostat parameters ---
// SETPOINT: the target temperature in Celsius
// >>> TRY CHANGING THIS: set it to 2 degrees above or below your room temp
//     to test the HEAT or COOL output. E.g., if room is 22C, try 20 or 24.
float SETPOINT = 22.0;

// HYSTERESIS: the allowed temperature band around the setpoint.
// The system only changes state when outside this band.
// >>> TRY CHANGING THIS: try 0.5, 2.0, or 5.0 and observe the difference.
float HYSTERESIS = 1.0;

// --- Object declarations ---
LiquidCrystal_I2C lcd(0x27, 16, 2);  // Change to 0x3F if screen blank
DHT dht(DHT_PIN, DHT_TYPE);

// Track current system state
enum State { HEATING, COOLING, OK };
State currentState = OK;

void setup() {
  Serial.begin(9600);

  // Set LED pins as outputs
  pinMode(HEATER_LED, OUTPUT);
  pinMode(COOLER_LED, OUTPUT);

  // Both LEDs off at startup
  digitalWrite(HEATER_LED, LOW);
  digitalWrite(COOLER_LED, LOW);

  // Initialize sensor and display
  dht.begin();
  lcd.init();
  lcd.backlight();

  // Startup message
  lcd.setCursor(0, 0);
  lcd.print("  Thermostat  ");
  lcd.setCursor(0, 1);
  lcd.print(" Initialising ");
  delay(2000);
  lcd.clear();

  Serial.println("Thermostat ready.");
  Serial.print("Setpoint: ");
  Serial.print(SETPOINT);
  Serial.println(" C");
}

void loop() {
  // --- READ temperature ---
  float tempC = dht.readTemperature();

  // Handle sensor errors
  if (isnan(tempC)) {
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Sensor Error!");
    lcd.setCursor(0, 1);
    lcd.print("Check DHT11");
    Serial.println("ERROR: DHT11 not responding.");
    delay(2000);
    return;
  }

  // --- COMPARE: determine state using hysteresis ---
  // Only change state if we have moved clearly outside the OK band.
  // This prevents rapid switching near the boundary.
  if (tempC < (SETPOINT - HYSTERESIS)) {
    currentState = HEATING;      // Too cold — need to heat
  } else if (tempC > (SETPOINT + HYSTERESIS)) {
    currentState = COOLING;      // Too hot — need to cool
  } else {
    currentState = OK;           // In the acceptable band
  }

  // --- ACTUATE: turn LEDs on/off based on state ---
  digitalWrite(HEATER_LED, (currentState == HEATING) ? HIGH : LOW);
  digitalWrite(COOLER_LED, (currentState == COOLING) ? HIGH : LOW);

  // --- DISPLAY on LCD ---
  lcd.clear();

  // Row 0: Current temperature and status
  lcd.setCursor(0, 0);
  lcd.print("T:");
  lcd.print(tempC, 1);            // 1 decimal place
  lcd.print("C ");

  // Print status label
  if (currentState == HEATING) {
    lcd.print("[HEAT]");
  } else if (currentState == COOLING) {
    lcd.print("[COOL]");
  } else {
    lcd.print(" [OK] ");
  }

  // Row 1: Setpoint and hysteresis band info
  lcd.setCursor(0, 1);
  lcd.print("Set:");
  lcd.print(SETPOINT, 0);         // No decimal for setpoint
  lcd.print("C H:");
  lcd.print(HYSTERESIS, 1);
  lcd.print("C");

  // --- REPORT to Serial Monitor ---
  Serial.print("Temp: ");
  Serial.print(tempC);
  Serial.print(" C  |  Setpoint: ");
  Serial.print(SETPOINT);
  Serial.print("  |  State: ");
  if (currentState == HEATING) Serial.println("HEATING");
  else if (currentState == COOLING) Serial.println("COOLING");
  else Serial.println("OK");

  // Update every 2 seconds (DHT11 needs at least 1 second between readings)
  // >>> TRY CHANGING THIS: reduce to 1000 (1 second) for faster updates
  delay(2000);
}


// ===== CHALLENGE =====
// 1. ADJUSTABLE SETPOINT: Connect a 10 kΩ potentiometer to A0.
//    Map its value to a temperature range:
//    SETPOINT = map(analogRead(A0), 0, 1023, 15, 35);
//    Now the user can "dial in" a temperature setpoint!
//
// 2. SERVO AS VENT: Add the servo from Session 21 on pin 9.
//    When state == COOLING, open the servo to 90° (vent open).
//    When state == HEATING or OK, close it to 0° (vent closed).
//    This models a real HVAC vent damper!
//
// 3. BUZZER ALARM: Add a buzzer on pin 8. If the temperature goes
//    MORE than 5°C away from the setpoint, sound an alarm.
//    Use tone(8, 1000) for the alarm and noTone(8) to stop.
//
// 4. MIN/MAX LOG: Track the highest and lowest temperatures since
//    startup. Display on LCD row 1 by alternating with the setpoint.
//
// 5. CELSIUS/FAHRENHEIT TOGGLE: Add a button on pin 4. Pressing
//    it switches the LCD display between °C and °F.
```

---

## 5. Student Worksheet

### Session 24 — Build a Thermostat
**Name(s):** _________________________ **Date:** _________ **Kit #:** ______

**Objective:** Build a working thermostat with a temperature sensor, LCD display, and LED indicators, and connect the feedback loop to thermodynamics.

---

#### What I Already Know (Warm-Up)

1. From Session 23: what is "hysteresis" and why does a thermostat need it?
   > ___________________________________________________________________

2. What happens to heat energy in a warm room if the heating is turned off on a cold day? (Hint: think about the Second Law of Thermodynamics.)
   > ___________________________________________________________________

3. In your house, which device measures the actual temperature for the thermostat? Where in the house is it usually placed, and why?
   > ___________________________________________________________________

---

#### Build It — Step by Step

- [ ] LCD wired: GND, VCC, SDA → 20, SCL → 21
- [ ] DHT11 wired: GND, VCC → 5V, DATA → pin 2
- [ ] Red LED (heater): pin 7 → 220 Ω → LED(+), LED(−) → GND
- [ ] Blue LED (cooler): pin 6 → 220 Ω → LED(+), LED(−) → GND
- [ ] Partner check complete
- [ ] Code uploaded — LCD shows temperature reading

---

#### Predict!

1. If the room temperature is 22°C and `SETPOINT = 22.0`, `HYSTERESIS = 1.0`, which LEDs will be on?
   > ___________________________________________________________________

2. If you change `SETPOINT = 15.0` (below room temperature), what do you predict will happen?
   > ___________________________________________________________________

3. If `HYSTERESIS = 0.1` (a very narrow band), how often do you think the LEDs will switch state? What problem might this cause in a real heater?
   > ___________________________________________________________________

---

#### Observe / Data Table

| Experiment | SETPOINT | HYSTERESIS | Temp Reading | Red LED | Blue LED | LCD Status |
|-----------|---------|-----------|-------------|---------|---------|-----------|
| 1 — Room temp | 22 | 1.0 | | | | |
| 2 — Set below room | 15 | 1.0 | | | | |
| 3 — Set above room | 30 | 1.0 | | | | |
| 4 — Breathe on sensor | 22 | 1.0 | | | | |
| 5 — Tiny hysteresis | 22 | 0.5 | | | | |

---

#### What Did You Notice?

1. When you set the setpoint to 15°C (below room temperature), which LED came on? Why?
   > ___________________________________________________________________

2. What happened when you breathed on the DHT11 sensor? (Your breath is ~37°C.) Which LED responded, and does this match what the thermostat should do?
   > ___________________________________________________________________

3. With `HYSTERESIS = 0.5`, did the LEDs flicker more than with `HYSTERESIS = 1.0`? What does this tell you about the trade-off between accuracy and stability?
   > ___________________________________________________________________

4. Draw the complete feedback loop for your thermostat. Include: DHT11, Arduino, LED, air temperature, and the feedback path. Label the setpoint and show where the "error" is calculated.

   *(Draw here)*

---

#### Science Connection

1. The Second Law of Thermodynamics says heat flows from hot to cold. A thermostat in a cold winter day must constantly fight this flow. Where does the energy come from to do this, and what form is it in when it enters the system?
   > ___________________________________________________________________

2. Your body's "setpoint" is approximately 37°C. What happens to your body if your temperature rises to 40°C? What feedback mechanism kicks in, and what is its limitation at very high temperatures?
   > ___________________________________________________________________

3. A real home thermostat has a hysteresis of about 0.5°C–1°C. A sensitive laboratory incubator might have a hysteresis of 0.01°C. What type of control equipment would an incubator need (compared to a simple LED) to achieve such precise temperature control? Why is precision so important in a laboratory?
   > ___________________________________________________________________

---

#### Challenge Extension

1. **Adjustable setpoint:** Add a potentiometer to A0 and map its range to 15–35°C as the setpoint. Now you can "dial in" any temperature.

2. **Servo vent:** Add the servo from Session 21. Make it open (90°) when cooling is needed and close (0°) when heating or in the OK zone.

3. **Min/max record:** Track the highest and lowest temperature recorded since startup. Display them alternating on LCD row 1.

---

## 6. Safety Notes

**This session's components and hazards:**

- **DHT11:** Double-check VCC polarity before powering on. A bare DHT11 wired backwards gets warm quickly — if the sensor feels warm to the touch immediately after power-on, unplug USB and check the wiring. The module version is more tolerant of accidental reversal (has a protection diode) but still check carefully.
- **LEDs:** Both LEDs require 220 Ω current-limiting resistors. Forgetting a resistor will burn out the LED within seconds. Check both resistors are in place before uploading.
- **Multiple GND connections:** This circuit has many components sharing the Arduino's GND. Make sure all GND wires reach the GND rail on the breadboard properly — a loose GND is a common cause of erratic readings.
- **DHT11 temperature sensor accuracy:** The DHT11 reads ±2°C — be aware that readings near the setpoint may appear to "jump" by 1–2 degrees due to sensor resolution. This is normal and is why hysteresis is important.

**Build → Check → Power on.** With more components this session, do a systematic partner check: go through each numbered wiring step in the Fritzing table and confirm one wire at a time.

**If something goes wrong:**
- DHT11 sensor or LCD gets warm → unplug USB immediately, check power polarity.
- Both LEDs stuck on or off → check `currentState` logic in Serial Monitor; check setpoint value.

---

## 7. Assessment Rubric

### Formative Check (not graded)

| Look-For | Not Yet | Got It |
|----------|---------|--------|
| Circuit complete: all 4 components wired correctly (DHT11, LCD, red LED, blue LED) | One or more components missing or wired incorrectly | All four components functional and correct |
| Thermostat logic correct: red LED ON when temp < setpoint−hysteresis; blue LED ON when temp > setpoint+hysteresis | LEDs do not respond correctly to temperature changes | LEDs respond correctly; demonstrated by changing setpoint above/below room temperature |
| Can explain hysteresis and its purpose | "It prevents flickering" without further explanation | Explains hysteresis as a dead zone that prevents rapid state changes, with a real-world analogy |

---

## 8. Differentiation

### Support

- **Pre-wired LCD:** If the LCD was already working in Session 20, use that same setup. Students only add the DHT11 and LEDs.
- **Code simplification:** Provide a version without the `enum` (just use integer constants 0, 1, 2 or simple boolean flags) for students who find the `enum State` confusing.
- **Printed pseudocode card:** Provide the pseudocode from the Direct Instruction section as a card. Students trace their code against the pseudocode line by line.
- **Sentence starters:** "The red LED turns ON when the temperature is ____ because ____. The hysteresis prevents ____ from happening."

### Extension

- **PID-style control:** Research proportional control. Instead of just ON/OFF, have the LED blink faster/slower or use PWM brightness proportional to the error. `int brightness = constrain(abs(error) * 10, 0, 255); analogWrite(LED_PIN, brightness);`
- **Data logging:** Log temperature + state (HEAT/COOL/OK) to the Serial Monitor in CSV format every 10 seconds. Copy into a spreadsheet and plot temperature vs. time, marking the setpoint as a horizontal line.
- **Multi-zone:** Add a second DHT11 on pin 3 representing a "second room." Display both zones on the LCD alternating every 3 seconds.
- **Real heater safe demo:** Use a transistor (or relay module if available) to switch a 5V resistive load (e.g., a 10 Ω resistor — barely warm) instead of an LED. Demonstrate true heater switching under Arduino control.

### Visual / Kinesthetic Accommodations

- **Feedback loop poster:** Print a large thermostat feedback loop diagram (setpoint, comparator, controller, heater, room, sensor, feedback) and have students stick on the name of each physical component.
- **Temperature timeline graph:** Provide graph paper with temperature on the Y-axis and time on the X-axis. Students mark data points every minute and draw horizontal dashed lines for setpoint ± hysteresis.
- **Concrete analogy:** "The hysteresis band is like the QUIET ZONE between a "too cold" alarm and a "too hot" alarm at a zoo animal enclosure — you don't want the heater clicking on for a tiny fluctuation."
- **Glossary card:** thermostat, setpoint, hysteresis, dead zone, negative feedback, HEATING state, COOLING state, OK zone, thermodynamics, thermal equilibrium.
