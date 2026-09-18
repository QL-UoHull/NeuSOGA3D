# NeuSOGA3D

**NeuSOGA3D** (Neuro-Symbolic Observation-Guided Geometric Abstraction in 3D) is a research framework founded on the hypothesis that three-dimensional geometric understanding should emerge from interpretable symbolic abstraction and geometric reasoning rather than solely from latent statistical representations.

NeuSOGA3D extends the NeuSOGA framework by transforming unorganized point-cloud observations into explicit symbolic geometric representations, enabling explainable and CAD-compatible three-dimensional reconstruction.

---
![NeuSOGA3D Demo](https://github.com/QL-UoHull/NeuSOGA3D/raw/main/Demo/Demo-github.png)
---

## Philosophy

Most contemporary neural reconstruction systems encode geometry within high-dimensional latent representations. While effective for reconstruction, the resulting geometric knowledge is often difficult to interpret, edit, verify, or reuse in engineering workflows.

NeuSOGA3D investigates an alternative paradigm:

> Geometric understanding should emerge through the interaction of learned perception and symbolic geometric reasoning.

Rather than learning a hidden geometric representation, the framework progressively transforms observations into explicit symbolic entities that remain interpretable throughout the reconstruction process.

---

## Overview

NeuSOGA3D combines:

- Observation-guided perceptual abstraction
- Symbolic geometric reasoning
- Implicit spline modeling
- Constructive Solid Geometry (CSG)
- Volumetric reconstruction using Partial Shape-Preserving Splines (PSPS)

The framework converts point-cloud observations into a hierarchy of symbolic geometric representations including:

- Control polygons
- Implicit spline contours
- Cross-sectional abstractions
- Volumetric spline lofts
- CSG operators
- CAD-compatible geometric models

---

## Reconstruction Pipeline

### Stage 1: Multi-View Observation

An unorganized point cloud is projected onto principal orthographic views:

- Top view
- Front view
- Side view

Symbolic implicit spline contours are then extracted from each observation.

### Stage 2: Symbolic Visual Hull Construction

The projected representations are fused through shape-preserving Constructive Solid Geometry (CSG) operations to generate a coarse visual-hull hypothesis.

### Stage 3: Cross-Sectional Geometric Reasoning

Additional geometric structure is recovered through:

- Multi-axial decomposition
- Cross-sectional analysis
- Symbolic geometric abstraction

### Stage 4: Volumetric Reconstruction

Partial Shape-Preserving Splines (PSPS) are employed to construct continuous volumetric geometry while preserving structural and topological consistency.

### Stage 5: CAD-Compatible Representation

The resulting geometry remains explicit and interpretable, supporting conversion to:

- B-Splines
- NURBS
- Functional representations (FRep)
- Boundary representations (B-Rep)
- Other CAD-compatible formats

---

## Key Characteristics

### Explainable Reconstruction

Every reconstruction stage is represented by interpretable symbolic structures rather than latent neural parameters.

### Geometric Traceability

Geometric features can be traced back to the observations and symbolic abstractions from which they were generated.

### Shape Preservation

Shape-preserving implicit spline operators maintain essential geometric structure during blending and reconstruction.

### CAD Readiness

The generated representations are suitable for downstream engineering and design workflows.

---

## Experimental Validation

NeuSOGA3D has been evaluated on all 40 categories of the ModelNet40 benchmark.

Experimental results demonstrate:

- Robust reconstruction across diverse object categories
- Preservation of structural geometric features
- Explainable symbolic abstraction from point-cloud observations
- Generation of CAD-compatible geometric representations

---

## Relationship to NeuSOGA

NeuSOGA3D builds upon:

> **NeuSOGA: Neuro-Symbolic Observation-Guided Abstraction**
>
> From observations to symbolic mathematical representations.

While NeuSOGA focuses on transforming observations into symbolic geometric representations, NeuSOGA3D extends these principles to full three-dimensional geometric reconstruction and volumetric modeling.

---
NeuSOGA was developed and validated primarily in **Google Colab**. The easiest way to reproduce the results is through Colab or a compatible Jupyter environment.

---

# Running the demo


## Google Colab Setup

Run the following commands in the first notebook cell to install the required dependencies and fetch the Meta SAM weights:

```bash
!pip install pandas pyarrow opencv-python matplotlib scipy scikit-image numba
!pip install git+[https://github.com/facebookresearch/segment-anything.git](https://github.com/facebookresearch/segment-anything.git)
!wget -q [https://dl.fbaipublicfiles.com/segment_anything/sam_vit_b_01ec64.pth](https://dl.fbaipublicfiles.com/segment_anything/sam_vit_b_01ec64.pth)

```

The downloaded checkpoint (`sam_vit_b_01ec64.pth`) will be detected and used automatically by the NeuSOGA pipeline.

### Recommended Colab Configuration

For significantly faster neural prior extraction, enable hardware acceleration:

`Runtime → Change runtime type → GPU`

GPU acceleration is highly recommended but not strictly required.

---

## Local Python Environment

### 1. Clone the Repository

```bash
git clone [https://github.com/QL-UoHull/NeuSOGA3D.git](https://github.com/QL-UoHull/NeuSOGA3D.git)
cd NeuSOGA3D

```

### 2. Create a Virtual Environment

**Linux / macOS**

```bash
python -m venv .venv
source .venv/bin/activate

```

**Windows**

```cmd
python -m venv .venv
.venv\Scripts\activate

```

### 3. Install Dependencies

Install the required packages for the math engine, rendering pipeline, and data streaming:

```bash
pip install torch pandas pyarrow opencv-python matplotlib scipy scikit-image numba
pip install git+[https://github.com/facebookresearch/segment-anything.git](https://github.com/facebookresearch/segment-anything.git)

```

### 4. Download SAM Checkpoint (Optional)

The NeuSOGA Python script is designed to **automatically download** the required SAM checkpoint if it is missing from your working directory.

If you prefer to download it manually, run:

```bash
wget [https://dl.fbaipublicfiles.com/segment_anything/sam_vit_b_01ec64.pth](https://dl.fbaipublicfiles.com/segment_anything/sam_vit_b_01ec64.pth)

```

---

## External Resources

### ModelNet40 Dataset

NeuSOGA accesses the ModelNet40 dataset (`jxie/modelnet40`) using a direct Hugging Face Parquet stream via `pandas`.

* **No manual download is required.**
* The data streams directly into memory, bypassing local `.zip` extraction and avoiding known Python 3.13 circular import bugs in the official `datasets` library.

### Segment Anything (SAM)

NeuSOGA employs topology-guided perception using Meta's Segment Anything Model.

* Required checkpoint: `sam_vit_b_01ec64.pth`
* The framework automatically handles loading the model into the correct precision environment.

---

## Hardware Requirements

The framework automatically detects available hardware:

* **CPU:** Fully supported. The explicit math engine (PSPS) is heavily accelerated via `@njit` Numba vectorization and runs efficiently on CPUs.
* **CUDA GPU:** Automatically detected and utilized by PyTorch for the SAM inference stage. Highly recommended for large-scale robustness experiments.

---

## Running NeuSOGA3D

### Command Line Execution

To run the primary pipeline and generate the 1x3 publication-ready render:

```bash
python neusoga3d_demo.py

```

### Jupyter / Colab Notebooks

If you are exploring the framework via notebooks, we suggest the following workflow:

1. Open the notebook in your environment.
2. Run the dependency installation cell (if on Colab).
3. Execute the cells sequentially.

**Available Notebooks:**

* `notebooks/NeuSOGA3D_Demo.ipynb`: End-to-end demonstration of the NeuSOGA pipeline, featuring point cloud ingestion, SAM hull extraction, and PSPS mathematical lofting.
* `notebooks/NeuSOGA_Robustness.ipynb`: Robustness evaluation across various ModelNet40 object categories and topological structures.

```

```

---


## Paper

### NeuSOGA3D

**NeuSOGA3D: A Neuro-Symbolic Framework for Explainable 3D Geometric Reconstruction**

arXiv:2609.20323

https://arxiv.org/abs/2609.20323

### NeuSOGA

**Neuro-Symbolic Geometric Abstraction (NeuSOGA): From Observations to Symbolic Mathematical Representations**

arXiv:2609.01408

https://arxiv.org/abs/2609.01408

---

## Citation

```bibtex
@article{Li2026NeuSOGA3D,
  title={NeuSOGA3D: A Neuro-Symbolic Framework for Explainable 3D Geometric Reconstruction},
  author={Li, Qingde and Hong, Qingqi and Li, Zihan and Tian, Jie},
  journal={arXiv preprint arXiv:2609.20323},
  year={2026}
}
```

---

## Status

🚧 Active Research Project

This repository accompanies the NeuSOGA3D preprint. Community feedback, comments, and contributions are welcome.

---

## Vision

NeuSOGA3D explores the broader hypothesis that geometric intelligence can emerge from the integration of learned perception, symbolic abstraction, and explicit geometric reasoning, providing a pathway toward interpretable artificial geometric intelligence.
