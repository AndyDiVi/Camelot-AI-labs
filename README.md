# Applied AI Module — Computer Vision and NLP

Four educational notebooks from a module sponsored by Camelot. This is a student laboratory collection, not an official Camelot product or an endorsement by the sponsor.

## Contents

| Notebook | Focus |
|---|---|
| `01_image_classification.ipynb` | Flower-image classification: CNNs, normalization, augmentation, dropout, checkpoint selection, custom classification-error metric, and TensorFlow Lite export/inference |
| `02_image_segmentation_unet.ipynb` | Brain MRI exploration and 2D binary whole-tumor segmentation with U-Net, Dice metrics/loss, and validation-based threshold selection |
| `03_nlp_scientific_papers.ipynb` | arXiv discovery, PDF text extraction, chunking, MiniLM embeddings, FAISS retrieval, FLAN-T5 question answering, and parent-document retrieval |
| `04_final_laboratory_mnist.ipynb` | MNIST CNN comparisons with five-fold validation, PCA + Random Forest, CNN features + Random Forest, and Grad-CAM |

## Setup

Use Python 3.10 or 3.11 in a dedicated environment:

```bash
python -m venv .venv
# Linux/macOS
source .venv/bin/activate
# Windows PowerShell alternative:
# .venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
jupyter lab
```

Run Jupyter from the repository directory. Open a notebook and run cells in order from a clean kernel. Dependencies target TensorFlow/Keras 2 and LangChain 0.2; do not assume compatibility with newer major versions. GPU installation is platform-specific and is not configured by this repository.

## 1. Image Classification

The introductory laboratory downloads the TensorFlow flower-photo dataset and constructs a training/validation split. It compares a basic convolutional classifier with augmentation and dropout configurations, monitors accuracy/loss, selects a model checkpoint, predicts a new flower image, and exports the selected model to TensorFlow Lite.

The output layer produces logits; softmax is applied when displaying prediction scores. The custom error metric is the proportion of incorrectly classified examples. Validation results are not an independent final test benchmark.

TensorFlow tutorial copyright and Apache 2.0 notices included in the source notebook are retained. Preserve these notices when redistributing derived material.

## 2. Brain Tumor Segmentation

This notebook uses T2 MRI slices as input. Original segmentation labels 1, 2, and 4 are merged into one whole-tumor foreground mask. It does not predict separate tumor subregions.

Patients are split before slice extraction. The notebook includes label validation, nearest-neighbor mask resizing, consistent per-patient normalization, a U-Net, binary cross-entropy training, Dice loss as an available alternative, checkpoint selection, threshold search on validation data, and pooled thresholded test Dice.

### External BraTS data

Obtain BraTS 2020 separately through an authorized distribution and follow its data-use terms. No MRI data or masks are bundled.

Set the `BRATS_DATA_DIR` environment variable to the directory containing the patient folders, or use:

```text
data/MICCAI_BraTS2020_Data/
  BraTS20_Training_001/
    BraTS20_Training_001_t1.nii
    BraTS20_Training_001_t1ce.nii
    BraTS20_Training_001_t2.nii
    BraTS20_Training_001_flair.nii
    BraTS20_Training_001_seg.nii
```

Both `.nii` and `.nii.gz` are supported. A minimum of five complete patients is required by the example split; experiments use up to 100. Volumes must cover the selected axial slice range, 60–134. Patient identifiers and source affine orientation must be reviewed when adapting to another dataset; displayed anatomical-plane labels are illustrative.

This is an educational experiment, not a clinically validated system. It must not be used for diagnosis or patient decisions.

## 3. Scientific-Paper Retrieval and Question Answering

The NLP laboratory retrieves recent arXiv papers, extracts text, creates overlapping chunks, embeds them with MiniLM, and performs FAISS search. FLAN-T5 generates answers from retrieved text. A parent retriever searches smaller child chunks and returns larger parent documents.

Internet access is required for papers and model weights. Downloaded paper metadata is saved for traceability. PDF extraction does not include OCR and may lose layout/reading order. Chunk sizes use characters, whereas the model input budget uses tokens. Context is bounded and can be truncated; inspect retrieved source text before accepting an answer.

No paid inference API or API key is required. Public document and model terms apply separately. Generated answers can be incorrect even when retrieval finds relevant text.

## 4. Final Laboratory — MNIST

The final laboratory remains separate from the introductory flower-classification notebook. It uses the first 4000 MNIST training examples for its main experiments and retains the official test set for additional evaluation.

The workflow includes:

- Class-distribution exploration and one-hot labels.
- A baseline CNN with deliberately aggressive augmentation.
- A revised CNN with smaller rotations/zoom, dropout, Adam, early stopping, and learning-rate reduction.
- Five-fold comparison with fresh model/augmentation layers per fold.
- Metrics and predictions from the checkpoint selected by validation accuracy.
- PCA + Random Forest, with scaling/PCA fitted inside the cross-validation pipeline.
- A separately trained CNN feature extractor followed by Random Forest.
- Grad-CAM for the CNN output, not for the Random Forest classifier.

Several components change between CNN configurations, so this is not an ablation study. Scores used to tune configurations are not unbiased final generalization estimates. Repeatedly inspecting official test performance makes that test exploratory. Grad-CAM visualizations do not establish causal explanations.

## Outputs and Reproducibility

Notebook execution outputs have been cleared. No historical metrics are presented as results from the completed code. Run the notebooks to generate scores and figures.

Checkpoints, logs, and exported models are written under `artifacts/`; arXiv papers are stored under `data/papers/`. Flower images and MNIST are downloaded through Keras. Exclude generated datasets, checkpoints, logs, `.venv`, and notebook cache directories from commits unless redistribution is intentional and permitted.

Seeds improve repeatability but do not guarantee bitwise-identical GPU execution. Full cross-validation, segmentation training, and model downloads may require substantial time, memory, and disk space.

## Verification

The package was checked for valid notebook structure and Python-cell syntax. Missing exercise implementations were filled in, and no executable Ellipsis placeholders or TODO code cells remain (Ellipsis used for NumPy slicing is intentional).

Full dependency installation, dataset downloads, training, TensorFlow Lite conversion, and NLP inference have not been executed in the packaging environment. The requirements specify a proposed compatibility environment, not an end-to-end certified installation. No runtime or numerical guarantees are implied by the static checks.

## Attribution

Original source filenames:

- `lab1_classificazione.ipynb`
- `DiVia2_image_segmentation_unet.ipynb`
- `lab2_NLP.ipynb`
- `Catalano_DiVia_Final_laboratory.ipynb`

The final laboratory is joint work attributed to Catalano and Di Via in the supplied material. Preserve original module/tutorial references and copyright notices. Exercise completions and maintenance changes were prepared with AI assistance. Confirm permission to publish coauthored work, teaching material, and embedded third-party images before making the repository public. Sponsor involvement does not itself grant redistribution rights; this README does not assign a blanket license to third-party content.

## Repository Description

Applied AI notebooks covering flower classification, U-Net MRI segmentation, scientific-paper RAG, and a final MNIST study with CNNs and classical ML baselines.
