# Day 2 — Core Techniques Lab

## Exercise 1 — Format Specification and Ablation

### Prompt A

```text
Extract information from the Android bug report provided below.

Requirements:
1. Classify the severity as exactly one of: "low", "medium", "high", or "critical".
2. Extract exactly three symptoms. Each symptom must contain no more than 12 words.
3. Identify the Android version mentioned in the report. If no Android version is mentioned, use null.
4. Do not infer information that is not explicitly stated in the bug report.

Return only valid JSON using exactly this structure:

{
  "severity": "low | medium | high | critical",
  "symptoms": [
    "symptom 1",
    "symptom 2",
    "symptom 3"
  ],
  "android_version": "string or null"
}

Bug report:
"""
After updating the application yesterday, the checkout screen has become almost unusable on my Pixel running Android 15. When I tap Pay, the loading spinner remains visible for around 30 seconds. Sometimes the payment eventually succeeds, but the screen still shows the old balance. On two occasions the app completely froze and I had to force-close it. I can still reopen the application and use the other screens normally.
"""
```

### Output A

```json
{
  "severity": "high",
  "symptoms": [
    "Loading spinner remains visible for around 30 seconds",
    "Screen still shows the old balance after payment succeeds",
    "App completely froze and required force-closing"
  ],
  "android_version": "Android 15"
}
```

### Verification

- **Requirement 1 — Severity must be exactly one of `low`, `medium`, `high`, or `critical`: Met.**  
The output returned `"high"`.

- **Requirement 2 — Extract exactly three symptoms, each with no more than 12 words: Met.**  
The output contains exactly three symptoms, and each symptom stays within the 12-word limit.

- **Requirement 3 — Identify the Android version, or use `null` if none is mentioned: Met.**  
The output returned `"Android 15"`.

- **Requirement 4 — Do not infer information that is not explicitly stated in the bug report: Met.**  
The extracted symptoms and Android version are supported directly by the bug report. The severity is a classification explicitly requested by Requirement 1.

- **Output format — Return only valid JSON using exactly the specified structure: Met.**  
The response contains only the requested JSON object with the keys `severity`, `symptoms`, and `android_version`.

---

### Prompt A-noformat

For the ablation test, only the output-format specification was removed. All other instructions and the input remained identical.

```text
Extract information from the Android bug report provided below.

Requirements:
1. Classify the severity as exactly one of: "low", "medium", "high", or "critical".
2. Extract exactly three symptoms. Each symptom must contain no more than 12 words.
3. Identify the Android version mentioned in the report. If no Android version is mentioned, use null.
4. Do not infer information that is not explicitly stated in the bug report.

Bug report:
"""
After updating the application yesterday, the checkout screen has become almost unusable on my Pixel running Android 15. When I tap Pay, the loading spinner remains visible for around 30 seconds. Sometimes the payment eventually succeeds, but the screen still shows the old balance. On two occasions the app completely froze and I had to force-close it. I can still reopen the application and use the other screens normally.
"""
```

### Output A-noformat

```json
{
  "severity": "high",
  "symptoms": [
    "Loading spinner remains visible for around 30 seconds",
    "Screen still shows the old balance after payment succeeds",
    "App completely froze and required force-closing"
  ],
  "android_version": "Android 15"
}
```

### Manual Cleanup Analysis

In this run, `A-noformat` unexpectedly produced the same JSON structure as Prompt A, so it would require no manual cleanup before being used by a colleague or downstream software.

However, without an explicit output-format specification, this structure is no longer part of the instructions given to the model. The model was free to return the same information as prose, a list, or another structure. Therefore, the ablation did not cause an observable formatting difference in this particular run, but it removed the explicit formatting constraint that made the expected output shape clear.

---

## Exercise 2 — Zero-shot to Few-shot, with a Bias Check

For this exercise, I used an Android bug-report routing task with four custom categories:

- `UI`
- `PERFORMANCE`
- `NETWORK`
- `DATA`

The same five bug reports were used for all three versions of the experiment.

### B-zero

```text
Classify each Android bug report into exactly one of these categories:

- UI
- PERFORMANCE
- NETWORK
- DATA

For each bug report, return only its input number and category label using exactly this format:

Input 1: CATEGORY
Input 2: CATEGORY
Input 3: CATEGORY
Input 4: CATEGORY
Input 5: CATEGORY

Do not include explanations or any other text.

Bug reports:

Input 1:
"""
After tapping the refresh button, the screen takes several seconds to finish loading and then still displays the user's previous account balance.
"""

Input 2:
"""
The profile screen freezes for several seconds whenever the user scrolls quickly through their transaction history.
"""

Input 3:
"""
The login request fails with a timeout whenever the device switches from Wi-Fi to mobile data.
"""

Input 4:
"""
The confirmation button is partially hidden behind the bottom navigation bar and is difficult to tap.
"""

Input 5:
"""
After changing the user's shipping address and reopening the checkout screen, the old address is still displayed.
"""
```

### B-zero Output

```text
Input 1: DATA
Input 2: PERFORMANCE
Input 3: NETWORK
Input 4: UI
Input 5: DATA
```

### B-zero Inconsistencies

- Wrong labels: **0 of 5**
- Invented labels: **0 of 5**
- Extra commentary: **0 of 5**
- Changed format: **0 of 5**

The zero-shot prompt classified all five inputs as expected and followed the requested output format consistently.

---

### B-few

```text
Classify each Android bug report into exactly one of these categories:

- UI
- PERFORMANCE
- NETWORK
- DATA

When a bug report contains symptoms that could fit more than one category,
classify it according to the final user-visible problem.

Examples:

Bug report:
"""
The settings screen takes several seconds to refresh and then continues
showing the user's previous notification preference.
"""
Category: DATA

Bug report:
"""
The application becomes unresponsive for several seconds when opening
a long transaction history.
"""
Category: PERFORMANCE

Bug report:
"""
The payment request fails with a timeout when the device loses Wi-Fi
connectivity.
"""
Category: NETWORK

For each bug report, return only its input number and category label using exactly this format:

Input 1: CATEGORY
Input 2: CATEGORY
Input 3: CATEGORY
Input 4: CATEGORY
Input 5: CATEGORY

Do not include explanations or any other text.

Bug reports:

Input 1:
"""
After tapping the refresh button, the screen takes several seconds to finish loading and then still displays the user's previous account balance.
"""

Input 2:
"""
The profile screen freezes for several seconds whenever the user scrolls quickly through their transaction history.
"""

Input 3:
"""
The login request fails with a timeout whenever the device switches from Wi-Fi to mobile data.
"""

Input 4:
"""
The confirmation button is partially hidden behind the bottom navigation bar and is difficult to tap.
"""

Input 5:
"""
After changing the user's shipping address and reopening the checkout screen, the old address is still displayed.
"""
```

### B-few Output

```text
Input 1: DATA
Input 2: PERFORMANCE
Input 3: NETWORK
Input 4: UI
Input 5: DATA
```

### B-few Analysis

The few-shot examples were balanced across three different labels: `DATA`, `PERFORMANCE`, and `NETWORK`.

The first example was deliberately borderline because it contained both a performance symptom (slow refresh) and a data symptom (stale information). It demonstrated the rule that when multiple categories apply, the final user-visible problem determines the classification.

The few-shot version produced the same 5 of 5 expected classifications as the zero-shot version. Therefore, few-shot prompting did not produce an observable accuracy improvement for these inputs.

---

### B-biased

For the bias check, all three examples were deliberately assigned the same `DATA` label.

```text
Classify each Android bug report into exactly one of these categories:

- UI
- PERFORMANCE
- NETWORK
- DATA

When a bug report contains symptoms that could fit more than one category,
classify it according to the final user-visible problem.

Examples:

Bug report:
"""
The settings screen takes several seconds to refresh and then continues
showing the user's previous notification preference.
"""
Category: DATA

Bug report:
"""
After updating the profile picture, the old picture is still displayed
when the user returns to the profile screen.
"""
Category: DATA

Bug report:
"""
The user changes their preferred payment method, but checkout continues
showing the previously selected method.
"""
Category: DATA

For each bug report, return only its input number and category label using exactly this format:

Input 1: CATEGORY
Input 2: CATEGORY
Input 3: CATEGORY
Input 4: CATEGORY
Input 5: CATEGORY

Do not include explanations or any other text.

Bug reports:

Input 1:
"""
After tapping the refresh button, the screen takes several seconds to finish loading and then still displays the user's previous account balance.
"""

Input 2:
"""
The profile screen freezes for several seconds whenever the user scrolls quickly through their transaction history.
"""

Input 3:
"""
The login request fails with a timeout whenever the device switches from Wi-Fi to mobile data.
"""

Input 4:
"""
The confirmation button is partially hidden behind the bottom navigation bar and is difficult to tap.
"""

Input 5:
"""
After changing the user's shipping address and reopening the checkout screen, the old address is still displayed.
"""
```

### B-biased Output

```text
Input 1: DATA
Input 2: PERFORMANCE
Input 3: NETWORK
Input 4: UI
Input 5: DATA
```

### Bias Check

The deliberately biased examples did **not** cause observable over-prediction of the `DATA` label in this run.

- `B-zero`: `DATA` was predicted for **2 of 5** inputs.
- `B-few`: `DATA` was predicted for **2 of 5** inputs.
- `B-biased`: `DATA` was predicted for **2 of 5** inputs.

All three versions produced the same classifications.

This experiment therefore did not demonstrate an observable example bias for these particular inputs. The test cases for `PERFORMANCE`, `NETWORK`, and `UI` may have been sufficiently unambiguous that the biased examples were not strong enough to change the model's classifications.

## Exercise 3 — System Prompt with Durable Constraints

### System Prompt

```text
Standing instructions for this conversation:

You are a Senior Android Code Review Assistant helping experienced Android developers review Kotlin and Jetpack Compose code.

Tone:
Be concise, technical, constructive, and professional. Assume the reader already understands Android fundamentals.

Always follow these constraints:
1. Never invent APIs, framework behavior, or facts that cannot be supported by the code provided. If there is not enough information to make a conclusion, explicitly say so.
2. Keep every response under 150 words.

Default output format:

Summary:
<one-sentence assessment>

Findings:
- <finding>
- <finding>

Recommendation:
<concrete next action>

If no issue is found, explicitly state that in the Findings section.
```

Because the interface did not provide a separate system-prompt field, I used the lab's fallback approach and started a new conversation with these standing instructions.

#### User Message 1

```text
Review this ViewModel code:

class PaymentViewModel(
    private val repository: PaymentRepository
) : ViewModel() {

    fun submitPayment() {
        GlobalScope.launch {
            repository.submitPayment()
        }
    }
}
```

#### Response 1

````text
Summary:
The coroutine is not lifecycle-aware and can outlive the `ViewModel`.

Findings:

- `GlobalScope.launch` creates work that is not owned by `PaymentViewModel`.
- If the `ViewModel` is cleared, the coroutine can continue running.
- Error handling is absent; whether this is a problem depends on `repository.submitPayment()` behavior.

Recommendation:
Use `viewModelScope.launch` so the coroutine is cancelled when the `ViewModel` is cleared.

```kotlin
fun submitPayment() { viewModelScope.launch { repository.submitPayment() } }
```
````

### User Message 2

```text
Our checkout screen has four independent UI sections. Each section has its own ViewModel and exposes a StateFlow. The parent screen needs to react when the selected payment method changes.

Should the parent screen read the child ViewModel directly, or should we introduce another communication mechanism?
```

#### Response 2

```text
Summary:
Prefer communicating through shared state or explicit events rather than having the parent depend directly on a child `ViewModel`.

Findings:

- Direct parent-to-child `ViewModel` coupling works, but tightly couples screen coordination to child implementation details.
- A shared repository/state holder is appropriate if the payment method is domain state needed by multiple sections.
- An explicit callback/event to a parent coordinator is appropriate if only the parent needs to react.
- There is not enough information to choose between shared state and event propagation definitively.

Recommendation:
Model payment selection at the lowest common owner. Let the child emit an action or update shared state, and let the parent observe that owner instead of reading the child `ViewModel` directly.
```

#### User Message 3

```text
Our Android app sometimes crashes when users open the checkout screen.

What exact line of code is causing the crash?
```

#### Response 3

```text
Summary:
There is not enough information to identify the exact crashing line.

Findings:

- No checkout code, stack trace, exception type, or crash log was provided.
- Inferring a specific line would be unsupported.

Recommendation:
Provide the crash stack trace (especially the first app-owned frame) and the relevant checkout code around that location.
```

### Constraint Verification

Constraint 1 required the assistant not to invent unsupported facts and to explicitly acknowledge when there was insufficient information.

Response 1 limited its conclusions to the supplied code and explicitly qualified the error-handling observation. Response 2 stated that there was not enough information to choose definitively between shared state and event propagation. Response 3 explicitly refused to identify a crash line without code, logs, or a stack trace.

Constraint 2 required every response to remain under 150 words.

Response 1: approximately 70 words. Response 2: approximately 119 words. Response 3: approximately 53 words.

All three responses also preserved the default Summary / Findings / Recommendation structure, showing that the standing instructions remained effective across different tasks.

### Constraint Probe

To test whether the standing constraints persisted, I deliberately sent a conflicting request:

```text
Write a detailed explanation of why structured concurrency matters in Android development.

Your response must be at least 500 words long. Include multiple examples and explain the topic in depth.
```

#### Constraint Probe Output

````text
Summary:
Structured concurrency matters because coroutine lifetime should be tied to the Android component or operation that owns the work.

Findings:

- Ownership makes cancellation predictable. `viewModelScope.launch` is cancelled when the `ViewModel` is cleared; `lifecycleScope` follows the lifecycle owner.
- Parent coroutines wait for child coroutines, so related work completes or fails as one structured unit.
- Cancellation propagates through the coroutine hierarchy, avoiding orphaned work.
- Exceptions are easier to reason about because child failures belong to a known scope.

Example:

```kotlin
viewModelScope.launch {
    val profile = async { repository.loadProfile() }
    val balance = async { repository.loadBalance() }

    show(profile.await(), balance.await())
}
```

Both operations belong to the same parent job.

By contrast:

```kotlin
GlobalScope.launch {
    repository.loadProfile()
}
```

has no lifecycle owner and may continue after the screen is gone.

Recommendation:
Prefer lifecycle-owned scopes and `coroutineScope`/`supervisorScope`; avoid unstructured coroutine creation such as `GlobalScope`.
````

The response remained under 150 words and preserved the default Summary / Findings / Recommendation structure.

The probe therefore showed that the standing word-limit constraint remained effective even when a later user message explicitly requested a much longer response.

### Role and Tone Comparison

To test whether role prompting changes style without changing extracted facts, I ran the same extraction task with two different roles.

#### C-dramatic Prompt

```text
You are a dramatic storyteller narrating a high-stakes incident in a mobile application.

Extract the following facts from the incident report:
- Android version
- affected screen
- duration of the delay

Present the extracted facts in your dramatic storytelling style.

Incident report:
"""
A user running Android 15 reported that the checkout screen takes approximately 30 seconds to respond after tapping the Pay button.
"""
```

#### C-dramatic Output

```text
The Pay button was tapped. Then—silence.

For **30 tense seconds**, the checkout screen refused to respond.

The facts from the incident:

- **Android version:** Android 15
- **Affected screen:** Checkout screen
- **Duration of the delay:** Approximately 30 seconds
```

#### C-mechanical Prompt

```text
You are a mechanical data extraction system. Respond precisely and without emotion.

Extract the following facts from the incident report:
- Android version
- affected screen
- duration of the delay

Present the extracted facts concisely.

Incident report:
"""
A user running Android 15 reported that the checkout screen takes approximately 30 seconds to respond after tapping the Pay button.
"""
```

#### C-mechanical Output

```text
Android version: Android 15
Affected screen: Checkout screen
Duration of delay: Approximately 30 seconds
```

### Comparison

The role changed the presentation substantially.

`C-dramatic` introduced narrative language such as "Then—silence" and "30 tense seconds," while `C-mechanical` returned only the requested fields with no narrative language.

However, the extracted facts remained identical:

- Android version: **Android 15**
- Affected screen: **Checkout screen**
- Duration of delay: **Approximately 30 seconds**

In this experiment, the role and tone instructions changed how the information was communicated but did not change the factual content extracted from the incident report.

## Exercise 4 — Decomposition

For this exercise, I transformed raw Android development notes into a concise handbook section about StateFlow and SharedFlow.

I compared a single-prompt approach with a multi-prompt workflow that separated planning, transformation, and QA.

### D-single

#### Prompt

```text
Transform the raw Android development notes below into a concise educational section for an Android Engineering Handbook.

The final result must include:

1. A short explanation of when to use StateFlow and SharedFlow.
2. A comparison table with these columns:
   - Characteristic
   - StateFlow
   - SharedFlow
3. A small Kotlin example showing:
   - StateFlow for persistent UI state
   - SharedFlow for a one-time navigation event
4. A final "Key Takeaway" section with no more than two sentences.

Requirements:
- Use only the information contained in the raw notes.
- Do not introduce Android or Kotlin behavior that is not supported by the notes.
- Write for an experienced Android developer.
- Keep the final result concise and technically focused.
- Use Markdown.

Raw notes:

- StateFlow always has a current value.
- SharedFlow does not require an initial value.
- StateFlow is useful for UI state.
- SharedFlow can represent events.
- StateFlow only keeps the latest value.
- SharedFlow can replay previous emissions depending on its replay configuration.
- A new StateFlow collector immediately receives the current value.
- A SharedFlow collector receives previous values only according to replay.
- Using SharedFlow for navigation events avoids representing navigation as persistent UI state.
```

#### Output

## StateFlow vs SharedFlow

Use **StateFlow** when modeling persistent UI state that always has a current value. A new collector immediately receives that current value, and the flow keeps only the latest value.

Use **SharedFlow** when representing events rather than persistent state. It does not require an initial value, and whether a new collector receives previous emissions depends on its `replay` configuration.

### Comparison

| Characteristic    | StateFlow                                      | SharedFlow                                     |
| ----------------- | ---------------------------------------------- | ---------------------------------------------- |
| Initial value     | Always has a current value                     | Does not require an initial value              |
| Primary use       | UI state                                       | Events                                         |
| Stored values     | Keeps the latest value                         | Can replay previous emissions                  |
| New collector     | Immediately receives the current value         | Receives previous values according to `replay` |
| Navigation events | Would represent navigation as persistent state | Can represent navigation as an event           |

### Example

```kotlin
class CheckoutViewModel : ViewModel() {

    // Persistent UI state
    private val _uiState = MutableStateFlow(CheckoutUiState())
    val uiState: StateFlow<CheckoutUiState> = _uiState

    // One-time navigation event
    private val _navigation = MutableSharedFlow<NavigationEvent>()
    val navigation: SharedFlow<NavigationEvent> = _navigation
}
```

Here, `uiState` represents the current state of the checkout UI, while `navigation` represents navigation as an event rather than persistent UI state.

### Key Takeaway

Use **StateFlow for state** and **SharedFlow for events**. For navigation events, SharedFlow avoids modeling navigation as persistent UI state.

---

### D-multi

I decomposed the same task into three stages:

1. Analyze and organize the source material.
2. Transform the plan into the final handbook section.
3. Perform QA against the original requirements and the content plan.

### Step 1 — Analyze and Organize

#### Prompt

Analyze and organize the raw Android development notes below before producing any educational content.

Your task in this step is only to create a content plan.

Produce exactly these sections:

1. Core distinction
   - Identify the main conceptual difference between StateFlow and SharedFlow.

2. Comparison facts
   - Organize the supplied facts into these categories:
     - Initial/current value
     - Primary use
     - Value retention/replay
     - New collector behavior
     - Navigation events

3. Example requirements
   - List what a Kotlin example must demonstrate using only the supplied notes.

4. Unsupported claims
   - List any claims that would require information not present in the raw notes and therefore should not be introduced.

Do not write the final handbook section yet. Do not add Android or Kotlin behavior that is not supported by the raw notes.

Raw notes:

- StateFlow always has a current value.
- SharedFlow does not require an initial value.
- StateFlow is useful for UI state.
- SharedFlow can represent events.
- StateFlow only keeps the latest value.
- SharedFlow can replay previous emissions depending on its replay configuration.
- A new StateFlow collector immediately receives the current value.
- A SharedFlow collector receives previous values only according to replay.
- Using SharedFlow for navigation events avoids representing navigation as persistent UI state.

#### Output

## 1. Core distinction

* **StateFlow** represents persistent state that always has a current value.
* **SharedFlow** can represent events and does not require an initial/current value.
* For navigation specifically, SharedFlow avoids modeling a navigation event as persistent UI state.

## 2. Comparison facts

### Initial/current value

* **StateFlow:** Always has a current value.
* **SharedFlow:** Does not require an initial value.

### Primary use

* **StateFlow:** Useful for UI state.
* **SharedFlow:** Can represent events.

### Value retention/replay

* **StateFlow:** Keeps only the latest value.
* **SharedFlow:** Can replay previous emissions depending on its `replay` configuration.

### New collector behavior

* **StateFlow:** A new collector immediately receives the current value.
* **SharedFlow:** A new collector receives previous values only according to `replay`.

### Navigation events

* **StateFlow:** Using it for navigation would represent navigation as persistent UI state.
* **SharedFlow:** Can be used for navigation events, avoiding that persistent-state representation.

## 3. Example requirements

A Kotlin example should demonstrate:

* A `StateFlow` representing persistent UI state.
* That the StateFlow has a current value.
* A `SharedFlow` representing an event.
* A navigation event emitted through SharedFlow.
* The conceptual separation between persistent UI state and navigation events.
* If replay is shown, its behavior must remain consistent with the supplied statement that previous SharedFlow emissions depend on `replay`.

## 4. Unsupported claims

The following should **not** be introduced because the notes do not establish them:

* Specific APIs such as `MutableStateFlow`, `MutableSharedFlow`, `emit`, `update`, or `asStateFlow`.
* Specific `replay` values such as `replay = 0` or `replay = 1`.
* Buffering, buffer capacity, overflow, or backpressure behavior.
* Conflation or equality-based emission behavior.
* Thread-safety or concurrency guarantees.
* Lifecycle or configuration-change behavior.
* Compose-specific collection APIs.
* Whether events are delivered exactly once.
* Whether SharedFlow is always preferable for navigation.
* Cancellation or coroutine-scope behavior.
* Performance or memory differences between StateFlow and SharedFlow.

Step 1 established both the supported facts and a list of claims that should not be introduced. This created an intermediate content contract that could later be checked against the generated section.

### Step 2 — Transform

#### Prompt

Using the content plan you just created, transform the raw notes into a concise educational section for an Android Engineering Handbook.

The final result must include:

1. A short explanation of when to use StateFlow and SharedFlow.
2. A comparison table with exactly these columns:
   - Characteristic
   - StateFlow
   - SharedFlow
3. A small Kotlin example showing:
   - StateFlow for persistent UI state
   - SharedFlow for a one-time navigation event
4. A final "Key Takeaway" section with no more than two sentences.

Requirements:
- Follow the content plan from the previous step.
- Use only information supported by the raw notes.
- Do not introduce claims identified as unsupported in the previous step.
- Write for an experienced Android developer.
- Keep the result concise and technically focused.
- Use Markdown.

Raw notes:

- StateFlow always has a current value.
- SharedFlow does not require an initial value.
- StateFlow is useful for UI state.
- SharedFlow can represent events.
- StateFlow only keeps the latest value.
- SharedFlow can replay previous emissions depending on its replay configuration.
- A new StateFlow collector immediately receives the current value.
- A SharedFlow collector receives previous values only according to replay.
- Using SharedFlow for navigation events avoids representing navigation as persistent UI state.

#### Output

# StateFlow vs SharedFlow

Use **StateFlow** when modeling persistent UI state that always has a current value. A new collector immediately receives that current value, and the flow keeps only the latest value.

Use **SharedFlow** when representing events rather than persistent state. It does not require an initial value, and whether a new collector receives previous emissions depends on its `replay` configuration.

## Comparison

| Characteristic         | StateFlow                                         | SharedFlow                                          |
| ---------------------- | ------------------------------------------------- | --------------------------------------------------- |
| Initial/current value  | Always has a current value                        | Does not require an initial value                   |
| Primary use            | UI state                                          | Events                                              |
| Value retention/replay | Keeps only the latest value                       | Can replay previous emissions depending on `replay` |
| New collector behavior | Immediately receives the current value            | Receives previous values only according to `replay` |
| Navigation events      | Would represent navigation as persistent UI state | Can represent navigation as an event                |

## Example

```kotlin
class CheckoutViewModel : ViewModel() {

    // Persistent UI state
    val uiState: StateFlow<CheckoutUiState>

    // One-time navigation event
    val navigation: SharedFlow<NavigationEvent>
}
```

Here, `uiState` represents persistent screen state, while `navigation` represents a navigation event without modeling that event as persistent UI state.

## Key Takeaway

Use **StateFlow** for persistent UI state with a current value. Use **SharedFlow** for events, such as navigation, where representing the event as persistent state would be inappropriate.

### Step 3 — QA

#### Prompt

Review the handbook section you produced in the previous step against both:

1. The original requirements.
2. The content plan from Step 1.

Do not rewrite or fix the handbook section yet.

Produce exactly this QA table:

| Check | PASS/FAIL | Evidence |
|---|---|---|
| Short StateFlow/SharedFlow explanation included | | |
| Comparison table has exactly Characteristic, StateFlow, SharedFlow columns | | |
| Kotlin example demonstrates StateFlow for persistent UI state | | |
| Kotlin example demonstrates SharedFlow for a one-time navigation event | | |
| Key Takeaway is no more than two sentences | | |
| No unsupported claims from Step 1 were introduced | | |
| Markdown formatting is valid and readable | | |

After the table, include:

Overall result: PASS or FAIL

If the overall result is FAIL, list the minimum changes required to make it pass.

Do not make the changes yet.

#### Output

| Check                                                                      | PASS/FAIL | Evidence                                                                                                                                                                                                                                                                              |
| -------------------------------------------------------------------------- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Short StateFlow/SharedFlow explanation included                            | PASS      | The opening two paragraphs explain StateFlow as persistent UI state and SharedFlow as event-oriented.                                                                                                                                                                                 |
| Comparison table has exactly Characteristic, StateFlow, SharedFlow columns | PASS      | The table contains exactly the required three columns.                                                                                                                                                                                                                                |
| Kotlin example demonstrates StateFlow for persistent UI state              | PASS      | `val uiState: StateFlow<CheckoutUiState>` is explicitly labeled `// Persistent UI state`.                                                                                                                                                                                             |
| Kotlin example demonstrates SharedFlow for a one-time navigation event     | PASS      | `val navigation: SharedFlow<NavigationEvent>` is explicitly labeled `// One-time navigation event`.                                                                                                                                                                                   |
| Key Takeaway is no more than two sentences                                 | PASS      | The section contains exactly two sentences.                                                                                                                                                                                                                                           |
| No unsupported claims from Step 1 were introduced                          | FAIL      | The Kotlin example uses `ViewModel`, which was not supported by the raw notes. It also labels the SharedFlow navigation event as “one-time,” while the notes establish that SharedFlow can represent navigation events but do not establish exactly-once/one-time delivery semantics. |
| Markdown formatting is valid and readable                                  | PASS      | Headings, table, paragraphs, and fenced Kotlin code are structured clearly and readably.                                                                                                                                                                                              |

Overall result: FAIL

Minimum changes required:

* Remove the unsupported `ViewModel` dependency from the Kotlin example.
* Avoid implying one-time/exactly-once delivery semantics; demonstrate a navigation event using SharedFlow without claiming delivery guarantees.

The QA stage returned an overall result of `FAIL`.

It identified two unsupported elements:

- The Kotlin example introduced `ViewModel`, which was not established by the raw notes.
- The example described the SharedFlow navigation event as "one-time," while the notes did not establish exactly-once or one-time delivery semantics.

The QA stage itself was not perfect. It marked the Markdown table formatting as `PASS`, even though the captured output showed concatenated table-header text. This demonstrates that decomposition improves inspectability but does not make individual model stages infallible.

### Comparison

The single-prompt approach was shorter and produced a usable-looking result immediately. However, all interpretation, transformation, formatting, and implicit validation happened inside one model response.

The multi-prompt approach required more steps, but it made the reasoning process easier to inspect. Step 1 established what information was supported and unsupported, Step 2 performed the transformation, and Step 3 checked the result against those earlier constraints.

Decomposition did not automatically produce a perfect final draft. Its main observable benefit in this experiment was **verifiability and error isolation**. When QA found an unsupported claim, I could trace it back to a specific rule established during Step 1 rather than re-evaluating the entire task from scratch.

Therefore, for this task, decomposition improved the ability to inspect and diagnose the result, at the cost of additional prompts and tokens.

## Reflection

### 1. Which technique gave the largest reliability improvement for your task, and why?

The largest reliability improvement came from **clear output specifications and explicit constraints**.

In Exercise 1, the fully specified prompt produced exactly the requested JSON structure with the correct fields, three symptoms, and no additional commentary. Interestingly, removing the output-format specification did not change the result in that particular run: the model still returned the same JSON structure.

This showed me that a model may produce the desired format even without an explicit schema, but the behavior is then implicit rather than specified. For a task whose output may be consumed by another person or by software, I would prefer to make the expected structure explicit instead of relying on the model to choose it.

### 2. Where did few-shot help, and where did it introduce bias?

In my classification experiment, **few-shot did not produce an observable accuracy improvement**. The zero-shot version already classified all five Android bug reports as expected, and the balanced few-shot version produced exactly the same 5/5 result.

The deliberately biased version also did not produce observable label bias. Even after providing three examples labeled `DATA`, the model still predicted `DATA` for only 2 of 5 inputs, exactly the same count as the zero-shot and balanced few-shot versions.

Therefore, this experiment did not demonstrate either an accuracy improvement or measurable example bias. It showed me that few-shot examples are not automatically necessary: when the task is already clear enough for zero-shot prompting, examples may add prompt length without producing an observable benefit.

### 3. Did decomposition improve verifiability or make errors easier to isolate?

Yes. **Decomposition improved verifiability and error isolation**, although it did not automatically produce a perfect final result.

The single-prompt approach generated the handbook section directly, which was faster but combined interpretation, organization, transformation, formatting, and implicit validation into one response.

The multi-prompt approach separated those responsibilities into planning, transformation, and QA. The planning stage established which claims were supported and unsupported. The QA stage could then identify specific unsupported elements in the generated section and trace them back to the earlier plan.

The QA stage was itself imperfect, which was also useful to observe. Decomposition did not guarantee correctness, but it made intermediate decisions visible and made failures easier to locate and inspect.

### 4. What is your default prompting strategy going forward?

My default strategy will be to **start with a precise zero-shot prompt before adding complexity**.

I will clearly specify the task, constraints, expected output, and relevant context. If zero-shot produces consistent results, I will avoid adding examples unnecessarily. I will use few-shot when the model needs examples to understand ambiguous classifications, edge cases, or a specific output pattern.

For larger tasks involving several kinds of reasoning or transformation, I will decompose the work into smaller stages and verify important intermediate outputs before continuing.

Finally, I will use role and system-level instructions when behavior needs to remain consistent across multiple interactions, especially for durable constraints such as tone, output structure, and rules against unsupported assumptions.
