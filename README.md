
```markdown
# People Counting and Waiting-Area Analysis

This project detects and tracks people in video footage, counts movement direction, and identifies people waiting inside user-defined regions of interest (ROIs).

The system uses a YOLO-based model with ByteTrack tracking and provides a Gradio interface for analysing images and video clips.

## Features

- People detection using a trained YOLO model
- Person tracking across video frames
- Movement classification:
  - `person_left`
  - `person_right`
- User-defined left and right waiting areas
- Waiting detection based on:
  - Time spent inside the ROI
  - Maximum speed in pixels per second
  - Detection-gap tolerance
  - ROI edge tolerance
- Optional video stabilisation
- Annotated video output
- JSON analysis results
- Interactive Gradio interface

## Project Files

### `L4_local_laptop.ipynb`

The main notebook containing:

- People detection
- Video tracking
- ROI selection
- Waiting detection
- Speed calculation
- Video annotation
- Gradio interface

### `best_1.pt`

The trained YOLO model used for people detection and classification.

## Requirements

- Python 3.10 or later
- Jupyter Notebook or JupyterLab
- OpenCV
- NumPy
- Ultralytics
- Gradio
- Pillow
- Matplotlib

Install the dependencies:

```bash
pip install opencv-python numpy ultralytics gradio pillow matplotlib
```

## Running the Project

Clone the repository:

```bash
git clone https://github.com/choppedtomato277/C239-004-TEAM-4-CA1.git
cd C239-004-TEAM-4-CA1
```

Open the notebook:

```bash
jupyter notebook L4_local_laptop.ipynb
```

Run the notebook cells in order. Finally, run the Gradio interface cell and open the generated URL.

## Using the Interface

### Image Analysis

1. Upload a photograph.
2. Adjust the photo confidence threshold if necessary.
3. Click `Count or analyse`.
4. View the annotated image and JSON response.

### Video Analysis

1. Upload a video clip.
2. Use the first-frame previews to draw the waiting areas.
3. Select exactly four points for each ROI.
4. Configure the sliders.
5. Click `Count or analyse`.
6. View the annotated video and JSON response.

If the user does not provide an ROI, that ROI is not drawn or analysed.

## Configuration

### Photo Confidence Threshold

Controls the minimum confidence required for image detections.

### Clip Confidence Threshold

Controls the minimum confidence required for video detections.

### Minimum Time in ROI

The minimum time a person must remain inside an ROI before being classified as waiting.

### Maximum Waiting Speed

The maximum movement speed allowed for a person to be classified as waiting.

Speed is measured in image pixels per second using the tracked foot points. The default value is:

```text
30 pixels per second
```

### Allowed Detection Gap

Allows brief periods where a person is not detected without ending the same waiting event.

### Edge Tolerance

Allows a person's detected foot points to be slightly outside the ROI boundary.

### Stabilisation

Optionally stabilises video footage recorded with a moving or shaking camera.

## Waiting Detection Logic

A person is classified as waiting only when:

1. The person is successfully tracked.
2. Their foot points are inside or near a user-defined ROI.
3. Their dwell time meets the minimum ROI duration.
4. Their median speed is below the selected maximum speed.
5. Detection gaps remain within the configured tolerance.

## Output

The system produces an annotated video showing:

- Detected people
- Track IDs
- Movement class
- Waiting status
- User-defined ROI boundaries

The JSON response contains:

- People counts
- Direction counts
- Waiting count
- Waiting areas
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

## Important Notes

- ROI coordinates are based on the uploaded video's frame dimensions.
- Draw ROIs carefully around the intended waiting areas.
- Avoid overlapping left and right ROIs.
- Speed is measured in pixels per second, not real-world distance.
- Different cameras and resolutions may require new ROI coordinates.
- The public Gradio link is temporary and should not be treated as a permanent deployment.

## Repository

https://github.com/choppedtomato277/C239-004-TEAM-4-CA1
```
