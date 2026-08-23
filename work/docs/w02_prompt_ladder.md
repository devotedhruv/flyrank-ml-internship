# Prompt Ladder: From a Weak Prompt to an Engineered ML Prompt

**Track:** Machine Learning
**Phase:** Foundations
**Assignment:** The Prompt Ladder
**Project Context:** AI Traffic Monitoring System (SadakDrishti)
**Total Runs:** 6 — Baseline + 5 improved versions

---

## 1. Baseline — Weak Prompt

### Prompt

> Help me improve my AI traffic monitoring model.

### Layer Added

**None — Baseline**

This is intentionally weak. It does not explain what the model does, what "improve" means, what data is being used, or what kind of answer is expected.

### Representative Output

> To improve your AI traffic monitoring model, you can try several approaches:
>
> 1. Use a better object detection model such as YOLOv8 or YOLO11.
> 2. Collect more training data.
> 3. Improve the quality of your dataset.
> 4. Tune the model parameters.
> 5. Use tracking algorithms such as ByteTrack or DeepSORT.
> 6. Improve image quality and preprocessing.
> 7. Evaluate the model using metrics such as precision, recall, and mAP.
>
> You should also consider optimizing the model for real-time performance.

### Notes

**What changed in the prompt:**
Nothing. This is the deliberately weak baseline prompt.

**What actually improved in the output:**
Nothing can be improved yet because this is the starting point. The response gives generally correct ML advice but is very broad.

**What still failed:**
The answer treats the project like a generic object-detection problem. It does not distinguish between detection, tracking, speed estimation, number-plate recognition, or traffic violations. It also gives no clear order of action.

**What I would try next:**
Define the actual goal of the task so the model knows what "improve" means.

---

# 2. Version 1 — Clearer Goal

### Single Layer Added

**Layer: Clearer goal**

### Prompt

> Help me improve my AI traffic monitoring model by increasing vehicle detection accuracy while keeping it fast enough for real-time traffic monitoring.

### Representative Output

> To improve the system, focus on the trade-off between detection accuracy and inference speed.
>
> 1. Evaluate your current model using precision, recall, mAP50, and inference FPS.
> 2. Identify which vehicle classes are being detected poorly.
> 3. Add more training examples for difficult cases such as small, distant, partially occluded, or crowded vehicles.
> 4. Compare your current model with a larger YOLO model.
> 5. Optimize the final model using image-size and confidence-threshold tuning.
> 6. Measure whether accuracy improvements reduce real-time performance.
>
> Your main objective should be to find the best accuracy/speed balance rather than simply choosing the largest model.

### Notes

**What changed in the prompt:**
I explicitly stated the goal: improve vehicle detection accuracy while maintaining real-time performance.

**What actually improved in the output:**
The response became more focused. Instead of suggesting random improvements to the entire system, it concentrated on the accuracy-versus-speed trade-off.

**What still failed:**
The advice is still generic. It does not know what model, dataset, tracking method, or hardware I am actually using.

**What I would try next:**
Add real project context so the recommendations are based on my existing system rather than a generic traffic-monitoring system.

---

# 3. Version 2 — Real Context

### Single Layer Added

**Layer: Real context**

### Prompt

> Help me improve my AI traffic monitoring model by increasing vehicle detection accuracy while keeping it fast enough for real-time traffic monitoring.
>
> My project is a Nepal-focused traffic monitoring system called SadakDrishti. It currently uses Ultralytics YOLO for vehicle detection, ByteTrack/BoT-SORT for tracking, and OpenCV-based trajectory and homography methods for speed estimation. The system is intended to process traffic-camera video in real time. I am primarily detecting vehicles such as cars, motorcycles, buses, and trucks.

### Representative Output

> Given your current architecture, I would improve the system in this order:
>
> **1. Establish a baseline**
>
> Record your current precision, recall, mAP50, mAP50-95, FPS, and latency for each vehicle class.
>
> **2. Improve the detection dataset**
>
> Since the system is intended for Nepalese roads, prioritize local traffic scenes. Include motorcycles, buses, trucks, cars, and difficult conditions such as crowded roads, shadows, nighttime footage, rain, and partial occlusion.
>
> **3. Analyze class-specific errors**
>
> Check whether motorcycles or distant vehicles have substantially lower recall than cars. This will tell you whether additional data or better image resolution is required.
>
> **4. Tune the detector**
>
> Experiment with image resolution, confidence threshold, and model size. Larger models may improve accuracy but can reduce FPS.
>
> **5. Validate the tracking stage**
>
> Detection improvements should not be evaluated independently from tracking because missed or unstable detections can cause ID switches and incorrect speed estimates.
>
> **6. Measure the complete pipeline**
>
> The final evaluation should consider detection accuracy, tracking stability, speed-estimation error, and real-time performance.

### Notes

**What changed in the prompt:**
I added the actual project architecture and operating context.

**What actually improved in the output:**
The answer became much more specific. It recognized that detection affects tracking and speed estimation, and it suggested Nepal-specific traffic conditions for the dataset.

**What still failed:**
The response gives good recommendations but is still somewhat difficult to use as an implementation plan. It mixes metrics, dataset work, model tuning, and tracking without presenting them in a consistent structure.

**What I would try next:**
Specify exactly how I want the answer organized.

---

# 4. Version 3 — Specified Output Format

### Single Layer Added

**Layer: Specified output format**

### Prompt

> Help me improve my AI traffic monitoring model by increasing vehicle detection accuracy while keeping it fast enough for real-time traffic monitoring.
>
> My project is a Nepal-focused traffic monitoring system called SadakDrishti. It currently uses Ultralytics YOLO for vehicle detection, ByteTrack/BoT-SORT for tracking, and OpenCV-based trajectory and homography methods for speed estimation. The system is intended to process traffic-camera video in real time. I am primarily detecting vehicles such as cars, motorcycles, buses, and trucks.
>
> Give me the answer in a table with these columns: **Priority, Problem, Recommended Action, Metric to Measure, and Expected Effect**. Then give me a short 5-step implementation order.

### Representative Output

| Priority | Problem                                  | Recommended Action                                     | Metric to Measure          | Expected Effect               |
| -------- | ---------------------------------------- | ------------------------------------------------------ | -------------------------- | ----------------------------- |
| 1        | Poor detection of small/distant vehicles | Add more small-object examples to the training dataset | Per-class recall, mAP      | Better detection              |
| 2        | Class imbalance                          | Increase samples for underrepresented vehicle classes  | Class distribution, recall | More balanced predictions     |
| 3        | Detection/tracking instability           | Tune confidence and IoU thresholds                     | ID switches, IDF1          | More stable tracking          |
| 4        | Accuracy/FPS trade-off                   | Compare different YOLO model sizes                     | mAP, FPS, latency          | Better deployment choice      |
| 5        | Difficult traffic conditions             | Add night, rain, shadows, and crowded scenes           | Per-condition recall       | Better robustness             |
| 6        | Speed estimation errors                  | Validate homography and calibration                    | MAE/RMSE of speed          | More reliable speed estimates |

### 5-Step Implementation Order

1. Establish baseline metrics.
2. Analyze class-specific and condition-specific errors.
3. Improve the training dataset.
4. Tune and compare YOLO configurations.
5. Evaluate the complete detection → tracking → speed pipeline.

### Notes

**What changed in the prompt:**
I required a specific output format: a table followed by five implementation steps.

**What actually improved in the output:**
The information became much easier to scan and act on. Each recommendation is now connected to a measurable metric instead of being presented as a loose list.

**What still failed:**
The table is useful, but some recommendations are still broad. For example, "add more data" does not tell me how much data to add or how to decide whether the change actually worked.

**What I would try next:**
Add constraints that force the recommendations to remain realistic for a student project and real-time deployment.

---

# 5. Version 4 — Constraints

### Single Layer Added

**Layer: Constraints**

### Prompt

> Help me improve my AI traffic monitoring model by increasing vehicle detection accuracy while keeping it fast enough for real-time traffic monitoring.
>
> My project is a Nepal-focused traffic monitoring system called SadakDrishti. It currently uses Ultralytics YOLO for vehicle detection, ByteTrack/BoT-SORT for tracking, and OpenCV-based trajectory and homography methods for speed estimation. The system is intended to process traffic-camera video in real time. I am primarily detecting vehicles such as cars, motorcycles, buses, and trucks.
>
> Give me the answer in a table with these columns: **Priority, Problem, Recommended Action, Metric to Measure, and Expected Effect**. Then give me a short 5-step implementation order.
>
> **Constraints:** Assume this is a student project with limited GPU resources. Do not recommend completely replacing the existing architecture. Prioritize changes that can be tested using the current YOLO + tracking pipeline. Prefer low-cost dataset and training improvements before recommending a larger model. Every recommendation should have a measurable metric.

### Representative Output

| Priority | Problem                | Recommended Action                                                        | Metric to Measure        | Expected Effect             |
| -------- | ---------------------- | ------------------------------------------------------------------------- | ------------------------ | --------------------------- |
| 1        | Dataset weaknesses     | Review false positives/negatives and add targeted local traffic examples  | Precision, recall, mAP   | Higher accuracy             |
| 2        | Class imbalance        | Add samples for poorly performing vehicle classes                         | Per-class recall         | Better class performance    |
| 3        | Small/distant vehicles | Add representative small-object examples and test higher input resolution | Small-object recall, FPS | Better distant detection    |
| 4        | Threshold selection    | Tune confidence and IoU thresholds on a validation set                    | Precision/recall, FPS    | Better prediction balance   |
| 5        | Model capacity         | Compare current model with one larger YOLO variant                        | mAP, FPS, latency        | Evidence-based model choice |

### 5-Step Implementation Order

1. Establish the current baseline.
2. Analyze false positives and false negatives.
3. Improve the dataset based on those errors.
4. Tune confidence, IoU, and image-size settings.
5. Only then compare against a larger YOLO model.

### Notes

**What changed in the prompt:**
I added practical constraints: limited GPU resources, keep the existing architecture, prioritize low-cost improvements, and require measurable metrics.

**What actually improved in the output:**
The recommendations became much more realistic for my situation. Instead of immediately recommending a larger model, the response prioritized dataset analysis and parameter tuning.

**What still failed:**
This version is more practical, but the answer still assumes that every recommendation will produce an improvement. It does not explicitly tell me how to reject a change if the experiment produces worse results.

**What I would try next:**
Add a verification requirement so the model has to explain how each proposed improvement should be tested and when it should be considered unsuccessful.

---

# 6. Version 5 — Verification Requirements

### Single Layer Added

**Layer: Verification requirements**

### Prompt

> Help me improve my AI traffic monitoring model by increasing vehicle detection accuracy while keeping it fast enough for real-time traffic monitoring.
>
> My project is a Nepal-focused traffic monitoring system called SadakDrishti. It currently uses Ultralytics YOLO for vehicle detection, ByteTrack/BoT-SORT for tracking, and OpenCV-based trajectory and homography methods for speed estimation. The system is intended to process traffic-camera video in real time. I am primarily detecting vehicles such as cars, motorcycles, buses, and trucks.
>
> Give me the answer in a table with these columns: **Priority, Problem, Recommended Action, Metric to Measure, and Expected Effect**. Then give me a short 5-step implementation order.
>
> **Constraints:** Assume this is a student project with limited GPU resources. Do not recommend completely replacing the existing architecture. Prioritize changes that can be tested using the current YOLO + tracking pipeline. Prefer low-cost dataset and training improvements before recommending a larger model. Every recommendation should have a measurable metric.
>
> **Verification requirement:** For every recommendation, explain how I should run a fair before/after comparison using the same validation/test data. State what result would count as an improvement and what result would mean I should reject or reverse the change. Do not assume that a proposed change will improve performance.

### Representative Output

| Priority | Problem                | Recommended Action                                                 | Metric to Measure        | Expected Effect                                |
| -------- | ---------------------- | ------------------------------------------------------------------ | ------------------------ | ---------------------------------------------- |
| 1        | Dataset weaknesses     | Add targeted examples based on false positives and false negatives | mAP50, recall, precision | Higher detection quality                       |
| 2        | Class imbalance        | Add data for underperforming vehicle classes                       | Per-class recall         | More balanced performance                      |
| 3        | Small/distant vehicles | Add representative small-object samples and test input resolution  | Small-object recall, FPS | Better distant detection                       |
| 4        | Threshold selection    | Tune confidence and IoU thresholds                                 | Precision, recall, FPS   | Better operating point                         |
| 5        | Model capacity         | Compare current YOLO model against one larger variant              | mAP, FPS, latency        | Determine whether extra capacity is worthwhile |

### Verification Method

For every experiment:

1. Freeze the validation/test dataset.
2. Record the current baseline metrics.
3. Change only one variable.
4. Run the new model on exactly the same evaluation data.
5. Compare precision, recall, mAP, FPS, and latency.
6. Keep the change only if the improvement is meaningful and does not violate the real-time requirement.

For example, if a new model increases mAP but reduces FPS below the project's real-time target, it should **not automatically be considered an improvement**.

If a dataset change improves overall mAP but significantly reduces recall for motorcycles, investigate the class-specific result before accepting it.

### 5-Step Implementation Order

1. Benchmark the existing model.
2. Identify the largest error categories.
3. Make one targeted dataset improvement.
4. Measure the change against the frozen baseline.
5. Repeat the experiment cycle before changing model architecture.

### Notes

**What changed in the prompt:**
I added a verification requirement requiring controlled before/after experiments and explicit criteria for accepting or rejecting changes.

**What actually improved in the output:**
This was the biggest improvement in the ladder. The response stopped treating recommendations as automatically good ideas. It introduced experimental discipline: fixed evaluation data, one change at a time, measurable metrics, and explicit rejection criteria.

**What still failed:**
The answer still does not provide exact numerical targets because the current baseline performance and hardware specifications are unknown. It correctly avoids inventing those numbers.

**What I would try next:**
For a real project, I would supply actual baseline metrics, hardware, dataset size, and target FPS. However, that would be additional context rather than part of this ladder, so I would not add it here.

---

# Comparison Across the Six Runs

| Version   | Layer Added               | Main Output Improvement                                     |
| --------- | ------------------------- | ----------------------------------------------------------- |
| Baseline  | None                      | Very generic ML advice                                      |
| Version 1 | Clearer goal              | Focused on accuracy vs. real-time speed                     |
| Version 2 | Real context              | Recommendations matched SadakDrishti's architecture         |
| Version 3 | Output format             | Information became easier to scan and implement             |
| Version 4 | Constraints               | Recommendations became more realistic for a student project |
| Version 5 | Verification requirements | Recommendations became testable rather than assumptions     |

---

# Honest Finding

The most interesting result was that **not every prompt improvement produced a dramatic improvement**.

Adding the output format made the answer cleaner, but it did not necessarily make the technical recommendations better. The table improved usability more than reasoning quality.

The verification layer produced the strongest improvement because it changed the response from:

> "Here are things you should try."

to:

> "Here is what you should test, how to test it fairly, and when you should reject it."

This showed that prompt engineering is not simply about adding more instructions. The useful question is whether the added instruction changes the **quality of the resulting output**.

---

# Final Reusable Prompt

The following is the cleaned-up prompt that someone else working on an ML traffic-monitoring project could use without needing my personal explanation.

> **Task:** Help me improve a real-time vehicle detection system.
>
> **Project context:** The system is a traffic-monitoring application using an Ultralytics YOLO model for vehicle detection, ByteTrack or BoT-SORT for object tracking, and OpenCV-based trajectory/homography methods for speed estimation. The target classes include cars, motorcycles, buses, and trucks. The system is intended for real-time traffic-camera video.
>
> **Goal:** Improve vehicle detection accuracy while maintaining sufficient real-time performance.
>
> **Constraints:**
>
> * Assume limited GPU/computing resources.
> * Do not completely replace the existing architecture.
> * Prioritize dataset and training improvements before recommending a larger model.
> * Every recommendation must have a measurable evaluation metric.
>
> **Output format:**
> Provide a table with:
>
> 1. Priority
> 2. Problem
> 3. Recommended Action
> 4. Metric to Measure
> 5. Expected Effect
>
> Then provide a 5-step implementation order.
>
> **Verification requirement:**
> For every recommendation, explain how to perform a fair before/after comparison using the same validation/test dataset. Identify the metrics that should improve and explain when the change should be rejected. Do not assume that a proposed change will improve performance.
>
> **Important:** Distinguish between overall performance and class-specific performance. Also consider the trade-off between detection accuracy, tracking stability, speed-estimation reliability, FPS, and latency.

---

# Reflection

This exercise changed how I think about prompting. My original prompt asked the AI to "improve my model," which left almost everything unspecified. The later versions progressively removed ambiguity.

The biggest lesson was that **more words do not automatically mean a better prompt**. Each useful layer had a specific job. The goal focused the answer, context made it relevant, formatting made it usable, constraints made it realistic, and verification made it scientifically testable.

For ML work, the verification layer was especially valuable. A model recommendation is not useful merely because it sounds technically impressive. It needs to be tested against a baseline using consistent data and measurable metrics.

The final prompt is therefore not just a request for advice. It defines the problem, the environment, the desired output, the practical limits, and the standard by which the answer should be judged.
