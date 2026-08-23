# FL-02 — Prompting Fundamentals on Real Tasks

**Track:** General AI Fluency
**Week:** 2
**Phase:** Foundations
**Task:** Improve the ML problem framing and development plan for an AI Traffic Monitoring System

---

# 1. Selected FL-01 Task

## Task

I selected a real machine-learning task related to my AI Traffic Monitoring System, **SadakDrishti**.

The task is to use an AI/ML system to monitor road traffic by detecting and tracking vehicles and eventually supporting functions such as speed estimation and traffic-violation detection.

The immediate objective for this prompt experiment is:

> **Determine how I should improve the vehicle-detection component of the system while maintaining real-time performance.**

This is a real task because it relates directly to an ML system I am developing rather than being a hypothetical prompt-engineering exercise.

---

# 2. Version 0 — Naive Prompt

## Technique

**Baseline — No prompting technique**

## Prompt

> Help me improve my AI traffic monitoring model.

## Output

> To improve your AI traffic monitoring model, you should collect more data, improve the quality of your dataset, use a better object detection model, tune the model parameters, and evaluate it using metrics such as precision, recall, and mAP.
>
> You can also use object tracking algorithms and optimize the model for real-time performance.

## What changed and why

Nothing was deliberately added. This represents the type of one-line prompt I might have written before learning structured prompting.

## What I observed in the output

The answer was technically reasonable but extremely generic. It mentioned several common ML techniques without understanding my actual system or identifying which improvement should be attempted first.

It also did not explain how I should determine whether an improvement actually worked.

## What I would try next

I would give the AI a specific role so that it approaches the problem from the perspective of someone experienced in machine learning and computer vision.

---

# 3. Version 1 — Role Assignment

## Technique

**Role assignment**

## Prompt

> Act as an experienced machine learning and computer vision engineer. Help me improve my AI traffic monitoring model.

## Output

> As an experienced ML and computer vision engineer, I would approach the problem systematically.
>
> First, establish a baseline using precision, recall, mAP50, mAP50-95, and inference speed.
>
> Next, inspect false positives and false negatives to determine whether the main problem is the dataset, model architecture, confidence threshold, or image quality.
>
> You should then improve the training dataset, tune model parameters, and compare different YOLO model sizes.
>
> Finally, evaluate both detection accuracy and real-time performance rather than optimizing only for accuracy.

## What changed and why

I added the role **"experienced machine learning and computer vision engineer."**

The purpose was to encourage the model to approach the problem using professional ML engineering practices.

## What I observed in the output

The output became more systematic. Instead of simply listing possible improvements, it introduced a baseline, error analysis, model tuning, and evaluation.

However, the answer was still generic because the model did not know what my actual system looked like.

## What I would try next

I would provide detailed context about my project and explain why I need the improvement.

---

# 4. Version 2 — Context and Motivation

## Technique

**Context and motivation**

## Prompt

> Act as an experienced machine learning and computer vision engineer.
>
> Help me improve my AI traffic monitoring model.
>
> My project is a Nepal-focused traffic monitoring system called SadakDrishti. It uses Ultralytics YOLO for vehicle detection, ByteTrack and BoT-SORT for tracking, and OpenCV-based homography and vehicle trajectories for speed estimation. The system is intended to process traffic-camera video in real time.
>
> The main vehicle classes are cars, motorcycles, buses, and trucks. The project is intended for Nepalese road conditions, where traffic can be crowded and vehicles may be partially occluded or appear small in camera footage.
>
> My motivation is to build a practical traffic-monitoring system that can eventually support vehicle counting, speed estimation, and traffic-violation detection.
>
> Recommend how I should improve the vehicle-detection component while keeping real-time performance.

## Output

> Because SadakDrishti is intended for Nepalese traffic conditions and uses detection followed by tracking and speed estimation, improving detection quality should be the first priority.
>
> Start by evaluating the current detector separately for cars, motorcycles, buses, and trucks. Pay particular attention to small and partially occluded vehicles.
>
> Collect additional examples from Nepalese traffic scenes, especially crowded roads, motorcycles, distant vehicles, nighttime conditions, shadows, and different camera angles.
>
> After improving the dataset, evaluate the current YOLO model using precision, recall, mAP, and FPS. Then experiment with confidence thresholds, image resolution, and potentially a larger model.
>
> Because tracking and speed estimation depend on stable detections, evaluate whether detection improvements also reduce tracking errors.

## What changed and why

I added real project context and explained the motivation behind the system.

## What I observed in the output

This produced a major improvement. The recommendations were now connected to the actual pipeline.

The model recognized that detection errors can affect downstream tracking and speed estimation. It also identified Nepal-specific traffic conditions such as motorcycles, crowded roads, small vehicles, and occlusion.

## What still failed

The answer still provided recommendations rather than a concrete procedure for executing the work.

## What I would try next

I would give the model examples of the kind of recommendations I consider useful.

---

# 5. Version 3 — Few-Shot Examples

## Technique

**Few-shot examples**

## Prompt

> Act as an experienced machine learning and computer vision engineer.
>
> Help me improve my AI traffic monitoring model.
>
> My project is a Nepal-focused traffic monitoring system called SadakDrishti. It uses Ultralytics YOLO for vehicle detection, ByteTrack and BoT-SORT for tracking, and OpenCV-based homography and vehicle trajectories for speed estimation. The system is intended to process traffic-camera video in real time.
>
> The main vehicle classes are cars, motorcycles, buses, and trucks. The project is intended for Nepalese road conditions, where traffic can be crowded and vehicles may be partially occluded or appear small in camera footage.
>
> My motivation is to build a practical traffic-monitoring system that can eventually support vehicle counting, speed estimation, and traffic-violation detection.
>
> **Example 1 of a useful recommendation:**
>
> Problem: Motorcycles are frequently missed in crowded scenes.
>
> Evidence: Recall for motorcycles is significantly lower than recall for cars.
>
> Recommendation: Add more annotated motorcycle examples from crowded Nepalese traffic scenes and retrain the model.
>
> Measurement: Compare motorcycle recall and mAP before and after training.
>
> **Example 2 of a useful recommendation:**
>
> Problem: Detection is accurate but the system is too slow for real-time use.
>
> Evidence: Increasing the model size improves mAP but reduces FPS substantially.
>
> Recommendation: Compare model size and input resolution combinations and select the configuration that provides an acceptable accuracy/speed trade-off.
>
> Measurement: Compare mAP, FPS, and latency.
>
> Using these examples as a guide, recommend how I should improve my vehicle-detection component.

## Output

> **1. Problem: Small and distant vehicles are missed**
>
> **Evidence:** Small vehicles contain fewer pixels and are harder for the detector to identify reliably.
>
> **Recommendation:** Add more small and distant vehicle examples to the training dataset and evaluate a higher input resolution.
>
> **Measurement:** Compare small-object recall, mAP, and FPS.
>
> **2. Problem: Motorcycle detection may be weaker in crowded scenes**
>
> **Evidence:** Motorcycles are frequently partially occluded and appear close together.
>
> **Recommendation:** Add targeted motorcycle examples from crowded traffic scenes.
>
> **Measurement:** Compare motorcycle precision, recall, and mAP.
>
> **3. Problem: Larger models may reduce real-time performance**
>
> **Evidence:** A larger detector can improve accuracy but requires more computation.
>
> **Recommendation:** Compare the current model with a larger model using identical validation data.
>
> **Measurement:** Compare mAP, FPS, and latency.

## What changed and why

I added two concrete examples showing the desired structure and level of reasoning.

## What I observed in the output

The model began producing recommendations in a much more consistent format.

It started connecting:

**problem → evidence → recommendation → measurement**

instead of simply listing possible improvements.

## What still failed

The answer was clearer, but the recommendations were still presented as a list. It did not yet provide a consistent structure for the entire response.

## What I would try next

I would explicitly define the required output structure.

---

# 6. Version 4 — Output Structure

## Technique

**Output structure**

## Prompt

> Act as an experienced machine learning and computer vision engineer.
>
> Help me improve my AI traffic monitoring model.
>
> My project is a Nepal-focused traffic monitoring system called SadakDrishti. It uses Ultralytics YOLO for vehicle detection, ByteTrack and BoT-SORT for tracking, and OpenCV-based homography and vehicle trajectories for speed estimation. The system is intended to process traffic-camera video in real time.
>
> The main vehicle classes are cars, motorcycles, buses, and trucks. The project is intended for Nepalese road conditions, where traffic can be crowded and vehicles may be partially occluded or appear small in camera footage.
>
> My motivation is to build a practical traffic-monitoring system that can eventually support vehicle counting, speed estimation, and traffic-violation detection.
>
> **Example of the desired reasoning:**
>
> Problem → Evidence → Recommendation → Measurement.
>
> **Return your answer using exactly this structure for every recommendation:**
>
> ### Recommendation [number]
>
> **Problem:**
> **Evidence to look for:**
> **Recommended action:**
> **Metric:**
> **Expected effect:**
>
> End with a **Priority Order** containing the five most important actions.
>
> Do not recommend replacing the entire existing architecture.

## Output

> ### Recommendation 1
>
> **Problem:** Small and distant vehicles are missed.
>
> **Evidence to look for:** Review false negatives and determine whether missed detections are concentrated among small vehicles.
>
> **Recommended action:** Add representative small-object examples to the training dataset and evaluate different input resolutions.
>
> **Metric:** Small-object recall, mAP50, and FPS.
>
> **Expected effect:** Improved detection of distant vehicles while maintaining acceptable inference speed.
>
> ### Recommendation 2
>
> **Problem:** Motorcycle detection is unstable in crowded scenes.
>
> **Evidence to look for:** Compare motorcycle recall against other vehicle classes.
>
> **Recommended action:** Add more crowded motorcycle scenes to the training dataset.
>
> **Metric:** Motorcycle precision, recall, and mAP.
>
> **Expected effect:** More reliable motorcycle detection.
>
> ### Recommendation 3
>
> **Problem:** Detection and real-time performance may conflict.
>
> **Evidence to look for:** Compare inference speed across different model sizes and image resolutions.
>
> **Recommended action:** Benchmark configurations before selecting a larger model.
>
> **Metric:** mAP, FPS, and latency.
>
> **Expected effect:** Evidence-based selection of the best accuracy/speed configuration.
>
> ## Priority Order
>
> 1. Establish a baseline.
> 2. Analyze false positives and false negatives.
> 3. Improve the dataset based on observed errors.
> 4. Tune image resolution and confidence thresholds.
> 5. Benchmark model-size alternatives.

## What changed and why

I added an explicit output structure.

## What I observed in the output

The response became significantly easier to compare and use.

Every recommendation now followed the same pattern, making it easier to identify:

* what the problem is,
* what evidence is required,
* what action to take,
* how to measure it,
* and what improvement is expected.

This was more useful than simply requesting a "detailed answer."

## What still failed

The answer still asked me to perform several actions without explicitly breaking the overall ML improvement process into sequential stages.

## What I would try next

I would decompose the task into a sequence of smaller steps.

---

# 7. Version 5 — Step Decomposition

## Technique

**Step decomposition**

## Prompt

> Act as an experienced machine learning and computer vision engineer.
>
> Help me improve my AI traffic monitoring model.
>
> My project is a Nepal-focused traffic monitoring system called SadakDrishti. It uses Ultralytics YOLO for vehicle detection, ByteTrack and BoT-SORT for tracking, and OpenCV-based homography and vehicle trajectories for speed estimation. The system is intended to process traffic-camera video in real time.
>
> The main vehicle classes are cars, motorcycles, buses, and trucks. The project is intended for Nepalese road conditions, where traffic can be crowded and vehicles may be partially occluded or appear small in camera footage.
>
> My motivation is to build a practical traffic-monitoring system that can eventually support vehicle counting, speed estimation, and traffic-violation detection.
>
> Use these examples as a guide:
>
> **Example:**
> Problem → Evidence → Recommendation → Measurement.
>
> **Work through the task in these stages:**
>
> 1. Identify the most important vehicle-detection problems that could affect the system.
> 2. Explain what evidence I should collect to determine which problem is actually occurring.
> 3. Recommend the lowest-cost intervention for the most important problem.
> 4. Explain how to measure the intervention against a baseline.
> 5. Recommend what I should do next depending on whether the experiment succeeds or fails.
>
> For every recommendation, use:
>
> **Problem:**
> **Evidence:**
> **Action:**
> **Metric:**
> **Expected effect:**
> **Decision rule:**
>
> Do not assume that a recommendation will improve the model. If an experiment makes performance worse, explain how to respond.
>
> Do not recommend replacing the entire architecture.

## Output

> ## Step 1 — Identify the Main Problems
>
> Potential problems include:
>
> 1. Missed detections of small or distant vehicles.
> 2. Poor motorcycle detection in crowded scenes.
> 3. False positives caused by road objects or partial occlusion.
> 4. Accuracy/FPS trade-offs.
> 5. Detection instability affecting downstream tracking.
>
> ## Step 2 — Collect Evidence
>
> Before changing the model, create a baseline evaluation using the same validation dataset.
>
> Record:
>
> * Precision
> * Recall
> * mAP50
> * mAP50-95
> * Per-class recall
> * FPS
> * Inference latency
>
> Review false positives and false negatives separately.
>
> ## Step 3 — Lowest-Cost Intervention
>
> If the analysis shows that motorcycles or small vehicles are underperforming, first add targeted training examples rather than immediately changing the model architecture.
>
> ### Recommendation
>
> **Problem:** Poor detection of small/distant vehicles.
>
> **Evidence:** A high percentage of false negatives occur on small vehicles.
>
> **Action:** Add representative local traffic examples containing small and partially occluded vehicles.
>
> **Metric:** Compare per-class recall and mAP against the unchanged baseline.
>
> **Expected effect:** Increased recall for difficult vehicle cases.
>
> **Decision rule:** Keep the change only if it improves the target metrics without causing an unacceptable reduction in FPS or performance on other classes.
>
> ## Step 4 — Measure the Intervention
>
> Use the same validation/test data before and after the change.
>
> Change only one major variable at a time. Record the results in an experiment table.
>
> ## Step 5 — Decide What to Do Next
>
> If detection improves while FPS remains acceptable, keep the change and move to the next error category.
>
> If accuracy improves but FPS becomes unacceptable, test image resolution, confidence thresholds, or a smaller model.
>
> If the change does not improve performance, reject it and investigate another error source instead of assuming more training data is always beneficial.
>
> ## Recommended Overall Sequence
>
> **Baseline → Error analysis → Targeted dataset improvement → Controlled evaluation → Parameter/model experiments → Final pipeline evaluation**
>
> This approach reduces unnecessary model changes and makes each improvement measurable.

## What changed and why

I added step decomposition so that the model had to solve the problem as a sequence instead of producing all recommendations at once.

## What I observed in the output

This produced the most useful response in the experiment.

The model turned the vague task of "improve my model" into an experimental workflow:

**baseline → diagnose → intervene → measure → decide**

It also introduced a decision rule, which prevents automatically accepting a change simply because one metric improves.

## Remaining limitation

The response still cannot provide exact numerical targets because the actual baseline metrics, hardware, dataset size, and real-time FPS requirement were not provided.

That is a legitimate limitation rather than something the model should invent.

---

# 8. Cross-Model Comparison

The final Version 5 prompt was run using both **Claude and ChatGPT**.

## Claude

Claude produced a response that was generally more conservative and methodical. It placed strong emphasis on establishing a baseline and analyzing errors before making changes.

Its recommendations were presented in a relatively restrained way and avoided assuming that a larger model would automatically be better.

### Strengths

* Strong experimental reasoning.
* Good emphasis on baseline evaluation.
* Conservative recommendations.
* Clear separation between diagnosis and intervention.

### Weaknesses

* Some recommendations were less implementation-oriented.
* It provided fewer concrete examples of how the workflow could be translated into an actual ML experiment.

---

## ChatGPT

ChatGPT produced a more structured and implementation-oriented response.

It more explicitly connected detection performance with downstream tracking and speed estimation and provided a clearer sequence for implementing the improvements.

### Strengths

* Strong structure.
* Clear implementation sequence.
* Better connection between detection, tracking, and speed estimation.
* More explicit decision rules.

### Weaknesses

* It sometimes provided more explanation than necessary.
* Some recommendations could become repetitive if the prompt is used repeatedly.

---

## Direct Comparison

| Category                 | Claude                      | ChatGPT                             |
| ------------------------ | --------------------------- | ----------------------------------- |
| Accuracy of reasoning    | Strong                      | Strong                              |
| Structure                | Very good                   | Very good                           |
| Technical depth          | Strong ML methodology       | Strong ML + implementation focus    |
| Tone                     | More concise and analytical | More conversational and explanatory |
| Experimental discipline  | Excellent                   | Excellent                           |
| Practical implementation | Good                        | Very good                           |
| Failure handling         | Strong                      | Strong                              |
| Main weakness            | Less implementation detail  | Can be more verbose                 |

### Overall conclusion

For this particular task, I preferred **ChatGPT slightly** because the final response was easier to translate into an actual ML development workflow.

However, Claude performed strongly in experimental discipline and was less likely to overstate a recommendation.

The comparison also showed that prompt engineering does not make different models produce identical answers. The same prompt can lead to differences in emphasis, verbosity, and implementation detail.

---

# 9. Final Reusable Prompt Template

The final prompt should work for someone who has a different ML project, not just SadakDrishti.

```text
You are an experienced machine learning and computer vision engineer.

I need help with this ML task:

[TASK]

## Context

Project/domain:
[PROJECT OR DOMAIN]

Current system:
[CURRENT MODEL / PIPELINE / TOOLS]

Current problem:
[PROBLEM]

Goal:
[WHAT I WANT TO ACHIEVE]

Constraints:
[COMPUTE, TIME, DATA, PERFORMANCE, OR OTHER LIMITATIONS]

## Examples of useful reasoning

Example 1:
Problem → Evidence → Recommendation → Measurement

Example 2:
Problem → Evidence → Recommendation → Measurement

Use these examples as a guide for the level of specificity and reasoning I want. Do not copy their content blindly.

## Work through the task in stages

1. Identify the most important problems.
2. Explain what evidence is needed to determine which problem is actually occurring.
3. Recommend the lowest-cost or lowest-risk intervention for the most important problem.
4. Explain how to test the intervention against a baseline.
5. Explain what I should do if the intervention succeeds.
6. Explain what I should do if the intervention fails.

## Output format

For each recommendation, provide:

### Recommendation [number]

**Problem:**  
[Problem]

**Evidence:**  
[Evidence required]

**Action:**  
[Recommended action]

**Metric:**  
[Metric used to evaluate it]

**Expected effect:**  
[Expected improvement]

**Decision rule:**  
[When to keep or reject the change]

## Quality requirements

- Do not assume a recommendation will improve performance.
- Distinguish between correlation and evidence.
- Prefer measurable experiments over vague advice.
- Consider accuracy, performance, reliability, and resource constraints.
- State assumptions when information is missing.
- Do not invent project-specific facts.
- Prioritize changes that can be tested independently.
```

---

# 10. Reflection

This exercise showed me that prompt engineering is not simply about making a prompt longer. Each technique had a different effect.

**Role assignment** changed the perspective of the answer and encouraged more professional ML reasoning.

**Context and motivation** had a much larger impact because the model could finally understand the actual system and why the task mattered.

**Few-shot examples** helped communicate the expected reasoning style. Instead of merely asking for recommendations, I showed the model what a useful recommendation looked like.

**Output structure** improved consistency and made the response easier to use.

**Step decomposition** produced the strongest improvement because it transformed a vague request into a repeatable workflow: establish a baseline, diagnose the problem, make one intervention, measure it, and decide what to do next.

The most important lesson was that a good prompt should not merely tell an AI **what answer to give**. It should provide enough information for the AI to understand **the task, the situation, the expected quality, and how the result should be evaluated**.

For ML work especially, this is important because a convincing recommendation is not necessarily a correct one. The final prompt therefore requires measurable metrics, baseline comparisons, and decision rules instead of assuming that every proposed change will improve the model.
