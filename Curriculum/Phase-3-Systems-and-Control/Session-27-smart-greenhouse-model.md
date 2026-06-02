# Week 14, Session 27 — Smart Greenhouse Model

**Phase:** Phase 3 — Systems & Control
**Week:** 14 | **Session:** 27 of 36

---

## Learning Objectives

By the end of this session, students will be able to:
- Integrate at least two sensors (DHT11 temperature + LDR light), an LCD, a servo ("vent"), and an LED ("grow light") into a single coordinated smart greenhouse system.
- Write code that uses multiple sensor readings simultaneously to make independent control decisions.
- Explain how a greenhouse represents a real-world multi-sensor feedback system and connect it to ecosystem science.
- Interpret a multi-sensor system diagram and trace the input→process→output path for each subsystem.

---

**Science Curriculum Link:** Ecosystems, environmental control, and multi-sensor systems (Grade 8 Biology — Ecosystems & Environmental Science; Grade 8 Systems & Technology).
A greenhouse is an engineered ecosystem that must maintain specific ranges of temperature and light for plant growth. Modern commercial greenhouses use exactly the sensors and actuators students will build today: temperature sensors trigger ventilation, light sensors trigger supplemental LED grow lights. This session bridges ecosystems science (what conditions plants need) with systems engineering (how to automate those conditions).

---

## Materials Checklist (per pair)

- [ ] 1 × Arduino Mega 2560 + USB cable
- [ ] 1 × Solderless breadboard
- [ ] 1 × 16×2 LCD with I2C backpack (SDA → pin 20, SCL → pin 21)
- [ ] 4 × M-F jumper wires for LCD
- [ ] 1 × DHT11 module (temperature + humidity)
- [ ] 3 × jumper wires for DHT11
- [ ] 1 × LDR (photoresistor) + 10 kΩ pull-down resistor
- [ ] 1 × SG90 servo motor (the "vent" — keeps fingers clear when powered)
- [ ] 3 × M-F jumper wires for servo
- [ ] 1 × Green LED (the "grow light")
- [ ] 1 × 220 Ω resistor for LED
- [ ] Several M-M jumper wires (various colours)
- [ ] Computer with Arduino IDE (`DHT sensor library` + `Servo.h` + `LiquidCrystal_I2C` — all installed from prior sessions)

---

## 2. Teacher Guide (45-minute breakdown)

### 0–5 min — Hook / Warm-Up

**Show a photo of a commercial greenhouse** (search: "commercial smart greenhouse interior" — rows of plants under LED grow lights with automatic roof vents).

**Ask:** "What does a plant need to survive and grow? List everything you can think of."

*Expected answers:* Light, water, CO₂, warmth, nutrients, correct humidity.

**Follow-up:** "A commercial greenhouse controls ALL of these automatically. Today we're going to model TWO of them: temperature (controlled by a roof vent) and light (controlled by a grow light). If it gets too hot → vent opens. If it's too dark → grow light turns on. You are going to build that controller."

---

### 5–15 min — Direct Instruction

**Concept: Multi-sensor systems, ecosystem engineering, and sub-system integration**

*Words to say:*
"Real greenhouses are complex systems with many subsystems running in parallel. Each subsystem has its own sensor, setpoint, and actuator — and they all run independently. Today we build two:

**Sub-system 1 — Temperature control (ventilation):**
- Sensor: DHT11 reads temperature
- Setpoint: `TEMP_SETPOINT` (e.g., 28°C — a warm threshold to trigger venting)
- Actuator: SG90 servo — when too hot, opens to 90° (vent open); otherwise stays at 0° (vent closed)

**Sub-system 2 — Light control (grow light):**
- Sensor: LDR reads ambient light level
- Setpoint: `LIGHT_THRESHOLD` (e.g., 400 — below this = too dark for plants)
- Actuator: Green LED — turns ON if too dark, OFF if bright enough

**Both subsystems update every 2 seconds, independently.**

**LCD display plan:**
- Row 0: `T:23.4C [VENT]` or `T:23.4C [OK] `
- Row 1: `L:387  [GROW] ` or `L:712  [OK]  `

**Ecosystem science connection:**
- Plants need light for photosynthesis (chlorophyll absorbs red + blue wavelengths — green LED light is less efficient but symbolic here).
- Plants suffer and wilt above ~35°C; photosynthesis slows dramatically.
- Humidity matters too — the DHT11 can trigger a misting system (we'll skip that today, but note it as an extension).

**System diagram (draw on board):**

```
          ┌─────── TEMPERATURE SUB-SYSTEM ───────┐
          │  DHT11 → Arduino → Servo (vent)       │
          │  if temp > setpoint → vent OPEN        │
          └──────────────────────────────────────┘

          ┌─────── LIGHT SUB-SYSTEM ─────────────┐
          │  LDR → Arduino → Green LED (grow light│
          │  if light < threshold → grow light ON  │
          └──────────────────────────────────────┘
          
                    ┌─── LCD ───┐
                    │ shows both│
                    │ status    │
                    └───────────┘
```

Both subsystems run in the same `loop()`. The Arduino checks both sensors every cycle and makes independent decisions."

---

### 15–35 min — Hands-On Build + Code

**Remind students:** Build → Check → Power on. Servo horn — keep fingers clear. With the most components so far, work methodically.

**Recommended build order (most reliable):**

1. **LCD first** (already tested in Sessions 19–20): GND → GND, VCC → 5V, SDA → pin 20, SCL → pin 21. Verify it displays during build.
2. **DHT11**: VCC → 5V (red), GND → GND (black), DATA → **pin 2** (green).
3. **LDR voltage divider**: 5V → LDR top, LDR bottom → A0 (green), 10 kΩ → GND (black).
4. **Green LED**: pin 5 → 220 Ω → LED anode (long), LED cathode → GND.
5. **Servo**: GND (brown/black) → GND, VCC (red) → 5V, Signal (orange) → **pin 9** (~PWM). **Attach servo horn before powering.**
6. Partner check: go through the full Fritzing table before plugging USB.
7. Upload code. Observe initial readings on LCD. Test by covering LDR (grow light should come on) and breathing on DHT11 (temperature rises → vent servo opens).

---

### 35–42 min — Testing & Debugging

**What success looks like:**
- LCD row 0 shows temperature + `[OK]` or `[VENT]`.
- LCD row 1 shows light reading + `[OK]` or `[GROW]`.
- Covering the LDR turns the green LED on.
- Warming the DHT11 (breath on it) opens the servo to 90°; letting it cool closes it to 0°.

**Troubleshooting table:**

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| LCD blank | Wrong I2C address; SDA/SCL swapped | Try 0x3F; check pins 20/21 |
| DHT11 reads "nan" | Data pin wrong; missing pull-up on bare sensor | Check DATA → pin 2; use module version (has built-in resistor) |
| Servo doesn't move | Signal on non-PWM pin; `myServo.attach()` missing | Confirm signal on pin 9; check `attach()` call in setup() |
| LDR always reads same value | Voltage divider not complete | Check 10 kΩ from A0 node to GND |
| LED always on or off | Light threshold needs adjustment | Read Serial Monitor, adjust LIGHT_THRESHOLD |
| Servo moves unexpectedly at startup | DHT11 reading error causes temp > setpoint | Add `if(isnan(tempC)) return;` check |

---

### 42–45 min — Reflection / Exit Ticket

Students write on a sticky note:

1. "List all of the components in your greenhouse model and say whether each is an INPUT, OUTPUT, or PROCESSOR."
2. "The temperature setpoint in the code is 28°C. Why might a real greenhouse use a higher setpoint (like 35°C) for tropical plants vs a lower one (like 18°C) for alpine plants?"
3. "What additional sensor would make this greenhouse model more realistic? What would it control?"

---

## 3. Circuit Diagram

```
  ARDUINO MEGA 2560
  ┌──────────────────────────────────────────────────────────────────┐
  │  pin 20 (SDA) ──────────────────────────────────────────────────┼──►[blue]   LCD SDA
  │  pin 21 (SCL) ──────────────────────────────────────────────────┼──►[yellow] LCD SCL
  │  5V ─────────────────────────────────────────────────────────── ┼──►[red]    LCD VCC
  │  GND ───────────────────────────────────────────────────────────┼──►[black]  LCD GND
  │                                                                  │
  │  pin 2  (digital)─────────────────────────────────────────────── ┼──►[green]  DHT11 DATA
  │  5V ─────────────────────────────────────────────────────────── ┼──►[red]    DHT11 VCC
  │  GND ───────────────────────────────────────────────────────────┼──►[black]  DHT11 GND
  │                                                                  │
  │  A0   (analog in) ─────────────────────────────────────────────┼──►[purple] LDR voltage divider
  │  5V ─────────────────────────────────────────────────────────── ┼──►[red]    LDR top
  │  GND ───────────────────────────────────────────────────────────┼──►[black]  10 kΩ to GND
  │                                                                  │
  │  pin 5  (digital out) ──────────────────────────────────────────┼──►[green]  220Ω──► LED(+)
  │  GND ───────────────────────────────────────────────────────────┼──►[black]  LED(−)
  │                                                                  │
  │  pin 9  (~PWM)  ────────────────────────────────────────────────┼──►[orange] Servo SIGNAL
  │  5V ─────────────────────────────────────────────────────────── ┼──►[red]    Servo VCC
  │  GND ───────────────────────────────────────────────────────────┼──►[black]  Servo GND
  └──────────────────────────────────────────────────────────────────┘
```

**Fritzing-style build description:**

| # | From | To | Wire Color | Notes |
|---|------|----|-----------|-------|
| 1–4 | LCD wiring | (see Session 19) | Blue/Yellow/Red/Black | SDA→20, SCL→21 |
| 5 | Arduino pin 2 | DHT11 DATA | Green | |
| 6 | Arduino 5V | DHT11 VCC | Red | |
| 7 | Arduino GND | DHT11 GND | Black | |
| 8 | Arduino 5V | LDR top leg | Red | |
| 9 | LDR bottom / junction | Arduino A0 | Purple | Sense node |
| 10 | 10 kΩ resistor bottom | Arduino GND | Black | LDR pull-down |
| 11 | Arduino pin 5 | 220 Ω resistor | Green | Grow light LED |
| 12 | 220 Ω resistor | Green LED anode (+) | Green | |
| 13 | Green LED cathode (−) | Arduino GND | Black | |
| 14 | Servo GND (brown) | Arduino GND | Black | M-F jumper |
| 15 | Servo VCC (red) | Arduino 5V | Red | M-F jumper |
| 16 | Servo Signal (orange) | Arduino pin 9 | Orange | M-F; ~ PWM pin |

[Google image search: "Arduino smart greenhouse DHT11 LDR servo LED temperature light control model"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 *  Session 27 — Smart Greenhouse Model
 *  Arduino Mega Teaching Curriculum — Phase 3
 * ============================================================
 *
 *  SCIENCE EXPLANATION:
 *  ─────────────────────────────────────────────────────────
 *  A greenhouse is an engineered ECOSYSTEM — humans control
 *  the environment to optimise plant growth.
 *
 *  Plants need specific conditions:
 *    TEMPERATURE: Most vegetables thrive at 18–28°C.
 *                 Too hot → stomata close, wilting, stress.
 *    LIGHT: Plants need specific wavelengths for photosynthesis.
 *           ~400–700 nm (PAR: Photosynthetically Active Radiation).
 *           Modern greenhouses use LED grow lights tuned to
 *           red (660 nm) and blue (450 nm) wavelengths.
 *    HUMIDITY: Affects transpiration and disease risk.
 *
 *  Our model implements two INDEPENDENT FEEDBACK LOOPS:
 *
 *  LOOP 1 — TEMPERATURE CONTROL:
 *    Sensor: DHT11 (temperature)
 *    Actuator: Servo ("roof vent")
 *    Rule: temp > TEMP_SETPOINT → open vent (90°)
 *          temp ≤ TEMP_SETPOINT → close vent (0°)
 *
 *  LOOP 2 — LIGHT CONTROL:
 *    Sensor: LDR (ambient light level)
 *    Actuator: Green LED ("grow light")
 *    Rule: ldrValue < LIGHT_THRESHOLD → grow light ON
 *          ldrValue ≥ LIGHT_THRESHOLD → grow light OFF
 *
 *  Both loops run in every iteration of loop(), checking and
 *  updating their actuator independently.
 * ============================================================
 *
 *  HARDWARE:
 *    DHT11 DATA     → pin 2
 *    LDR divider    → A0 (LDR top to 5V, 10 kΩ bottom to GND)
 *    Green LED      → pin 5 (via 220 Ω)
 *    Servo signal   → pin 9 (~PWM)
 *    LCD SDA        → pin 20
 *    LCD SCL        → pin 21
 *
 *  SAFETY: Keep fingers clear of servo horn during operation.
 * ============================================================
 */

#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <DHT.h>
#include <Servo.h>

// --- Pin definitions ---
#define DHT_PIN   2
#define DHT_TYPE  DHT11

const int LDR_PIN   = A0;
const int GROWLIGHT_PIN = 5;
const int SERVO_PIN = 9;

// --- Greenhouse setpoints ---
// >>> TRY CHANGING THIS: set TEMP_SETPOINT to 2°C above current room temp
// so you can trigger the vent by breathing on the sensor.
float TEMP_SETPOINT  = 28.0;    // °C — vent opens above this temperature
// >>> TRY CHANGING THIS: read current LDR value from Serial Monitor,
// then set threshold just above it (to trigger grow light by covering LDR)
int   LIGHT_THRESHOLD = 400;    // below this = too dark → grow light ON

// --- Hysteresis for temperature (prevents rapid vent cycling) ---
float TEMP_HYSTERESIS = 1.0;    // °C — same concept as Session 24

// --- State tracking (for hysteresis) ---
bool ventIsOpen = false;

// --- Objects ---
LiquidCrystal_I2C lcd(0x27, 16, 2);   // Change to 0x3F if screen blank
DHT dht(DHT_PIN, DHT_TYPE);
Servo ventServo;

// --- Constants for vent servo positions ---
const int VENT_CLOSED = 0;    // Servo angle for closed vent
const int VENT_OPEN   = 90;   // Servo angle for open vent

void setup() {
  Serial.begin(9600);

  // Set pin modes
  pinMode(GROWLIGHT_PIN, OUTPUT);
  digitalWrite(GROWLIGHT_PIN, LOW);

  // Initialise all three devices
  dht.begin();
  ventServo.attach(SERVO_PIN);
  ventServo.write(VENT_CLOSED);       // Start with vent closed
  delay(500);

  lcd.init();
  lcd.backlight();

  // Startup message
  lcd.setCursor(0, 0);
  lcd.print("Smart Greenhouse");
  lcd.setCursor(0, 1);
  lcd.print("Initialising...");
  delay(2000);
  lcd.clear();

  Serial.println("Smart Greenhouse — Session 27");
  Serial.println("Temp(C) | Light | Vent | Grow Light");
}

void loop() {
  // ============================================================
  // READ both sensors
  // ============================================================
  float tempC  = dht.readTemperature();
  int   ldrVal = analogRead(LDR_PIN);

  // Handle DHT11 error
  if (isnan(tempC)) {
    Serial.println("DHT11 error — check wiring!");
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Temp sensor err");
    delay(2000);
    return;
  }

  // ============================================================
  // SUB-SYSTEM 1: TEMPERATURE → VENT SERVO
  // ============================================================
  // Apply hysteresis: only change vent state when clearly outside band
  if (tempC > (TEMP_SETPOINT + TEMP_HYSTERESIS)) {
    ventIsOpen = true;
  } else if (tempC < (TEMP_SETPOINT - TEMP_HYSTERESIS)) {
    ventIsOpen = false;
  }
  // Write the vent position
  ventServo.write(ventIsOpen ? VENT_OPEN : VENT_CLOSED);

  // ============================================================
  // SUB-SYSTEM 2: LIGHT → GROW LIGHT LED
  // ============================================================
  // >>> TRY CHANGING THIS: change LIGHT_THRESHOLD to adjust sensitivity
  bool growLightOn = (ldrVal < LIGHT_THRESHOLD);
  digitalWrite(GROWLIGHT_PIN, growLightOn ? HIGH : LOW);

  // ============================================================
  // UPDATE LCD
  // ============================================================
  lcd.clear();

  // Row 0: Temperature and vent status
  lcd.setCursor(0, 0);
  lcd.print("T:");
  lcd.print(tempC, 1);
  lcd.print("C ");
  lcd.print(ventIsOpen ? "[VENT]" : " [OK] ");

  // Row 1: Light level and grow light status
  lcd.setCursor(0, 1);
  lcd.print("L:");
  lcd.print(ldrVal);
  lcd.print("  ");
  lcd.print(growLightOn ? "[GROW]" : " [OK] ");

  // ============================================================
  // SERIAL MONITOR report
  // ============================================================
  Serial.print(tempC);
  Serial.print(" C  |  LDR: ");
  Serial.print(ldrVal);
  Serial.print("  |  Vent: ");
  Serial.print(ventIsOpen ? "OPEN  " : "CLOSED");
  Serial.print("  |  Grow: ");
  Serial.println(growLightOn ? "ON " : "OFF");

  // Wait 2 seconds before next reading (DHT11 minimum interval)
  // >>> TRY CHANGING THIS: reduce to 1000 for faster updates (still within DHT11 spec)
  delay(2000);
}


// ===== CHALLENGE =====
// 1. HUMIDITY CONTROL: The DHT11 also measures humidity!
//    Add a third subsystem: if humidity > HUMIDITY_SETPOINT
//    (e.g., 70%), turn on a "fan" LED on pin 6 to "ventilate."
//    float humidity = dht.readHumidity();
//
// 2. ADJUSTABLE SETPOINTS: Connect two potentiometers (A1 for
//    temperature setpoint, A2 for light threshold). Map them to
//    sensible ranges:
//    TEMP_SETPOINT = map(analogRead(A1), 0, 1023, 15, 40);
//    LIGHT_THRESHOLD = map(analogRead(A2), 0, 1023, 100, 900);
//
// 3. ALARM: Add a buzzer on pin 8. If temperature exceeds
//    TEMP_SETPOINT + 5°C (critically hot), sound a warning.
//
// 4. TIME-BASED LIGHTING: Use millis() to simulate day/night
//    cycles. Light is ON only during a simulated "night" window.
//    (Real greenhouses provide supplemental light based on
//    photoperiod — day length — which affects flowering.)
//
// 5. BUILD THE PHYSICAL GREENHOUSE: Use cardboard to build a
//    small box representing the greenhouse. Mount the servo
//    on the "roof" with a cut-out that opens/closes with the horn.
//    Place the LDR inside the box to detect shading.
```

---

## 5. Student Worksheet

### Session 27 — Smart Greenhouse Model
**Name(s):** _________________________ **Date:** _________ **Kit #:** ______

**Objective:** Build a smart greenhouse controller with two independent sensor-actuator subsystems and connect it to ecosystem science.

---

#### What I Already Know (Warm-Up)

1. What is photosynthesis? Write the word equation:
   > ____________ + ____________ → ____________ + ____________

2. Name two environmental conditions (other than CO₂ and water) that affect plant growth rate.
   > ___________________________________________________________________

3. In a greenhouse on a hot summer day, what natural process do you think happens when vents are opened? Why does air movement help keep plants cool?
   > ___________________________________________________________________

---

#### Build It — Step by Step

*(Build in this order to make debugging easier)*

- [ ] LCD: GND, VCC, SDA → pin 20, SCL → pin 21 — verify it displays first
- [ ] DHT11: VCC → 5V, GND → GND, DATA → pin 2
- [ ] LDR voltage divider: 5V → LDR top, LDR bottom → A0, 10 kΩ → GND
- [ ] Green LED (grow light): pin 5 → 220 Ω → LED(+), LED(−) → GND
- [ ] Servo (vent): GND → GND, VCC → 5V, Signal → pin 9 — horn clear of fingers
- [ ] Partner check: all components
- [ ] Upload code — LCD shows T: and L: readings

---

#### Predict!

1. If the room temperature is 22°C and `TEMP_SETPOINT = 28`, what position will the servo be in at startup?
   > ___________________________________________________________________

2. How would you trigger the grow light to turn ON during testing (without actually making the room dark)?
   > ___________________________________________________________________

3. The two subsystems are independent — one doesn't affect the other. Give a real greenhouse scenario where BOTH subsystems would be active at the same time.
   > ___________________________________________________________________

---

#### Observe / Data Table

| Test | Temp (°C) | LDR Value | Vent Position | Grow Light | LCD Row 0 | LCD Row 1 |
|------|----------|----------|--------------|-----------|----------|----------|
| 1 — Startup (room conditions) | | | | | | |
| 2 — Cover LDR | | | | | | |
| 3 — Breathe on DHT11 | | | | | | |
| 4 — Both: cover LDR + breathe | | | | | | |
| 5 — Change TEMP_SETPOINT to 15°C | | | | | | |

---

#### What Did You Notice?

1. When you set `TEMP_SETPOINT = 15.0` (below room temperature), what happened to the servo immediately? Why?
   > ___________________________________________________________________

2. Did covering the LDR affect the servo? Did breathing on the DHT11 affect the grow light? What does this confirm about the two subsystems?
   > ___________________________________________________________________

3. When you triggered both simultaneously (cover LDR AND breathe on DHT11), did the system handle both correctly? What does this tell you about how the Arduino processes two tasks?
   > ___________________________________________________________________

4. Draw the system diagram for your smart greenhouse. Show both subsystems with their sensor, comparator, controller, actuator, and feedback paths. Label the setpoints.

   *(Draw here)*

---

#### Science Connection

1. Real greenhouse LED grow lights use red (660 nm) and blue (450 nm) wavelengths. Our green LED is not actually ideal for plants. Why do plants absorb red and blue light most efficiently? (Hint: look up the absorption spectrum of chlorophyll.)
   > ___________________________________________________________________

2. A natural ecosystem (like a rainforest) does not need any electronic sensors or actuators. What natural "feedback mechanisms" maintain temperature and humidity in a rainforest ecosystem? Compare these to your electronic greenhouse control.
   > ___________________________________________________________________

3. Commercial greenhouses are described as "controlled environment agriculture" (CEA). Name two advantages and one disadvantage of CEA compared to growing crops in an open field.
   > Advantage 1: _______________________________________________________
   > Advantage 2: _______________________________________________________
   > Disadvantage: _______________________________________________________

---

#### Challenge Extension

1. **Humidity control:** Add a third subsystem. Use `dht.readHumidity()` and turn on a "fan" LED when humidity exceeds 70%.

2. **Physical greenhouse box:** Build a small cardboard box and mount the servo on it as a roof vent. Place the LDR inside the box to detect when the "roof" shades it.

3. **Adjustable setpoints:** Connect two potentiometers (A1, A2) so the user can dial in the temperature and light setpoints without uploading new code.

---

## 6. Safety Notes

**This session's components and hazards:**

- **Servo motor:** This session's most significant physical hazard. The servo moves automatically in response to temperature changes — **keep fingers clear of the horn at all times while the circuit is powered.** When breathing on the DHT11 to test the vent, be aware the servo will move. Announce "servo is moving" when testing.
- **DHT11:** Use the module version (built-in resistor). Bare DHT11 reversed wiring causes rapid heating — check polarity before powering on.
- **Multiple power draws:** DHT11 (~1 mA), LDR divider (~0.5 mA), servo (~100–250 mA moving, ~5 mA stationary), LED (~15 mA), LCD (~50 mA backlight). Total ≈ 300 mA when servo moves — within USB limits for one servo.
- **Wire management:** This is the most complex circuit of Phase 3. Arrange wires in colour groups (red = power, black = GND, green = sensor signals, orange = servo). A tangled breadboard is a debugging nightmare.

**Build → Check → Power on.** Use the 5-step build order. Verify each component one at a time before adding the next.

**If something goes wrong:**
- Servo moves unexpectedly at startup → check for DHT11 error (`isnan` check in code should catch this).
- Any component warm → unplug USB, identify the warm component, check its polarity.

---

## 7. Assessment Rubric

### Formative Check (not graded)

This is the last non-graded session before Session 28 (graded mini-project). It serves as a rehearsal for the complexity expected in Session 28.

| Look-For | Not Yet | Got It |
|----------|---------|--------|
| All 5 components wired and functional | One or more not working | All 5 functional: LCD shows readings, vent servo responds to temp, grow light responds to LDR |
| Two independent subsystems both correct | Only one subsystem works | Both subsystems work independently and simultaneously |
| Can draw and explain the full system diagram | Cannot label components with correct system vocabulary | Correctly labels sensor, actuator, setpoint, feedback, and comparator for both subsystems |

---

## 8. Differentiation

### Support

- **Build in stages:** Tell students to test each component before adding the next. Provide a checklist: "Stage 1: LCD works? → Stage 2: DHT11 reads? → Stage 3: LDR reads? → Stage 4: LED works? → Stage 5: Servo sweeps?"
- **Code pre-loaded:** For very low-confidence students, provide the full code pre-typed. Their task is to read, understand, and annotate it — rather than type it.
- **Subsystem simplification:** Remove the servo (most complex component). Build with just DHT11 + LDR + two LEDs + LCD as a first pass.
- **System diagram template:** Provide a printed template with two loops drawn (boxes and arrows with blanks) for students to fill in component names only.

### Extension

- **Cardboard greenhouse model:** Build a physical box. Mount the servo on the roof with a cardboard flap. Place LDR inside the box to detect when the flap is closed (dark inside = grow light needed). This creates a true physical feedback loop.
- **CSV data logging:** Output all sensor readings and actuator states to Serial Monitor in CSV format. Copy into a spreadsheet over 5 minutes and graph temperature + vent state over time.
- **PI controller:** Research proportional control. Instead of full-open/full-closed vent, use `map(tempC, TEMP_SETPOINT, TEMP_SETPOINT + 10, 0, 90)` to partially open the vent proportional to how much too hot it is.
- **Water management:** Research hydroponics. What additional sensors would a professional system use? Design (on paper) a water control subsystem to add to the greenhouse model.

### Visual / Kinesthetic Accommodations

- **Role play:** Assign class roles: one student = DHT11 sensor (holds thermometer), one = Arduino (reads the temperature, makes a decision), one = servo (physically moves their arm to 0° or 90° based on the Arduino's instruction). Repeat for LDR + LED subsystem.
- **Two-colour wire rule:** Red wires ONLY for power, black ONLY for ground — colour-coded wiring is mandatory for this complex build.
- **Annotated photograph:** Provide a large printed photo of the correct completed circuit with labels pointing to each component and connection.
- **Glossary card:** ecosystem, photosynthesis, photosynthetically active radiation (PAR), setpoint, hysteresis, actuator, subsystem, controlled environment agriculture, transpiration, CEA.
