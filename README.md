# Parkinson's Disease Classification from YouTube Speech

This repository contains metadata used in a speech-based Parkinson's disease (PD) classification study. Speech recordings collected from YouTube videos were divided into fixed-length segments and labeled as either Parkinson's disease (PD) or healthy control (HC).

## Dataset Overview

The dataset is balanced at the segment level and contains 2,470 five-second speech segments.

| Class | Label | Number of segments | Duration |
|---|---:|---:|---:|
| Parkinson's disease | PD | 1,235 | 1:42:55 |
| Healthy control | HC | 1,235 | 1:42:55 |
| **Total** | — | **2,470** | **3:25:50** |

The repository includes two metadata files:

- `PD_YouTube_Segments.csv`: segments labeled as Parkinson's disease
- `HC_YouTube_Segments.csv`: segments labeled as healthy control

The audio recordings themselves are not included in these CSV files.

## CSV Structure

Each CSV file contains the following columns:

| Column | Description | Example |
|---|---|---|
| `Segment ID` | Unique identifier assigned to a five-second segment | `PD_001` or `HC_001` |
| `Class` | Classification label | `PD` or `HC` |
| `YouTube URL` | URL of the source YouTube video | `youtube.com/...` |
| `Start` | Beginning timestamp of the segment | `00:00` |
| `End` | Ending timestamp of the segment | `00:05` |

Example:

```csv
Segment ID,Class,YouTube URL,Start,End
PD_001,PD,youtube.com/...,00:00,00:05
PD_002,PD,youtube.com/...,00:05,00:10
HC_001,HC,youtube.com/...,00:00,00:05
```

The timestamps progress in consecutive five-second intervals. The first segment covers `00:00–00:05`, while the final segment covers `102:50–102:55` in each class-specific file.

> **Note:** The value `youtube.com/...` is a placeholder and should be replaced with the corresponding source-video URL when the metadata is prepared for public use.

## Label Mapping

The class labels used during binary classification are:

```text
HC = 0  # Healthy control
PD = 1  # Parkinson's disease
```

## Intended Use

The metadata was prepared for research on automatic Parkinson's disease detection from speech. The five-second segments can be used to train and evaluate machine-learning or deep-learning models that distinguish PD speech from healthy speech.

Typical processing steps include:

1. Extracting the specified time interval from the source video.
2. Converting the audio to mono.
3. Resampling all recordings to a common sampling rate, such as 16 kHz.
4. Normalizing or padding each segment to a fixed five-second duration.
5. Assigning the binary class label.
6. Training and evaluating the selected classification model.

## Speaker-Independent Evaluation

Multiple segments may originate from the same speaker. Therefore, segments belonging to a single speaker must not be distributed across both training and test sets. Data splitting should be performed at the speaker or source-video level rather than at the individual-segment level.

For cross-validation, a group-aware approach such as `StratifiedGroupKFold` is recommended. This prevents speaker leakage and provides a more reliable estimate of generalization to previously unseen speakers.

## Evaluation Metrics

Model performance can be reported using:

- Accuracy
- Precision
- Recall (Sensitivity)
- Specificity
- F1-score
- Area under the ROC curve (AUC)
- Confusion matrix

Results should clearly state whether evaluation was speaker-independent and whether metrics were calculated per fold or from pooled predictions.

## Ethical and Legal Considerations

YouTube videos remain subject to their original creators' rights, YouTube's terms of service, and applicable privacy and data-protection requirements. Before sharing or redistributing any audio, researchers should verify that they have the required permissions or a valid legal basis.

This dataset is intended solely for scientific research and methodological evaluation. Predictions produced by models trained with these data must not be treated as a clinical diagnosis. Clinical deployment requires external validation, regulatory review, and assessment by qualified healthcare professionals.

## Reproducibility Notes

For reproducible experiments, report at least the following information:

- Source-video and speaker grouping procedure
- Inclusion and exclusion criteria
- Audio sampling rate and preprocessing steps
- Segment extraction procedure
- Training, validation, and test split strategy
- Random seeds
- Model and hyperparameter configuration
- Class-balancing method, if used
- Evaluation metrics and confidence intervals

## Citation

If this metadata or the associated classification workflow is used in academic work, please cite the related publication. Replace the placeholder below with the final publication information:

```bibtex
@article{onal_parkinsons_speech_classification,
  author  = {Muharrem Onal and others},
  title   = {},
  journal = {To be added},
  year    = {To be added}
}
```

## Disclaimer

This repository is provided for research purposes only. It is not a medical device and must not be used for diagnosis, treatment decisions, or clinical screening without appropriate validation and professional oversight.
