# Pedestrian Crossing the Street — User Stories Workshop

---

## Introduction

As Business Analysts, one of the most important skills you will develop is the ability to **decompose real-world behaviour into clear, testable requirements**. This exercise uses something deceptively simple — a pedestrian crossing the street — to practice writing **Features**, **User Stories**, and **Given-When-Then (GWT) scenarios** in Gherkin syntax.

Why "deceptively simple"? Because the moment you start asking _"what could go wrong?"_ or _"what else is happening at the same time?"_, a simple street crossing becomes a rich domain full of rules, edge cases, and dependencies.

By the end of this exercise you should be able to:

- Identify **base cases** (the happy path — things working as expected).
- Identify **corner cases** (unusual, rare, or boundary conditions).
- Identify **dependencies** (things that must be true or exist for a scenario to be valid).
- Structure all of the above using **Feature → User Story → Given-When-Then**.

---

## The Framework: Feature → User Story → Given-When-Then

### Feature

A **Feature** is a high-level capability or behaviour of the system (or in our case, the real-world domain). It answers the question: _"What area of behaviour are we describing?"_

```
Feature: Pedestrian crosses a two-way street at a signalised intersection
```

### User Story

A **User Story** captures **who** wants something, **what** they want, and **why**. It follows the format:

```
As a [persona],
I want to [action],
So that [outcome / value].
```

Example:

```
As a pedestrian,
I want to cross the street safely when the walk signal is active,
So that I reach the other side without being struck by traffic.
```

> **Tip for BAs:** Always ask — _who else_ could be the persona? A child? A visually impaired person? A cyclist dismounting? Each persona can surface new stories.

### Given-When-Then (Gherkin Syntax)

Each User Story is validated through **scenarios** written in Given-When-Then:

| Keyword  | Purpose                                                        |
|----------|----------------------------------------------------------------|
| **Given**  | The preconditions — what is true _before_ the action happens |
| **When**   | The trigger — the action the user takes                      |
| **Then**   | The expected outcome — what should be true _after_           |
| **And**    | Continues any of the above with additional conditions        |
| **But**    | Adds a negative qualifier to Given, When, or Then            |

---

## Base Cases — The Happy Path

Base cases describe the **expected, normal behaviour** — when everything works as designed.

### Two-Way Street (Signalised Crossing)

```gherkin
Feature: Pedestrian crosses a two-way street at a signalised intersection

  Scenario: Pedestrian crosses on a walk signal
    Given the pedestrian is standing at the kerb
    And the traffic light for vehicles is red in both directions
    And the pedestrian walk signal shows "Walk"
    When the pedestrian steps onto the crosswalk
    And the pedestrian walks to the other side
    Then the pedestrian arrives safely on the opposite kerb
    And traffic remains stopped until the signal changes

  Scenario: Pedestrian waits when signal shows "Don't Walk"
    Given the pedestrian is standing at the kerb
    And the pedestrian signal shows "Don't Walk"
    When the pedestrian observes the signal
    Then the pedestrian remains on the kerb
    And the pedestrian waits for the signal to change to "Walk"
```

### One-Way Street (Signalised Crossing)

```gherkin
Feature: Pedestrian crosses a one-way street at a signalised intersection

  Scenario: Pedestrian crosses a one-way street on walk signal
    Given the pedestrian is standing at the kerb
    And traffic flows in one direction only (e.g., left to right)
    And the vehicle signal for that direction is red
    And the pedestrian walk signal shows "Walk"
    When the pedestrian steps onto the crosswalk
    Then the pedestrian only needs to check for traffic from one direction
    And the pedestrian crosses safely to the other side
```

> **Discussion point:** What makes the one-way street _simpler_ than the two-way street? What assumptions are we making? (Hint: cyclists, emergency vehicles.)

---

## Corner Cases — When Things Get Interesting

Corner cases are **unusual, boundary, or edge conditions** that are easy to overlook but critical to define. This is where strong BAs distinguish themselves.

### Timing and Signal Corner Cases

```gherkin
  Scenario: Signal changes to "Don't Walk" while pedestrian is mid-crossing
    Given the pedestrian has already entered the crosswalk on a valid "Walk" signal
    And the pedestrian is halfway across a two-way street
    When the signal changes to flashing "Don't Walk"
    Then the pedestrian should continue walking to the opposite kerb
    And vehicles should remain stopped until the pedestrian clears the crosswalk
    But the pedestrian should not reverse direction back to the original kerb

  Scenario: Pedestrian presses the crossing button but signal does not change
    Given the pedestrian is at a button-activated crossing
    And the pedestrian has pressed the crossing button
    When 120 seconds have elapsed without a signal change
    Then the system should be considered malfunctioning
    And the pedestrian must use their own judgement to cross or find an alternative
```

### Accessibility Corner Cases

```gherkin
  Scenario: Visually impaired pedestrian relies on audible signal
    Given the pedestrian is visually impaired
    And the crossing is equipped with an audible walk indicator
    When the audible signal activates
    Then the pedestrian begins crossing
    And tactile paving guides them to the opposite kerb

  Scenario: Wheelchair user encounters a missing kerb ramp
    Given the pedestrian uses a wheelchair
    And the walk signal shows "Walk"
    But the opposite kerb does not have a ramp
    When the pedestrian reaches the other side
    Then the pedestrian is unable to mount the kerb
    And the pedestrian is stranded in the roadway
```

### Environmental Corner Cases

```gherkin
  Scenario: Pedestrian crosses during heavy rain with reduced visibility
    Given visibility is severely reduced due to heavy rain
    And vehicle drivers may not see the pedestrian clearly
    When the walk signal shows "Walk"
    And the pedestrian begins to cross
    Then the pedestrian should make themselves visible (e.g., reflective clothing)
    And turning vehicles must yield despite reduced visibility

  Scenario: Crosswalk markings are faded or invisible
    Given the crosswalk paint has faded over time
    And a new pedestrian is unfamiliar with the intersection
    When the pedestrian looks for the crosswalk
    Then the pedestrian may not know the correct crossing path
    And may cross outside the intended zone
```

### Behavioural Corner Cases

```gherkin
  Scenario: Pedestrian jaywalks on a one-way street
    Given there is no signalised crossing within reasonable distance
    And the pedestrian is on a one-way street
    When the pedestrian checks for a gap in traffic from the single direction
    And steps onto the road
    Then the risk is reduced compared to a two-way street
    But the pedestrian is still exposed to hazards (e.g., emergency vehicle, speeding)

  Scenario: Multiple pedestrians cause a slow crossing
    Given a large group of pedestrians enters the crosswalk simultaneously
    And the walk signal duration is calibrated for an individual walking pace
    When slower members of the group are still crossing
    And the signal changes to "Don't Walk"
    Then vehicles must continue to wait for all pedestrians to clear
```

---

## Dependencies — What Must Be True?

Dependencies are the **preconditions, systems, infrastructure, or external factors** that a scenario relies on. If a dependency is missing or broken, the scenario may not be valid.

| Dependency                      | What It Affects                                                                 |
|---------------------------------|---------------------------------------------------------------------------------|
| **Traffic signal is operational**   | All signalised crossing scenarios. If the signal is out, behaviour is undefined. |
| **Crosswalk markings are visible**  | Pedestrian's ability to identify the safe crossing zone.                        |
| **Kerb ramps exist on both sides**  | Wheelchair and mobility-impaired scenarios.                                     |
| **Audible signal is installed**     | Visually impaired pedestrian scenarios.                                         |
| **Drivers obey traffic signals**    | Every scenario — the pedestrian's safety depends on driver compliance.          |
| **Button-activated signal works**   | Scenarios involving pedestrian-initiated crossing requests.                     |
| **Street direction is marked**      | One-way vs. two-way — pedestrian must know which direction to check.           |
| **Countdown timer is visible**      | Pedestrian's decision to start or continue crossing.                           |
| **Time of day / lighting**          | Night crossings depend on street lighting and signal visibility.               |

### Thinking About Dependencies as a BA

When you write scenarios, ask yourself:

1. **What infrastructure must exist?** (Signals, ramps, markings, buttons)
2. **What systems must be operational?** (Signal timing, audible indicators, countdown timers)
3. **What behaviours from others are assumed?** (Drivers stopping, other pedestrians moving at pace)
4. **What environmental conditions are assumed?** (Daylight, dry roads, clear visibility)

If you remove any one of these, at least one of your scenarios will break. That tells you it is a true dependency, and it should be documented.

---

## Putting It All Together — Exercise for Students

Now it is your turn. Pick **one** of the following prompts and write:

- 1 Feature statement
- 2 User Stories (different personas)
- 3 Scenarios per story (1 base case, 1 corner case, 1 dependency-focused)

### Prompts

1. **A pedestrian crossing a street with no traffic signals** (unmarked crossing).
2. **A child walking to school** who must cross a two-way street with a school crossing guard.
3. **A cyclist who dismounts to use a pedestrian crossing** on a one-way street.
4. **A pedestrian crossing at a roundabout** with multiple entry and exit points.

> **Challenge:** For each scenario, explicitly list at least **two dependencies** that must be true.

---

## Summary

| Concept         | What It Means                                                                 |
|-----------------|-------------------------------------------------------------------------------|
| **Feature**       | A high-level capability or area of behaviour being described.               |
| **User Story**    | Who wants what and why — written from the persona's perspective.            |
| **Given**         | The world as it is _before_ the action (preconditions).                     |
| **When**          | The action or event that triggers the behaviour.                            |
| **Then**          | The expected result _after_ the action.                                     |
| **Base Case**     | The normal, expected, "happy path" scenario.                                |
| **Corner Case**   | An unusual, edge, or boundary condition that still must be handled.         |
| **Dependency**    | Something that must exist or be true for the scenario to be valid.          |

### Key Takeaways

The real world is never as simple as it appears. A pedestrian crossing a street involves signals, infrastructure, human behaviour, environmental conditions, accessibility needs, and timing — all interacting at once. As a BA, your job is to **surface the invisible** — the assumptions everyone makes but nobody writes down.

If you can find the corner cases in a crosswalk, you can find them in any business domain.
