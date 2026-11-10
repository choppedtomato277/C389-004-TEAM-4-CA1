```markdown
# People Counting and Waiting-Area Analysis

This project detects and tracks people in video footage, counts movement direction, and identifies people waiting inside user-defined regions of interest (ROIs).

The system uses a YOLO-based model with ByteTrack tracking and provides a Gradio interface for analysing images and video clips.

## Features

- People detection using a trained YOLO model
- Person tracking across video frames
- Direction classification:
  - `person_left`
  - `person_right`
- User-defined left and right waiting areas
- Waiting detection based on:
  - Time spent inside the ROI
  - Maximum movement speed in pixels per second
  - Detection-gap tolerance
  - ROI edge tolerance
- Optional video stabilisation
- Annotated video output
- JSON analysis results
- Interactive Gradio web interface

## Files

| File | Description |
|---|---|
| `L4_local_laptop.ipynb` | Main notebook containing the detection, tracking, ROI analysis, and Gradio interface |
| `best_1.pt` | Trained YOLO model used for person detection and classification |

## Requirements

Recommended environment:

- Python 3.10 or later
- Jupyter Notebook or JupyterLab
- OpenCV
- NumPy
- Ultralytics
- Gradio
- Pillow
- Matplotlib

Install the main dependencies with:

```bash
pip install opencv-python numpy ultralytics gradio pillow matplotlib
```

## Running the Notebook

1. Clone the repository:

```bash
git clone https://github.com/choppedtomato277/C239-004-TEAM-4-CA1.git
cd C239-004-TEAM-4-CA1
```

2. Open the notebook:

```bash
jupyter notebook L4_local_laptop.ipynb
```

3. Run the notebook cells in order.

4. Run the final Gradio interface cell.

5. Open the generated local or public Gradio URL.

## Using the Interface

### Image Analysis

1. Upload a photograph.
2. Adjust the photo confidence threshold if necessary.
3. Run the analysis.
4. View the annotated image and JSON response.

### Video Analysis

1. Upload a video clip.
2. Use the first-frame previews to draw the waiting areas.
3. Select exactly four points for each ROI.
4. Configure the analysis sliders.
5. Click `Count or analyse`.
6. View the annotated video and JSON response.

If an ROI is not provided by the user, that ROI is not drawn and is not included in the waiting-area analysis.

## Configuration Sliders

### Photo Confidence Threshold

Controls the minimum confidence required for image detections.

### Clip Confidence Threshold

Controls the minimum confidence required for video detections.

### Minimum Time in ROI

The minimum amount of time a person must remain inside an ROI before being considered waiting.

### Maximum Waiting Speed

The maximum allowed movement speed in pixels per second.

The system calculates the median speed between consecutive tracked foot points. A person is classified as waiting only when their median speed is at or below the selected limit.

The default recommended value is:

```text
30 pixels per second
```

### Allowed Detection Gap

Allows brief periods where a person is not detected without immediately ending the same waiting event.

### Edge Tolerance

Allows a person's detected foot points to be slightly outside the ROI boundary.

### Stabilisation

Optional video stabilisation for footage recorded with a moving or shaking camera.

## Waiting Detection Logic

A person is reported as waiting only when all of the following conditions are satisfied:

1. The person is successfully tracked.
2. Their tracked foot points are inside or near a user-defined ROI.
3. Their dwell time meets the minimum ROI duration.
4. Their median movement speed is below the selected maximum speed.
5. Detection gaps do not exceed the configured gap tolerance.

## Output

The system produces:

- An annotated video showing:
  - Detected people
  - Track IDs
  - Movement class
  - Waiting status
  - Configured ROI boundaries
- A JSON response containing:
  - Total people counts
  - Direction counts
  - Waiting count
  - Waiting area
  - Track IDs
  - Start and end times
  - Dwell duration
  - Median speed
  - Maximum observed speed

Example waiting result:

```json
{
  "track_id": 12,
  "class_name": "person_left",
  "start_seconds": 4.2,
  "end_seconds": 8.7,
  "dwell_seconds": 4.5,
  "area": "left",
  "median_speed_px_s": 12.4,
  "max_speed_px_s": 35.8
}
```

## Model

The model file is:

```text
best_1.pt
```

The notebook loads this model for video tracking and analysis. Ensure that the model file remains in the same directory as the notebook unless the model path is updated in the notebook configuration.

## Notes

- The ROI coordinates are based on the uploaded video's frame dimensions.
- ROIs should be drawn carefully around the intended waiting areas.
- Avoid overlapping left and right ROIs because the same person may be detected in both regions.
- The speed threshold is measured in image pixels per second, not real-world distance.
- A different camera angle, resolution, or perspective may require new ROI coordinates and speed calibration.
- The generated public Gradio link is temporary and should not be treated as a permanent deployment.

## Repository

Repository:

https://github.com/choppedtomato277/C239-004-TEAM-4-CA1
```
