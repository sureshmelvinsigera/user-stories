# Pedestrian Crossing the Street — Writing Acceptance Criteria

#### Learning Objectives

- Write acceptance criteria that are specific, measurable, and testable.
- Understand the relationship between a user story and its acceptance criteria.
- Distinguish between functional, non-functional, and negative acceptance criteria.
- Recognise when acceptance criteria are too vague, too broad, or missing entirely.

#### Introduction

A user story without acceptance criteria is a wish. It tells you what someone wants, but it gives no one — not the developer, not the tester, not the stakeholder — a way to confirm that the work is actually done. Acceptance criteria are the contract between "I want this" and "here is how we know it works."

This workshop picks up where the user stories workshop left off. You already know how to write Features, User Stories, and Given-When-Then scenarios for a pedestrian crossing a street. Now the question changes: how do you write acceptance criteria that are tight enough to test, clear enough to build from, and complete enough that nothing slips through?

#### What Acceptance Criteria Actually Are

Acceptance criteria are the conditions that a scenario must satisfy for the user story to be considered complete. They are not descriptions of how something should be built. They are descriptions of what the finished behaviour looks like from the outside.

| Acceptance Criteria Are | Acceptance Criteria Are Not |
|---|---|
| Conditions that must be true when the story is done | Implementation instructions or technical specifications |
| Written from the user's perspective | Written from the developer's perspective |
| Testable — someone can verify pass or fail | Vague aspirations like "should be user-friendly" |
| Specific to one story | Broad quality standards that apply to everything |
| The basis for QA test cases | The test cases themselves |

A useful mental model: if a tester cannot read your acceptance criteria and write a test from them without asking you a single follow-up question, they are not specific enough.

#### The Anatomy of Good Acceptance Criteria

Good acceptance criteria share a few properties regardless of the domain. They are specific, measurable, independent of implementation, and written in language that both business and technical stakeholders understand.

Here is what those properties look like applied to the pedestrian crossing.

##### Specific, Not Vague

The difference between a vague criterion and a specific one is the difference between a requirement you can test and one you cannot.

| Vague | Specific |
|---|---|
| The pedestrian should be able to cross safely | The pedestrian arrives on the opposite kerb before the signal changes to "Don't Walk" |
| The signal should change in a reasonable time | The walk signal activates within 90 seconds of the crossing button being pressed |
| The crossing should be accessible | Kerb ramps with a gradient no steeper than 1:12 exist on both sides of the crossing |
| Drivers should be aware of pedestrians | The crosswalk is illuminated by overhead lighting visible from at least 50 metres in both directions |

Every entry in the right column can be measured. Every entry in the left column invites an argument about what "safely," "reasonable," "accessible," and "aware" actually mean.

##### Measurable

If a criterion includes a number, a threshold, or a defined state, it is measurable. If it relies on words like "quickly," "properly," "adequately," or "appropriately," it is not.

Some criteria are naturally binary — the ramp exists or it does not. Others need a defined threshold:

- The countdown timer displays at least 15 seconds of crossing time for a standard-width two-way street.
- The audible walk indicator produces a sound level between 60 and 80 decibels, audible above ambient traffic noise.
- The walk signal remains active for a minimum duration calculated at a walking speed of 1.2 metres per second across the full crosswalk width.

That last one is worth noting. The criterion does not say "long enough." It defines the calculation. A tester can measure the crosswalk, do the arithmetic, and check the signal duration. No ambiguity.

##### Independent of Implementation

Acceptance criteria describe what, not how. They should survive a complete change in the underlying technology without needing to be rewritten.

| Implementation-Dependent (Avoid) | Implementation-Independent (Preferred) |
|---|---|
| The system uses a 555 timer IC to control signal duration | The walk signal remains active for the calculated minimum crossing duration |
| The LED array displays a walking figure icon | The walk signal is clearly distinguishable from the "Don't Walk" signal in daylight and at night |
| The ramp is constructed from poured concrete with a brushed finish | The ramp surface provides sufficient traction for wheelchair tyres in wet conditions |

If the city replaces the timer chip, swaps LEDs for a different display, or rebuilds the ramp with a different material, the acceptance criteria on the right still apply. The ones on the left become obsolete the moment the technology changes.

#### Three Types of Acceptance Criteria

Not all acceptance criteria do the same job. They fall into three categories, and a complete user story needs all three.

##### Functional Criteria

These describe what the system does when things go right. They map directly to the base case scenarios.

For the user story "As a pedestrian, I want to cross the street safely when the walk signal is active, so that I reach the other side without being struck by traffic":

- The walk signal activates only when all vehicle signals in conflicting directions show red.
- The walk signal remains active for a minimum duration based on a crossing speed of 1.2 metres per second.
- The countdown timer begins displaying remaining seconds at the start of the flashing "Don't Walk" phase.
- Vehicle signals do not change to green until the walk signal phase and clearance interval have fully elapsed.

Each of these is a condition that must be true for the happy path to work. If any one fails, the story is not done.

##### Non-Functional Criteria

These describe how well the system performs, not what it does. They cover qualities like timing, visibility, accessibility, and durability.

| Quality | Acceptance Criterion |
|---|---|
| Responsiveness | The walk signal activates within 90 seconds of the crossing button being pressed |
| Visibility | The walk and "Don't Walk" signals are legible from a distance of 30 metres in direct sunlight |
| Audibility | The audible walk indicator is distinguishable from ambient noise at the kerb edge |
| Durability | Crosswalk markings remain visible after 12 months of normal traffic wear |
| Accessibility | Kerb ramps on both sides have a gradient no steeper than 1:12 and a minimum width of 1.2 metres |

Non-functional criteria are the ones most often left out, and they are frequently the ones that cause the most problems after delivery. A signal that activates but takes five minutes to respond technically meets the functional criteria. The non-functional criterion — 90 seconds — is what makes it usable.

##### Negative Criteria

These describe what should not happen. They map to corner cases and failure modes, and they are essential for preventing dangerous outcomes.

- The walk signal does not activate while any conflicting vehicle signal shows green.
- The countdown timer does not reset or restart once the flashing "Don't Walk" phase has begun.
- The vehicle signal does not change to green while any pedestrian is still within the crosswalk detection zone.
- The crossing button does not trigger a walk signal more than once per signal cycle, even if pressed multiple times.
- The system does not enter a state where both the walk signal and a conflicting vehicle green signal are active simultaneously.

That last criterion is the most critical one on the list. It describes a conflict state that should be physically impossible. Writing it down forces the question: what safeguard ensures this cannot happen? If no one can answer that, you have found a gap.

#### Acceptance Criteria for Each User Story

Here is what a complete set of acceptance criteria looks like when attached to the user stories from the previous workshop.

##### User Story 1 — Pedestrian Crossing a Two-Way Street

User Story: As a pedestrian, I want to cross the street safely when the walk signal is active, so that I reach the other side without being struck by traffic.

Functional criteria:

- The walk signal activates only when vehicle signals in both directions show red.
- The walk signal remains active for a minimum duration calculated at 1.2 metres per second across the full crosswalk width.
- The countdown timer begins at the start of the flashing "Don't Walk" phase and counts down to zero.
- Vehicle signals remain red until the walk phase and clearance interval have fully elapsed.
- Pressing the crossing button registers a request that is acknowledged within 2 seconds by a visible or audible confirmation.

Non-functional criteria:

| Quality | Criterion |
|---|---|
| Signal response time | Walk signal activates within 90 seconds of button press during normal operation |
| Signal visibility | Walk and "Don't Walk" indicators are legible from 30 metres in daylight and at night |
| Countdown accuracy | Timer counts down in real-time seconds with no skipping or freezing |
| Crosswalk visibility | Markings are visible to approaching drivers from a minimum distance of 50 metres |

Negative criteria:

- The walk signal does not activate while any conflicting vehicle signal shows green.
- The countdown timer does not reset once the "Don't Walk" phase has begun.
- The vehicle signal does not change to green while any pedestrian remains in the crosswalk.
- The system does not allow the walk signal and a conflicting green signal to be active at the same time.

##### User Story 2 — Wheelchair User Crossing a Two-Way Street

User Story: As a pedestrian who uses a wheelchair, I want both sides of the crossing to have kerb ramps, so that I can safely enter and exit the roadway.

Functional criteria:

- Kerb ramps exist on both sides of every signalised crossing.
- Each ramp connects the footpath to the road surface with no vertical lip greater than 6 millimetres.
- The ramp surface is flush with the crosswalk surface at the road level.
- Tactile warning indicators are installed at the top of each ramp to signal the transition from footpath to road.

Non-functional criteria:

| Quality | Criterion |
|---|---|
| Gradient | Ramp slope does not exceed 1:12 (approximately 8.3%) |
| Width | Ramp is at least 1.2 metres wide to accommodate standard wheelchairs |
| Surface traction | Ramp surface provides sufficient grip in wet conditions — no pooling water on the ramp |
| Maintenance | Ramp is free of cracks, debris, or obstructions that would prevent wheelchair passage |

Negative criteria:

- No signalised crossing has a ramp on one side but not the other.
- No ramp is blocked by permanent fixtures such as poles, bollards, or street furniture within the ramp width.
- No ramp directs the wheelchair user into traffic rather than onto the crosswalk.
- The ramp surface does not deteriorate to the point where a vertical lip greater than 6 millimetres forms at the road edge.

#### Common Mistakes — What Bad Acceptance Criteria Look Like

Recognising bad acceptance criteria is just as important as writing good ones. Here are the patterns that show up most often.

##### Too Vague

| Criterion | Why It Fails |
|---|---|
| The crossing should be safe | "Safe" is not measurable — safe for whom, under what conditions? |
| The signal should work properly | "Properly" has no definition anyone can test against |
| The ramp should be accessible | Accessible by what standard? What gradient, width, surface? |

The fix is always the same: replace the adjective with a measurement, a threshold, or a defined state.

##### Too Broad

| Criterion | Why It Fails |
|---|---|
| All pedestrians can cross the street at any time | This is a Feature, not a criterion — it covers dozens of scenarios |
| The system handles all weather conditions | Which conditions? Rain, ice, fog, extreme heat? Each needs its own criterion |
| The crossing meets all accessibility standards | Which standards? ADA? Local building codes? ISO 21542? Name them |

Broad criteria create an illusion of coverage. They feel complete on paper, but no one can test them without first decomposing them into the specific criteria you should have written in the first place.

##### Missing Negative Criteria

This is the most dangerous mistake. If you only write criteria for what should happen, you have said nothing about what must not happen. The conflict state — walk signal and green vehicle signal active at the same time — is the kind of defect that only gets caught if someone explicitly wrote a negative criterion prohibiting it.

A good rule of thumb: for every functional criterion, ask "what is the opposite of this, and would that opposite be dangerous or unacceptable?" If the answer is yes, write a negative criterion.

##### Untestable

| Criterion | Why It Fails |
|---|---|
| The pedestrian feels safe while crossing | Feelings cannot be objectively tested |
| The signal change is smooth | "Smooth" has no measurable definition |
| The crossing experience is intuitive | Intuitive to whom? Under what conditions? |

If a criterion contains a word that two reasonable people could disagree about, it is not testable. Replace it with something observable.


#### A Checklist for Reviewing Acceptance Criteria

Before you consider a user story complete, run each acceptance criterion through these questions:

| Question | If the Answer Is No |
|---|---|
| Can a tester verify this criterion as pass or fail? | Rewrite it with a measurable condition |
| Does this criterion belong to this specific story? | Move it to the correct story, or split it into its own |
| Is this criterion free of implementation details? | Rewrite to describe the what, not the how |
| Would this criterion survive a technology change? | Remove the technology-specific language |
| Have you included negative criteria? | Ask "what must not happen?" for each functional criterion |
| Is each criterion a single, atomic condition? | Split compound criteria into separate items |
| Can two people read this and agree on what it means? | Remove ambiguous adjectives and add thresholds |

If every criterion passes this checklist, your story is ready for development and testing. If any criterion fails even one question, it needs rework before the story moves forward.

#### Summary

- Acceptance criteria are the contract between the user story and the finished work. Without them, a story is a wish — there is no way to confirm it is done, no basis for testing, and no shared understanding of what "complete" actually means.

- Good criteria are specific, measurable, and independent of implementation. If a criterion uses words like "properly," "safely," or "reasonable" without defining what those mean in numbers or defined states, it is not ready. The fix is always the same: replace the adjective with a threshold.

- Three types of criteria cover the full picture. Functional criteria describe what happens when things go right. Non-functional criteria describe how well it happens — timing, visibility, durability, accessibility. Negative criteria describe what must not happen, and they are the most commonly omitted and the most dangerous to leave out.

- Common mistakes follow predictable patterns: too vague, too broad, missing negatives, or untestable. The review checklist — can a tester verify it, does it belong to this story, is it free of implementation details, would it survive a technology change — catches all of them before they become expensive problems downstream.

- Acceptance criteria are where BA work becomes engineering. The user story is the human side — who wants what and why. The acceptance criteria are the precision side — the exact conditions under which the story is satisfied. Both are necessary. Neither is sufficient alone.
