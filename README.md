# Pedestrian Crossing the Street — A User Stories Workshop

#### Learning Objectives

- Decompose real-world behaviour into Features, User Stories, and Given-When-Then scenarios.
- Distinguish between base cases, corner cases, and dependencies within a single domain.
- Write acceptance criteria that are specific, testable, and traceable.
- Recognise how a change in persona surfaces entirely new requirements.

#### Introduction

A pedestrian crossing the street sounds like the simplest thing in the world. You wait, you walk, you arrive on the other side. But the moment you sit down and try to write requirements for it — who is crossing, what kind of street, what signals exist, what happens when something breaks — it stops being simple very quickly.

That tension is exactly why this exercise works. If you can identify every rule, exception, and assumption hiding inside a crosswalk, you can do the same for any business domain you will ever work in. This workshop walks through the thinking first — the framework, the different types of scenarios, and the hidden dependencies — then brings it all together into complete user stories at the end.

#### The Framework

Three concepts sit at the heart of this exercise, and they nest inside each other like layers.

| Concept | What It Does | Analogy |
|---|---|---|
| Feature | Names the area of behaviour being described | The chapter title |
| User Story | Captures who wants what and why | A single scene within that chapter |
| Given-When-Then | Validates the story with testable scenarios | The proof that the scene plays out correctly |

A Feature is the highest level. Think of it as broad enough to contain several related stories, but narrow enough that someone reading it knows exactly what part of the world you are describing. A good Feature for this workshop might be: "Pedestrian crosses a two-way street at a signalised intersection."

A User Story sits one level below the Feature. It captures who wants something, what they want, and why it matters to them. The format is deliberate:

- As a (persona) — tells you whose perspective you are writing from.
- I want to (action) — tells you what behaviour to design for.
- So that (outcome) — tells you why it matters.

If you cannot articulate the "so that," you probably do not understand the requirement well enough yet.

Given-When-Then is how you validate each story. The keywords each serve a distinct purpose:

| Keyword | Purpose |
|---|---|
| Given | The preconditions — what is true before the action happens |
| When | The trigger — the action the persona takes |
| Then | The expected outcome — what should be true after |
| And | Continues any of the above with additional conditions |
| But | Adds a negative qualifier to Given, When, or Then |

These three layers — Feature, User Story, Given-When-Then — give you a shared language that business stakeholders, developers, and testers can all read without translation.

#### Base Cases — The Happy Path

A base case is the scenario where everything works exactly as designed. No surprises, no failures, no unusual conditions. It is the first thing you write because it establishes what "normal" looks like, and every corner case you write later is a deviation from it.

For a two-way street with traffic signals, the base case has two sides:

| Scenario | Signal State | Pedestrian Behaviour | Expected Outcome |
|---|---|---|---|
| Crossing permitted | Walk signal shows "Walk," traffic lights red in both directions | Pedestrian steps onto crosswalk and walks across | Pedestrian arrives safely on opposite kerb |
| Crossing not permitted | Signal shows "Don't Walk" | Pedestrian observes the signal | Pedestrian remains on kerb and waits |

The second scenario sounds obvious, but writing it down matters. You are defining the boundary between "system working correctly" and "pedestrian ignoring the system," and those two things have very different implications.

Now consider the same crossing on a one-way street. The preconditions shift:

- Traffic flows in a single direction only.
- The vehicle signal for that direction is red.
- The walk signal shows "Walk."
- The pedestrian only needs to account for traffic from one direction.

That last point — "only needs to check one direction" — is worth pausing on. It is an assumption, and assumptions are where corner cases hide. What about a cyclist riding against traffic? An emergency vehicle approaching from the opposite direction? A delivery truck reversing out of a side alley? The one-way street feels simpler, but the simplicity is conditional.

| Street Type | Directions to Check | Feels Simpler? | Hidden Risks |
|---|---|---|---|
| Two-way | Both directions | No | Standard — traffic from left and right |
| One-way | One direction | Yes | Cyclists against flow, emergency vehicles, reversing vehicles |

A good discussion question for your group: what exactly makes the one-way street simpler, and what are you assuming when you say that?

#### Corner Cases — When Things Get Interesting

Corner cases are the unusual, rare, or boundary conditions that the base case does not cover. They are easy to overlook and expensive to discover late. Strong BAs find them early.

The most natural corner cases for a crossing fall into four categories.

##### Timing and Signal Failures

A pedestrian enters the crosswalk on a valid "Walk" signal and is halfway across a two-way street when the signal changes to flashing "Don't Walk." What should happen?

- The pedestrian should continue forward to the opposite kerb.
- Vehicles should remain stopped until the crosswalk is clear.
- The pedestrian should not reverse direction — turning around mid-crossing introduces more risk, not less.

A related scenario: the pedestrian presses the crossing button, but the signal never changes. After a reasonable wait — say 120 seconds — the system should be considered malfunctioning. The pedestrian now has no guidance and must rely on their own judgement. This matters because your acceptance criteria just shifted from describing normal behaviour to describing a failure mode, and failure modes need their own stories.

##### Accessibility

A visually impaired pedestrian cannot rely on the visual walk signal at all. If the crossing is equipped with an audible indicator, that pedestrian begins crossing when the sound activates and follows tactile paving to the opposite kerb. But what if the audible indicator is broken? What if the tactile paving is damaged or missing? Each of those is a separate scenario.

A wheelchair user encounters a different problem. Both kerb ramps must exist. If the ramp on the departure side exists but the arrival side has no ramp, the pedestrian crosses successfully and then finds themselves stranded in the roadway, unable to mount the kerb. The walk signal did its job, the traffic stopped, and the pedestrian is still stuck. That is a corner case that only becomes visible when you change the persona.

##### Environmental Conditions

None of these change the signal logic, but all of them change the outcome:

| Condition | Impact on Crossing |
|---|---|
| Heavy rain | Reduced driver visibility — turning vehicles may not see the pedestrian |
| Faded crosswalk markings | Unfamiliar pedestrian unsure of where to cross, may walk outside the intended zone |
| Ice or snow on road surface | Crossing time extends beyond what the signal duration allows |
| Night with poor street lighting | Pedestrian and crosswalk both less visible to drivers |

##### Behavioural

- A pedestrian jaywalks on a one-way street because the nearest signalised crossing is too far away. The risk is lower than jaywalking on a two-way street, but hazards remain — emergency vehicles, speeding, obstructed sightlines.
- A large group enters the crosswalk simultaneously, and the slower members are still crossing when the signal changes. Vehicles must continue to wait, but the signal was never calibrated for group crossings in the first place.

Notice what happened across these four categories. You started with a simple crosswalk and ended up writing about infrastructure failures, accessibility gaps, weather, and human behaviour. That is not scope creep. That is thorough analysis.

| Category | What Breaks | Example |
|---|---|---|
| Timing / Signal | Signal behaviour or timing does not match reality | Signal changes mid-crossing, button never triggers |
| Accessibility | Infrastructure excludes a persona | No kerb ramp, no audible signal, damaged tactile paving |
| Environmental | External conditions degrade the crossing | Rain, ice, darkness, faded markings |
| Behavioural | Human behaviour deviates from design assumptions | Jaywalking, group crossing, distracted pedestrian |

#### Dependencies — What Must Be True

A dependency is something that must exist or be operational for a scenario to be valid. If you remove it, the scenario breaks — or worse, it produces a dangerous outcome that nobody planned for.

| Dependency | What It Affects |
|---|---|
| Traffic signal is operational | All signalised crossing scenarios — if it is out, no base case applies |
| Crosswalk markings are visible | Pedestrian's ability to identify the safe crossing zone |
| Kerb ramps exist on both sides | Wheelchair and mobility-impaired scenarios |
| Audible walk indicator is installed | Visually impaired pedestrian scenarios |
| Crossing button functions correctly | Pedestrian-initiated signal request scenarios |
| Drivers obey traffic signals | Every scenario — pedestrian safety depends on driver compliance |
| Street direction is signed or marked | One-way scenarios — pedestrian must know which direction to check |
| Countdown timer is visible | Pedestrian's decision to start crossing or wait for the next cycle |
| Street lighting is operational | All after-dark crossing scenarios |

Some of these are obvious — of course the traffic signal must work. Others are less so. Driver compliance is one that every scenario quietly assumes but rarely calls out. Street direction signage is another — on a one-way street, the pedestrian must know which direction to check, and that knowledge depends on visible markings or signs.

When writing scenarios, four questions will surface most of your dependencies:

- What infrastructure must exist? Signals, ramps, markings, buttons, tactile paving, lighting.
- What systems must be operational? Signal timing, audible indicators, countdown timers, the crossing button mechanism.
- What behaviours from other people are assumed? Drivers stopping on red, other pedestrians moving at a reasonable pace, emergency vehicles using sirens.
- What environmental conditions are assumed? Daylight, dry roads, clear visibility, temperatures above freezing.

Here is a useful test: take any dependency from your list and imagine it is missing. If at least one of your scenarios produces a different outcome, it is a true dependency and it belongs in your documentation. If removing it changes nothing, it was not really a dependency — it was just context.

#### User Stories — Bringing It All Together

Everything up to this point — the framework, the base cases, the corner cases, the dependencies — was preparation. This is where it lands. A well-written user story is not just the "As a / I want / So that" statement. It is that statement combined with scenarios that cover the happy path, the edge conditions, and the assumptions holding everything together.

##### User Story 1 — Pedestrian Crossing a Two-Way Street

Feature: Pedestrian crosses a two-way street at a signalised intersection.

User Story: As a pedestrian, I want to cross the street safely when the walk signal is active, so that I reach the other side without being struck by traffic.

| Scenario Type | Scenario Name | Given | When | Then |
|---|---|---|---|---|
| Base case | Successful crossing on walk signal | The pedestrian is standing at the kerb, and the traffic light for vehicles is red in both directions, and the walk signal shows "Walk" | The pedestrian steps onto the crosswalk and walks to the other side | The pedestrian arrives safely on the opposite kerb, and traffic remains stopped until the signal changes |
| Corner case | Signal changes mid-crossing | The pedestrian has entered the crosswalk on a valid "Walk" signal, and is halfway across the street | The signal changes to flashing "Don't Walk" | The pedestrian continues to the opposite kerb, vehicles remain stopped until the pedestrian clears the crosswalk, but the pedestrian does not reverse direction |
| Dependency exposed | Crossing button malfunction | The pedestrian is at a button-activated crossing, and has pressed the crossing button | 120 seconds elapse without a signal change | The system is considered malfunctioning, and the pedestrian must use their own judgement to cross or find an alternative route |

Dependencies for the malfunction scenario:

- The button mechanism must be operational.
- The signal system must be configured to respond to button input within a defined time window.

##### User Story 2 — Wheelchair User Crossing a Two-Way Street

Same Feature: Pedestrian crosses a two-way street at a signalised intersection.

User Story: As a pedestrian who uses a wheelchair, I want both sides of the crossing to have kerb ramps, so that I can safely enter and exit the roadway.

| Scenario Type | Scenario Name | Given | When | Then |
|---|---|---|---|---|
| Base case | Both ramps present | The pedestrian uses a wheelchair, and kerb ramps exist on both sides of the crossing, and the walk signal shows "Walk" | The pedestrian crosses the street | The pedestrian exits the roadway via the opposite ramp without obstruction |
| Corner case | Opposite ramp missing | The pedestrian uses a wheelchair, and the walk signal shows "Walk," but the opposite kerb does not have a ramp | The pedestrian reaches the other side | The pedestrian is unable to mount the kerb, and is stranded in the roadway |
| Corner case | Ramp obstructed | The pedestrian uses a wheelchair, and a kerb ramp exists on the opposite side, but the ramp is blocked by a parked vehicle or debris | The pedestrian reaches the other side | The pedestrian cannot use the ramp, and must find an alternative exit from the roadway |

Dependencies for the missing ramp scenario:

- Kerb ramp infrastructure must be present on both sides.
- A recent accessibility audit must have confirmed ramp availability.

Dependencies for the obstructed ramp scenario:

- Ramp clearance must be maintained.
- Enforcement or signage must prevent vehicles from blocking ramp access.

##### What to Notice

Two things are worth observing about these stories.

First, the structure is consistent. Every story has a clear persona and a clear reason. Every scenario follows Given-When-Then precisely. The base case establishes the happy path. The corner cases break something — a missing ramp, a blocked ramp, a malfunctioning button — and the expected outcome changes accordingly. The dependencies are called out explicitly so that no one has to guess what assumptions are baked in.

Second, the two user stories sit under the same Feature, but they describe completely different experiences of the same crosswalk. The able-bodied pedestrian and the wheelchair user walk the same path and encounter different realities. That is what changing the persona does — it does not add complexity for the sake of it. It reveals complexity that was always there.

#### Summary

- A Feature names the area of behaviour. A User Story captures who wants what and why. Given-When-Then turns each story into testable acceptance criteria. These three layers give every stakeholder — business, technical, and QA — a single language to work from.

- Base cases define normal. They are the happy path where everything works as expected, and they must be written first because every corner case is measured against them. If you do not know what "normal" looks like, you cannot recognise what "unusual" looks like either.

- Corner cases are where strong analysis lives. Changing the persona, changing the environment, introducing a failure, or hitting a timing boundary — each of these reveals scenarios the base case never anticipated. The wheelchair user who finds no ramp, the signal that never changes, the group that crosses too slowly — these are the requirements that get missed when analysis stays shallow.

- Dependencies are the invisible load-bearing walls of your scenarios. Infrastructure, operational systems, human behaviour, environmental conditions — if any one of them is removed and your scenario breaks, it is a dependency and it must be documented. The four-question framework (infrastructure, systems, behaviours, environment) is a reliable way to find them.

- The real world is never as simple as it appears. A pedestrian crossing involves signals, infrastructure, human behaviour, environmental conditions, accessibility needs, and timing — all interacting at the same time. If you can find the corner cases in a crosswalk, you can find them in any business domain you will ever work in.
