## Junction Detection Techniques for ALS Road Point Clouds - JunctionDetection.ipynb

This script implements multiple geometric techniques for detecting road junction regions in airborne laser scanning (ALS) point clouds. Since MLS road datasets are often topologically simple and ALS datasets frequently contain complex intersections and branching structures a reliable junction detection is required to make the roads into simpler components, improving downstream reconstruction stability and mesh generation.

All methods operate purely on spatial structure, no color, trajectory, or semantic labels are required.

### Implemented Methods

#### 1. Density Based Junction Highlighting

Detects candidate junction centers by analyzing local point density.

**Idea**

Junction regions contain higher neighborhood density compared to linear road segments.

**Method**

- radial neighbor search per point
- count neighbors inside a radius
- threshold density
- highlight dense regions

Output is a color coded point cloud where dense junction candidates are marked.

Best suited for:

- quick visual inspection
- coarse junction localization

#### 2. Local PCA Based Junction Detection (Recommended)

A structure aware method using local covariance analysis.

**Idea**

Linear road segments exhibit strong directional structure, while junctions show more isotropic spread.

**Method**

- radial neighborhood extraction
- covariance matrix computation
- eigenvalue decomposition
- junction score:

```
junction_score = λ₂ / λ₁
```

Where:

- λ₁ will give the dominant direction
- λ₂ will give the secondary spread

Higher ratios indicate multi-directional structure typical of junctions.


