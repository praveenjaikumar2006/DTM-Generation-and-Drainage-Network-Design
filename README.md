# DTM Generation and Drainage Network Design

Digital Terrain Model (DTM) generation from LiDAR data using deep learning (PointNet) for water body delineation and waterlogging prediction.

## Project Overview

This project uses PointNet neural network to classify LiDAR point clouds into ground and non-ground points, then generates high-quality DTMs suitable for hydrological analysis.

## Features

- Deep learning-based ground point classification using PointNet
- Strong ground filtering (keeps only lowest point per grid cell)
- Accurate boundary handling
- Clean DTM generation optimized for water body delineation
- Hydrological analysis ready

## Data

Training data is located in the `training/` folder:
- `AREA_1.las` - LiDAR point cloud data
- `AREA_1.csv` - Labeled training data (x, y, z, label)
  - Label 0: Non-ground (buildings, vegetation)
  - Label 1: Ground

## Requirements

```bash
pip install laspy numpy torch scikit-learn scipy rasterio matplotlib tqdm
```

## Usage

### 1. Train the Model

```bash
python train_simple.py
```

This will:
- Load training data from `training/AREA_1.csv`
- Train PointNet model for 10 epochs
- Save model as `pointnet_model.pt`
- Save normalization parameters as `model_params.npz`

### 2. Generate DTM

```bash
python generate_dtm_fast.py training/AREA_1.las
```

This will:
- Load trained model
- Predict ground points
- Apply strong ground filtering
- Generate DTM with proper boundaries
- Output: `output_dtm.tif` and `output_dtm_visualization.png`

## Output

- `output_dtm.tif` - GeoTIFF DTM (EPSG:32643)
- `output_dtm_visualization.png` - Terrain and hillshade visualization

## Project Goal

Generate accurate DTMs for:
1. Water body delineation
2. Waterlogging prediction
3. Hydrological modeling
4. Drainage network design

## Model Architecture

- PointNet for per-point classification
- Input: 4 features (x_norm, y_norm, z_norm, z_norm)
- Output: 2 classes (ground, non-ground)
- Training: 200K points, 10 epochs

## Post-Processing

1. Strong ground filtering - keeps only lowest point per 1m cell
2. Nearest neighbor interpolation
3. Boundary masking with erosion
4. Gaussian smoothing (sigma=1.0)

## Results

- Successfully removes buildings and vegetation
- Clean terrain surface
- Accurate boundaries
- Ready for hydrological analysis

## License

MIT License

## Author

Praveen Jaikumar
