# Capstone — Sample Exemplar Project 🌱
# "GreenKeeper": An Automatic Smart Greenhouse / Plant Monitor

> **For teachers and students.** This is a complete, gold-standard example of what a finished capstone looks like — the science, the plan, the circuit, the full code, the data, and the write-up. Use it as a model, not a thing to copy. Your project should be *yours*!

**Team:** (Exemplar by the curriculum authors) · **Time to build:** 4 build sessions

---

## 1. The Big Idea (Student-Facing Summary)
Plants in a greenhouse need the right **temperature** and **light** to grow well. If it gets too hot, a greenhouse needs to **vent** (open a window); if it gets too dark, plants may need a **grow light**. **GreenKeeper** is a small automatic system that **senses** temperature, humidity, and light, then **acts** — opening a vent with a servo and switching a grow-light LED — all by itself, while showing the status on an LCD and logging data to the computer.

It demonstrates **feedback loops & systems thinking**, **heat/thermodynamics**, and **light**, and it collects **real data** we can graph — a true science instrument.

---

## 2. Scientific Question & Hypothesis
**Question:** *Can an automatic control system keep a small enclosed space within a healthy temperature and light range better than leaving it alone?*

**Hypothesis (If… then… because…):**
> *If* we use a sensor-driven vent and grow light, *then* the enclosure will stay closer to the target range (22–26 °C, and never fully dark during "day"), *because* the system reacts to changes immediately with negative **feedback**, the way a thermostat or a living organism keeps itself stable (homeostasis).

**Variables:**
- **Independent (what we change):** control system ON vs OFF (open-loop vs closed-loop).
- **Dependent (what we measure):** temperature (°C), humidity (%), light level (0–1023), vent angle, grow-light state.
- **Controlled (kept the same):** the enclosure (a cardboard box / clear tub), the heat source (a small lamp), room, and time of day.

---

## 3. Materials List
| Component | Qty | Role in the system |
|-----------|-----|--------------------|
| Arduino Mega 2560 + USB | 1 | Brain |
| Breadboard + jumper wires | 1 set | Connections |
| DHT11 temperature/humidity sensor | 1 | INPUT — heat & humidity |
| LDR (photoresistor) + 10 kΩ resistor | 1 | INPUT — light level |
| SG90 servo motor | 1 | OUTPUT — opens/closes the vent flap |
| LED (white/blue) + 220 Ω resistor | 1 | OUTPUT — "grow light" |
| Passive buzzer | 1 | OUTPUT — overheat alarm |
| 16×2 I2C LCD | 1 | OUTPUT — live status display |
| Small box/tub + cardboard flap | 1 | The "greenhouse" |
| Desk lamp (heat/light source) | 1 | To test the system |

**Libraries:** `DHT sensor library`, `Servo` (built-in), `LiquidCrystal_I2C`.

---

## 4. Circuit Diagram

```
                         ARDUINO MEGA 2560
   ┌──────────────────────────────────────────────────────────┐
   │ 5V  ──► breadboard RED rail (+)                            │
   │ GND ──► breadboard BLACK rail (–)                          │
   │                                                            │
   │ Pin 7   ──────────────► DHT11 DATA   (temp/humidity in)    │
   │ A0      ──────────────► LDR divider midpoint (light in)    │
   │ Pin ~9  ──[220Ω]──►|── LED  ── GND   (grow light out)      │
   │ Pin ~6  ──────────────► SERVO signal (orange)             │
   │ Pin 8   ──────────────► BUZZER (+)  ── GND                 │
   │ Pin 20 (SDA) ─────────► LCD SDA                            │
   │ Pin 21 (SCL) ─────────► LCD SCL                            │
   └──────────────────────────────────────────────────────────┘

  LDR voltage divider:   5V ──[ LDR ]──┬──[ 10kΩ ]── GND
                                        │
                                        └──► A0 (the midpoint we read)

  DHT11 module:  (+)→5V, (–)→GND, (DATA/S)→Pin 7
  Servo SG90:    RED→5V, BROWN→GND, ORANGE→Pin ~6
  LCD I2C:       VCC→5V, GND→GND, SDA→Pin 20, SCL→Pin 21
```

**Fritzing-style build description:**
1. Run **5V (red)** and **GND (black)** from the Mega to the breadboard power rails. Every module's power/ground connects to these rails (**common ground is essential**).
2. **LDR divider:** 5V → LDR → node → 10 kΩ → GND. Wire the **node** to **A0** (yellow).
3. **DHT11:** + → 5V, – → GND, DATA → **pin 7** (green).
4. **Grow-light LED:** **pin ~9** → 220 Ω → LED long leg (+); LED short leg → GND (white/blue wire for fun).
5. **Servo (vent):** orange → **pin ~6**, red → 5V, brown → GND. Attach a cardboard flap to the horn.
6. **Buzzer:** + → **pin 8**, – → GND.
7. **LCD I2C:** VCC→5V, GND→GND, **SDA→pin 20, SCL→pin 21** (Mega's dedicated I2C pins).

[Google image search: **"Arduino Mega greenhouse DHT11 LDR servo LCD I2C breadboard wiring"**]

> ⚠️ The SG90 servo is fine on the Mega's 5V for one small flap. For a bigger/heavier flap, power the servo from a separate 5V supply with a shared ground.

---

## 5. Arduino Code (complete, fully commented)

```cpp
/*
  ===================================================================
  GREENKEEPER — Automatic Smart Greenhouse / Plant Monitor (CAPSTONE)
  ===================================================================
  THE SCIENCE — FEEDBACK LOOPS, HEAT & LIGHT:
  A greenhouse must stay in a healthy TEMPERATURE and LIGHT range.
  This system SENSES temperature/humidity (DHT11) and light (LDR),
  DECIDES with if/else rules, and ACTS:
     - opens a VENT (servo) when it's too hot  -> lets heat escape
     - turns on a GROW LIGHT (LED) when it's too dark
     - sounds an ALARM (buzzer) if dangerously hot
  Because each action changes what the sensors read next, this is a
  closed-loop NEGATIVE FEEDBACK system — the same principle a thermostat
  or a living body uses to stay stable (homeostasis).
  It also LOGS data to the Serial Monitor so we can graph it.
  ===================================================================
*/

#include <DHT.h>                 // Temperature/humidity sensor library
#include <Servo.h>               // Servo motor library
#include <Wire.h>                // I2C communication (for the LCD)
#include <LiquidCrystal_I2C.h>   // 16x2 I2C LCD library

// ---------- PIN SETUP ----------
#define DHTPIN 7                 // DHT11 data pin
#define DHTTYPE DHT11            // We are using a DHT11 (not DHT22)
DHT dht(DHTPIN, DHTTYPE);        // Create the sensor object

int   ldrPin   = A0;             // LDR light sensor (analog)
int   ledPin   = 9;             // Grow-light LED (PWM ~)
int   servoPin = 6;             // Vent servo (PWM ~)
int   buzzPin  = 8;             // Alarm buzzer

Servo ventServo;                 // Create the servo object
LiquidCrystal_I2C lcd(0x27, 16, 2); // LCD at I2C address 0x27 (try 0x3F if blank)

// ---------- SETPOINTS (the "healthy range") ----------
// >>> TRY CHANGING THESE to suit your plant!
float ventTempC   = 26.0;        // Above this temperature, open the vent
float alarmTempC  = 32.0;        // Above this, sound the overheat alarm
int   darkLevel   = 400;         // LDR reading below this = "too dark" -> grow light on
// (LDR wired so MORE light = HIGHER reading; calibrate with the Serial Monitor)

void setup() {
  Serial.begin(9600);            // Open serial logging
  dht.begin();                   // Start the temperature sensor
  ventServo.attach(servoPin);    // Connect the servo
  ventServo.write(0);            // Start with the vent CLOSED (0 degrees)
  pinMode(ledPin, OUTPUT);
  pinMode(buzzPin, OUTPUT);
  lcd.init();                    // Start the LCD
  lcd.backlight();               // Turn the backlight on
  lcd.print("GreenKeeper :)");   // Friendly boot message
  Serial.println("time_s,tempC,humidity,light,vent_deg,growlight");
  delay(2000);
  lcd.clear();
}

void loop() {
  // ---------- INPUT: read all sensors ----------
  float tempC    = dht.readTemperature();   // degrees Celsius
  float humidity = dht.readHumidity();       // percent
  int   light    = analogRead(ldrPin);       // 0..1023

  // DHT11 sometimes returns "nan" (not a number); skip those reads
  if (isnan(tempC) || isnan(humidity)) {
    Serial.println("Sensor read failed, retrying...");
    delay(1000);
    return;
  }

  // ---------- PROCESS + OUTPUT: the feedback rules ----------

  // 1) VENT control (negative feedback on temperature)
  int ventAngle;
  if (tempC > ventTempC) {
    ventAngle = 90;                 // Too hot -> OPEN the vent
  } else {
    ventAngle = 0;                  // Comfortable -> keep vent CLOSED
  }
  ventServo.write(ventAngle);

  // 2) GROW LIGHT control (feedback on light)
  bool growOn;
  if (light < darkLevel) {
    digitalWrite(ledPin, HIGH);     // Too dark -> grow light ON
    growOn = true;
  } else {
    digitalWrite(ledPin, LOW);      // Bright enough -> grow light OFF
    growOn = false;
  }

  // 3) OVERHEAT ALARM
  if (tempC > alarmTempC) {
    tone(buzzPin, 1000);            // 1000 Hz warning tone
  } else {
    noTone(buzzPin);               // Silence
  }

  // ---------- OUTPUT: live status on the LCD ----------
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("T:"); lcd.print(tempC, 1); lcd.print("C ");
  lcd.print("H:"); lcd.print(humidity, 0); lcd.print("%");
  lcd.setCursor(0, 1);
  lcd.print(growOn ? "Light:ON " : "Light:OFF");
  lcd.print(ventAngle > 0 ? " Vent" : " Shut");

  // ---------- DATA LOGGING: print one CSV row to Serial ----------
  Serial.print(millis() / 1000); Serial.print(",");
  Serial.print(tempC);           Serial.print(",");
  Serial.print(humidity);        Serial.print(",");
  Serial.print(light);           Serial.print(",");
  Serial.print(ventAngle);       Serial.print(",");
  Serial.println(growOn ? 1 : 0);

  delay(2000);   // Read/act every 2 seconds (>>> change for faster/slower logging)
}

/*
  // ===== EXTENSIONS WE COULD ADD =====
  // 1) PROPORTIONAL vent: map() temperature to a vent angle 0..90 so the
  //    vent opens gradually instead of snapping open/closed.
  // 2) A soil-moisture sensor + a small pump/LED to simulate watering.
  // 3) "Hysteresis": open the vent at 26C but only close it again below
  //    24C, to stop the servo flapping back and forth near the threshold.
  // 4) Save data to an SD card for all-day logging.
*/
```

---

## 6. Sample Data Table & Results

We ran two trials with a desk lamp shining on the box for 10 minutes: **Control OFF** (vent taped shut, no grow light) vs **GreenKeeper ON**.

| Time (min) | Temp °C — Control OFF | Temp °C — GreenKeeper ON | Light (ON trial) | Vent angle | Grow light |
|:----------:|:---------------------:|:------------------------:|:----------------:|:----------:|:----------:|
| 0 | 22.0 | 22.0 | 520 | 0° | OFF |
| 2 | 24.5 | 24.0 | 530 | 0° | OFF |
| 4 | 27.1 | 26.2 | 535 | 90° (opened) | OFF |
| 6 | 29.8 | 25.9 | 210 (lamp moved) | 90° | ON |
| 8 | 31.6 | 25.6 | 205 | 90° | ON |
| 10 | 33.2 (alarm!) | 25.4 | 215 | 90° | ON |

**What the numbers show:**
- With **GreenKeeper ON**, temperature peaked at **26.2 °C** and settled near **25.5 °C** — inside our 22–26 °C target.
- With the **control OFF**, temperature climbed to **33.2 °C**, past the alarm point.
- When we dimmed the light at minute 6, the grow light switched ON automatically (light reading dropped below 400).

[Suggested graph: a line chart with **time on the x-axis** and **temperature on the y-axis**, two lines (Control vs GreenKeeper). Google image search: **"line graph two series temperature over time"** for a model.]

---

## 7. Analysis & Conclusion
Our hypothesis was **supported**. The closed-loop system held the enclosure within the healthy range (peak 26.2 °C) while the uncontrolled box overheated to 33.2 °C — a **7 °C** difference by minute 10. The vent opening let warm air escape (a real example of **heat transfer** and **thermodynamics**), and the grow light responded correctly to falling **light** levels.

This works because of **negative feedback**: every time the temperature rose past the setpoint, the system acted to bring it *back down*, just like a thermostat or the way our bodies sweat to cool off (**homeostasis**). The system "decides" without a human — that's **systems thinking** in action: Input → Process → Output → Feedback.

**Sources of error / limits:** the DHT11 is only accurate to about ±2 °C and updates slowly; the servo vent is small; and near the 26 °C threshold the vent sometimes flapped open/closed (which is why our extension list includes **hysteresis**).

---

## 8. Reflection
- **Proudest of:** getting four different parts (sensor, servo, LED, LCD) to work together as one smart system.
- **Biggest challenge:** the LCD was blank at first — we learned the Mega uses **SDA 20 / SCL 21** (not A4/A5) and that we had to try I2C address **0x3F** instead of 0x27.
- **What we'd improve:** add **hysteresis** so the vent doesn't jitter, and log data to an SD card to run a full-day experiment.
- **What we learned:** how engineers use **feedback** to keep systems stable, and how to turn raw sensor numbers into a real, graphable experiment.

---

## 9. Why This Is an "Exemplary" Project (Rubric Tie-In)
- **Science Concept (4):** clearly demonstrates feedback, heat transfer, and light, with correct vocabulary and a real-world link (thermostat/homeostasis).
- **Engineering Design (4):** multi-part, reliable build that solves a real problem; shows iteration (hysteresis idea).
- **Code Quality (4):** organized, fully commented, handles sensor errors (`isnan`), logs data.
- **Presentation/Reflection (4):** a testable hypothesis, a controlled experiment, a data table, a graph, an honest analysis of limits, and clear next steps.
