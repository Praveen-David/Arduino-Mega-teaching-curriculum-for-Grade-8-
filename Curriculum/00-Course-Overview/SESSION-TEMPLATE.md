# Session File Template & Authoring Standards

Every session file MUST follow this exact structure and section order. File naming:
`Session-XX-short-title.md` (e.g., `Session-02-blink-an-led.md`), zero-padded.

Tone: student-facing = friendly, encouraging, Grade-8 reading level. Teacher-facing = professional, detailed, assumes NO prior coding knowledge.

---

## REQUIRED SECTIONS (in this order)

### 1. SESSION HEADER
- `# Week X, Session Y — <Title>`
- **Phase:** name
- **Learning Objectives** (3–4 bullets, start with action verbs)
- **Science Curriculum Link:** which Grade 8 concept + 1–2 sentences how it connects
- **Materials Checklist (per pair):** bulleted list with quantities & component values

### 2. TEACHER GUIDE (45-minute breakdown)
Use these exact time blocks as sub-headings:
- **0–5 min — Hook / Warm-Up:** a question or quick demo (give the actual question + expected answers)
- **5–15 min — Direct Instruction:** concept explanation WITH a concrete analogy; give the teacher the words to say
- **15–35 min — Hands-On Build + Code:** numbered steps for building circuit and entering/uploading code
- **35–42 min — Testing & Debugging:** what success looks like + a troubleshooting table (Symptom → Fix)
- **42–45 min — Reflection / Exit Ticket:** the actual exit-ticket question(s)

### 3. CIRCUIT DIAGRAM
- ASCII-art wiring diagram in a code block
- A "Fritzing-style build description": numbered connections naming **pin number, component, value, and wire color**
- A `[Google image search: "..."]` term so the teacher can find a visual

### 4. ARDUINO CODE
- A fenced ```cpp code block with a COMPLETE sketch
- Top comment block = the SCIENCE EXPLANATION
- Every meaningful line commented in plain English for beginners
- Clearly mark experiment points with `// >>> TRY CHANGING THIS:`
- A `// ===== CHALLENGE =====` commented section at the bottom
- (Sessions 1 & some review sessions may have minimal/no code — note that explicitly)

### 5. STUDENT WORKSHEET
Render as a self-contained sub-document with:
- Title & objectives (student-friendly)
- **What I Already Know** — 2–3 warm-up questions
- **Build It — Step by Step** — numbered instructions (diagrams described in words)
- **Predict!** — "What do you think will happen if…?" questions BEFORE testing
- **Observe / Data Table** — a markdown table to fill in
- **What Did You Notice?** — 3–4 reflection questions
- **Science Connection** — "How does this relate to <topic>?"
- **Challenge Extension** — a stretch task

### 6. SAFETY NOTES
- Component-specific hazards for THIS session
- Safe handling reminders
- "If something goes wrong" quick actions

### 7. ASSESSMENT RUBRIC
- For graded sessions: full 4-level table (Beginning/Developing/Proficient/Exemplary) across
  the 5 criteria (Circuit Construction, Code Functionality, Science Understanding, Collaboration, Reflection).
- For non-graded sessions: a short "Formative Check (not graded)" — 2–3 look-fors the teacher observes.

### 8. DIFFERENTIATION
- **Support:** simplified version / scaffolds
- **Extension:** for early finishers
- **Visual / Kinesthetic accommodations**

---

## STYLE RULES
- Use markdown headings, numbered steps, bullet points, and tables generously.
- All code uses clean C++ Arduino syntax and beginner comments.
- Always reinforce the safety mantra: **Build → Check → Power on** (wire only when unpowered).
- Reference real pin numbers consistent with the Course Overview pin card.
  - LCD I2C on Mega: **SDA = 20, SCL = 21**.
  - PWM pins marked `~`. Analog inputs A0–A15.
- Keep each session file complete and standalone (a sub teacher could pick it up cold).
- Wire color convention: **red = 5V/power, black = GND, other colors = signals**.
