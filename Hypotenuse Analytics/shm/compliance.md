# Standards-Focused Briefing for Hypotenuse Analytics  

---

## 1. What the standards cover  

### IRC 112 (2020) - Code of Practice for Concrete Road Bridges  
*Scope*: Design, construction and durability of reinforced- and prestressed-concrete road bridges, footbridges and culverts.  
*Key requirements*:  
- Minimum concrete grades (M25 for ordinary, M30 + for severe exposure) and exposure-driven cover, cement content and w/b limits [1].  
- Limit-state design (ULS, SLS, durability, fatigue) with separate checks for strength, serviceability (deflection, crack width) and corrosion risk [1].  
- Continuous monitoring is encouraged for compliance with serviceability limits (strain-to-deflection conversion) [2].  
*Intended outcome*: Safe, service-able, durable bridges that retain structural integrity throughout the design life.

### IRC 70-2017 - Guidelines for Regulation & Control of Mixed Traffic & Bridge Inspection  
*Scope*: Inspection, maintenance and repair of all classes of bridges in India, plus guidance on mixed-traffic urban roads.  
*Key requirements*:  
- Classification of inspections (routine, detailed, special) and prescribed frequencies.  
- Adoption of advanced technologies (e.g., drones, IoT sensors) for condition assessment [3].  
- Documentation of findings, defect grading and maintenance planning.  
*Intended outcome*: Systematic, risk-based bridge asset management that prevents sudden failures.

### IS 16700 (2017/2023) - Criteria for Structural Safety of Tall Concrete Buildings  
*Scope*: Reinforced-concrete buildings taller than 50 m or with height-to-width ratio > 4.  
 *Key requirements*:  
- Minimum concrete grade M30 + and reinforcement ratios (≥0.4 % longitudinal, ≥0.25 % transverse) [4].  
- Serviceability limits for wind-induced drift (H/500) and seismic drift (H/250) [5].  
- Mandatory 3-D dynamic analysis, time-history seismic analysis for very tall or irregular structures [5].  
*Intended outcome*: Buildings that remain stable under wind and earthquake actions, with controlled inter-storey drift and occupant comfort.

### IS 1893 (Part 1 2002) - Criteria for Earthquake-Resistant Design of Structures  
*Scope*: All structures (buildings, bridges, dams, retaining walls, etc.) subject to seismic loading in India.  
*Key requirements*:  
- Definition of seismic zones, importance factors and design spectra.  
- Load combinations, base shear calculation, drift limits (0.004 h for life-safety) [6].  
- Special provisions for bridges (see Part 3) and tall buildings (refer IS 16700).  
*Intended outcome*: Uniform seismic safety across the country, reducing loss of life and property.

### ISO/IEC 42001 (2023) - AI Management System (AIMS)  
*Scope*: Management of AI systems in any sector; establishes a governance framework for trustworthy AI.  
*Key requirements*:  
- AI policy, risk assessment, impact assessment, lifecycle management, and continual improvement [7].  
- Annex A lists 38 controls (e.g., AI roles, data inventory, bias mitigation, third-party oversight) [8].  
- Mandatory internal audits and senior-leadership commitment [9].  
*Intended outcome*: AI that is ethical, transparent, secure and compliant with emerging regulations.

### BIS IS 17428 (2020) - Trustworthy AI Standard (Parts 1 & 2)  
*Scope*: Establishes a Data-Privacy Management System (Part 1) and engineering-management guidelines (Part 2) for AI deployments in India.  
*Key requirements*:  
- Management-system requirements for privacy-by-design, data-handling, consent and breach reporting [10].  
- Optional engineering guidelines for model validation, explainability and bias testing [10].  
*Intended outcome*: AI solutions that respect privacy, are auditable and inspire public trust.

---

## 2. Who uses each standard  

| Standard | Typical Users | Common Sectors / Project Types |
|---|---|---|
| IRC 112 | Highway authorities (NHAI, MoRTH), bridge designers, contractors, asset owners, third-party auditors | National highways, state roads, urban bridges, culverts |
| IRC 70-2017 | Bridge owners (public agencies, private toll operators), inspection firms, maintenance contractors, railway authorities | All bridge classes, mixed-traffic urban corridors |
| IS 16700 | Structural consultants, high-rise developers, EPC contractors, building owners, municipal building-approval bodies | Residential towers, office skyscrapers, mixed-use podium-tower complexes |
| IS 1893 | Seismic-design engineers, government disaster-management agencies, consultants, bridge and dam owners | All new construction in seismic zones, retro-fit projects |
| ISO/IEC 42001 | Technology firms, AI product developers, regulated industries (finance, health, transport), corporate AI governance teams | AI-enabled services, predictive maintenance platforms, autonomous systems |
| BIS IS 17428 | AI solution providers, data-centric enterprises, regulators, privacy officers | Any AI-driven product that processes personal data, especially in public-sector deployments |

---

## 3. Why the standards were created  

*Safety & Risk Management* - IRC 112 and IS 1893 were drafted to codify proven engineering practices that prevent catastrophic bridge or building failures, especially under extreme loads or earthquakes.

*Regulatory Uniformity* - India’s rapid infrastructure growth required a single, nationally recognised reference (IRC 112, IS 16700) to avoid fragmented design approaches.

*Lifecycle Asset Management* - IRC 70-2017 responds to the need for systematic inspection cycles and data-driven maintenance, reducing life-cycle costs.

*Societal Trust in AI* - ISO/IEC 42001 and BIS IS 17428 address growing public concern over bias, opacity and privacy in AI, aligning Indian practice with global governance trends.

*Economic Efficiency* - By mandating performance-based design and continuous monitoring, the standards aim to extend asset life, lower repair budgets and improve public confidence.

---

## 4. How Hypotenuse Analytics satisfies each standard  

### IRC 112 & IRC 70-2017  
- **24 × 7 sensor network** (IoT, fiber-optic, GNSS) delivers the continuous strain/deflection data the code requires for serviceability verification [2].  
- **AI-driven anomaly detection** flags corrosion-induced strain patterns between scheduled inspections, meeting the “continuous monitoring” intent of IRC 70-2017.  
 - **Automated compliance reports** map measured deflections to the allowable limits in Clause 12 of IRC 112, providing auditable evidence for regulators.

### IS 16700 & IS 1893  
- **Real-time seismic response tracking** uses accelerometers and edge AI to compute instantaneous inter-storey drift, directly supporting the drift limits (H/500 wind, H/250 seismic) of IS 16700 [5].  
- **Post-event digital twin updates** allow engineers to re-run time-history analyses prescribed by IS 1893, ensuring that post-earthquake assessments are evidence-based.  
- **Predictive maintenance scoring** aligns with the “structural safety” objectives of IS 16700 by forecasting fatigue life of critical members.

### ISO/IEC 42001 & BIS IS 17428  
- **AI Governance Layer** implements the policy, risk-assessment and impact-assessment clauses of ISO 42001 (AI policy, AI roles, data inventory) [8].  
- **Privacy-by-Design data pipelines** encrypt sensor streams, log consent and support breach reporting, satisfying BIS IS 17428 Part 1 requirements [10].  
- **Audit-ready logs** (model versioning, bias metrics, explainability dashboards) meet Annex A controls such as “AI impact assessment” and “third-party oversight” [9].

### Cross-Standard Benefits  
- **Unified data repository** eliminates silos, fulfilling both SHM reporting (IRC 70) and AI data-management (ISO 42001).  
- **Secure cloud-native architecture** aligns with ISO 27001 (often referenced alongside ISO 42001) and supports regulator-approved data handling.

---

## 5. Mapping Matrix (Features ↔ Standard Clauses)  

| Hypotenuse Feature | IRC 112 (SLS/Deflection) | IRC 70-2017 (Inspection) | IS 16700 (Drift & Monitoring) | IS 1893 (Seismic Response) | ISO/IEC 42001 (A-Controls) | BIS IS 17428 (Privacy) |
|---|---|---|---|---|---|---|
| **IoT / Fiber-optic strain gauges** | Provides continuous strain → deflection verification (Clause 12) [2] | Supplies data for periodic condition reports (Section 4) [3] | Feeds real-time drift calculations (Clause 6.2) [5] | Captures ground-motion acceleration for base-shear checks (Clause 4) [6] | Recorded as AI data asset (A.4.2) [8] | Encrypted at source, audit-logged (Part 1, §4) [10] |
| **Edge AI anomaly detection** | Flags serviceability breaches between inspections (IRC 112) [1] | Generates alerts for inspection-triggered actions (IRC 70) [3] | Detects drift exceedance in real time (IS 16700) [5] | Identifies abnormal seismic response post-event (IS 1893) [6] | Implements “AI risk treatment” (A.5.3) [8] | Provides explainable alerts to satisfy transparency (Part 2, §5) [10] |
| **Physics-Informed Neural Networks (PINNs)** | Improves prediction of concrete creep/deflection, satisfying durability SLS (IRC 112) [1] | Supplies model-based forecasts for inspection planning (IRC 70) [3] | Generates drift forecasts under wind loads (IS 16700) [5] | Simulates seismic demand for performance-based design (IS 1893) [6] | Covered under “AI system life-cycle” (A.6) [8] | Model provenance recorded for audit (Part 1, §5) [10] |
| **Digital Twin & RUL scoring** | Enables “remaining useful life” reporting for bridge decks (IRC 112) [2] | Provides condition-based maintenance schedule (IRC 70) [3] | Supplies drift-trend dashboards for tall structures (IS 16700) [5] | Offers post-earthquake performance index (IS 1893) [6] | Aligns with “AI performance evaluation” (Clause 9) [7] | Stores RUL data under privacy-by-design controls (BIS IS 17428) [10] |
| **Secure Cloud & Audit Logs** | Ensures data integrity for regulatory submission (IRC 112) [1] | Meets documentation requirements for bridge audits (IRC 70) [3] | Supports traceability of drift calculations (IS 16700) [5] | Provides forensic evidence after seismic event (IS 1893) [6] | Satisfies “AI governance documentation” (Clause 4-10) [7] | Guarantees GDPR-like audit trails (BIS IS 17428) [10] |

---

## 6. Sample proposal language  

> **Compliance-Enabled SHM** - “Our platform continuously records strain, acceleration and environmental data, automatically translating raw measurements into the deflection and drift limits defined in **IRC 112** and **IS 16700**. The AI engine generates real-time alerts whenever a measured value approaches the serviceability thresholds, delivering the same evidential rigor required by **IRC 70-2017** inspections, but without the six-month lag.”

> **AI Governance Assurance** - “All AI models are governed by an ISO/IEC 42001-aligned Management System. We maintain a live AI policy, perform quarterly impact assessments and retain immutable audit logs, thereby meeting the 38 Annex A controls and the privacy-by-design obligations of **BIS IS 17428**.”

> **Regulatory Reporting Pack** - “At the end of each reporting period we deliver a standards-compliant dossier that includes: (i) bridge-deck deflection tables mapped to **IRC 112** Clause 12, (ii) drift-trend charts for tall structures aligned with **IS 16700** Clause 6.2, (iii) seismic response summaries per **IS 1893** Part 1, and (iv) AI governance dashboards satisfying **ISO 42001** Clause 9 and **BIS IS 17428** Part 1.”

### Pitch bullet points (for slide decks)

- **Continuous Insight** - 24 × 7 sensor + AI loop replaces periodic manual inspections (IRC 70).  
- **Code-Ready Data** - Directly feeds into bridge-deflection and building-drift calculations mandated by IRC 112, IS 16700 and IS 1893.  
- **Trusted AI** - Built on ISO/IEC 42001 and BIS IS 17428 frameworks; transparent, auditable, privacy-first.  
- **Lifecycle Savings** - Early-failure prediction cuts repair costs by up to 30 % (industry case studies).  
- **One-Stop Reporting** - Auto-generated compliance packages ready for regulators, auditors and insurers.

---

## 7. Compliance-ready reporting templates  

The Indian SHM Guidelines explicitly recommend “regular reporting, threshold-based alert generation and feature extraction” for bridge and building monitoring [11]. Hypotenuse provides a pre-configured template that includes:

1. **Instrumented Asset Register** - Sensor IDs, locations, calibration dates (fulfills IRC 70 §4).  
2. **Serviceability Dashboard** - Real-time strain-to-deflection conversion tables with pass/fail flags per IRC 112 Clause 12.  
3. **Drift & Acceleration Log** - Hourly drift percentages plotted against IS 16700 H/500 and IS 1893 0.004 h limits.  
4. **AI Governance Summary** - Policy version, risk register, impact-assessment outcomes, audit-log excerpt (ISO 42001 Clause 4-9).  
5. **Privacy & Data-Handling Statement** - Consent records, encryption status, breach-response plan (BIS IS 17428 Part 1).

These outputs are exportable in PDF, CSV and JSON, enabling seamless submission to NHAI, municipal engineering departments, or AI regulatory bodies.

---

**Bottom line:** By embedding continuous instrumentation, physics-informed AI, digital-twin analytics and a full AI-governance framework, Hypotenuse Analytics not only satisfies the technical performance criteria of India’s bridge, building and seismic codes but also delivers the responsible-AI assurance demanded by ISO/IEC 42001 and BIS IS 17428. The result is a single, audit-ready solution that turns compliance from a periodic burden into a strategic advantage.

---

### Sources
- [1] https://law.resource.org/pub/in/bis/irc/irc.gov.in.112.2020.pdf
- [2] https://www.ishms.org.in/assets/Docs/ISHMS_SHM%20Guidelines.pdf
- [3] https://www.roadvision.ai/blog/irc-code-70-2017-guidelines-for-inspection-and-maintenance-of-bridges
- [4] https://www.ijfmr.com/papers/2024/2/18198.pdf
- [5] https://infralens.in/code/IS-16700-2017
- [6] https://law.resource.org/pub/in/bis/S03/is.1893.1.2002.pdf
- [7] https://www.bsigroup.com/en-US/products-and-services/standards/iso-42001-ai-management-system
- [8] https://mindsetcyber.com.au/iso-42001-controls-list
- [9] https://aws.amazon.com/blogs/security/ai-lifecycle-risk-management-iso-iec-420012023-for-ai-governance
- [10] https://foxmandal.in/is-17428-a-new-privacy-assurance-standard-in-india
- [11] https://irispublishers.com/ctcse/pdf/CTCSE.MS.ID.000767.pdf
