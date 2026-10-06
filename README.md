# WheatHead-26M

### Cross-Domain Wheat Head Detection and Counting on GWHD 2021

![Python](https://img.shields.io/badge/Python-3.x-blue)
![YOLO](https://img.shields.io/badge/Object%20Detection-YOLOv8%20%7C%20YOLO26-6f42c1)
![Computer Vision](https://img.shields.io/badge/Computer%20Vision-Wheat%20Head%20Detection-2ea44f)
![Precision Agriculture](https://img.shields.io/badge/Application-Precision%20Agriculture-green)
![Status](https://img.shields.io/badge/Status-Accepted%20at%20IEEE%20WIECON--ECE%202026-success)

**WheatHead-26M** is a cross-domain wheat-head detection and counting study built on the **Global Wheat Head Detection (GWHD) 2021** benchmark. The work compares YOLOv8 and YOLO26 detectors under held-out domain evaluation, then studies image-level counting, computational efficiency, and domain-wise error behavior.

The strongest model, **YOLO26m**, is referred to as **WheatHead-26M** in the study.

> **Status:** Accepted for presentation at the **12th IEEE International Women in Engineering (WIE) Conference on Electrical and Computer Engineering 2026 (IEEE WIECON-ECE 2026)**.

---

## Highlights

- Cross-domain evaluation on **GWHD 2021**
- Five evaluated detectors:
  - YOLOv8n
  - YOLOv8s
  - YOLOv8m
  - YOLO26s
  - YOLO26m
- Independent held-out test domains preserved during evaluation
- Validation-guided confidence and IoU threshold selection for counting
- Accuracy-efficiency comparison using detection metrics, parameter count, and GFLOPs
- Image-level wheat-head counting
- Domain-wise error analysis across unseen test domains
- Final selected detector: **WheatHead-26M (YOLO26m)**

---

## Main Results

### Detection

| Model | Precision | Recall | F1 | mAP@0.50 | mAP@0.50:0.95 |
|---|---:|---:|---:|---:|---:|
| YOLOv8n | 0.798 | 0.599 | 0.684 | 0.670 | 0.302 |
| YOLOv8s | 0.779 | 0.621 | 0.691 | 0.677 | 0.285 |
| YOLOv8m | 0.835 | 0.670 | 0.743 | 0.734 | 0.333 |
| YOLO26s | 0.824 | 0.670 | 0.739 | 0.736 | 0.322 |
| **WheatHead-26M** | **0.851** | **0.691** | **0.763** | **0.762** | **0.363** |

Compared with YOLOv8m, WheatHead-26M used **15.8% fewer parameters** and **13.9% fewer GFLOPs**, while improving the reported detection metrics.

### Counting

| Model | MAE ↓ | RMSE ↓ | R² ↑ | MAPE ↓ |
|---|---:|---:|---:|---:|
| YOLOv8m | 8.918 | 11.759 | 0.826 | 24.84% |
| **WheatHead-26M** | **8.085** | **10.742** | **0.855** | **22.92%** |

The final model improved both detection performance and image-level counting accuracy, although dense and visually cluttered scenes remained challenging.

---

## Method Overview

1. Use the official GWHD 2021 train, validation, and test partitions.
2. Audit annotations and convert bounding boxes to normalized YOLO format.
3. Resize detector inputs to **640 × 640**.
4. Fine-tune YOLOv8 and YOLO26 variants.
5. Evaluate all detectors on held-out test domains.
6. Select the strongest medium model from each YOLO generation.
7. Choose counting thresholds using validation data only.
8. Compare image-level counting performance.
9. Analyze counting error across individual test domains.

---

## Dataset

The study uses the **Global Wheat Head Detection 2021 (GWHD 2021)** dataset.

| Split | Records | Boxes | Domains |
|---|---:|---:|---:|
| Training | 3,657 | 163,690 | 18 |
| Validation | 1,476 | 44,347 | 11 |
| Test | 1,382 | 67,424 | 18 |

The supplied partitions were preserved instead of randomly mixing images across domains so that evaluation better reflects generalization to unfamiliar field conditions.

The dataset itself is **not redistributed** in this repository.

Dataset reference:

**Global Wheat Head Detection 2021**  
DOI: `10.34133/2021/9846158`

---

## Repository Structure

```text
WheatHead-26M/
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   └── 01_wheathead26m_detection_counting.ipynb
└── paper/
    └── README.md
```

---

## Notebook

### `01_wheathead26m_detection_counting.ipynb`

The current notebook contains the main implementation workflow for the **YOLO26 / WheatHead-26M** pipeline, including data preparation, model training and evaluation, counting, and domain-wise analysis.

> **Reproducibility note:** The paper reports the complete comparison across YOLOv8n/s/m and YOLO26s/m. The notebook currently included in this repository primarily documents the YOLO26/WheatHead-26M workflow rather than every experiment reported in the paper.

---

## Training Setup

The experiments used:

- Input size: **640 × 640**
- Batch size: **16**
- Maximum epochs: **80**
- Early-stopping patience: **15**
- Cosine learning-rate scheduling
- YOLOv8 optimization with AdamW
- YOLO26 training with automatic optimizer selection and stronger augmentation

YOLO26 augmentation included HSV variation, small rotations, translation, scaling, flips, mosaic, and a small MixUp probability.

---

## Domain-Wise Analysis

Performance varied across the held-out domains. The largest counting errors appeared in dense domains such as **UQ_8** and **UQ_9**, while lower errors were observed in domains such as **NAU_2**, **CIMMYT_2**, and **NAU_3**.

Visual analysis showed that difficult cases were often associated with:

- densely packed wheat heads,
- partial occlusion by leaves or awns,
- small heads,
- visually complex backgrounds.

These observations reinforce that **cross-domain robustness remains an open challenge** even when overall detection and counting metrics are strong.

---

## Limitations

The current study has several limitations:

- YOLOv8 and YOLO26 were trained with different optimization and augmentation settings.
- All models used a fixed **640 × 640** input resolution.
- Evaluation was limited to static RGB images from GWHD 2021.
- Dense and heavily occluded wheat heads remained difficult to detect consistently.

Future work can explore standardized training protocols, domain adaptation, higher-resolution feature learning, occlusion-aware methods, and evaluation on additional real-world agricultural datasets.

---

## Paper

**Title:**  
*Cross-Domain Wheat Head Detection and Counting Using YOLOv8 and YOLO26: An Accuracy–Efficiency Study on GWHD 2021*

**Status:**  
Accepted for presentation at **IEEE WIECON-ECE 2026**.

The publisher-formatted paper is not redistributed in this repository. Official publication and DOI information can be added after the bibliographic record becomes available.

---

## Citation

Official citation details will be added once the final conference bibliographic record is available.

```bibtex
@misc{wheathead26m2026,
  title = {Cross-Domain Wheat Head Detection and Counting Using YOLOv8 and YOLO26: An Accuracy--Efficiency Study on GWHD 2021},
  note  = {Accepted for presentation at IEEE WIECON-ECE 2026},
  year  = {2026}
}
```

---

## License

No open-source license is currently attached to this repository.
