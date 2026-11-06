# Parkinson's Disease Classification from YouTube Speech

## Overview

This repository provides metadata associated with a research dataset for binary classification of Parkinson's disease (PD) and healthy-control (HC) speech from publicly accessible YouTube videos. The study uses five-second speech segments. The metadata identifies segment records, their class labels, and the corresponding source-video URLs.

**No WAV files, extracted audio clips, or other audio recordings are distributed in this repository.** Source-video links and available segment metadata are shared to support transparency, traceability, and reproducibility of the research. Researchers wishing to reconstruct the audio must independently obtain lawful access to the original videos and follow the documented extraction and preprocessing procedure.

## Dataset Overview

The dataset contains **2,470 five-second segment records**, balanced at the segment level:

| Class | Label | Number of segments | Nominal segment duration |
|---|---|---:|---:|
| Parkinson's disease | PD | 1,235 | 1:42:55 |
| Healthy control | HC | 1,235 | 1:42:55 |
| **Total** | — | **2,470** | **3:25:50** |


## Repository File and Metadata Structure

The current combined metadata workbook is:

- `Youtube_URL_Dagitilmis_2470.xlsx`
  - **`Sayfa1`**: the segment-level metadata (2,470 rows).
  - **`Dagitim Kontrolu`**: an audit of requested versus applied repetitions of the source-video URLs, following adjustments made to produce 1,235 records per class. This sheet is a distribution audit, not segment timestamps or independently verified speaker identities.

### Columns in `Sayfa1`

| Column | Meaning | Example |
|---|---|---|
| `speaker_id` | Record identifier in the current workbook; **not a verified unique real-world speaker identifier** | `P01`, `H01` |
| `class` | Binary class assignment | `PD`, `HC` |
| `youtube_url` | Link to the associated source YouTube video | `https://www.youtube.com/watch?v=...` |

Each row corresponds to one segment record. The `speaker_id` field follows the sequence of record labels (`P01`–`P1235` and `H01`–`H1235`) and should **not** be used as a participant grouping key for speaker-independent evaluation without a separate verified speaker mapping.

The current workbook **does not include per-record `start` and `end` timestamps**, exact segment offsets within the original video, or a verified mapping from records to individual speakers. Therefore, the metadata supports identification of source videos and segment records, but **does not by itself permit exact re-extraction of every five-second audio interval**. To enable full reconstruction, a future metadata release should specify a start time and end time (or start time plus duration) for each segment, along with any relevant extraction parameters and speaker-group IDs.

## Label Mapping

For binary classification, the labels can be encoded as:

```text
HC = 0  # Healthy control
PD = 1  # Parkinson's disease
```

These are research labels attached to the dataset records; they should not be interpreted as independently verified clinical diagnoses solely on the basis of video metadata.

## Audio Extraction and Preprocessing

For experimental replication, the intended processing workflow is to:

1. Resolve the `youtube_url` to the original video, subject to its continuing availability and applicable permissions.
2. Identify the target speech interval using the original segmentation log or independently documented start/end timestamps (**not included in the current workbook**).
3. Extract the specified five-second segment, without assuming that consecutively listed records represent consecutive intervals of a video.
4. Convert to mono and resample to the target rate (for example, 16 kHz), if this matches the experimental setup.
5. Apply the documented normalization, cropping, and padding rules, maintaining a five-second model input.
6. Assign the class from the `class` column and retain its provenance link.

Sample rate and preprocessing options listed above describe an illustrative replication workflow; exact values, software versions, and segmentation decisions should be reported with the experiment. The URL repetition audit must not be used to infer exact extraction offsets.

## Evaluation and Data Leakage

The distribution is balanced **by segment**, not necessarily by speaker. Multiple segments from the same video or speaker are statistically dependent. A random row-level train/test split may therefore produce overly optimistic results.

For **speaker-independent** evaluation, researchers must first establish verified speaker identities and group **all segments from the same person** into the same fold. With those group identifiers, `StratifiedGroupKFold` (or an equivalent group-aware protocol) can be used. Grouping by source-video URL alone may reduce same-video leakage but does not guarantee speaker independence when a speaker appears across multiple videos. Do **not** use the row-level `speaker_id` values in this workbook as speaker-group identifiers.

Recommended reporting includes the number of distinct verified speakers in each class, the number of videos, fold-level class and speaker distributions, training/test grouping rules, and whether metrics come from fold averages or pooled out-of-fold predictions.

## Recommended Evaluation Metrics

Accuracy, precision, recall/sensitivity, specificity, F1-score, ROC-AUC, and confusion matrices may be reported, together with the evaluation protocol and uncertainty estimates where appropriate.

## Reproducibility and Availability Statement

**The original WAV audio files and extracted five-second audio segments are not shared.** Instead, the repository provides source YouTube video URLs, segment-record identifiers, class assignments, and a URL-distribution audit to support traceability and reproducibility. Original videos remain hosted and controlled by their respective publishers. Availability and contents may change or videos may be removed.

**Current limitation:** while the records represent five-second segments, the published workbook does not contain the per-segment source-video time offsets or verified speaker identities needed for exact extraction and leakage-free speaker-level replication. These must be supplied in a separate segmentation and speaker-mapping manifest if full reproducibility is intended. Providing the original links does not guarantee that an independently downloaded audio file will reproduce the original analysis exactly.

## Ethical, Copyright, and Legal Considerations

The source videos and their audio remain subject to their respective creators' rights, YouTube's terms of service, and applicable privacy and data-protection laws. The inclusion of a URL does not grant permission to download, reuse, or redistribute audiovisual content. Researchers are responsible for ensuring they have the appropriate permissions or lawful basis for their own processing. No original WAV files are redistributed here.

## Disclaimer

This metadata is provided for scientific research and methodological evaluation only. A classification model trained using these records is not a clinical diagnostic device and must not be used for clinical screening, diagnosis, or treatment decisions without appropriate external validation and professional oversight.
