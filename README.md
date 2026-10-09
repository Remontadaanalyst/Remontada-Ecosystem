# Remontada-Ecosystem
 ⚡ The Remontada Protocol: A Tactical Ecosystem
**Engineering the Comeback: A 3-Part Offline-First Tactical Analytics Architecture**

Football matches are lost in the margins—a missed tactical shift at half-time, a player losing focus in the 70th minute, a miscalculated pass. The Remontada Protocol is a proprietary, end-to-end tactical ecosystem engineered to solve the three biggest blind spots in modern coaching: network-congested live data capture, momentum-blind spatial models, and result-biased cognitive profiling.

---

## 📱 Phase 1: Remontada Command Center (Touchline Telemetry Suite)
### **Tag. Detect. Report.** — *Offline-First Distributed State Machine & Heuristic Scouting Engine*

A complete offline-first football match analysis and talent scouting system built for the sidelines[cite: 5]. **Zero backend. Zero API keys. Pure browser power[cite: 5].**

### 🧠 Master System Architecture Flowchart
```mermaid
flowchart TB
    subgraph TAB1["📡 Tab 1: Spatial Input (index.html)"]
        A[Touch / Click Event] --> B["Bounding Rect Normalization<br/>X = round(clientX - rect.left)<br/>Y = round(clientY - rect.top)"]
        B --> C{"Goal-Zone Check<br/>0.36 ≤ Y/H ≤ 0.64<br/>X ≤ 0.07W or X ≥ 0.93W"}
        C -- "True" --> D["Mutate Payload:<br/>eventType: 'shot' | result: 'goal'"]
        C -- "False" --> E["Standard Payload:<br/>eventType: 'tap' | result: null"]
    end

    subgraph IPC["🌉 Cross-Context IPC Bus (Window.localStorage)"]
        D & E --> F[("Key: 'remontada_live_tap'<br/>JSON Serialization (<5ms)")]
    end

    subgraph TAB2["🎛️ Tab 2: Touchline Console (commad-center.html)"]
        F -. "window.onstorage Listener" .-> G["pendingLocation Buffer<br/>{x, y, matchTime}"]
        
        H["🎙️ Hold-to-Speak (>350ms)<br/>Web Speech API + JSGF Grammar"] --> I["Regex Homophone Sanitizer<br/>+ Lexical Number Accumulator"]
        I --> J["DOM Traversal: Auto-Select<br/>.jersey-number Node"]
        
        G & J --> K["⚡ fuseEvent() State Machine"]
        K --> L["In-Memory LIFO Stack<br/>(masterMatchData[])"]
        K --> M[("Persistent Local DB<br/>Key: 'scoutai_matches'")]
        L -- "pop() / slice(-10)" --> N["O(1) Undo Rollback &<br/>10-Tap Film Bookmarking"]
    end

    subgraph TAB3["💎 Module 3: RAE Debiasing (bias-detector.html)"]
        O["Raw Scout Rating (0-100)<br/>+ Birth Month (m ∈ 1..12)"] --> P["Quarterly Multiplier Lookup<br/>β_m ∈ [1.40 (Jan) .. 0.70 (Dec)]"]
        P --> Q["Age-Adjusted Formulation<br/>R_adj = min(100, round(R_raw / β_m))"]
        Q --> R{"Hidden Gem Heuristic<br/>R_raw < 70 ∧ R_adj > 85"}
        R -- "True" --> S["⭐ Flag Undervalued Prospect"]
    end

    subgraph TAB4["📄 Module 4: Dossier Engine (report-generatorai.html)"]
        M & S --> T["Match JSON Schema Ingestion"]
        T --> U["Spatial & Temporal Heuristics<br/>Flank Overload (35%/65% W)<br/>High-Press Index (X > 0.5W)"]
        T --> V["5-Axis Polar-to-Cartesian<br/>Inline SVG Radar Generator"]
        T --> W["Conditional NLG Verdict<br/>Grade Assignment (A / B+ / B)"]
        U & V & W --> X["html2canvas (Scale: 2) + jsPDF<br/>Client-Side A4 PDF Export"]
    end
🔬 Deep-Dive Engineering LogicCross-Context IPC & Spatial Normalization (index.html ↔ commad-center.html): Decouples coordinate plotting from roster selection using the browser's Window.localStorage event loop as an asynchronous IPC bus. Touch vectors are normalized relative to the responsive pitch container ($900 \times 550\text{px}$) and checked against goal-mouth hitboxes for auto-goal detection.   Hold-to-Speak Voice NLP State Machine: Custom wrapper around the Web Speech API with JSGF Acoustic Grammar constraining the search space to number tokens + action verbs. A phonetic sanitizer corrects stadium noise homophones (e.g., /\bfor\b/g $\rightarrow$ "four").   Dual-State Persistence & Deterministic LIFO Stack: Fused events are pushed to an in-memory array (masterMatchData) and committed to localStorage (scoutai_matches). A 10-frame windowing function flags tactical build-up sequences for video review, while $O(1)$ LIFO rollbacks delete erroneous entries.   Relative Age Effect (RAE) Debiasing: Corrects physical maturation bias in youth scouting by normalizing raw performance scores against empirical birth-month representation multipliers ($\beta_m \in [1.40 \dots 0.70]$).   Zero-Dependency SVG Radar & PDF Engine: Parses match JSON to compute polar-to-Cartesian coordinates for inline SVG radar charts, piping the DOM directly into html2canvas and jsPDF for 1-click A4 export.   📐 Phase 2: Ghost Ball EV (Vectorized Spatial Physics Engine)Track. Project. Exploit. — Zero-Loop SIMD Pitch Control, Ray-Cast Passing Interference & Biomechanical KinematicsStandard football tracking data only tells coaches where players are. Ghost Ball EV is a continuous off-ball scoring opportunity engine that evaluates all $7,072$ square meters of the pitch ($104 \times 68$ grid) simultaneously at $>60\text{ FPS}$ in pure vectorized NumPy—identifying open half-spaces, defender momentum traps, and optimal "Ghost Ball" passing targets in real time.   🧠 Master Spatial-Physics Architecture FlowchartCode snippetflowchart TB
    subgraph CV["🎥 1. Computer Vision & Kinematics (main.py)"]
        A["Broadcast Frame (1280x720)"] --> B["YOLOv8m + ByteTrack<br/>Classes: 0 (Person), 32 (Ball)"]
        B --> C["Perspective Homography (cv2.findHomography)<br/>PixelFeet (u,v) ➔ Pitch Meters (X∈[0,104], Y∈[0,68])"]
        B --> D["Upper-Torso HSV Extraction<br/>K-Means (k=2) + 30-Frame Temporal Voting"]
        C --> E["Central-Difference Kinematics<br/>Velocity Clamping (v_max ≤ 10.0 m/s) & Linear Ball Interp"]
    end

    subgraph ENGINE["⚡ 2. Vectorized 7,072-Cell SIMD Physics (remontada_engine.py)"]
        E & D --> F["Phase 1 & 2: Non-Linear xT Grid (104x68)<br/>+ Dynamic 2nd-Last Defender Offside Masking"]
        F --> G["Phase 3: Time-to-Intercept Kinematics<br/>t_player = t_react + (d / v_max) + τ_turn(cos θ)"]
        G --> H["Logistic Sigmoid Arrival Waves<br/>P_i(g) = 1 / (1 + exp(-3.2 * (t_ball - t_player)))"]
        H --> I["Phase 4: Vectorized Ray-Cast Interference<br/>Gaussian Lane Shadowing (σ_block) Across All Defenders"]
        I --> J["Phase 5: Localized Carrier Pressure (R ≤ 5.0m)<br/>+ Backward Safe-Pass Gaussian Boost + 10-Frame EMA"]
    end

    subgraph AR["👁️ 3. Broadcast AR Projection (remontada_overlay.py)"]
        J --> K{"Laser Tripwire Check<br/>0.08 < t < 0.95 ∧ d_ray < 1.15m"}
        K -- "Blocked" --> L["Render Crimson Ray +<br/>9-Shard Vector Shatter ('BLOCKED')"]
        K -- "Clear" --> M["Inverse Homography Projection (H⁻¹)<br/>Render Pulsing Ghost Ball Orb + Dominance Ripples"]
    end
🔬 Deep-Dive Engineering LogicPlanar Homography & Kinematics (main.py): Translates 2D broadcast pixels into true meters using a $3 \times 3$ homography matrix ($H$), anchoring ground contact strictly to bounding-box feet. Unsupervised HSV K-Means clustering with a 30-frame temporal vote classifies teams robustly.   Dynamic xT & Offside Masking: The continuous xT matrix is dynamically bounded by a real-time offside line tracking the second-last defender's $X$-coordinate.   Biomechanical Turnaround Penalty ($\tau_{\text{orient}}$): A defender sprinting away from the ball incurs a penalty up to $0.85\text{s}$ to decelerate and pivot, computed via vectorized dot products ($\cos\theta = \frac{\vec{v} \cdot \vec{d}}{\Vert{}\vec{v}\Vert{} \Vert{}\vec{d}\Vert{}}$).   Vectorized Ray-Cast Interference: Casts $7,072$ rays from the ball to every grid cell, projecting all $M_{\text{def}}$ defenders onto every ray simultaneously. Gaussian shadowing degrades arrival probability based on perpendicular proximity to the ray.   EMA Filtering & Laser Tripwire: 10-frame Exponential Moving Average ($\alpha \approx 0.18$) stabilizes the EV grid. The peak $\text{EV}$ coordinate is rendered as a pulsing Ghost Ball unless intercepted by a defender's $1.15\text{m}$ reach, triggering the BLOCKED laser tripwire.   🧠 Phase 3: AI Psychiatrist (Cognitive Profiling & Computer Vision)Lock. Track. Analyze. — Measuring "La Pausa", Tunnel Vision, and Cognitive Decay via CPU-Optimized VisionThe AI Psychiatrist Engine translates raw broadcast pixels into a live cognitive profile—quantifying mental fatigue, spatial scanning frequency, and Bayesian fault isolation without wearable GPS trackers.   🧠 Master Cognitive Pipeline FlowchartCode snippetflowchart TB
    subgraph STAGE1["🎯 Stage 1 & 2: Acquisition & Ball Tracking"]
        A["cv2.selectROI() Target Locking"] --> B["Save Target BBox + Name to JSON"]
        C["Class 32 YOLO Ball Scanner"] --> D{"Ball Lost?"}
        D -- "Yes (<30 frames)" --> E["Memory Buffer Interpolation"]
        D -- "No" --> F["Log {cx, cy, visible: True}"]
    end

    subgraph STAGE3["🧠 Stage 3: Telemetry & BotSORT Tracking"]
        B & F --> G["YOLOv8s + BotSORT (IOU > 0.2 Matching)"]
        G --> H["Macro-Heading Filter (Lookback = 5 frames)"]
        H --> I["Circular Standard Deviation (σ > 10.0° ➔ Scanning)"]
        I --> J["10-Frame Possession State Machine Buffer"]
    end

    subgraph STAGE4["⏱️ Stage 4: Orchestrator & Verdict Logic"]
        J --> K["Cognitive Decay (e^(-k*t))"]
        J --> L["Accidental Genius Check (Vector Dot Product)"]
        J --> M["Tactical Blueprint (V_adj & Space Value Delta)"]
        K & L & M --> N["Bayesian Fault Isolation P(A|E)"]
    end

    subgraph RENDER["🎬 HUD Rendering"]
        N --> O["Project Vision Cones & 0-100% Focus Bars"]
        O --> P["Overlay 'La Pausa' vs 'Hesitation' Text"]
    end
🔬 Deep-Dive Engineering LogicBotSORT Target Handshake: User-drawn ROIs are locked to YOLO bounding boxes via IoU $> 0.20$ matching.   Circular Standard Deviation for Spatial Scanning: To prevent grapple-jitter from reading as scanning, a 5-frame macro-heading filter extracts net displacement. Circular statistics (averaging $\cos\theta$ and $\sin\theta$) correctly compute angular variance. $\sigma > 10.0^\circ$ triggers a validated shoulder check (gold HUD vision cone).   10-Frame Possession Buffer: Prevents temporal timer wipes when the ball is occluded behind a referee. The processing timer ($t_{\text{proc}}$) runs through interference, ensuring "La Pausa" calculations aren't destroyed by visual noise.   Cognitive Mathematical Models: Focus degradation under pressure is modeled as exponential decay $C(t_d) = e^{-k \cdot t_d}$. "Accidental Genius" verifies true premeditation by checking if the initial scan vector $\vec{u}$ aligns with the final pass vector $\vec{v}$ within $10.0^\circ$.   Bayesian Fault Isolation: Employs Bayes' Theorem ($P(A \mid E) = \frac{P(E \mid A) P(A)}{P(E)}$) to distribute blame probabilistically across the Front 3 based on structural failure likelihoods, eliminating result-biased scouting.
