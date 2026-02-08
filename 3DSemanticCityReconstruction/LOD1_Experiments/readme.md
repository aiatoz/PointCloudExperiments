# LoD1 Building Reconstruction Techniques

---

## A. Mesh Normal Alignment for Better Shading : ReworkMesh.ipynb

This script rebuilds triangle meshes with **flat (per-face) normals** from existing PLY meshes. It is designed as a post-processing especially for reconstructed building meshes, ensuring consistent shading and compatibility with visualization or downstream geometry pipelines.


---

## B. LoD1Mesh-MLS.ipynb
# MLS Cuboid & LoD Reconstruction Experiments

Geometric experiments for reconstructing buildings from Mobile Laser Scanning (MLS) point clouds

## Implemented Methods

### 1. Simple Cuboid Construction

To :
- Understand mesh topology
- Validate PyVista mesh construction
- Establish visualization workflow

---

### 2. Axis-Aligned Bounding Cuboid (LoD1)

- minimum and maximum XYZ extents
- direct bounding box enclosure

---

### 3. PCA-Based Oriented Bounding Cuboid

Better mechanism, with more accuracte pose estimation
- performing PCA on the building point cloud
- aligning the model with dominant axes
- computing the cuboid in rotated space
- transforming back to world coordinates

---

### 4. Centerline-Guided Pose Estimation

Context aware reconstruction mechanism
- finds nearest road centerline to building centroid
- extracts local road neighborhood
- estimates dominant orientation via PCA or tangent direction
- aligns cuboid using urban structure context


---

### 5. Premilinary LoD1.2 Experiments - Convex, Concave Hull Footprint Extraction

Convex :
- projecting building points to XY
- computing convex hull footprint
- extruding vertically

Concave : Enhances footprint realism using
- k-nearest neighbor concave hull tracing
- adaptive boundary following
- vertical extrusion

  
---

## C. ALS LiDAR Point Cloud Experiments : LoD1Mesh-ALS.ipynb

The focus is on robust pose estimation, ground aware alignment, and footprint orientation without relying on machine learning techniques.

The implementations are designed to progressively address common ALS challenges such as:
- Roof dominant point distributions
- Weak vertical structures (like walls)
- Sloped terrain (Ground alignment)
- Noisy or incomplete data

Each method represents a different strategy for estimating building orientation and constructing watertight cuboid meshes.

### Implemented Reconstruction Methods

#### 1. Ground-Aware PCA Cuboid (Baseline)

A simple yet robust cuboid reconstruction method that:

- estimates local ground elevation using nearby terrain points
- normalizes building height relative to ground
- performs 2D PCA on the footprint
- constructs an oriented bounding cuboid

This approach works well for clean, rectangular structures but can struggle when roof geometry dominates orientation.

#### 2. Rasterized Minimum Rectangle Cuboid

Improves footprint orientation by:

- rasterizing projected building points
- extracting dominant connected footprint contours
- computing a minimum-area rectangle
- aligning the cuboid to the rectangle orientation

Fallback mechanisms ensure stability when contour extraction fails.

#### 3. Directional Consensus Cuboid (Edge + RANSAC Voting)

A more advanced orientation estimator that:

- builds a concave hull footprint
- extracts edge directions
- iteratively applies RANSAC line fitting
- performs angle voting to detect dominant building axes
- constructs a cuboid aligned with consensus orientation

This method is designed to preserve façade alignment and handle complex footprints.

#### 3.1 Robust Voting + Ground Plane Refinement

An enhanced version of Method 3 with additional safeguards:

- circular angle voting for stability
- PCA fallback when consensus is weak
- roof-only detection handling
- sloped ground plane fitting
- cuboid snapping to terrain

This improves reconstruction reliability for difficult ALS scenarios.

#### 4. Minimum-Area Rectangle Footprint Cuboid

A geometry-driven method that:

- computes convex hull footprints
- extracts minimum-area rectangles
- estimates height relative to ground
- extrudes cuboid meshes

Simple and fast, suitable as a fallback or baseline method.

#### 5. Ridge-Based Orientation Cuboid (Contour + Multi-Line RANSAC)

The most advanced pipeline, designed for complex roofs:

- adaptive ridge point extraction via height percentiles
- contour-based footprint analysis
- multi-line RANSAC direction voting
- dominant ridge orientation estimation
- cuboid construction aligned with roof geometry

This approach is especially effective for multi-gable or irregular roof structures.

---

## Dependencies

- Python 3.x
- Open3D
- PyVista
- NumPy
- SciPy
- Shapely
- scikit-learn
- alphashape

---

## Input Expectations

- Segmented building point clouds in PLY format (Could utilize clustering techniques or panoptic segmentation)
- Corresponding ground/terrain point clouds

---

## Outputs

- Oriented LoD1 cuboid meshes (PLY format)
- Ground aligned reconstruction

