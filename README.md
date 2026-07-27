# FineViP

**FineViP** is a **Fine**-grained, multi-label **Vi**sual **P**rivacy dataset for studying privacy-sensitive visual content. It provides category-risk annotations that capture not only whether an image contains privacy-related information, but also what kind of privacy concern is visible and how sensitive the visual evidence is.

The dataset contains **24,783 annotated images** and **23 labels**, including **1 Public label** and **22 fine-grained privacy labels** formed by 11 privacy categories with High- and Low-risk levels.

You can also download the dataset resources from [Google Drive](https://drive.google.com/drive/folders/19aAviYJdiVtoKSAjssYNr-qu8TmIFgAJ?usp=drive_link).

## Dataset Files

The recommended files for using FineViP are:

| File | Description |
|---|---|
| `annotations.csv` | Final multi-label annotations for all 24,783 images. This is the main label file for training, validation, testing, and benchmark comparison. |
| `urls.csv` | Publicly releasable image URLs for the currently accessible subset of 17,670 images. |
| `label_definition.csv` | Definitions, examples, and special cases for all privacy labels. |
| `label_counts.csv` | Number of positive samples for each label in `annotations.csv`. |
| `dataset_summary.csv` | Dataset-level summary statistics. |
| `split_summary.csv` | Number of samples in each official split. |
| `splits/train.csv` | Training split with image IDs and labels. |
| `splits/val.csv` | Validation split with image IDs and labels. |
| `splits/test.csv` | Test split with image IDs and labels. |
| `splits/all_splits.csv` | Mapping from each image ID to its split. |

The older files `datasets_label.csv` and `dataset_url.csv` are kept for compatibility with the initial GitHub release. New users should use `annotations.csv` and `urls.csv`.

## Dataset Scale

| Item | Count |
|---|---:|
| Final annotated images | 24,783 |
| Released image URLs | 17,670 |
| Total labels | 23 |
| Train images | 19,824 |
| Validation images | 2,469 |
| Test images | 2,490 |

For ethical and copyright reasons, this repository does not directly host image files. Instead, it provides image IDs, labels, and URLs when public links are available. Some images have final annotations but no released URL because the original links expired or are not suitable for public redistribution.

## Annotation Format

`annotations.csv` and the split files use the following format:

```csv
image_id,Public,Personal Information-H,...,Minors-L
```

Each label column is binary:

- `1`: the privacy label is present.
- `0`: the privacy label is absent.

FineViP is a multi-label dataset. A single image may contain multiple privacy labels simultaneously, such as `Minors-H`, `Sensitive Location-H`, and `Sensitive Relationships-L`.

## Label Space

FineViP uses a two-dimensional label space consisting of privacy categories and risk levels. The category dimension identifies the type of privacy concern, while the risk dimension captures the severity and identifiability of the exposed information.

The final label space includes 11 privacy categories, each with High- and Low-risk levels, plus a Public label:

| Category | High-risk label | Low-risk label |
|---|---|---|
| Personal Information | Personal Information-H | Personal Information-L |
| Financial Information | Financial Information-H | Financial Information-L |
| Health Condition | Health Condition-H | Health Condition-L |
| Sensitive Behaviors | Sensitive Behaviors-H | Sensitive Behaviors-L |
| Personal Space | Personal Space-H | Personal Space-L |
| Sensitive Occupations | Sensitive Occupations-H | Sensitive Occupations-L |
| Sensitive Location | Sensitive Location-H | Sensitive Location-L |
| Sensitive Relationships | Sensitive Relationships-H | Sensitive Relationships-L |
| Sensitive Interests | Sensitive Interests-H | Sensitive Interests-L |
| Religious Beliefs | Religious Beliefs-H | Religious Beliefs-L |
| Minors | Minors-H | Minors-L |

The `Public` label is used only when no identifiable privacy-related information is visible.

Detailed label definitions, visual examples, and boundary cases are provided in `label_definition.csv`. Examples are marked using cues such as `[Object]`, `[Scene]`, and `[Individual]` inside the examples field.

## Annotation Guidelines

FineViP follows flexible, example-based annotation guidelines because visual privacy depends heavily on context. The guidelines combine category definitions, representative visual cues, and boundary cases.

General annotation principles:

1. High-risk labels are assigned when the visual evidence is clear, privacy-sensitive, and can be associated with an identifiable individual or a strongly identifiable context, such as a clear face, readable document, distinctive location, explicit relationship, or sensitive scene.
2. Low-risk labels are assigned when the visual evidence is relevant but weaker, less sensitive, less identifiable, or contextually uncertain.
3. If both High- and Low-risk labels from the same category apply, the High-risk label should be retained.
4. If `Public` conflicts with any concrete privacy category, `Public` should be removed.
5. Multiple labels may be assigned to the same image when different privacy concerns are visible.

## Image Sources

Images are collected from existing privacy datasets and public web sources, including Flickr, Instagram, and Bing Search Engine, to support diverse visual privacy scenarios.

The dataset builds on and extends prior privacy image resources, including:

- Privacy-aware image classification and search [1]
- Visual Privacy Advisor [2]
- PrivacyAlert [3]

## Usage Example

```python
import pandas as pd

annotations = pd.read_csv('annotations.csv')
urls = pd.read_csv('urls.csv')

# Join labels with released URLs when available.
data = annotations.merge(urls, on='image_id', how='left')

label_cols = [c for c in annotations.columns if c != 'image_id']
print(len(annotations), len(label_cols))
```

To use the official split:

```python
train = pd.read_csv('splits/train.csv')
val = pd.read_csv('splits/val.csv')
test = pd.read_csv('splits/test.csv')
```

## Ethical Use

FineViP is intended for research on visual privacy understanding, privacy risk measurement, privacy-preserving machine learning, and image privacy protection. Please use the dataset responsibly.

FineViP should not be used for identifying, profiling, tracking, surveilling, or re-identifying individuals. Researchers are responsible for complying with applicable laws, ethical guidelines, and the terms of the original image sources when accessing images through provided URLs.

## License

FineViP annotations, taxonomy, and related resources are released under the 
[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-nc-sa/4.0/).

You are free to share and adapt these resources for non-commercial purposes, provided that appropriate attribution is given and derivative works are distributed under the same license.

The original images are not redistributed by FineViP and remain subject to the licenses and terms of their respective sources.

## Citation

If you use FineViP in your research, please cite the corresponding paper. A BibTeX entry will be added after publication.

## References

[1] Sergej Zerr, Stefan Siersdorfer, Jonathon Hare, and Elena Demidova. Privacy-aware image classification and search. In Proceedings of the 35th International ACM SIGIR Conference on Research and Development in Information Retrieval, pages 35-44, 2012.

[2] Tribhuvanesh Orekondy, Bernt Schiele, and Mario Fritz. Towards a visual privacy advisor: Understanding and predicting privacy risks in images. In Proceedings of the IEEE International Conference on Computer Vision, pages 3686-3695, 2017.

[3] Chenye Zhao, Jasmine Mangat, Sujay Koujalgi, Anna Squicciarini, and Cornelia Caragea. PrivacyAlert: A dataset for image privacy prediction. In Proceedings of the International AAAI Conference on Web and Social Media, volume 16, pages 1352-1361, 2022.
