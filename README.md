# Parkinson's Disease Classification from YouTube Speech

## Overview

This repository provides metadata associated with a research dataset for binary classification of Parkinson's disease (PD) and healthy-control (HC) speech from publicly accessible YouTube videos. The study uses five-second speech segments. The metadata identifies segment records, their class labels, and the corresponding source-video URLs.

**No WAV files, extracted audio clips, or other audio recordings are distributed in this repository.** Source-video links and available segment metadata are shared to support transparency, traceability, source identification, and partial reproducibility of the research. Researchers wishing to reconstruct the audio must independently obtain lawful access to the original videos. 

## Dataset Overview

The dataset contains **2,470 five-second segment records**, balanced at the segment level:

|Class|Label|Number of segments|Total duration of five-second segments|
|-|-|-:|-:|
|Parkinson's disease|PD|1,235|1:42:55|
|Healthy control|HC|1,235|1:42:55|
|**Total**|—|**2,470**|**3:25:50**|

The durations above are calculated as the number of segment records multiplied by five seconds; they do not represent the duration of any single source video.

## Repository File and Metadata Structure

The current combined metadata workbook is:

* [`Youtube_URL.xlsx`](./Youtube_URL.xlsx)

  * **`Sayfa1`**: segment-level metadata (2,470 rows, as described in this repository).

### Columns in `Sayfa1`

|Column|Meaning|Example|
|-|-|-|
|`speaker_id`|Record identifier in the current workbook; **not a verified unique real-world speaker identifier**|`P01`, `H01`|
|`class`|Binary class assignment|`PD`, `HC`|
|`youtube_url`|Link to the associated source YouTube video|`https://www.youtube.com/watch?v=...`|

Each row corresponds to one segment record. The `speaker\_id` field follows the sequence of record labels (`P01`–`P1235` and `H01`–`H1235`) and should **not** be used as a participant grouping key for speaker-independent evaluation without a separate verified speaker mapping.

The current workbook **does not include per-record `start` and `end` timestamps**, exact segment offsets within the original video, or a verified mapping from records to individual speakers. Accordingly, it supports source identification and segment-record traceability.

## Label Mapping

For binary classification, the labels can be encoded as:

```text
HC = 0  # Healthy control
PD = 1  # Parkinson's disease
```

These are research labels based on source information. They should not be interpreted as independently verified clinical diagnoses solely on the basis of video metadata. The PD and HC labels assigned in this study were not independently clinically verified.

## Audio Extraction and Preprocessing

An independent reconstruction effort would require the following steps:

1. Resolve the `youtube_url` to the original video, subject to continuing availability and applicable permissions.
2. Identify the target speech interval from an independently available segmentation log or documented start/end timestamps (**not included in the current workbook**).
3. Extract the specified five-second speech segment. Do **not** assume consecutively listed records are consecutive intervals of a source video.
4. Convert the audio to mono and resample it to the target rate (e.g., 16 kHz), **only where consistent with the study's documented experimental setup**.
5. Apply the normalization, cropping, and padding procedures documented for the experiment to obtain a five-second model input.
6. Retain the original source URL and class assignment in any derived metadata.

The steps above describe the processing requirements, **not a complete independently executable reproduction protocol**. Exact extraction offsets are not provided in this repository. 

## Evaluation and Data Leakage

The dataset is balanced **by segment**, not necessarily by speaker. Multiple segments from the same video or speaker may be statistically dependent. A random row-level train/test split may therefore lead to overly optimistic estimates of generalization.

For **speaker-independent** evaluation, researchers must first establish verified real-world speaker identities and place **all segments from each speaker** in the same fold. With such identifiers, `StratifiedGroupKFold` or an equivalent group-aware validation method can be used. Grouping only by source-video URL may reduce same-video leakage but does not guarantee speaker independence when a speaker appears in multiple videos.

**Do not use the row-level `speaker\_id` values in this workbook as speaker-group identifiers.** The repository does not provide the verified mapping necessary to reconstruct a speaker-independent split from this workbook alone.

Recommended reporting includes the number of verified distinct speakers in each class, the number of source videos, fold-level class and speaker distributions, the grouping rules used for train/test separation, and whether metrics are fold averages or pooled out-of-fold estimates.

## Recommended Evaluation Metrics

Accuracy, precision, recall/sensitivity, specificity, F1-score, ROC-AUC, and confusion matrices may be reported together with the evaluation protocol and uncertainty estimates where appropriate.

## Reproducibility and Availability Statement

**The original YouTube videos, WAV audio files, and extracted five-second audio segments are not redistributed in this repository.** The shared [`Youtube_URL.xlsx`](./Youtube_URL.xlsx) workbook provides source-video URLs, segment-record identifiers, and class assignments. These metadata support data provenance, source identification, transparency, and **partial reproducibility**, rather than exact reconstruction of the original analysis.

The PD and HC labels were assigned based on available source information and were **not independently clinically verified** in this study.

## Ethical, Copyright, and Legal Considerations

The videos and their audio remain subject to their creators' rights, YouTube's terms of service, and applicable privacy and data-protection laws. The inclusion of a source URL does not itself grant permission to download, reuse, or redistribute audiovisual content. Researchers are responsible for establishing appropriate permissions or another lawful basis for their own processing. **No original WAV files or extracted audio segments are redistributed here.**

## Disclaimer

This metadata is provided for scientific research and methodological evaluation only. A classification model trained using these records is not a clinical diagnostic device and must not be used for clinical screening, diagnosis, or treatment decisions without appropriate external validation and professional oversight.

