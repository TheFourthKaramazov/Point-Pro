# Point-Pro - Point Clouds from Single Single Images

This project provides a set of tools for depth map generation, point cloud creation, and distance analysis from single images using Apple's Depth-Pro model for monocular depth map genration.

![image](https://github.com/user-attachments/assets/1ea2b7a8-f3cb-405c-9bd4-f82b8e223834)


## Features

- Depth map generation from single RGB images
- Point cloud creation from depth maps
- Distance calculation and analysis
- Visualization of results

## Dependencies

This project relies on the following Python libraries:

- depth_pro (https://github.com/apple/ml-depth-pro)
- numpy
- torch
- matplotlib
- open3d
- opencv-python (cv2)
- Pillow (PIL)

## Installation

1. Install Depth Pro:

Follow the installation and virtual enviornment creation instructions at https://github.com/apple/ml-depth-pro/blob/main/README.md

2. Clone this repository:

```bash 
git clone https://github.com/your-username/depth-analysis-tool.git](https://github.com/TheFourthKaramazov/point_pro.git
```

## Credit

This project uses the Depth Pro model for depth estimation. The model is Developped by Apple's ML lab and the github repository can be found here:
https://github.com/apple/ml-depth-pro


@article{Bochkovskii2024:arxiv,
  author     = {Aleksei Bochkovskii and Ama\"{e}l Delaunoy and Hugo Germain and Marcel Santos and
               Yichao Zhou and Stephan R. Richter and Vladlen Koltun}
  title      = {Depth Pro: Sharp Monocular Metric Depth in Less Than a Second},
  journal    = {arXiv},
  year       = {2024},
  url        = {https://arxiv.org/abs/2410.02073},
}
