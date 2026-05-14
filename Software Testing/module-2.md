# Software Testing Notes (My Notes - Ownership Style)

---

# Module 1: Manual and Automation Testing

## Introduction to Software Testing
I define **software testing** as evaluating software by comparing **actual output** with **expected output**.

### Why I do testing:
- I ensure software quality.
- I ensure reliability.
- I ensure user satisfaction.
- I detect defects early.
- I reduce development cost.

---

## Manual Testing
I execute tests **manually** without automation tools.

### I use it for:
- Exploratory testing
- Usability testing
- Ad-hoc testing

### Key points:
- Flexible
- Human-driven
- Slow
- Error-prone
- Best for short-term/small projects

---

## Automation Testing
I execute tests using **tools and scripts**.

### I use it for:
- Regression testing
- Load testing
- Performance testing

### Key points:
- Fast
- Accurate
- Reusable
- Needs initial setup and scripting

---

## Manual vs Automation

| Manual Testing | Automation Testing |
|---|---|
| Slow | Fast |
| No tools needed | Requires tools/scripts |
| More human errors | High accuracy |
| Low initial cost | High setup cost |
| Best for short-term | Best for long-term |

---

## Examples I remember

### Manual:
- Website usability testing
- Mobile app exploratory testing

### Automation:
- Selenium → regression testing
- JMeter → performance testing
- Jenkins → CI/CD testing

---

## Popular Tools I know
- Selenium
- JUnit
- TestNG
- UFT (QTP)
- JMeter
- Appium

---

# Module 2: Verification / Validation

## Verification
I define **verification** as:

**“Am I building the product right?”**

It is **process-oriented**.

---

## Static Verification
I verify software **without running code**.

### Methods I use:
- Reviews
  - Code Review
  - Design Review
- Walkthrough
- Inspection
- Static Analysis Tools

### Why I use it:
- Detect defects early
- Lower cost

### Examples:
- Verify SRS
- Verify SDD
- Check code syntax/security

---

## Walkthrough
I define walkthrough as:

- Author presents document/code
- 2–7 members attend
- Only presenter prepares
- Participants ask doubts
- Good for knowledge sharing

---

## Inspection
I define inspection as the **most formal verification method**.

### Features:
- 3–6 members
- Led by moderator
- Everyone prepares
- Moderator writes final report
- Highly effective
- Expensive

---

## Dynamic Verification
I verify software **by executing code**.

### Purpose:
- Ensure software works correctly

### Activities:
- Unit Testing
- Integration Testing
- System Testing
- Acceptance Testing

Question I answer:

**“Does the product actually work right?”**

---

## Static vs Dynamic

| Static Verification | Dynamic Verification |
|---|---|
| Code not executed | Code executed |
| Early stage | Later stage |
| Reviews/Inspections | Testing |
| Checks artifacts | Checks behavior |
| Cheaper | Costlier |

---

## Banking Example
Static:
- I verify transfer module design has balance validation.

Dynamic:
- I run transfer and check whether it works.

---

# Types of Verification

## 1. SRS Verification
I verify **requirements**.

### I check:
- correctness
- completeness
- clarity
- no ambiguity

Question:

**Are requirements correct?**

---

## 2. SDD Verification
I verify **design**.

### I check:
- design matches SRS
- architecture is correct
- interfaces are correct
- implementation is feasible

Question:

**Is design correct?**

---

## 3. Program Verification
I verify **code**.

### I check:
- code follows SDD
- coding standards
- naming conventions
- logic correctness

Question:

**Does code follow design?**

---

## My Verification Flow
Requirements → Design → Code

---

# Types of Testing

## Unit Testing
I test one module/component in isolation.

---

## Integration Testing
I test whether modules work together.

---

## System Testing
I test the complete software system.

---

## Regression Testing
I test after changes to ensure nothing old breaks.

---

# Coverage in Verification

## Statement Coverage
I define statement coverage as:

How many code statements are executed by my test cases.

### Goal:
Every statement should run at least once.

### Formula:
Statement Coverage = (Executed Statements / Total Statements) × 100

### Meaning:
Higher coverage = better code coverage.

---
