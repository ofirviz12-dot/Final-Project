# System Requirements Specification: Detection of AI-Generated or AI-Modified Visual Content

**Version:** 1.0 | **Status:** Planning; performance testing not yet conducted | **Milestone:** Submit by class 6  
**Design basis:** Updated Evidence Review, November 2026. All thresholds below are proposed acceptance targets, not measured achievements.

## 1. Purpose, Stakeholders, and Scope

The system helps users assess whether images or videos show signs of AI generation or modification. It **does not** verify whether a depicted event occurred or whether accompanying claims are true. Primary users are social-media users; other stakeholders are the development team and supervisor, who require reproducible experiments.

**Core user journey:** Open the website, select an image, validate the file, run detectors, and view a cautious assessment, score, and suspected manipulation type. The user can then submit another image.

**Phased scope:** The initial minimum viable product (MVP) is a website detecting images **fully generated** by AI. Phase 2 adds video detection and a separate image-manipulation **localization** pipeline, including AI-based inpainting. Checking signed provenance credentials (C2PA) is optional. A browser extension and an Android floating button are deferred. **Out of scope:** Text detection, legal determinations of authenticity, and fact-checking. Detection of every generator or local edit is not guaranteed.

## 2. Prioritized Functional Requirements

Priorities are **Must** for the initial MVP, **Should / Phase 2** for planned additions, **Could** for optional features, and **Later** for future extensions. Tests use documented detector versions and decision rules.

| ID | Requirement | Testable acceptance criterion | Priority |
|---|---|---|---|
| `FR-01` | Image upload | A user can select a local file, submit it, and receive a request ID; a valid file proceeds to analysis. | Must |
| `FR-02` | Input validation | Only `JPG`/`PNG` files of 1–10,000,000 bytes are accepted. Empty, oversized, extension/magic-byte-mismatched, or undecodable files are rejected **before inference**, with an explicit reason. | Must |
| `FR-03` | Detection inference | A valid image reaches the global detector and returns a normalized score plus quality/agreement information required for the decision, or a defined error code. | Must |
| `FR-04` | Cautious result display | Every completed analysis shows exactly one of the three labels below, a visual-suspicion score of `0.00–1.00` to two decimal places, suspected type (`Fully generated` / `Locally modified` / `Undetermined`), processing time in seconds, and a fixed warning. `Locally modified` is available only when localization is enabled. | Must |
| `FR-05` | Failure handling | Detector failure or a 30-second timeout produces a visible error state, with no classification label, invented score, or success message. | Must |
| `FR-06` | Repeat analysis | A new image can be submitted after a result or error; the previous result is not presented as the new request's result. | Should |
| `FR-07` | Video analysis | Only valid `MP4` files up to 30 seconds and 50,000,000 bytes are accepted. Sample up to 16 frames at uniform temporal intervals; report undecodable frames and reject if none is valid. Display a video-level decision. | Phase 2 |
| `FR-08` | Local manipulation localization | A valid image enters a separate pipeline that returns a pixel-level suspicion heatmap or tamper mask at the original image dimensions, plus a regional reliability map/score. If no reliable output exists, do not claim a modified region was found. | Should / Phase 2 |
| `FR-09` | Signed provenance verification | When enabled, check `C2PA 2.4` Content Credentials and report `Valid`, `Invalid`, or `Missing` separately. Missing credentials are not evidence of AI generation and do not automatically alter the visual score. | Could |
| `FR-10` | Browser extension | A user can send an image from a webpage to the detection engine through an extension. | Later |
| `FR-11` | Android floating button | A user can initiate a check from another app only after the appropriate permissions and consent. | Later |

**`FR-04` decision rule:** The three labels are **“Signs of AI generation/manipulation detected,” “Insufficient signs detected,” and “Inconclusive.”** The last applies when media quality is poor, confidence is insufficient, or detectors disagree, according to thresholds calibrated on validation data and stored in a versioned configuration. The initial global-only MVP must not report localized modification detection. The score is a **normalized suspicion measure**, not a proven probability. Fixed warning: **“This result is a statistical assessment, not proof of the content's origin or authenticity.”**

## 3. Acceptance Scenarios and Failure Cases

**Test A — Successful analysis (`FR-01`, `FR-03`, `FR-04`):** **Given** a valid `PNG` and a controlled detector response containing score `0.87`, a positive decision, and processing time `2.40` seconds; **when** the user uploads the image; **then** the exact positive label, `0.87`, type `Fully generated`, `2.40` seconds, and fixed warning appear. The request ID matches the response.

**Test B — Invalid input (`FR-02`):** **Given**, separately, a `PDF`, an empty file, a 10,000,001-byte file, a `PDF` renamed with a `.jpg` extension, and a corrupted `PNG`; **when** each is submitted; **then** it is rejected with the appropriate reason, no inference request is made, and the user can retry.

**Test C — Abstention (`FR-04`):** **Given** a heavily compressed image or a controlled detector response classified by the configuration as insufficient or contradictory; **when** analysis completes; **then** the label **“Inconclusive”**, calculated score (if available), type `Undetermined`, processing time, and warning appear. No claim that the image is authentic is displayed.

**Test D — Service failure/timeout (`FR-05`):** **Given** an unavailable detector or one that fails to respond within 30 seconds; **when** a valid image is submitted; **then** a controlled error appears without a classification, score, or success message. Retrying remains possible. Controlled responses ensure deterministic tests.

## 4. Quality Requirements, Test Environment, and Constraints

**Shared evaluation protocol:** Use a locked test set of at least 1,000 unique `GenImage` images: at least 500 authentic photographs and 500 synthetic images, including at least one generator absent from training and calibration. In each class, evaluate at least 100 JPEG-compressed images and at least 100 resized images; subsets may overlap. Split by source, remove duplicates, and use one fixed model/version and local machine. Record CPU, GPU (if any), RAM, operating system, libraries, image sizes, and detector configuration. Processing is sequential, with no more than one active user.

| ID | Quality requirement | Pass condition and evidence |
|---|---|---|
| `QR-01` | Classification performance | `Macro-F1 >= 0.80` over the complete set of 1,000+ images. For this threshold, `Inconclusive` counts as incorrect against the ground-truth class. Also report abstention rate and results by generator and compression. **Evidence:** confusion matrix and metric report. |
| `QR-02` | False-positive rate | `FPR <= 15%` among at least 500 real photographs in the same set, including JPEG-compressed samples. Report abstentions separately. **Evidence:** false-positive count and exact denominator. |
| `QR-03` | Response time | At least 95% of 100 sequential single-image requests (up to `1920×1080` resolution; one concurrent user) finish within 15 seconds, measured from request receipt to result display on documented local hardware. **Evidence:** per-request times and 95th percentile. |
| `QR-04` | Stability | Zero system crashes and zero false success messages across 20 malformed/exceptional inputs and two simulated service-failure/timeout scenarios. **Evidence:** execution logs, error codes, and UI state. |

Use of models and datasets is subject to licensing and available compute. For localization, additionally measure pixel-level F1 and intersection over union (`IoU`) against ground-truth masks, separately from image-level classification. Targets require independent validation.

## 5. Rationale for Design Choices and Empirical Questions

1. **Locally run open models:** Support reproducibility, data-processing control, and independence from ongoing commercial API access. External services, if used, serve only as comparison points.
2. **Global detection using pretrained representations:** Evaluate `CLIP ViT-L/14` and other baselines because of evidence on generalization to unseen generators [1]. Comparing a single model, simple averaging, and quality-aware fusion is a research objective, not a proven result.
3. **Independent localization pipeline:** `TruFor`, using `Noiseprint++`, is a candidate for masks and reliability estimates [12]. Inpainting tests, including generalization challenges documented by `TGIF2` [13], prevent presenting global classification as proof of local manipulation.
4. **Three-way decision and separate provenance:** Generalization limitations motivate abstention. `C2PA 2.4` [10] supplies complementary provenance evidence, not a substitute for visual detection or proof of factual truth.
5. **Web first:** Manual upload reduces integration complexity. The browser extension and Android button are deferred because of scope and screen-capture permissions [14]. Video starts with uniform frame sampling, then compares alternatives at the same frame budget.

**To be determined experimentally:** Suspicion and abstention thresholds, fusion-score calibration, a suitable localization detector, optimal video frame count, and runtime feasibility on available hardware. Calibration must not access the locked test set.

## 6. Exit Check: Unambiguous Evidence for a Core Requirement

**Requirement:** `FR-02` — Input validation. **Acceptance criterion:** Only decodable `JPG`/`PNG` files of 1–10,000,000 bytes with matching extensions and file signatures reach inference. Every other input is rejected with an explanatory message and no engine call. **Test evidence:** A table covering all five rejection cases in Test B, one valid control file, screenshots of the UI outcomes, and request logs showing **zero detector calls** for rejected inputs. An external reviewer can therefore decide pass or fail without further interpretation.
