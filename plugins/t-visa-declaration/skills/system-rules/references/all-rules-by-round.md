# System Rules Log

Records all rules added to the pipeline from attorney feedback and draft-vs-final comparisons.

---

## Round 1 — Initial Rules (Pre-Jose Mauricio)

Rules added during initial development from first feedback rounds:
- **Active Voice**: Trafficker as subject of action verbs
- **Sexual Violence Language**: "sexual slave/servitude" only
- **Background Limit**: STRICTLY 2 paragraphs
- **Process Language**: EMP Process uses trafficking verbs
- **Threats in Quotes**: Direct quoted speech for threats
- **Escape Barriers**: Explicitly explain why victim couldn't escape
- **PP Trafficking Nexus**: Every PP paragraph causally connected
- **Country Conditions**: Required in Extreme Hardship
- **Pivotal Event Context**: Key incidents need contextual setup
- **Paragraph Opening Variety**: Max 2-3 open with trafficker's name

---

## Round 2 — Jose Mauricio Case (2026-02-16)

**Source**: Attorney feedback on Jose Mauricio draft + draft-vs-final comparison report

**Attorney's core insight**: "por cómo está escrito, el tráfico está débil, pero en realidad reescribiendo la declaración tendríamos un caso muy fuerte." — The facts support a strong case but the writing frames it as VAWA instead of trafficking.

### CRITICAL RULES ADDED

#### RULE: VAWA-vs-TRAFFICKING FILTER
- **Feedback**: "Hasta el párrafo 5 no hay nada de tráfico. Suena mucho a VAWA." / paras 10, 11 also sound VAWA
- **Pattern**: Paragraphs describing control/abuse without connecting to labor/servitude/financial exploitation read as domestic violence (VAWA), not trafficking
- **Rule**: Every trafficking paragraph must connect MEANS (force/fraud/coercion) to ENDS (labor/servitude/financial exploitation). Paragraph self-test required.
- **Files modified**: `phase-3-section-trafficking.md`, `phase-4-system.md` (check 6h), `quality-benchmarks.md`
- **Pass/Fail**: FAIL if any trafficking paragraph only describes emotional abuse without tying to exploitation End

#### RULE: MEANS-ENDS MANDATORY PAIRING
- **Feedback**: "Tenemos un Mean muy fuerte pero no está atado a ningún End" (para 12)
- **Pattern**: Means (threats, force) disconnected from Ends = VAWA, not trafficking
- **Rule**: Every paragraph describing Means must explicitly connect to a specific End
- **Files modified**: `phase-3-section-trafficking.md`, `phase-4-system.md` (check 6i), `quality-benchmarks.md`
- **Pass/Fail**: FAIL if orphaned Means without Ends

#### RULE: COERCED COMPLIANCE FRAMING
- **Feedback**: "'I would do those chores fast and right, because my goal was to keep the peace.' Entonces el lo hacía voluntariamente. No fue traficado."
- **Pattern**: Language showing voluntary compliance destroys trafficking framing
- **Rule**: All compliance must be framed as fear-driven, never voluntary/peace-seeking
- **Files modified**: `phase-3-section-trafficking.md`, `phase-4-system.md` (check 6j), `quality-benchmarks.md`
- **Pass/Fail**: FAIL if voluntary-sounding compliance language

### IMPORTANT RULES ADDED

#### RULE: SURVEILLANCE AS TRAFFICKING TOOL
- **Feedback**: "'I would send my location because I was trying to avoid accusations.' Suena a VAWA no a T visa"
- **Pattern**: Surveillance framed as relationship dynamics instead of labor/service control
- **Rule**: All monitoring/surveillance must be framed as labor/earnings/service control mechanisms
- **Files modified**: `phase-3-section-trafficking.md`, `phase-4-system.md` (check 6h), `quality-benchmarks.md`
- **Pass/Fail**: FAIL if surveillance framed as jealousy/VAWA

#### RULE: FINANCIAL EXPLOITATION FRAMING
- **Feedback**: "'I was the one paying for everything' No, 'Vanessa forced me to pay for everything... describir que cosas pagaba, montos, % salario'"
- **Pattern**: Financial control needs trafficking framing + specifics
- **Rule**: Trafficker as active agent forcing payment + items, amounts, % salary, work hours
- **Files modified**: `phase-3-section-trafficking.md`, `quality-benchmarks.md`
- **Pass/Fail**: FAIL if generic "I paid for everything"

#### RULE: LABOR BURDEN DOCUMENTATION
- **Feedback**: "describir el horario en que él trabajaba, horas extras, si ella lo llamaba al trabajo"
- **Pattern**: Work burden details must be tied to trafficker's demands
- **Rule**: Include hours, overtime, workplace monitoring tied to financial demands
- **Files modified**: `phase-3-section-trafficking.md`, `quality-benchmarks.md`
- **Pass/Fail**: FAIL if forced labor described without scope of burden

#### RULE: CONTROL MECHANISM ESTABLISHMENT
- **Feedback**: "cómo fue eso de que le quitó la tarjeta, se la sacó de la billetera? Le exigió que se la diera?"
- **Pattern**: Control methods need HOW they were established
- **Rule**: Each control method must explain the mechanism of how trafficker gained control
- **Files modified**: `phase-3-section-trafficking.md`, `quality-benchmarks.md`
- **Pass/Fail**: FAIL if control just appears without mechanism

#### RULE: ENFORCEMENT MECHANISM REQUIRED
- **Feedback**: "Cómo lo forzaba? Además de insultarlo le amenazaba con algo?"
- **Pattern**: "She forced me" without explaining HOW is not enough
- **Rule**: Every forced action must specify the enforcement mechanism (threats + consequences)
- **Files modified**: `phase-3-section-trafficking.md`, `quality-benchmarks.md`
- **Pass/Fail**: FAIL if "forced" without explaining how

#### RULE: SERVITUDE CONTRAST
- **Feedback**: "expliquemos que ella no hacía nada... obligándolo a él a ser su sirviente"
- **Pattern**: Domestic servitude needs power dynamic contrast
- **Rule**: Contrast trafficker's idleness/refusal to contribute with victim's forced labor
- **Files modified**: `phase-3-section-trafficking.md`, `quality-benchmarks.md`
- **Pass/Fail**: FAIL if no power dynamic contrast shown

#### RULE: RECRUITMENT INFORMATION EXTRACTION
- **Feedback**: "Vanessa le preguntaba cosas de la vida de él, sus traumas... él creyó que ella lo hacía porque estaba interesada"
- **Pattern**: Recruitment must show HOW trafficker extracted vulnerabilities
- **Rule**: Show trafficker learning victim's weaknesses under pretense of genuine interest
- **Files modified**: `phase-3-section-trafficking.md`, `quality-benchmarks.md`
- **Pass/Fail**: FAIL if recruitment is just "we started a relationship"

#### RULE: THREAT IMPACT ELABORATION
- **Feedback**: "'I didn't want her to start with the threats' — hay que colocar el miedo que le daban esas amenazas"
- **Pattern**: Threats need described fear and believed consequences
- **Rule**: Always describe the FEAR threats produced and specific consequences victim believed would happen
- **Files modified**: `phase-3-section-trafficking.md`, `quality-benchmarks.md`
- **Pass/Fail**: FAIL if threats mentioned without describing their terrifying effect

#### RULE: QUANTIFICATION IMPACT CHECK
- **Feedback**: "'it would take me about half an hour.' Solo media hora?"
- **Pattern**: Time/effort quantities must support, not undermine, the claim
- **Rule**: Contextualize time estimates to show burden, or frame broader workload
- **Files modified**: `phase-3-section-trafficking.md`, `quality-benchmarks.md`
- **Pass/Fail**: FAIL if raw time estimate minimizes forced labor

#### RULE: CHILD SEPARATION FRAMING
- **Feedback**: "'My son stayed with her, and I told her it was okay' — Suena a que no quería al hijo"
- **Pattern**: Child separation framed as voluntary undermines the case
- **Rule**: Parent framed as wanting child but unable to take custody due to trafficking
- **Files modified**: `phase-3-section-trafficking.md`, `phase-4-system.md` (check 6n), `quality-benchmarks.md`
- **Pass/Fail**: FAIL if separation sounds voluntary

#### RULE: PP CONTENT BOUNDARY
- **Feedback**: "el PP incluye información que en realidad pertenece al hardship"
- **Pattern**: PP contains hardship content (country fears, life-built-here)
- **Rule**: PP must ONLY contain trafficking-nexus content; country/life content goes to EH
- **Files modified**: `phase-3-section-physical-presence.md`, `phase-4-system.md` (check 6k), `quality-benchmarks.md`
- **Pass/Fail**: FAIL if PP contains non-trafficking-nexus content

#### RULE: LEA BREVITY + ALTRUISTIC FRAMING
- **Feedback**: "El LEA cooperation estaba bien antes ahora está muy largo"
- **Pattern**: LEA over-expanded into evidence inventory
- **Rule**: 1 concise paragraph with cooperation intent + altruistic motivation
- **Files modified**: `phase-3-section-law-enforcement.md`, `phase-4-system.md` (check 6m), `quality-benchmarks.md`
- **Pass/Fail**: FAIL if reads as evidence inventory

#### RULE: CONCLUSION BREVITY
- **Source**: Draft-vs-final comparison (Finding 8)
- **Pattern**: Conclusion recaps entire case instead of focused plea
- **Rule**: 3-5 sentences max. Plea, not summary.
- **Files modified**: `phase-3-section-conclusion.md`, `phase-4-system.md` (check 6l), `quality-benchmarks.md`
- **Pass/Fail**: FAIL if conclusion recaps trafficking details

### ADDITIONAL RULES FROM COMPARISON REPORT (implemented simultaneously)

#### RULE: SCENE-GROUNDING
- **Source**: Comparison Finding 2
- **Pattern**: Abstract pattern descriptions instead of specific trigger scenes
- **Rule**: At least one specific trigger scene per major control method
- **Files modified**: `phase-3-section-trafficking.md`, `quality-benchmarks.md`

#### RULE: ESCALATION TRIGGER
- **Source**: Comparison Finding 3
- **Pattern**: "Things got worse" without tying to specific life event
- **Rule**: Tie escalation to specific life events (pregnancy, moving in, injury)
- **Files modified**: `phase-3-section-trafficking.md`, `quality-benchmarks.md`

#### RULE: CONVERSATIONAL AUTHENTICITY
- **Source**: Comparison Finding 9
- **Pattern**: Declaration reads as legal template, not person telling their story
- **Rule**: Include 3-5 natural speech markers (memory anchors, hedging, self-corrections)
- **Files modified**: `phase-3-section-trafficking.md`, `quality-benchmarks.md`

#### RULE: RECRUITMENT NARRATIVE VOICE
- **Source**: Comparison Finding 11
- **Pattern**: Recruitment narrated with retrospective trafficking analysis
- **Rule**: Narrate from client's perspective at the time, not with hindsight
- **Files modified**: `phase-3-section-trafficking.md`, `quality-benchmarks.md`

#### RULE: PP FORWARD-LOOKING
- **Source**: Comparison Finding 10
- **Pattern**: PP re-narrates trafficking instead of focusing on present consequences
- **Rule**: Brief trafficking reference, then focus on present consequences and current needs
- **Files modified**: `phase-3-section-physical-presence.md`, `quality-benchmarks.md`

#### RULE: BACKGROUND BOUNDARY (US-met traffickers)
- **Source**: Comparison Finding 5
- **Pattern**: Background introduces trafficker even when met in the US
- **Rule**: If trafficker was met in the US, Background ends at arrival; trafficker opens Trafficking
- **Files modified**: `phase-3-section-background.md`, `quality-benchmarks.md`

---

## Round 3 — Paragraph Opening Variety Strengthening (2026-02-16)

**Source**: Jose Mauricio v2 draft review — rule existed but was violated

**Issue**: The Paragraph Opening Variety rule existed in Round 1 but was insufficiently enforced. The rule said "max 2-3 paragraphs should open with the trafficker's name as the very first word" — but v2 draft had 9 of 14 trafficking paragraphs with the trafficker as subject of the first main clause (including paragraphs like "After I moved in, Vanessa forced..." where a short prepositional phrase preceded the name). The Phase 4 check wasn't catching these violations.

#### RULE: PARAGRAPH OPENING VARIETY — STRENGTHENED
- **Feedback**: V2 draft had 9/14 trafficking paragraphs opening with "Vanessa" as first-clause subject, violating the max 2-3 limit
- **Pattern**: The rule was too narrow ("as the very first word") and didn't catch paragraphs where a brief prepositional phrase preceded the trafficker's name
- **Rule Strengthening**: Clarified that the 2-3 limit applies to ALL paragraphs where the trafficker is the subject of the FIRST MAIN CLAUSE, even if preceded by a short time/location marker like "After I moved in, Vanessa..." or "At home, Vanessa..." The opening sentence should establish the victim's experience, the emotional impact, or the pattern/technique BEFORE introducing the trafficker as agent.
- **Phase 4 Check**: Strengthened the check to count first-clause subjects, not just literal first words
- **Files modified**: `phase-3-section-trafficking.md`, `phase-4-system.md`, `quality-benchmarks.md`
- **Pass/Fail**: FAIL if more than 3 trafficking paragraphs have the trafficker's name as the subject of the first main clause

**Acceptable paragraph openers that DON'T count against limit**:
- Victim's experience: "My paycheck never felt like mine. She forced me..."
- Emotional impact: "The threat I feared most was deportation, and she used it..."
- Pattern/technique: "The insults were not random — they were tools..."
- Full contextual scene: "Every evening after I finished work, I faced more unpaid labor at home. She forced me..."

**Examples that DO count against limit** (even though trafficker isn't literally first word):
- "After I moved in, Vanessa forced..." — trafficker is subject of first main clause
- "At home, Vanessa forced..." — trafficker is subject of first main clause

**Critical distinction**: This rule changes HOW paragraphs BEGIN, not WHO acts. The trafficker must still be the active subject of action verbs within the paragraph body. This is about opening sentence structure for readability and natural flow, not about removing active voice.

---

## Round 4 — Maria De Los Angeles Melgar Guevara Comparison (2026-02-17)

**Source**: Draft-vs-Final comparison report (`decs - Maria De Los Angeles Melgar Guevara/comparison-report.md`)
**Core findings**: 11 total (3 CRITICAL, 5 IMPORTANT, 3 MINOR). Biggest systemic issues: over-verbosity (draft 80% longer than final), PP massive over-expansion, missing psychological coercion extraction, argumentative tone instead of experiential voice.

### PHASE 1 RULES ADDED

#### RULE: PSYCHOLOGICAL COERCION EXTRACTION
- **Source**: Comparison Finding 6
- **Pattern**: Pipeline missed "be grateful" / opportunity scarcity manipulation as a distinct Means category. Final included a full paragraph on supervisors saying "nobody else would hire you," "people lining up," "doing you a favor" — draft had none of this.
- **Rule**: Extract psychological manipulation as distinct from direct threats: "be grateful" framing, opportunity scarcity claims, gaslighting about conditions, minimizing exploitation. These are Means (fraud/coercion) and must be captured separately.
- **Files modified**: `phase-1-system.md` (Rule 8), `phase-1-user.md` (added `means_psychological_manipulation` array)
- **Pass/Fail**: FAIL if psychological manipulation statements present in transcript but not extracted

#### RULE: TASK PROCEDURAL DETAIL
- **Source**: Comparison Finding 8
- **Pattern**: Draft described work as "tagged clothing, packed boxes" while final described step-by-step: "put tickets on garments, added hangers, re-bagged items, repacked them, placed into correct boxes for different stores."
- **Rule**: Extract procedural SEQUENCE of labor tasks, not just categories. The chain shows scope of forced labor.
- **Files modified**: `phase-1-system.md` (Rule 9), `phase-1-user.md` (added `specific_tasks_sequence` field)
- **Pass/Fail**: FAIL if labor tasks extracted as generic categories when transcript provides step-by-step detail

#### RULE: PROMISED vs ACTUAL CONDITIONS
- **Source**: Comparison Finding 10
- **Pattern**: Final established the promised schedule (8AM-4:30PM) before describing exploitative reality (6AM-1AM). Draft jumped straight to exploitation without the baseline contrast. Promise-vs-reality gap is evidence of fraud.
- **Rule**: Extract BOTH what was promised/stated AND what was actually imposed for schedule, pay, and duties.
- **Files modified**: `phase-1-system.md` (Rule 10), `phase-1-user.md` (added `promised_schedule`, `promised_pay`, `actual_schedule`, `actual_pay`, `overtime_details` fields)
- **Pass/Fail**: FAIL if transcript mentions promised conditions but only actual conditions are extracted

#### RULE: DEPARTURE CONTEXT
- **Source**: Comparison Finding 11
- **Pattern**: Final includes "my mother insisted I go with my older sister... even though I did not want to leave" — shows reduced agency. Draft frames departure as client's survival decision without this nuance.
- **Rule**: Extract WHO decided the departure and whether the client wanted to go. Involuntary departure increases vulnerability.
- **Files modified**: `phase-1-system.md` (Rule 11), `phase-1-user.md` (added `departure_voluntary`, `departure_decided_by`, `client_wanted_to_leave` fields)
- **Pass/Fail**: FAIL if departure circumstances present in transcript but not captured

#### RULE: MULTIPLE TRAFFICKING EPISODES FLAG
- **Source**: Comparison Finding 1
- **Pattern**: Draft included 4 paragraphs of journey/coyote trafficking that the attorney removed entirely, focusing only on workplace trafficking. Pipeline lacked mechanism to flag multiple episodes for strategic attorney decision.
- **Rule**: When transcript describes distinct trafficking episodes (different perpetrators/contexts), extract each separately and flag the count. This enables Phase 2 to make strategic scope recommendations.
- **Files modified**: `phase-1-system.md` (Rule 12), `phase-1-user.md` (added `trafficking_episodes_flag` section)
- **Pass/Fail**: FAIL if multiple distinct trafficking episodes present but not separately flagged

### PHASE 3 + PHASE 4 RULES ADDED (Length / Conciseness)

#### RULE: PP BREVITY
- **Source**: Comparison Finding 2
- **Pattern**: Draft PP was 3 paragraphs (~550 words), final was 1 paragraph (~80 words). PP re-narrated trafficking incidents (19-hour shifts, bus compartment, sexual servitude) instead of briefly referencing and focusing on present consequences. Root cause: PP prompt had 4 required elements each marked "(1 paragraph)" which structurally invited 3 paragraphs.
- **Rule**: PP must be 1 paragraph (2 maximum only with extensive medical evidence for BOTH physical AND mental health). Word target: 60-120 words. Must not exceed 150 words. Opening references trafficking in one clause, not a summary. No re-narration of specific trafficking incidents.
- **Files modified**: `phase-3-section-physical-presence.md` (restructured elements A-D into one paragraph, changed opening instruction, added word target), `phase-4-system.md` (added check 6o), `quality-benchmarks.md` (updated PP header, added brevity/word count checklist items and pass/fail rows), `phase-2-system.md` (updated PP paragraph target)
- **Pass/Fail**: FAIL if PP is 3+ paragraphs or >200 words or re-narrates trafficking incidents

#### RULE: EH CONCISENESS
- **Source**: Comparison Finding 4
- **Pattern**: Draft EH was 5 paragraphs (~850 words), final was 3 paragraphs (~350 words). EH re-elaborated sexual servitude and trafficking trauma already told in Trafficking and PP. The 4-paragraph recommended structure (Medical, Country, Family, Safety) defaulted to 4 paragraphs. No anti-re-narration rule existed for EH.
- **Rule**: EH must be 3 paragraphs maximum (4 only with extensive medical evidence). Word target: 250-400 words. Must not exceed 500 words. Safety and Country Conditions merged into one paragraph. Added EH Forward-Looking Rule: no re-narration of trafficking incidents, focus on future consequences of removal.
- **Files modified**: `phase-3-section-extreme-hardship.md` (reduced limit, added forward-looking rule, restructured to 3 paragraphs), `phase-4-system.md` (added check 6p), `quality-benchmarks.md` (updated EH header, added brevity/word count/forward-looking checklist items and pass/fail rows), `phase-2-system.md` (updated EH paragraph target)
- **Pass/Fail**: FAIL if EH is 5+ paragraphs or >500 words or re-narrates trafficking incidents

#### RULE: GLOBAL CONCISENESS + WORD COUNT TARGET
- **Source**: Comparison Finding 3
- **Pattern**: Draft was 5,180 words vs final's 2,870 words (80% longer). Every section over-expanded. Writer added argumentative analysis, restated points multiple ways, used retrospective legal framing. No conciseness constraint or word count target existed in the pipeline.
- **Rule**: Total declaration target 2,500-3,500 words (FAIL if >4,000). State facts once — no multi-angle restatement, no argumentative analysis. No retrospective legal analysis, systems-level conclusions, or pattern labeling. Declaration is first-person account, not legal brief. Per-section targets provided. Also added PP exception to global "3-8 sentences minimum" paragraph rule.
- **Files modified**: `phase-3-system.md` (added CONCISENESS RULE with word count target + PP sentence exception), `phase-4-system.md` (added check 8 for overall word count, updated proportions check 3), `quality-benchmarks.md` (added word count and re-narration pass/fail rows to Phase 3 and Phase 4 tables)
- **Pass/Fail**: FAIL if total declaration >4,000 words

---

## Round 5 — Maria De Los Angeles V2 Review (2026-02-17)

**Source**: V2 draft review — Spanish phrases appearing in English-language declaration

**Core finding**: V2 draft contained 6 instances of Spanish words/phrases/quotes ("¡Ya, ya, ya!", "peroles", "maletero", "El que no fuera que ya no se presente el lunes", "Si no queríamos trabajar, hay muchísima gente esperando por un trabajo", "Usted ya no hace parte de la empresa"). The V1 draft had 2 Spanish instances; V2 had 6 — the problem worsened. The existing instruction "Use direct quotes when available (in English)" was too subtle and was being ignored.

### CRITICAL RULE ADDED

#### RULE: ENGLISH-ONLY LANGUAGE
- **Source**: V2 draft of Maria De Los Angeles — 6 Spanish phrases in English declaration
- **Pattern**: Writer includes Spanish words/quotes from the transcript instead of translating to English. Happens with direct threats, exclamations, and cultural nouns. Root cause: Phase 1 extracts quotes in original language + English, and Phase 3 writer uses the Spanish version or both.
- **Rule**: Entire declaration must be in English. All quotes must be translated. No Spanish nouns as parenthetical terms. No Spanish exclamations. No "Spanish phrase — English translation" pattern. Only exception: attorney-approved cultural term with explicit "in Spanish we call it..." framing.
- **Files modified**: `phase-3-system.md` (added ENGLISH-ONLY RULE with 4 WRONG/RIGHT examples), `phase-3-section-trafficking.md` (strengthened quote instruction), `phase-4-system.md` (added check 7 LANGUAGE CHECK with common violations list), `quality-benchmarks.md` (added English-only to Phase 3 pass/fail, Phase 4 pass/fail, and Trafficking checklist)
- **Pass/Fail**: HARD FAIL if any Spanish text remains in the final declaration

---

## Round 6 — Template Alignment (2026-02-19)

*See `memory/MEMORY.md` Round 6 summary for rules added from attorney PDF template analysis.*

---

## Round 7 — Edgar Case (2026-02-19)

**Source**: Attorney feedback on Edgar draft (`desc - Edgar/feedback.md`)
**Full analysis**: `desc - Edgar/feedback-analysis.md`
**Core issues**: (1) Background over-expansion with vulnerability that undermines trafficking, (2) Agreement framing violation, (3) Missing specificity (wages, insults, threat transmission, enforcement), (4) Language that damages trafficking argument (firing fear, plural voice, Spanglish), (5) Forward-referencing concepts before introducing them.

### CRITICAL RULES ADDED

#### RULE: FIRING/VOLUNTARY EXIT FEAR PROHIBITION
- **Feedback**: "afraid they would fire me. esto daña por completo el argumento de trafico porque da a entender que la persona tenia posibilidades de renunciar"
- **Pattern**: Framing fear as "afraid of being fired" implies voluntary employment — victim COULD quit or be fired. This destroys trafficking because trafficking requires the victim is TRAPPED.
- **Rule**: NEVER frame victim's fear as fear of job loss. Fear must be about immigration consequences, physical harm, or destitution.
- **Files modified**: `phase-3-section-trafficking.md`, `phase-3-system.md`, `phase-4-system.md` (check 6v), `quality-benchmarks.md`
- **Pass/Fail**: HARD FAIL if any firing/job-loss fear language in declaration

#### RULE: FIRST-PERSON SINGULAR ONLY
- **Feedback**: "no podemos hablar en plural. el statement debe estar centrado en el cliente"
- **Pattern**: Declaration uses "us/we/our" for group experience, diluting individual trafficking claim and making it read like a class action complaint.
- **Rule**: "I/me" exclusively for victim experience. Never "us/we/our" for worker group. "We" acceptable only for family unit.
- **Files modified**: `phase-3-system.md` (VOICE RULES), `phase-4-system.md` (check 6u), `quality-benchmarks.md`
- **Pass/Fail**: HARD FAIL if "us/we/our" in victim-experience context

### IMPORTANT RULES ADDED

#### RULE: US VULNERABILITY PARAGRAPH PROHIBITION
- **Feedback**: ¶3 "no es necesario" — Background paragraph establishing desperation in the US
- **Pattern**: A "vulnerability in the US" paragraph establishes pre-trafficking desperation ("willing to take any job," "no options") which frames the case as VAWA (victim's vulnerability) instead of trafficking (trafficker's scheme).
- **Rule**: When trafficker met in US, Background ¶2 covers ONLY children/immigration context. No desperation narratives.
- **Files modified**: `phase-3-section-background.md`, `phase-4-system.md` (check 6ab), `quality-benchmarks.md`
- **Pass/Fail**: FAIL if Background establishes pre-trafficking desperation for US-met trafficker cases

#### RULE: WAGE EXPLOITATION FORMULA
- **Feedback**: "Digamos cuantas horas le pagaron vs cuantas horas trabajó"
- **Pattern**: Draft states flat payment amount but not the comparison. Fraud is proven by the math, not a dollar amount.
- **Rule**: When pay fraud involved, include: hours worked, promised rate, expected pay, actual pay, effective rate.
- **Files modified**: `phase-3-section-trafficking.md`, `phase-4-system.md` (check 6y), `quality-benchmarks.md`
- **Pass/Fail**: FAIL if pay fraud described without hours-vs-pay comparison

#### RULE: INSULT CONTENT SPECIFICITY
- **Feedback**: "Mencionemos los insultos"
- **Pattern**: Declaration says "insults" and "degrading language" but never states actual insults. Generic references lack persuasive power.
- **Rule**: Include actual insults or their specific nature. If unavailable, flag for attorney.
- **Files modified**: `phase-3-section-trafficking.md`, `phase-4-system.md` (check 6z), `quality-benchmarks.md`
- **Pass/Fail**: FAIL if only "insults" or "degrading language" without specifics

#### RULE: THREAT TRANSMISSION SPECIFICITY
- **Feedback**: "como le transmitian eso?" on "Everyone in that kitchen knew..."
- **Pattern**: Immigration threats described as ambient knowledge ("everyone knew") instead of specific acts of communication. USCIS needs specific coercion directed at the client.
- **Rule**: Show WHO told the client, WHEN, and WHAT exact words. Must be a specific scene.
- **Files modified**: `phase-3-section-trafficking.md`, `phase-4-system.md` (check 6aa), `quality-benchmarks.md`
- **Pass/Fail**: FAIL if threats described as atmosphere rather than specific communication acts

### MODERATE RULES ADDED

#### RULE: PLAIN LANGUAGE / NO IDIOMATIC EXPRESSIONS
- **Feedback**: "suena a spanglish" on "they had used to get me in the door"
- **Pattern**: English idioms that sound like literal translations from Spanish undermine credibility.
- **Rule**: Use plain, direct language. No idioms or colloquial expressions.
- **Files modified**: `phase-3-system.md` (VOICE RULES), `phase-4-system.md`, `quality-benchmarks.md`
- **Pass/Fail**: FAIL if contains Spanglish-sounding idioms

#### RULE: FORWARD REFERENCE PROHIBITION
- **Feedback**: "Es la primera vez que se menciona esto, pero está escrito como si se hubiese comentado antes"
- **Pattern**: Writer uses "the constant insults" with definite article and frequency words before the concept has been concretely introduced.
- **Rule**: Establish concrete facts first, then characterize the pattern. Never use "the [X]" or "constant/ongoing [X]" as first mention.
- **Files modified**: `phase-3-system.md` (CONSISTENCY RULES), `phase-4-system.md` (check 6x), `quality-benchmarks.md`
- **Pass/Fail**: FAIL if concepts characterized before concretely introduced

### EXISTING RULES STRENGTHENED

#### RULE: AGREEMENT FRAMING (Round 6) — STRENGTHENED
- **Feedback**: "Siempre tenemos que decir que el cliente acepta por lo que le prometen" on "I needed the work badly"
- **Action**: Added "I needed the work badly" as explicit BAD example. Added Phase 4 check (6w). Added sentence "Any sentence containing 'I needed,' 'I was desperate,' or 'I had no choice but to accept' in the agreement context is a FAIL."
- **Files modified**: `phase-3-section-trafficking.md`, `phase-4-system.md` (check 6w)

#### RULE: ENFORCEMENT MECHANISM REQUIRED (Round 2) — ELEVATED TO CRITICAL
- **Feedback**: "como lo forzaron?" on "forced me to clean the kitchen"
- **Action**: Elevated from MANDATORY to CRITICAL. Added labor-specific WRONG/RIGHT example. Added note that this rule is repeatedly violated across cases.
- **Files modified**: `phase-3-section-trafficking.md`

---

## Round 8 — Cesar Ortega Comparison (2026-03-02)

**Source**: Draft-vs-Final comparison (`examples/decs - Cesar Ortega/`)
**Core findings**: AI draft vs writer's declaration. Key issues: (1) AI includes details that undermine isolation/entrapment, (2) labor packed into dense paragraphs instead of granular focused ones, (3) compressed recruitment arc for gradual trafficking, (4) missing exploitation onset marker, (5) no separate aftermath paragraph.

### CRITICAL RULE ADDED

#### RULE: ISOLATION INTEGRITY
- **Source**: Comparison Findings 1 & 2
- **Pattern**: AI included third-party warnings to leave ("her daughters told me to leave," "my sister said the same") and family visits proving ongoing contact ("parents visited from Mexico"). These details UNDERMINE entrapment — USCIS can argue victim had outside support and was told to leave. Writer omitted all of these.
- **Rule**: Do NOT include details that prove victim had access to outside support: (a) third-party exit advice, (b) family visits proving contact, (c) dual-persona details (kind to visitors, cruel to victim = VAWA pattern). Exclude entirely or reframe ONLY as control mechanism.
- **Files modified**: `t-visa-plugin/agents/phase-3-writer.md`, `t-visa-plugin/agents/phase-4-reviewer.md` (check 34), `t-visa-plugin/skills/quality-benchmarks/SKILL.md`
- **Pass/Fail**: FAIL if declaration includes details proving victim had outside support that undermine entrapment

### IMPORTANT RULES ADDED

#### RULE: LABOR PARAGRAPH GRANULARITY
- **Source**: Comparison Finding 3
- **Pattern**: AI packed cooking + cleaning + enforcement into 1-2 dense paragraphs. Writer broke into separate focused paragraphs (cooking ¶ with procedural detail, cleaning ¶ with specific tasks, enforcement ¶, financial demands ¶). Each gets its own weight and Means-Ends pairing.
- **Rule**: Domestic servitude / forced labor must be broken into separate paragraphs by labor category. Each must include procedural detail (specific tasks, items, sequences) + its own enforcement mechanism.
- **Files modified**: `t-visa-plugin/agents/phase-3-writer.md`, `t-visa-plugin/agents/phase-4-reviewer.md` (check 35), `t-visa-plugin/skills/quality-benchmarks/SKILL.md`
- **Pass/Fail**: FAIL if all labor compressed into a single dense paragraph

#### RULE: RECRUITMENT PROGRESSION
- **Source**: Comparison Finding 4
- **Pattern**: AI condensed multi-year gradual recruitment (meeting → trust → job → cohabitation → exploitation) into 1 paragraph. Writer used 4 paragraphs showing each stage. The gradual progression shows HOW trafficking develops over time.
- **Rule**: For gradual/relationship-based trafficking, setup phase MAY span 2-4 paragraphs. Each paragraph = one recruitment stage. For immediate-captivity cases, 1 paragraph remains appropriate. Refines existing "First ¶ = Process/Setup" rule.
- **Files modified**: `t-visa-plugin/agents/phase-3-writer.md`, `t-visa-plugin/agents/phase-4-reviewer.md` (check 36), `t-visa-plugin/skills/quality-benchmarks/SKILL.md`
- **Pass/Fail**: FAIL if multi-year gradual recruitment compressed into a single paragraph

#### RULE: EXPLOITATION ONSET MARKER
- **Source**: Comparison Finding 7
- **Pattern**: Writer had a clear turning-point paragraph ("Everything started to change in 2020 during the COVID pandemic") marking when exploitation began. AI integrated temporal changes within existing paragraphs without a clear boundary.
- **Rule**: For cases where relationship predated exploitation, include a distinct short paragraph marking the turning point. Must identify: triggering change, how dynamic shifted, what new demands began.
- **Files modified**: `t-visa-plugin/agents/phase-3-writer.md`, `t-visa-plugin/agents/phase-4-reviewer.md` (check 37), `t-visa-plugin/skills/quality-benchmarks/SKILL.md`
- **Pass/Fail**: FAIL if exploitation begins abruptly without a turning-point marker in gradual cases

#### RULE: TRAFFICKING AFTERMATH PARAGRAPH
- **Source**: Comparison Finding 8
- **Pattern**: Writer dedicated a full paragraph to post-trafficking financial aftermath (debt, lost vehicle, financial devastation). AI mentioned some within the final incident paragraph but didn't separate it.
- **Rule**: If trafficking resulted in measurable financial/legal/practical aftermath, include a dedicated CLOSING paragraph in trafficking section. Must state concrete losses connected to trafficker's actions. Separate from the final incident paragraph.
- **Files modified**: `t-visa-plugin/agents/phase-3-writer.md`, `t-visa-plugin/agents/phase-4-reviewer.md` (check 38), `t-visa-plugin/skills/quality-benchmarks/SKILL.md`
- **Pass/Fail**: FAIL if aftermath folded into final incident paragraph when significant consequences exist

---

## Round 9 — Misael Sanchez Cubas Review (2026-03-03)

**Source**: Declaration review — 5 systemic issues identified
**Core issues**: (1) Substance/body control paragraphs missing trafficking Ends connection, (2) Passive voice in journey/process narration, (3) Vague witnessed-consequence enforcement, (4) Servitude contrast limited to domestic servitude only, (5) Introduction section outputting heading.

### CRITICAL RULE ADDED

#### RULE: SUBSTANCE/BODY CONTROL VAWA TRAP
- **Source**: Declaration review — forced substance use paragraph had Means but no Ends
- **Pattern**: Paragraphs describing forced drug use, alcohol, cigarettes, sleep deprivation, or body/lifestyle control show Means (anger, degradation, pressure) but no explicit connection to trafficking Ends (labor/servitude/escape prevention). This reads as general abuse (VAWA).
- **Rule**: Substance/body control paragraphs MUST explicitly connect to trafficking Ends — creating dependency to prevent escape, keeping victim compliant for forced labor, weakening resistance to servitude. Without Ends connection = VAWA.
- **Files modified**: `t-visa-plugin/agents/phase-3-writer.md` (VAWA filter sub-rule), `t-visa-plugin/agents/phase-4-reviewer.md` (check #4 expanded), `t-visa-plugin/skills/quality-benchmarks/SKILL.md` (trafficking requirements + new example), `t-visa-plugin/skills/system-rules/SKILL.md` (VAWA filter section)
- **Pass/Fail**: FAIL if substance/body control paragraph has no explicit trafficking Ends connection

### IMPORTANT RULES ADDED

#### RULE: ACTIVE VOICE — JOURNEY/PROCESS NARRATION ZONE
- **Source**: Declaration review — passive constructions "I was told to leave," "I was told to follow a guide," "I was still being forced to drive"
- **Pattern**: Journey and process narration (transportation, stash house orders, border crossing instructions) is a high-frequency zone for passive voice violations. Trafficker/handler giving orders loses grammatical subject position.
- **Rule**: Journey/process narration flagged as specific active voice violation zone. Trafficker/handler/armed men must remain grammatical subject in all travel/order narration.
- **Files modified**: `t-visa-plugin/agents/phase-3-writer.md` (active voice rule expanded), `t-visa-plugin/agents/phase-4-reviewer.md` (check #11 expanded), `t-visa-plugin/skills/quality-benchmarks/SKILL.md` (new active voice section), `t-visa-plugin/skills/system-rules/SKILL.md` (active voice section expanded)
- **Pass/Fail**: FAIL if journey narration uses passive voice

#### RULE: VAGUE-CONSEQUENCE PROHIBITION (ENFORCEMENT)
- **Source**: Declaration review — "I had seen what they did to people who complained" (WHAT?), "I did not know what they would do to me" (not specific enforcement)
- **Pattern**: Enforcement mechanisms described as vague witnessed consequences or fear of unknown rather than specific acts. These are unsubstantiated claims.
- **Rule**: Enforcement must state SPECIFIC consequences — WHAT the victim witnessed or WHAT threat was already established. "I had seen what they did" must say WHAT. "I did not know what they would do" must reference a known threat.
- **Files modified**: `t-visa-plugin/agents/phase-3-writer.md` (enforcement rule expanded), `t-visa-plugin/agents/phase-4-reviewer.md` (new check #39), `t-visa-plugin/skills/quality-benchmarks/SKILL.md` (enforcement requirement + new example), `t-visa-plugin/skills/system-rules/SKILL.md` (enforcement section expanded)
- **Pass/Fail**: FAIL if enforcement describes only vague consequences

#### RULE: SERVITUDE CONTRAST — ALL FORCED LABOR (BROADENED)
- **Source**: Declaration review — stash house case where traffickers "smoked, drank, used drugs" while victim cooked/cleaned/drove, but no contrast drawn because rule was limited to "domestic servitude"
- **Pattern**: Existing rule only triggered for "domestic servitude" cases. Stash house labor, forced driving, or other forced labor equally benefits from contrasting trafficker behavior.
- **Rule**: Broadened from "domestic servitude paragraphs" to "whenever victim performs forced labor of ANY type." Contrast must show trafficker's idleness/recreational behavior during the same period.
- **Files modified**: `t-visa-plugin/agents/phase-3-writer.md` (new explicit bullet), `t-visa-plugin/agents/phase-4-reviewer.md` (new check #40), `t-visa-plugin/skills/quality-benchmarks/SKILL.md` (broadened requirement), `t-visa-plugin/skills/system-rules/SKILL.md` (rule language broadened)
- **Pass/Fail**: FAIL if forced labor described without trafficker behavior contrast when facts are available

### MODERATE RULE ADDED

#### RULE: INTRODUCTION — NO HEADING
- **Source**: Declaration review — "I. INTRODUCTION" heading appearing in output
- **Pattern**: Phase 3 writer had no introduction-specific rules, so it defaulted to generating a heading like other sections. Phase 4 had the rule but it was not catching all violations.
- **Rule**: Introduction section writer must NOT output any heading. Introduction flows directly under declaration title and preamble. Added to Phase 3 writer (which previously had no FOR INTRODUCTION block) and strengthened in Phase 4.
- **Files modified**: `t-visa-plugin/agents/phase-3-writer.md` (new FOR INTRODUCTION block), `t-visa-plugin/agents/phase-4-reviewer.md` (check #32 strengthened), `t-visa-plugin/skills/quality-benchmarks/SKILL.md` (new introduction requirement), `t-visa-plugin/skills/system-rules/SKILL.md` (new structural rule)
- **Pass/Fail**: FAIL if any introduction heading appears
