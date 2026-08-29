## Exercise 1 — Does reasoning actually help here?

### Multi-step problem

A developer qualifies for the Senior Android certification if all of the following are true:

- They have at least 5 years of Android development experience.
- They have at least 3 years of Kotlin experience.
- They have shipped at least 2 production applications.
- If they have fewer than 7 years of Android experience, they must also have at least 2 years of Jetpack Compose experience.

Candidate:

- 6 years of Android experience
- 4 years of Kotlin experience
- 3 production applications shipped
- 1 year of Jetpack Compose experience

Does the candidate qualify for the Senior Android certification?

### C-direct

#### Prompt

```text
A developer qualifies for the Senior Android certification if all of the following are true:

- They have at least 5 years of Android development experience.
- They have at least 3 years of Kotlin experience.
- They have shipped at least 2 production applications.
- If they have fewer than 7 years of Android experience, they must also have at least 2 years of Jetpack Compose experience.

Candidate:

- 6 years of Android experience
- 4 years of Kotlin experience
- 3 production applications shipped
- 1 year of Jetpack Compose experience

Does the candidate qualify for the Senior Android certification?

Give only the final answer, with no explanation.
```

#### Run 1

Output:
> No.

Correct: Yes

#### Run 2

Output:
> No.

Correct: Yes

#### Run 3

Output:
> No.

Correct: Yes

**Result: 3/3 correct.**

#### C-cot

#### Run 1

```text
A developer qualifies for the Senior Android certification if all of the following are true:

- They have at least 5 years of Android development experience.
- They have at least 3 years of Kotlin experience.
- They have shipped at least 2 production applications.
- If they have fewer than 7 years of Android experience, they must also have at least 2 years of Jetpack Compose experience.

Candidate:

- 6 years of Android experience
- 4 years of Kotlin experience
- 3 production applications shipped
- 1 year of Jetpack Compose experience

Does the candidate qualify for the Senior Android certification?

Think step by step, then give the final answer.
```

Output:

```text
```python
def check_qualification(android_exp, kotlin_exp, apps_shipped, compose_exp):
    cond1 = android_exp >= 5
    cond2 = kotlin_exp >= 3
    cond3 = apps_shipped >= 2
    
    if android_exp < 7:
        cond4 = compose_exp >= 2
    else:
        cond4 = True
        
    return {
        "Android >= 5 yrs": (android_exp, cond1),
        "Kotlin >= 3 yrs": (kotlin_exp, cond2),
        "Apps >= 2": (apps_shipped, cond3),
        "Compose >= 2 yrs (since Android < 7)": (compose_exp, cond4),
        "Overall Qualify": cond1 and cond2 and cond3 and cond4
    }

candidate = {
    "android_exp": 6,
    "kotlin_exp": 4,
    "apps_shipped": 3,
    "compose_exp": 1
}

print(check_qualification(**candidate))
```
```

```text
{'Android >= 5 yrs': (6, True), 'Kotlin >= 3 yrs': (4, True), 'Apps >= 2': (3, True), 'Compose >= 2 yrs (since Android < 7)': (1, False), 'Overall Qualify': False}

**Evaluation of Qualification Criteria**

* **Android Experience Requirement:** At least 5 years.
* **Candidate:** 6 years ($6 \ge 5$) — **Met**


* **Kotlin Experience Requirement:** At least 3 years.
* **Candidate:** 4 years ($4 \ge 3$) — **Met**


* **Production Applications Requirement:** At least 2 shipped.
* **Candidate:** 3 applications ($3 \ge 2$) — **Met**


* **Jetpack Compose Conditional Requirement:** If Android experience is less than 7 years, at least 2 years of Jetpack Compose experience is required.
* **Candidate:** Android experience is 6 years ($6 < 7$), triggering this condition. The candidate has 1 year of Jetpack Compose experience ($1 < 2$) — **Not Met**



Since the candidate fails to meet the conditional Jetpack Compose requirement, the candidate **does not qualify** for the Senior Android certification.
```

Correct: Yes

#### Run 2

```text
```python
def check_qualification(android_exp, kotlin_exp, apps_shipped, compose_exp):
    rule1 = android_exp >= 5
    rule2 = kotlin_exp >= 3
    rule3 = apps_shipped >= 2
    rule4 = True
    if android_exp < 7:
        rule4 = compose_exp >= 2
    
    print(f"Rule 1 (Android >= 5 yrs): {rule1} ({android_exp} yrs)")
    print(f"Rule 2 (Kotlin >= 3 yrs): {rule2} ({kotlin_exp} yrs)")
    print(f"Rule 3 (Apps shipped >= 2): {rule3} ({apps_shipped} apps)")
    print(f"Rule 4 (If Android < 7 yrs, Compose >= 2 yrs): {rule4} (Android: {android_exp} yrs, Compose: {compose_exp} yrs)")
    
    qualifies = rule1 and rule2 and rule3 and rule4
    return qualifies

qualifies = check_qualification(6, 4, 3, 1)
print(f"Final Qualification Result: {qualifies}")
```

```text
Rule 1 (Android >= 5 yrs): True (6 yrs)
Rule 2 (Kotlin >= 3 yrs): True (4 yrs)
Rule 3 (Apps shipped >= 2): True (3 apps)
Rule 4 (If Android < 7 yrs, Compose >= 2 yrs): False (Android: 6 yrs, Compose: 1 yrs)
Final Qualification Result: False


**1. Android Experience**

* **Requirement:** At least 5 years.
* **Candidate Status:** 6 years.
* **Result:** Satisfied ($6 \ge 5$).

**2. Kotlin Experience**

* **Requirement:** At least 3 years.
* **Candidate Status:** 4 years.
* **Result:** Satisfied ($4 \ge 3$).

**3. Production Applications**

* **Requirement:** At least 2 shipped applications.
* **Candidate Status:** 3 shipped applications.
* **Result:** Satisfied ($3 \ge 2$).

**4. Conditional Jetpack Compose Requirement**

* **Requirement:** If Android experience is under 7 years, at least 2 years of Jetpack Compose experience is required.
* **Candidate Status:** The candidate has 6 years of Android experience ($6 < 7$), triggering the condition. However, they only have 1 year of Jetpack Compose experience.
* **Result:** Not satisfied ($1 < 2$).

The candidate does **not** qualify for the Senior Android certification because they fail the Jetpack Compose requirement for candidates with fewer than 7 years of Android experience.
```

Correct: Yes

#### Run 3

```text
```python
def check_qualification(android_years, kotlin_years, apps_shipped, compose_years):
    cond1 = android_years >= 5
    cond2 = kotlin_years >= 3
    cond3 = apps_shipped >= 2
    
    if android_years < 7:
        cond4 = compose_years >= 2
    else:
        cond4 = True
        
    return {
        "Android >= 5": cond1,
        "Kotlin >= 3": cond2,
        "Apps >= 2": cond3,
        "Compose >= 2 (since Android < 7)": cond4,
        "Overall Qualify": cond1 and cond2 and cond3 and cond4
    }

candidate = {
    "android_years": 6,
    "kotlin_years": 4,
    "apps_shipped": 3,
    "compose_years": 1
}

res = check_qualification(**candidate)
print(res)

```

```text
{'Android >= 5': True, 'Kotlin >= 3': True, 'Apps >= 2': True, 'Compose >= 2 (since Android < 7)': False, 'Overall Qualify': False}


Evaluating each requirement for the Senior Android certification against the candidate's profile:

* **Android Experience:** **Meets requirement.** The candidate has 6 years, which meets the baseline requirement of at least 5 years.
* **Kotlin Experience:** **Meets requirement.** The candidate has 4 years, satisfying the minimum requirement of at least 3 years.
* **Production Applications Shipped:** **Meets requirement.** The candidate has shipped 3 applications, exceeding the minimum threshold of 2.
* **Jetpack Compose Experience:** **Fails requirement.** Because the candidate has 6 years of Android experience (which is fewer than 7 years), the conditional requirement applies, necessitating at least 2 years of Jetpack Compose experience. The candidate only has 1 year.

**Final Answer**
The candidate **does not qualify** for the Senior Android certification because they do not meet the Jetpack Compose requirement.
```


Correct: Yes

#### C-sections

#### Run 1

```text
A developer qualifies for the Senior Android certification if all of the following are true:

- They have at least 5 years of Android development experience.
- They have at least 3 years of Kotlin experience.
- They have shipped at least 2 production applications.
- If they have fewer than 7 years of Android experience, they must also have at least 2 years of Jetpack Compose experience.

Candidate:

- 6 years of Android experience
- 4 years of Kotlin experience
- 3 production applications shipped
- 1 year of Jetpack Compose experience

Does the candidate qualify for the Senior Android certification?

Respond in exactly two sections:
Reasoning: <work through each condition>
Answer: <the final answer only>
```

Output:

```text
Reasoning:

1. **At least 5 years of Android development experience:** The candidate has 6 years, which satisfies this condition.
2. **At least 3 years of Kotlin experience:** The candidate has 4 years, which satisfies this condition.
3. **Shipped at least 2 production applications:** The candidate has shipped 3, which satisfies this condition.
4. **Jetpack Compose requirement:** The candidate has 6 years of Android experience, which is fewer than 7 years. Therefore, they must have at least 2 years of Jetpack Compose experience. The candidate only has 1 year, so they fail this condition.

Answer: No
```

**Correct:** Yes

**Mechanically extractable Answer line:** Yes — the final result is clearly separated as `Answer: No`.

### Harder Problem — C-direct

#### Run 1
```text
A developer qualifies for the Senior Android certification if all of the following are true:

- They have at least 5 years of Android development experience.
- They have at least 3 years of Kotlin experience.
- They have shipped at least 2 production applications.
- If they have fewer than 7 years of Android experience, they must also have at least 2 years of Jetpack Compose experience.
- However, candidates with fewer than 2 years of Jetpack Compose experience may still qualify if they have at least 8 years of Android experience and have shipped at least 5 production applications.

Candidate:

- 6 years of Android experience
- 4 years of Kotlin experience
- 5 production applications shipped
- 1 year of Jetpack Compose experience

Does the candidate qualify for the Senior Android certification?

Give only the final answer, with no explanation.
```

Output:
> No

Correct: Yes

### Harder Problem — C-cot

#### Run 1

```text
A developer qualifies for the Senior Android certification if all of the following are true:

- They have at least 5 years of Android development experience.
- They have at least 3 years of Kotlin experience.
- They have shipped at least 2 production applications.
- If they have fewer than 7 years of Android experience, they must also have at least 2 years of Jetpack Compose experience.
- However, candidates with fewer than 2 years of Jetpack Compose experience may still qualify if they have at least 8 years of Android experience and have shipped at least 5 production applications.

Candidate:

- 6 years of Android experience
- 4 years of Kotlin experience
- 5 production applications shipped
- 1 year of Jetpack Compose experience

Does the candidate qualify for the Senior Android certification?

Think step by step, then give the final answer.
```

Output:
```text
```python
def check_qualification(android_exp, kotlin_exp, apps_shipped, compose_exp):
    req1 = android_exp >= 5
    req2 = kotlin_exp >= 3
    req3 = apps_shipped >= 2
    
    # Requirement regarding Compose / Android exp:
    # Baseline condition 4: If android_exp < 7, must have compose_exp >= 2.
    # Exception/alternative condition 5: If compose_exp < 2, candidate can STILL qualify IF android_exp >= 8 AND apps_shipped >= 5.
    
    # Let's formulate the compose qualification check:
    if android_exp >= 7:
        compose_ok = True
    else:
        # android_exp < 7
        if compose_exp >= 2:
            compose_ok = True
        else:
            # compose_exp < 2 and android_exp < 7:
            # Can they qualify under the exception? "at least 8 years of Android experience..."
            # Since android_exp < 7, they cannot meet android_exp >= 8.
            compose_ok = False
            
    return req1 and req2 and req3 and compose_ok, {
        "req1 (Android >= 5)": req1,
        "req2 (Kotlin >= 3)": req2,
        "req3 (Apps >= 2)": req3,
        "compose_ok": compose_ok
    }

candidate = {
    "android_exp": 6,
    "kotlin_exp": 4,
    "apps_shipped": 5,
    "compose_exp": 1
}

res, details = check_qualification(**candidate)
print("Result:", res)
print("Details:", details)


```

```text
Result: False
Details: {'req1 (Android >= 5)': True, 'req2 (Kotlin >= 3)': True, 'req3 (Apps >= 2)': True, 'compose_ok': False}


```

To evaluate whether the candidate qualifies for the Senior Android certification, each requirement is checked step-by-step against the candidate's credentials:

* **Android Experience:** The candidate has **6 years** of experience (Requirement: at least 5 years) — **Met**
* **Kotlin Experience:** The candidate has **4 years** of experience (Requirement: at least 3 years) — **Met**
* **Production Applications Shipped:** The candidate has shipped **5 applications** (Requirement: at least 2 applications) — **Met**
* **Jetpack Compose Requirement:**
* Because the candidate has fewer than 7 years of Android experience (6 years), standard rules require at least **2 years** of Jetpack Compose experience. The candidate only has **1 year**.


* **Exception Clause for Compose Experience:**
* Candidates with under 2 years of Compose experience can qualify if they have at least **8 years** of Android experience and **5 shipped applications**.
* While the candidate meets the application threshold (5 apps), they only have **6 years** of Android experience, failing to reach the required 8 years.



The candidate fails to meet the Jetpack Compose criteria under both the standard requirement and the exception clause. Therefore, the candidate **does not qualify** for the Senior Android certification.
```

Correct: Yes

### Finding

The direct version was already reliable for this problem and model.

- C-direct: 3/3 correct
- C-cot: 3/3 correct
- Harder C-direct: 1/1 correct
- Harder C-cot: 1/1 correct

Explicit step-by-step reasoning did not improve accuracy in these runs. It did, however, produce much longer outputs and made the evaluation of each condition explicit.

For this task, reasoning added transparency rather than measurable accuracy.