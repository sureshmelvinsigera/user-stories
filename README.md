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

A Feature is the highest level. It names an area of behaviour. Think of it as the chapter title — broad enough to contain several related stories, narrow enough that someone reading it knows exactly what part of the world you are describing. A good Feature for this workshop might be: "Pedestrian crosses a two-way street at a signalised intersection."

A User Story sits one level below the Feature. It captures who wants something, what they want, and why it matters to them. The format is deliberate:

As a (persona), I want to (action), so that (outcome).

Every word earns its place. The persona tells you whose perspective you are writing from. The action tells you what behaviour to design for. The outcome tells you why it matters — and if you cannot articulate the "so that," you probably do not understand the requirement well enough yet.

Given-When-Then is how you validate each story. Given describes the world before anything happens — the preconditions. When describes the trigger — the action the persona takes. Then describes what should be true afterward. Two additional keywords round out the syntax: And continues any clause with more conditions, and But adds a negative qualifier.

These three layers — Feature, User Story, Given-When-Then — give you a shared language that business stakeholders, developers, and testers can all read without translation.

#### Base Cases — The Happy Path

A base case is the scenario where everything works exactly as designed. No surprises, no failures, no unusual conditions. It is the first thing you write because it establishes what "normal" looks like, and every corner case you write later is a deviation from it.

For a two-way street with traffic signals, the base case for a pedestrian is straightforward. The pedestrian stands at the kerb. Traffic lights are red in both directions. The walk signal shows "Walk." The pedestrian steps onto the crosswalk, walks to the other side, and arrives safely. Traffic remains stopped until the signal changes.

There is a second base case worth writing separately: the pedestrian arrives at the kerb and the signal shows "Don't Walk." The expected behaviour is that the pedestrian remains on the kerb and waits. It sounds obvious, but writing it down matters. You are defining the boundary between "system working correctly" and "pedestrian ignoring the system," and those two things have very different implications.

Now consider the same crossing on a one-way street. The preconditions shift. Traffic flows in a single direction. The vehicle signal for that direction is red. The walk signal shows "Walk." The pedestrian steps onto the crosswalk and only needs to account for traffic from one direction.

That last detail — "only needs to check one direction" — is worth pausing on. It is an assumption, and assumptions are where corner cases hide. What about a cyclist riding against traffic? An emergency vehicle approaching from the opposite direction? A delivery truck reversing out of a side alley? The one-way street feels simpler, but the simplicity is conditional.

A good discussion question for your group: what exactly makes the one-way street simpler, and what are you assuming when you say that?

#### Corner Cases — When Things Get Interesting

Corner cases are the unusual, rare, or boundary conditions that the base case does not cover. They are easy to overlook and expensive to discover late. Strong BAs find them early.

The most natural corner cases for a crossing fall into four categories.

Timing and signal failures come first. A pedestrian enters the crosswalk on a valid "Walk" signal and is halfway across a two-way street when the signal changes to flashing "Don't Walk." What should happen? The pedestrian should continue forward to the opposite kerb. Vehicles should remain stopped until the crosswalk is clear. The pedestrian should not reverse direction — turning around mid-crossing introduces more risk, not less.

A related scenario: the pedestrian presses the crossing button, but the signal never changes. After a reasonable wait — say 120 seconds — the system should be considered malfunctioning. The pedestrian now has no guidance and must rely on their own judgement. This matters because your acceptance criteria just shifted from describing normal behaviour to describing a failure mode, and failure modes need their own stories.

Accessibility surfaces an entirely different set of corner cases. A visually impaired pedestrian cannot rely on the visual walk signal at all. If the crossing is equipped with an audible indicator, that pedestrian begins crossing when the sound activates and follows tactile paving to the opposite kerb. But what if the audible indicator is broken? What if the tactile paving is damaged or missing? Each of those is a separate scenario.

A wheelchair user encounters a different problem. Both kerb ramps must exist. If the ramp on the departure side exists but the arrival side has no ramp, the pedestrian crosses successfully and then finds themselves stranded in the roadway, unable to mount the kerb. The walk signal did its job, the traffic stopped, and the pedestrian is still stuck. That is a corner case that only becomes visible when you change the persona.

Environmental conditions create their own category. Heavy rain reduces visibility for drivers. Faded crosswalk markings leave an unfamiliar pedestrian unsure of where to cross. Ice on the road surface extends crossing time beyond what the signal duration allows. None of these change the signal logic, but all of them change the outcome.

Behavioural corner cases round out the picture. A pedestrian jaywalks on a one-way street because the nearest signalised crossing is too far away. A large group enters the crosswalk simultaneously, and the slower members are still crossing when the signal changes. In the group scenario, vehicles must continue to wait — but the signal was never calibrated for group crossings in the first place.

Notice what happened across these four categories. You started with a simple crosswalk and ended up writing about infrastructure failures, accessibility gaps, weather, and human behaviour. That is not scope creep. That is thorough analysis.

#### Dependencies — What Must Be True

A dependency is something that must exist or be operational for a scenario to be valid. If you remove it, the scenario breaks — or worse, it produces a dangerous outcome that nobody planned for.

The traffic signal itself is a dependency for every signalised crossing scenario. If it is out of service, none of your base cases apply. Crosswalk markings are a dependency for the pedestrian's ability to identify the safe crossing zone. Kerb ramps are a dependency for wheelchair and mobility-impaired users. The audible walk indicator is a dependency for visually impaired pedestrians. The crossing button is a dependency for any scenario involving a pedestrian-initiated signal request.

Some dependencies are less obvious. Driver compliance is one — every scenario assumes that vehicles actually stop when the signal is red. Street direction signage is another — on a one-way street, the pedestrian must know which direction to check, and that knowledge depends on visible markings or signs. A countdown timer, if present, is a dependency for the pedestrian's decision about whether to start crossing or wait for the next cycle. Time of day and street lighting affect visibility at night, making lighting infrastructure a dependency for after-dark scenarios.

When writing scenarios, four questions will surface most of your dependencies.

What infrastructure must exist? Signals, ramps, markings, buttons, tactile paving, lighting.

What systems must be operational? Signal timing, audible indicators, countdown timers, the crossing button mechanism.

What behaviours from other people are assumed? Drivers stopping on red. Other pedestrians moving at a reasonable pace. Emergency vehicles using sirens.

What environmental conditions are assumed? Daylight. Dry roads. Clear visibility. Temperatures above freezing.

Here is a useful test: take any dependency from your list and imagine it is missing. If at least one of your scenarios produces a different outcome, it is a true dependency and it belongs in your documentation. If removing it changes nothing, it was not really a dependency — it was just context.

#### User Stories — Bringing It All Together

Everything up to this point — the framework, the base cases, the corner cases, the dependencies — was preparation. This is where it lands. A well-written user story is not just the "As a / I want / So that" statement. It is that statement combined with scenarios that cover the happy path, the edge conditions, and the assumptions holding everything together.

Here is what a complete user story looks like for a pedestrian crossing a two-way street.

Feature: Pedestrian crosses a two-way street at a signalised intersection.

User Story: As a pedestrian, I want to cross the street safely when the walk signal is active, so that I reach the other side without being struck by traffic.

Scenario — base case, successful crossing on walk signal: Given the pedestrian is standing at the kerb, and the traffic light for vehicles is red in both directions, and the pedestrian walk signal shows "Walk." When the pedestrian steps onto the crosswalk and walks to the other side. Then the pedestrian arrives safely on the opposite kerb, and traffic remains stopped until the signal changes.

Scenario — corner case, signal changes mid-crossing: Given the pedestrian has already entered the crosswalk on a valid "Walk" signal, and the pedestrian is halfway across the street. When the signal changes to flashing "Don't Walk." Then the pedestrian should continue walking to the opposite kerb, and vehicles should remain stopped until the pedestrian clears the crosswalk, but the pedestrian should not reverse direction back to the original kerb.

Scenario — dependency exposed, crossing button malfunction: Given the pedestrian is at a button-activated crossing, and the pedestrian has pressed the crossing button. When 120 seconds have elapsed without a signal change. Then the system should be considered malfunctioning, and the pedestrian must use their own judgement to cross or find an alternative route. Dependencies: the button mechanism must be operational, and the signal system must be configured to respond to button input within a defined time window.

Now the same Feature, but with a different persona.

User Story: As a pedestrian who uses a wheelchair, I want both sides of the crossing to have kerb ramps, so that I can safely enter and exit the roadway.

Scenario — base case, both ramps present: Given the pedestrian uses a wheelchair, and kerb ramps exist on both sides of the crossing, and the walk signal shows "Walk." When the pedestrian crosses the street. Then the pedestrian exits the roadway via the opposite ramp without obstruction.

Scenario — corner case, opposite ramp missing: Given the pedestrian uses a wheelchair, and the walk signal shows "Walk," but the opposite kerb does not have a ramp. When the pedestrian reaches the other side. Then the pedestrian is unable to mount the kerb, and the pedestrian is stranded in the roadway. Dependencies: kerb ramp infrastructure must be present on both sides, and a recent accessibility audit must have confirmed ramp availability.

Scenario — corner case, ramp obstructed: Given the pedestrian uses a wheelchair, and a kerb ramp exists on the opposite side, but the ramp is blocked by a parked vehicle or debris. When the pedestrian reaches the other side. Then the pedestrian cannot use the ramp, and must find an alternative exit from the roadway. Dependencies: ramp clearance must be maintained, and enforcement or signage must prevent vehicles from blocking ramp access.

Notice the structure. Each user story has a clear persona and a clear reason. Each scenario follows Given-When-Then precisely. The base case establishes the happy path. The corner cases break something — a missing ramp, a blocked ramp, a malfunctioning button — and the expected outcome changes accordingly. The dependencies are called out explicitly so that no one has to guess what assumptions are baked in.

One more detail worth observing: the two user stories sit under the same Feature, but they describe completely different experiences of the same crosswalk. The able-bodied pedestrian and the wheelchair user walk the same path and encounter different realities. That is what changing the persona does. It does not add complexity for the sake of it — it reveals complexity that was always there.

#### Summary

- A Feature names the area of behaviour. A User Story captures who wants what and why. Given-When-Then turns each story into testable acceptance criteria. These three layers give every stakeholder — business, technical, and QA — a single language to work from.

- Base cases define normal. They are the happy path where everything works as expected, and they must be written first because every corner case is measured against them. If you do not know what "normal" looks like, you cannot recognise what "unusual" looks like either.

- Corner cases are where strong analysis lives. Changing the persona, changing the environment, introducing a failure, or hitting a timing boundary — each of these reveals scenarios the base case never anticipated. The wheelchair user who finds no ramp, the signal that never changes, the group that crosses too slowly — these are the requirements that get missed when analysis stays shallow.

- Dependencies are the invisible load-bearing walls of your scenarios. Infrastructure, operational systems, human behaviour, environmental conditions — if any one of them is removed and your scenario breaks, it is a dependency and it must be documented. The four-question framework (infrastructure, systems, behaviours, environment) is a reliable way to find them.

- The real world is never as simple as it appears. A pedestrian crossing involves signals, infrastructure, human behaviour, environmental conditions, accessibility needs, and timing — all interacting at the same time. If you can find the corner cases in a crosswalk, you can find them in any business domain you will ever work in.
