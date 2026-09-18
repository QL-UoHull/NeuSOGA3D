# NeuSOGA3D

**NeuSOGA3D** (Neuro-Symbolic Observation-Guided Geometric Abstraction in 3D) is a research framework founded on the hypothesis that three-dimensional geometric understanding should emerge from interpretable symbolic abstraction and geometric reasoning rather than solely from latent statistical representations.

NeuSOGA3D extends the NeuSOGA framework by transforming unorganized point-cloud observations into explicit symbolic geometric representations, enabling explainable and CAD-compatible three-dimensional reconstruction.

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

## Google Colab Setup

Run the following commands in the first notebook cell:

```bash
!pip install rembg
!pip install opencv-python matplotlib
!pip install git+https://github.com/facebookresearch/segment-anything.git
!wget -q https://dl.fbaipublicfiles.com/segment_anything/sam_vit_b_01ec64.pth
```

The downloaded checkpoint:

```text
sam_vit_b_01ec64.pth
```

will be used automatically by the NeuSOGA pipeline.

### Recommended Colab Configuration

```text
Runtime → Change runtime type → GPU
```

GPU acceleration is recommended but not required.

---

## Local Python Environment

### 1. Clone the Repository

```bash
git clone https://github.com/QL-UoHull/NeuSOGA3D.git

cd NeuSOGA
```

### 2. Create a Virtual Environment

#### Linux / macOS

```bash
python -m venv .venv

source .venv/bin/activate
```

#### Windows

```cmd
python -m venv .venv

.venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install rembg
pip install opencv-python matplotlib
pip install git+https://github.com/facebookresearch/segment-anything.git
```

### 4. Download SAM Checkpoint

```bash
wget https://dl.fbaipublicfiles.com/segment_anything/sam_vit_b_01ec64.pth
```

or manually download from:

https://github.com/facebookresearch/segment-anything

---

# External Resources

## ModelNet40 Dataset

NeuSOGA automatically downloads and extracts the ModelNet40 dataset when executed for the first time.

The dataset is not bundled with this repository and no manual download is required.

---

## Segment Anything (SAM)

NeuSOGA employs topology-guided perception using Meta's Segment Anything Model.

Required checkpoint:

```text
sam_vit_b_01ec64.pth
```

Download:

```bash
wget -q https://dl.fbaipublicfiles.com/segment_anything/sam_vit_b_01ec64.pth
```

---

## Hardware Requirements

The framework automatically detects:

```text
CPU
CUDA GPU
```

CPU execution is fully supported.

GPU acceleration is recommended for large-scale robustness experiments but is not required.

---

# Running NeuSOGA

## Google Colab

After executing the installation cell:

```python
!python neusoga3d_demo.py
```

or execute the notebook cells sequentially.

---

## Local Execution

Run:

```bash
python neusoga3d_demo.py
```

The script automatically:

1. Downloads ModelNet40 (if required).
2. Loads SAM.
3. Processes representative objects from all 40 ModelNet40 categories.
4. Generates arbitrary-view projections along:

```text
[1, 1, 1]
```

5. Executes the complete:

```text
O → T → G → S
```

abstraction hierarchy.

---

# Outputs

Results are written to:

```text
robustness_results/
```

For each object, NeuSOGA generates an eight-stage visualization illustrating:

```text
1. Observation (O)
2. Euclidean Distance Transform
3. Topology Nodes (T)
4. Topology-Guided Segmentation
5. Scale-Space Contour
6. Control Polygon (G)
7. Area Spline Field
8. Symbolic Boundary F(x,y)=0 (S)
```

These visualizations provide a transparent view of how symbolic mathematical representations emerge from geometric observations.

---

# Colab and Jupyter Notebook Workflows

## Google Colab

Recommended workflow:

1. Open the notebook in Colab.
2. Run the dependency installation cell shown above.
3. Ensure the SAM checkpoint has been downloaded.
4. Enable GPU runtime (optional but recommended).
5. Execute all notebook cells sequentially.

---

## Local Jupyter Notebook

Launch Jupyter:

```bash
jupyter notebook
```

or

```bash
jupyter lab
```

Then:

1. Open the desired NeuSOGA notebook.
2. Install the required dependencies.
3. Download the SAM checkpoint.
4. Execute notebook cells in order.

---

## Suggested Notebooks

```text
notebooks/NeuSOGA3D_Demo.ipynb
```

End-to-end demonstration of the NeuSOGA pipeline.

```text
notebooks/NeuSOGA_Robustness.ipynb
```

Robustness evaluation across object categories and viewpoints.

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
