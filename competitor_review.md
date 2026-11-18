# Competitor and Existing-Solutions Review

**Review date:** November 18, 2026  
**Project scope:** Detecting signs that **images or videos** were generated or manipulated using AI, with a user-facing checking workflow and a research component focused on combining detector outputs.

## 1. Purpose and evaluation criteria

This review serves two different purposes:

1. **Product competition:** Identify tools that already help ordinary people check online images and videos, particularly while browsing or using social media.
2. **Technical feasibility:** Identify existing detector APIs and complementary open-source tools that the project might legally and technically integrate, evaluate, or use as benchmarks.

We compare image/video coverage, interaction model, availability of an API or reusable software, free access and cost, output interpretability, and relevance to our intended user experience. **Vendor-reported accuracy is not independent evidence of comparative performance.** Availability and pricing may change. This review documents public claims and does not imply that accounts or API keys have been tested.

## 2. Direct and closely related competitors

### 2.1 BitMind — browser-based and mobile AI detection

**Official product:** https://bitmind.ai/extension  
**Other product information:** https://bitmind.ai/product/applications  
**Capabilities documented:** a free Chrome extension that can flag AI-generated images and videos while a person browses. Its website demonstrates checking images by hovering over them and also advertises mobile tools for checking content from social platforms.  
**Fit for target user:** **very high**. This is the closest identified alternative to the proposed vision of checking suspicious social-media content without a separate manual upload.  
**Comparison angle:** since browser-integrated detection already exists, **“we can check social-media content” is not by itself a differentiator**. The project should instead assess whether its own detection fusion, uncertainty handling, false-positive control, and video-frame selection offer a measurable contribution.  
**Important qualification:** the presence of a mobile app does **not** establish that it implements an Android system-wide floating button or analyzes all third-party app screens automatically; that exact capability needs hands-on verification.

### 2.2 AI or Not — manual detection website and API

**Official product:** https://www.aiornot.com/ai-image-detector  
**Pricing:** https://www.aiornot.com/pricing  
**Capabilities documented:** image checking via a web page and an API; the vendor also advertises video, audio, and deepfake checking. The product distinguishes AI-generation from some deepfake-related signals and presents detection breakdowns.  
**Fit for target user:** direct competitor for manual upload-and-check workflows.  
**Comparison angle:** our proposed contribution is the *independent evaluation of a model-fusion algorithm and an explicit inconclusive state*, not just the existence of an upload tool or a score. We must not claim the vendor lacks features that have not been evaluated.

### 2.3 F-Secure Deepfake Detector — free consumer-facing website

**Official product:** https://www.f-secure.com/en/deepfake-detector  
**Launch announcement:** https://company.f-secure.com/en/newsroom/f-secure-launches-free-deepfake-detector-to-help-consumers-verify-suspicious-content  
**Capabilities documented:** free browser-based checks for potentially AI-generated or manipulated images and videos. The product reports support for JPEG, PNG, WEBP, MP4, and MOV, with limits of 10 MB per image and 50 MB per video. The website notes that compression and video screenshots can produce false positives.  
**Fit for target user:** **high**. It directly addresses ordinary consumers concerned about scams or misleading posts.  
**Comparison angle:** it sets a meaningful baseline for usability and accessibility. The project cannot claim novelty based on providing a free image/video upload website alone. A possible academic contribution is independently evaluated detection fusion and explicit uncertainty, not a claim of superior performance without experiments.  
**Integration status:** the free website is a product for end users, **not a confirmed public developer API**.

### 2.4 DeepFake-O-Meter — research platform with multiple detectors

**Official platform:** https://zinc.cse.buffalo.edu/ubmdfl/deep-o-meter/  
**University overview:** https://www.buffalo.edu/community-health-equity-institute/news.host.html/content/shared/university/news/news-center-releases/2024/09/ub-deepfake-o-meter-democratizes-deepfake-detection.detail.html  
**Models overview:** https://zinc.cse.buffalo.edu/ubmdfl/deep-o-meter/models  
**Capabilities documented:** a free, research-oriented web platform from the University at Buffalo for image/video/audio analysis using multiple detection algorithms. Users can select different detectors and compare their assessments. The university describes the platform as open-source; individual detector implementations and weights may be governed by separate licenses and conditions.  
**Fit for target user:** a close competitor to the research/multi-detector aspect of our work, although the interface is research-focused rather than a social-app overlay.  
**Comparison angle:** running multiple detectors in one system is **not novel on its own**. The project must define and test a specific improvement: e.g., adaptive fusion, calibrated abstention, performance under compression, or efficient video sampling.  
**Integration status:** the free public website does **not** imply an unrestricted, documented public API for our application. We should reuse openly licensed underlying models separately where possible, after confirming licensing and availability.

## 3. Commercial APIs — competitors *and* possible project components

An API provider may compete with a product while also supplying detection signals to that product. Access to a remote model is **not equivalent to owning or running the model locally**: the service controls its implementation, billing, quotas, terms and availability. API outputs may not be directly comparable or calibrated across vendors.

### 3.1 Sightengine

**Image detection API:** https://sightengine.com/docs/ai-generated-image-detection  
**Video detection API:** https://sightengine.com/docs/ai-generated-video-detection  
**Deepfake detection API:** https://sightengine.com/docs/deepfake-detection  
**Pricing:** https://www.sightengine.com/pricing  
**Capabilities documented:** APIs for fully generated images, generated videos, and face-based deepfake manipulation; a video API can sample frames.  
**Accessibility (verified from public pricing/FAQ, Nov. 2026):** a limited **Free** plan includes AI-image detection; **AI-video detection is not available on Free**. Starter is advertised at **$29/month** with **10,000 operations/month**, including video detection subject to limits.  
**Potential role in our project:** a convenient third-party **image benchmark** during early development; optionally video comparison under a funded paid plan.  
**Risk:** requests to the service consume quotas and require sending media outside the local environment. Confirm acceptable terms, privacy and the exact charging model before using it for a large experiment.

### 3.2 AI or Not (API capability)

**API pricing:** https://www.aiornot.com/pricing  
**Accessibility (Nov. 2026):** the Free offering advertises **20 AI-image checks**, **$5 in introductory credits**, and an **API key**. Pro starts at **$5/month**. These are limited trial/plan benefits, not an unlimited free inference service. Video pricing/eligibility must be checked for the selected plan.  
**Potential role:** an easy external baseline or additional image detector, particularly for a small number of qualitative comparisons.  
**Risk:** limited initial credit and opaque proprietary model behavior.

### 3.3 Hive / Deploy

**Detector docs:** https://docs.thehive.ai/docs/ai-image-and-video-detection  
**Current pricing page:** https://deploy.com/pricing  
**Capabilities documented:** image/video APIs with separate classes for AI generation and deepfake detection, depending on endpoint.  
**Accessibility (Nov. 2026):** the public price table lists **“Contact Sales”** for **AI Image + Deepfake Classification** and **AI Video Detection**; therefore we should **not assume they are free or included in any general developer credit**.  
**Potential role:** commercial reference implementation or comparison only if access, academic terms, and pricing can be confirmed.  
**Risk:** not suitable as a required dependency until a usable account/endpoint has been tested.

### 3.4 Reality Defender / RealAPI

**API:** https://www.realitydefender.com/product/realapi  
**Capabilities documented:** an API and SDKs for image, video and audio assessment, with structured results and supporting model detail where available.  
**Accessibility (Nov. 2026):** **Free $0/month** plan with **50 image scans per month** and API-key access. **Video/audio are not included in Free**; the listed Business plan is **$399** under the displayed annual-billing setting and includes image/audio/video, with specified quotas.  
**Potential role:** free small-scale image benchmark; perhaps a commercial video baseline only if paid access is secured.  
**Risk:** limited free sample size makes it unsuitable for a full research dataset.

## 4. Complementary verification methods (not detection APIs)

### 4.1 InVID / WeVerify verification plugin

**Official overview:** https://www.invid-project.eu/tools-and-services/invid-verification-plugin/  
**Capabilities documented:** browser tools for extracting video keyframes, reverse image search, metadata, and contextual/forensic investigation. It is a **verification toolkit**, not simply a binary AI image-detection API.  
**Potential role:** help guide manual verification, dataset investigations, and the distinction between digitally generated content and false contextual claims. **Do not presume that its features are available as a callable developer API.**

### 4.2 C2PA / Content Credentials — open-source software for provenance

**Specification:** https://spec.c2pa.org/  
**Official open-source SDK overview:** https://opensource.contentauthenticity.org/docs/introduction/  
**Python library:** https://github.com/contentauth/c2pa-python  
**Capabilities:** verify cryptographically signed assertions about origin/editing history when the media includes credentials.  
**Accessibility:** **free and open-source SDKs** are available, including a Python binding appropriate for our FastAPI backend. A paid cloud service is **not necessary** just to attempt local credential verification.  
**Potential role:** a complementary evidence channel alongside ML detection. The absence of credentials is not evidence of fabrication, and valid signed provenance does not guarantee the factual truth of depicted events.

## 5. Comparison matrix

| Solution | Images | Video | User interface / route | Free/public integration status | Role for our project |
| --- | --- | --- | --- | --- | --- |
| **BitMind** | Yes | Advertised | Browser extension, web, mobile | Free browser extension; API terms separate | Most relevant social-browsing product competitor |
| **AI or Not** | Yes | Advertised | Web upload, API | API key and 20 free image checks advertised | Direct website competitor **and** potential small API baseline |
| **F-Secure Deepfake Detector** | Yes | Yes | Free web upload | Free end-user tool; public API not confirmed | Direct consumer-website competitor |
| **DeepFake-O-Meter** | Yes | Yes | Free research website | Website free; unrestricted public API not confirmed | Multi-detector research competitor / reference |
| **Sightengine** | Yes | Yes | Developer API | Free **image** testing; video paid | Practical image API benchmark |
| **Hive / Deploy** | Yes | Yes | Developer API | Detection price: contact sales | Possible commercial comparison, not a planned free dependency |
| **Reality Defender** | Yes | Yes | Developer API / enterprise | 50 image scans per month free; video paid | Small image API benchmark |
| **InVID / WeVerify** | Images, keyframes | Investigation | Browser verification toolkit | Plugin available; integration API not confirmed | Contextual media verification reference |
| **C2PA / Content Credentials** | Supported media | Supported media | Open-source verifier SDK | Python SDK available free | Local provenance evidence component |

**Interpretation:** a free consumer website or browser extension is not equivalent to an API license. Published prices, free tiers and operational constraints should be rechecked immediately before implementation. "Yes" indicates vendor-documented support, not independently verified detector quality.


## 8. Sources and verification notes

Sources checked on November 18, 2026:

- BitMind extension: https://bitmind.ai/extension
- BitMind applications: https://bitmind.ai/product/applications
- AI or Not pricing: https://www.aiornot.com/pricing
- F-Secure consumer detector: https://www.f-secure.com/en/deepfake-detector
- University at Buffalo DeepFake-O-Meter: https://www.buffalo.edu/community-health-equity-institute/news.host.html/content/shared/university/news/news-center-releases/2024/09/ub-deepfake-o-meter-democratizes-deepfake-detection.detail.html
- Sightengine pricing and FAQ: https://www.sightengine.com/pricing and https://www.sightengine.com/faq/do-you-offer-a-free-trial
- Hive / Deploy AI detection pricing: https://deploy.com/pricing
- Reality Defender RealAPI pricing: https://www.realitydefender.com/product/realapi
- Content Authenticity Initiative SDK: https://opensource.contentauthenticity.org/docs/introduction/

**Verification caveats:** public documentation was reviewed; no personal vendor accounts were opened, prices were not confirmed at checkout, and no comparative accuracy experiments have yet been run. Marketing claims and free-trial offers are subject to change.
