# Soft Computing (Complete Revision Notes)

---
# Module 1: Fuzzy Logic (7 hrs)
---

## 1. Introduction to Soft Computing
I start with basics.

Soft Computing = solving problems with approximation + tolerance.

Main idea:
"Perfect answer not needed, useful answer needed."

Characteristics:
- handles uncertainty
- tolerant to imprecision
- adaptive
- human-like reasoning

Applications:
- AC
- washing machine
- robotics
- medical diagnosis
- autonomous vehicles

### Soft vs Hard Computing
Hard:
- exact
- strict
- binary logic

Soft:
- approximate
- flexible
- fuzzy logic

---

## 2. Fuzzy Logic Basics

### Crisp vs Fuzzy Set

Crisp:
member is either:
0 or 1

Example:
Age > 18 = adult

Fuzzy:
membership between [0,1]

Example:
Age 17 may be adult = 0.7

---

## 3. Fuzzy Set Operations

Union:
μA∪B = max(μA, μB)

Intersection:
μA∩B = min(μA, μB)

Complement:
μĀ = 1 − μA

Difference:
A − B = min(μA, 1−μB)

Cartesian Product:
A × B

---

## 4. Membership Functions

Triangular:
easy, common

Trapezoidal:
flat top

Gaussian:
smooth bell shape

Sigmoid:
S-shape

Remember:
triangle = simple
gaussian = smooth

---

## 5. Fuzzy Rules

If x is A
Then y is B

Example:
If temperature high
Then fan fast

---

## 6. Fuzzy Reasoning

Input → rules → output

Uses:
- Mamdani
- Sugeno

---

## 7. Fuzzy Relations

Union:
max

Intersection:
min

Complement:
1-μ

Composition:
max(min)

Important:
always:
first min
then max

---

## 8. Fuzzification

Convert:
crisp → fuzzy

Example:
30°C → medium(0.7), high(0.3)

---

## 9. Defuzzification

Convert:
fuzzy → crisp

Methods:
- Lambda cut
- Maxima
- CoG
- CoS
- Weighted Average

Most important:
CoG:
x* = Σ(xμ)/Σμ

---

## 10. Fuzzy Logic Controller

Flow:
Input
→ Fuzzification
→ Rule base
→ Inference engine
→ Defuzzification
→ Output

Case Study:
Temperature control

---

# Module 2: Genetic Algorithm (7 hrs)
---

## 1. Evolutionary Algorithm
Inspired by natural evolution.

Idea:
best survives.

---

## 2. Genetic Algorithm (GA)

Steps:
1. initialize population
2. evaluate fitness
3. selection
4. crossover
5. mutation
6. replacement
7. repeat

---

## 3. Genetic Representation

How solution is stored:
chromosome

Example:
101101

---

## 4. Fitness Function

Measures quality.

Higher fitness = better solution

---

## 5. Selection Methods

- Roulette wheel
- Tournament
- Rank selection

Idea:
choose best parents

---

## 6. Crossover

Combine parents.

Example:
P1 = 111000
P2 = 000111

Child = 111111

Types:
- one point
- two point
- uniform

---

## 7. Mutation

Random change.

Example:
10101 → 10001

Purpose:
avoid local minima

---

## 8. Survival of Fittest

Best survives next generation.

---

## 9. Ranking

Rank method
Rank space method

---

## Case Studies
- Traveling Salesman Problem
- Search optimization

---

# Module 3: Swarm Intelligence (8 hrs)
---

## 1. Ant Colony Optimization (ACO)

Inspired by ants.

Ants leave pheromones.

Best path gets strongest pheromone.

Used in:
routing

---

## 2. Max-Min Ant System

Controls pheromone limits.

Prevents premature convergence.

---

## 3. Ant Miner

ACO for classification rules.

---

## 4. Snake-Ant Algorithm

Hybrid search technique.

---

## 5. Particle Swarm Optimization (PSO)

Inspired by bird flocking.

Terms:
- particle
- velocity
- global best
- personal best

Used in:
resource allocation

---

## 6. Artificial Bee Colony (ABC)

Inspired by bees.

Types:
- employed bee
- onlooker bee
- scout bee

Used in:
scheduling

---

## 7. Cuckoo Search

Inspired by cuckoo eggs.

Uses:
Levy flight

Strong global search.

---

## 8. Concepts

Fitness landscape:
solution space

Adaptive mechanisms:
system learns

Convergence:
moving toward optimum

Co-evolution:
solutions evolve together

Plasticity:
learn during lifetime

Lamarckian learning:
learned traits inherited

---

## No Free Lunch Theorem

No single algorithm solves everything best.

Important theory.

---

## Hybrid Systems

Fuzzy + GA
Fuzzy + controller

---

# Module 4: Multi-objective Optimization (6 hrs)
---

## 1. Multi-objective Optimization

Optimize multiple goals.

Example:
min cost
max quality

Both together.

---

## 2. Pareto Optimality

No solution improves one objective
without worsening another.

Called:
Pareto front

---

## 3. Pareto vs Non-Pareto

Pareto:
dominance based

Non-pareto:
weighted methods

---

## 4. Dominance

A dominates B if:
better in one
not worse in others

---

## 5. Crowding Distance

Maintains diversity.

Important in NSGA-II

---

## 6. NSGA-II

Most important algorithm.

Steps:
1. sort fronts
2. calculate crowding distance
3. selection
4. crossover
5. mutation

Advantages:
fast
elitism
diverse solutions

---

## Case Studies

- vehicle routing
- neuro-fuzzy tuning

---

# Final 1-Day Revision Strategy

Day before exam:

1. Fuzzy logic (highest weight)
2. Defuzzification formulas
3. GA cycle
4. ACO vs PSO vs ABC
5. Pareto + NSGA-II

Must remember:
CoG formula
GA steps
PSO formula idea
ACO pheromone idea
Pareto front

Done.
