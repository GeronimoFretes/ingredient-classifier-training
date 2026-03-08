# Ingredient Classifier Training — From Dataset Construction to ONNX Export

The model development side of an end-to-end ML product for ingredient recognition.

This repository contains the **full training pipeline** behind **Cocin.IA**: a computer vision project for recognizing kitchen ingredients from images, training an image classifier, evaluating performance, and exporting the final model to **ONNX** for browser-based inference.

This is the repository where the model was built.  
The companion repository shows how that exported model was integrated into a user-facing product.

## Companion Repository: Product Integration

This repository is one half of the full project.

The trained ONNX artifact produced here is used in the companion repository below, where I integrated the model into a Next.js application that scans ingredients, builds prompts, and generates recipes through an LLM workflow:

**Web app / product repo:**  
https://github.com/GeronimoFretes/ingredient-recipe-generator

Together, both repositories represent the full end-to-end system:

- **`ingredient-classifier-training`** → dataset construction, training, evaluation, export
- **`ingredient-recipe-generator`** → browser inference, product UX, and recipe generation

---

## Live Demo

This repository also includes a lightweight browser demo for testing the exported ONNX model with a webcam.

**[Live demo](https://geronimofretes.github.io/ingredient-classifier-training/index.html)**

---

## What this repository contains

This repo focuses on the **model development lifecycle**:

- image dataset construction
- automated image retrieval
- caption-based filtering
- CLIP-based ranking
- train / validation / test split
- model training with PyTorch
- model evaluation
- ONNX export for deployment
- lightweight browser demo for inference testing

This is the repository that documents how the classifier was actually created, not just how it was consumed inside an application.

---

## Pipeline overview

The workflow implemented in this repository follows these main stages:

1. **Dataset construction**  
   Images are collected automatically for each ingredient class using scripted retrieval.

2. **Filtering and ranking**  
   Candidate images are filtered through caption rules and ranked with **CLIP** to keep more relevant training examples.

3. **Dataset split**  
   The dataset is split into train / validation / test subsets.

4. **Model training**  
   A **PyTorch EfficientNet-B0** image classifier is trained on the ingredient dataset.

5. **Evaluation**  
   The trained model is evaluated on the test split, with optional confusion matrix and classification reporting.

6. **ONNX export**  
   The best checkpoint is exported to ONNX so it can be used in browser-based inference.

7. **Frontend testing**  
   The exported model can be tested through the included browser demo.

---

## Current model scope

The current classifier is a **closed-set image classification model**.

At this stage, it supports the following classes:

- sugar
- banana
- flour
- eggs
- egg carton (displayed in the UI as "eggs" for simplicity)
- leche
- manteca

Because this is a closed-set classifier, every prediction is mapped to one of those trained categories.  
This means the model should be interpreted as a **constrained prototype for ingredient recognition**, not as a general-purpose food recognition system.

---

## Why this project matters in my portfolio

This repository is meant to show that I can build the **model side** of a production-style ML project, not just consume pretrained tools.

It demonstrates work across:

- **dataset creation**
- **data filtering and quality control**
- **computer vision model training**
- **evaluation and error analysis**
- **model export for deployment**
- **bridging ML development with product integration**

The companion repository shows the application layer.  
This repository shows the **training and export** layer.

Together, they form a complete end-to-end ML product.

---

## Tech stack

- **Python**
- **PyTorch**
- **Torchvision**
- **OpenCLIP**
- **Playwright**
- **ONNX**
- **ONNX Runtime**
- **GitHub Pages** (demo)

---

## Repository structure

```bash
.
├── models/
│   ├── best_model.pth        # Trained PyTorch checkpoint
│   └── best_model.onnx       # Exported ONNX model
├── src/
│   ├── build_dataset.py      # Image retrieval + filtering + CLIP ranking
│   ├── split_dataset.py      # Train / val / test split
│   ├── train.py              # Model training
│   ├── eval.py               # Evaluation and optional confusion matrix
│   └── export_onnx.py        # ONNX export
├── classes.json              # Label mapping
├── index.html                # Browser demo for ONNX inference
├── requirements.txt          # Core dependencies
└── README.md
```

---

## How the training pipeline works

### 1. Install dependencies

```bash
pip install -r requirements.txt
```

Depending on your evaluation workflow, you may also want:

```bash
pip install scikit-learn pandas altair selenium
```

### 2. Build the dataset

```bash
python src/build_dataset.py
```

This stage:

* retrieves candidate images
* applies caption-based filtering
* reranks candidates with CLIP
* saves the selected images into `data/raw/`

### 3. Split the dataset

```bash
python src/split_dataset.py
```

This creates the standard structure under `data/split/`:

* `train/`
* `val/`
* `test/`

### 4. Train the model

```bash
python src/train.py --checkpoint models/best_model.pth
```

The training script uses **EfficientNet-B0** as the backbone and saves the best-performing checkpoint.

### 5. Evaluate the model

```bash
python src/eval.py --weights models/best_model.pth --confmat
```

This can generate:

* top-1 accuracy
* classification report
* confusion matrix
* optional heatmap output

### 6. Export to ONNX

```bash
python src/export_onnx.py --weights models/best_model.pth --out models/best_model.onnx --dynamic
```

This produces the ONNX artifact later used in the companion web application.

---

## Relationship to the web app repo

The final ONNX model exported here is the same artifact type used by the companion product repository:

**[https://github.com/GeronimoFretes/ingredient-recipe-generator](https://github.com/GeronimoFretes/ingredient-recipe-generator)**

That repository demonstrates the next step: turning a trained classifier into a deployable user-facing experience with browser inference and LLM-based recipe generation.

---

## Recommended reading order

If you are viewing this project as part of my portfolio, the best order is:

1. Review this repository to understand the **model development pipeline**
2. Try the browser demo to see the exported ONNX model in action
3. Visit the companion repository to see how the model was integrated into a full product

### Links

* **[Model training repo](https://github.com/GeronimoFretes/ingredient-classifier-training)**

* **[Web app repo](https://github.com/GeronimoFretes/ingredient-recipe-generator)**

* **[Live app](https://cocinia.vercel.app/)**
