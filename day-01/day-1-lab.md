# Day 1 Lab — Diagnose and Rebuild a Prompt

## Exercise 1 — The Assumption Audit

### v1 Prompt
Help me reply to this LinkedIn message: 
"Hi Jorge, 

I came across your profile and wanted to reach out about a Senior Android Engineer opportunity. The position is fully remote and we're looking for someone with strong Kotlin and Jetpack Compose experience. 

Would you be interested in learning more?"

### v1 Output
Hi! Thanks for reaching out. Yes, I’d definitely be interested in learning more about the opportunity. The role sounds well aligned with my Android experience, especially Kotlin and Jetpack Compose.

I’d be happy to hear more about the company, the project, and what you’re looking for in the role.

Thanks!
Jorge

### Assumption Audit

| Assumption | What the model guessed | Was it acceptable? |
|---|---|---|
| My interest in the opportunity | It assumed I am interested and wrote "Yes, I’d definitely be interested." | Acceptable for this case, but I never explicitly said I was interested. |
| Tone | It chose a friendly and professional tone. | Acceptable. |
| Length | It assumed the reply should be short, around two paragraphs. | Acceptable for LinkedIn. |
| My Android background | It inferred that Kotlin and Jetpack Compose align with my experience. | Reasonable, but the prompt never explicitly provided my experience. |
| What information I want next | It decided to ask about the company, project, and role expectations. | Acceptable, but I never specified which information I wanted. |
| Response format | It produced ready-to-send prose rather than bullets, alternatives, or commentary. | Acceptable and useful, but not specified. |
| Closing/signature | It chose "Thanks! Jorge" as the closing. | Acceptable, but the desired closing style was not specified. |

### Audit Summary

The v1 output is usable, but the model had to make several decisions that were not specified in the prompt. Most of its guesses happened to be acceptable, especially the tone and length, but important details such as my actual interest in the position, my relevant experience, and what information I wanted from the recruiter were inferred rather than provided.

## Exercise 2 — The Four-Block Rewrite
Instruction:
Write a reply to the LinkedIn recruiter message below expressing interest in learning more about the Senior Android Engineer opportunity.

Context:
I'm a Senior Android Engineer with over 10 years of experience designing, building, and evolving large-scale Android applications, including fintech and cryptocurrency products used by millions of users worldwide. I have strong expertise in Kotlin, Java, Jetpack Compose, Coroutines, Flow, and modern Android application architecture (MVVM, MVI, Clean Architecture).

The recipient is a recruiter who contacted me through LinkedIn. I am interested in the opportunity and would like to learn more about the company. Do not ask about salary or benefits at this stage.

Input:
"""
Hi Jorge,

I came across your profile and wanted to reach out about a Senior Android Engineer opportunity. The position is fully remote and we're looking for someone with strong Kotlin and Jetpack Compose experience.

Would you be interested in learning more?
"""

Output format:
Write the response in English as a ready-to-send LinkedIn message.

Use a friendly and professional tone. Keep it to a maximum of two short paragraphs. Close the message with "Looking forward to hearing more about the position" followed by my name, Jorge.

Return only the final message, without explanations or additional commentary.

### v2 Output

Hi! Thanks for reaching out. Yes, I’d be interested in learning more about the Senior Android Engineer opportunity. The role sounds well aligned with my experience, particularly with Kotlin and Jetpack Compose. I’d also love to hear more about the company, the product, and what the team is currently working on.

Looking forward to hearing more about the position,
Jorge

### v1 vs. v2 Comparison

The Context block made the biggest difference. In v1, the model had to infer that I was interested in the opportunity, that Kotlin and Jetpack Compose matched my experience, and what information I wanted from the recruiter. In v2, those details were explicitly provided, so the output reflected my actual background and intent rather than relying on assumptions.

### Resolved Assumptions

The v2 prompt resolved the following assumptions from Exercise 1:

- My interest in the opportunity is now explicit.
- My Android background and relevant technical experience are provided.
- The recipient is explicitly identified as a LinkedIn recruiter.
- The information I want next is specified.
- Salary and benefits are explicitly excluded at this stage.
- The tone is defined as friendly and professional.
- The length is limited to two short paragraphs.
- The response format is defined as a ready-to-send English LinkedIn message.
- The closing is explicitly specified.

There are no important assumptions from the original audit still open for this task. Minor wording choices remain intentionally left to the model.

### Output Format Ablation
Instruction: 
Write a reply to the LinkedIn recruiter message below expressing interest in learning more about the Senior Android Engineer opportunity. 

Context: 
I'm a Senior Android Engineer with over 10 years of experience designing, building, and evolving large-scale Android applications, including fintech and cryptocurrency products used by millions of users worldwide. I have strong expertise in Kotlin, Java, Jetpack Compose, Coroutines, Flow, and modern Android application architecture (MVVM, MVI, Clean Architecture). The recipient is a recruiter who contacted me through LinkedIn. I am interested in the opportunity and would like to learn more about the company. Do not ask about salary or benefits at this stage. 

Input:
 """ 
 Hi Jorge, 
 
 I came across your profile and wanted to reach out about a Senior Android Engineer opportunity. The position is fully remote and we're looking for someone with strong Kotlin and Jetpack Compose experience. 
 
 Would you be interested in learning more? """

### Ablation Output
Hi! Thanks for reaching out. Yes, I’d be interested in learning more about the Senior Android Engineer opportunity. The role sounds well aligned with my experience, particularly with Kotlin and Jetpack Compose. I’d also love to hear more about the company, the product, and what the team is currently working on.

Looking forward to hearing more about the position,
Jorge



## Exercise 3 — Token Budget and Variability
### Part A — Estimate Before You Send

For the original input, I used the full "Anatomy of a Prompt" section from the Prompt Engineering Foundations material.

#### Original Input

- Characters: 3,028
- Words: 509
- Estimated tokens: 757

I then chose a specific question:

**Where should the output format be placed in a prompt, and why?**

Most of the original text was not necessary to answer this question. I kept only the relevant portion.

#### Trimmed Input

> Order matters too. A reliable pattern is: instruction first, then context, then the clearly delimited input, then a restatement of the output format. Putting the format near the end keeps it fresh as the model begins generating.

- Characters: 228
- Words: 37
- Estimated tokens: 57

#### Token Reduction

Original estimated tokens: 757  
Trimmed estimated tokens: 57

Percentage cut:

(757 - 57) / 757 × 100 = **92.5%**

The trimmed input contains approximately 92.5% fewer estimated tokens while still containing the information needed to answer the specific question.

#### Prompt Using the Trimmed Input

Answer the following question using only the provided text.

Question:
Where should the output format be placed in a prompt, and why?

Text:
"""
Order matters too. A reliable pattern is: instruction first, then context, then the clearly delimited input, then a restatement of the output format. Putting the format near the end keeps it fresh as the model begins generating.
"""

### Part B — Observe Variability

#### Open-Ended Prompt

Suggest a name for a community garden.

#### Run 1

Here are some name ideas, depending on the vibe you want:

The Green Haven — peaceful and welcoming
Common Ground Garden — emphasizes community and connection
Roots & Neighbors — warm and community-focused
The Growing Together Garden — emphasizes collaboration
Harvest Corner — simple and friendly
Green Roots Community Garden — natural and memorable
The Shared Patch — casual and charming
Bloom & Belong — modern and uplifting
Sunflower Commons — cheerful and inviting
Seed & Soil Community Garden — straightforward and earthy

My favorite is Common Ground Garden because “common ground” works both literally—the shared land—and figuratively as a place that brings people together.

#### Run 2

How about Common Ground Garden? It feels warm and community-focused, with a nice double meaning: shared land and bringing people together.

#### Run 3

How about **Common Ground Garden**? It suggests both shared land and a place where neighbors come together.

### Variability Comparison

The open-ended prompt produced more variable output. Although all three runs suggested "Common Ground Garden", the structure and amount of information varied significantly. The first run returned ten suggestions and selected a favorite, while the second and third runs returned only one suggestion with different amounts of explanation.

The tightly specified prompt produced identical output in all three runs:

`2026-03-01`

The difference comes from how much freedom each prompt gives the model. "Suggest a name for a community garden" allows many valid answers and does not specify the number of suggestions, response length, or format. In contrast, the ISO 8601 prompt defines both a specific factual answer and an exact output format, leaving very little room for variation.

This demonstrates that tighter specifications can reduce output variability, although they do not make an LLM inherently deterministic.

## Reflection

### 1. Which of your real tasks need consistency between runs, and which benefit from variety?

Some of my tasks require consistency, especially when I need structured or predictable outputs. For example, classifying a customer support issue or extracting specific information should produce a consistent structure and should not depend on creative interpretation.

Other tasks benefit from variety. For example, when brainstorming ideas for technical content, article titles, or ways to explain an engineering concept, different outputs can help me explore alternatives that I might not have considered.

### 2. What is the one habit from today you will apply to the next prompt you write, before you write it?

The main habit I will apply is to think about what information the model actually needs before writing the prompt. I want to clearly specify what I need, provide only the relevant context and input, and define the expected output when it matters, instead of forcing the model to guess my intent.

I also learned that providing more context does not mean copying everything available into the prompt. Relevant and precise context can reduce ambiguity while also reducing unnecessary token usage.