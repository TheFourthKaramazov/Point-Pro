# Usage Guide for Depth Analysis Tool in Jupyter Notebooks

This guide explains how to use Point-Pro efficiently in Jupyter Notebooks.

## Setup

1. Import required modules:

    ```python
    import depth_pro
    from point_pro import *
    import matplotlib.pyplot as plt
    %matplotlib inline
    ```

2. Load the model (do this only once per notebook session):

    ```python
    device = setup_device()
    model, transform = depth_pro.create_model_and_transforms()
    model = model.to(device)
    model.eval()
    ```

## Creating and Analyzing Depth Map

To analyze an image:

1. Load and process the image:

    ```python
    image_path = "path/to/your/image.jpeg"
    image_tensor, f_px = load_and_process_image(image_path, transform, device)
    ```

2. Run depth inference:

    ```python
    depth_map, focallength_px = run_depth_inference(model, image_tensor, f_px)
    ```

3. Visualize results:

    ```python
    visualize_results(ensure_image_format(image_tensor), depth_map)
    ```

4. Calculate distances:

    ```python
    overall_distances = calculate_distances(depth_map)
    print("Overall distances:")
    print(f"Minimum distance: {overall_distances['min_distance']:.2f} meters")
    print(f"Maximum distance: {overall_distances['max_distance']:.2f} meters")
    print(f"Mean distance: {overall_distances['mean_distance']:.2f} meters")
    ```

5. Calculate distances for specific points:

    ```python
    points_of_interest = [(100, 200), (150, 300), (200, 400)]  # Example coordinates
    specific_distances = calculate_distances(depth_map, points_of_interest)
    
    print("\nDistances at specific points:")
    for point, distance in specific_distances.items():
        if distance is not None:
            print(f"{point}: {distance:.2f} meters")
        else:
            print(f"{point}: Invalid depth")
    ```

6. Visualize depth map with points of interest:

    ```python
    plt.figure(figsize=(10, 8))
    plt.imshow(depth_map, cmap='viridis')
    plt.colorbar(label='Distance (meters)')
    plt.title('Depth Map')
    
    for y, x in points_of_interest:
        plt.plot(x, y, 'ro')  # Mark points of interest with red dots
    
    plt.show()
    ```

## Creating Point Cloud

 1. Create and Save Point Cloud

    ```python
    # create a point cloud
    point_cloud = create_point_cloud(depth_map, image_tensor, focallength_px)

    # save point cloud
    save_point_cloud(point_cloud, "path/to/your/directory")
    ```
    

 2. Use software such as CloudCompare or MeshLab to visualize results


## Tips for Efficient Use

1. Load the model only once at the beginning of your notebook session.
2. Reuse the `model` and `transform` variables for multiple images.
3. Clear output of cells that generate large visualizations to save memory.
4. Use `%matplotlib inline` to display plots directly in the notebook.
5. If working with multiple images, consider using functions to encapsulate the analysis process.

## Troubleshooting

- If you encounter memory issues, try restarting the kernel and running the setup steps again.
- Ensure all dependencies are correctly installed in your Jupyter environment.
- For large images, consider downsampling before processing to reduce memory usage.
