# Usage Guide for Depth Analysis Tool in Jupyter Notebooks

This guide explains how to use **Point-Pro** efficiently in Jupyter Notebooks.

## Setup

1. **Import required modules**:

    ```python
    import depth_pro
    from point_pro import *
    import matplotlib.pyplot as plt
    %matplotlib inline
    ```

2. **Load the model** (do this only once per notebook session):

    ```python
    device = setup_device()
    model, transform = depth_pro.create_model_and_transforms()
    model = model.to(device)
    model.eval()
    ```

## Creating and Analyzing Depth Maps

To analyze **all images in a directory**:

1. **Load and process images from a directory**:

    ```python
    image_dir = "path/to/image_directory"  # Path to the directory containing images
    image_paths, image_tensors, focal_lengths = load_and_process_images_from_directory(image_dir, transform, device)
    ```

2. **Run depth inference on all images**:

    ```python
    depth_maps, focal_px_values = run_depth_inference(model, image_tensors, focal_lengths)
    ```

3. **Visualize results**:

    ```python
    visualize_results(image_tensors, depth_maps)
    ```

4. **Calculate distances**:

    ```python
    overall_distances = calculate_distances(depth_maps)
    print("Overall distances:")
    for img_idx, distances in overall_distances.items():
        print(f"\nImage {img_idx}:")
        print(f"Minimum distance: {distances['min_distance']:.2f} meters")
        print(f"Maximum distance: {distances['max_distance']:.2f} meters")
        print(f"Mean distance: {distances['mean_distance']:.2f} meters")
    ```

5. **Calculate distances for specific points**:

    ```python
    points_of_interest = [
        [(100, 200), (150, 300)],  # Points for image 1
        [(200, 400), (250, 450)]   # Points for image 2
    ]
    
    specific_distances = calculate_distances(depth_maps, points_of_interest)
    
    print("\nDistances at specific points:")
    for img_idx, distances in specific_distances.items():
        print(f"\nImage {img_idx}:")
        for point, distance in distances.items():
            if distance is not None:
                print(f"{point}: {distance:.2f} meters")
            else:
                print(f"{point}: Invalid depth")
    ```

6. **Visualize depth map with points of interest**:

    ```python
    import matplotlib.pyplot as plt

    for i, (depth_map, points) in enumerate(zip(depth_maps, points_of_interest)):
        plt.figure(figsize=(10, 8))
        plt.imshow(depth_map, cmap='viridis')
        plt.colorbar(label='Distance (meters)')
        plt.title(f'Depth Map {i+1}')

        for y, x in points:
            plt.plot(x, y, 'ro')  # Mark points of interest with red dots

        plt.show()
    ```

## Creating and Saving Point Clouds

1. **Create and save point clouds for all images**:

    ```python
    # Create point clouds
    point_clouds = create_point_clouds(depth_maps, image_tensors, focal_px_values)

    # Save point clouds automatically in 'generated_pointclouds/' directory
    save_point_clouds(point_clouds, image_paths)
    ```

2. **Use software such as CloudCompare or MeshLab to visualize results.**

## Tips for Efficient Use

1. Load the model only once at the beginning of your notebook session.
2. Reuse the `model` and `transform` variables for multiple images.
3. Clear the output of cells that generate large visualizations to save memory.
4. Use `%matplotlib inline` to display plots directly in the notebook.
5. If working with multiple images, consider using functions to encapsulate the analysis process.

## Troubleshooting

- If you encounter memory issues, try restarting the kernel and running the setup steps again.
- Ensure all dependencies are correctly installed in your Jupyter environment.
- For large images, consider downsampling before processing to reduce memory usage.
