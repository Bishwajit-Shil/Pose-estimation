# Pose Estimation

## Supervision

Supervision is a Python library designed for annotating labels and drawing visualizations on images and videos, particularly focusing on object detection and keypoint estimation tasks. It provides various utilities and annotators to streamline the process of annotating and visualizing detection results.

## Installation

You can install Supervision via pip:

```bash
pip install supervision
```

Additionally, you might need to install the Ultralytics library for object detection models:

```bash
pip install ultralytics
```

## Getting Started

To start using Supervision, first, ensure you have installed the necessary dependencies. Then, you can import the library and utilize its functionalities. Here's a basic example of how to annotate keypoints on a video:

```python
from supervision import get_video_frames_generator, VideoSink, KeyPoints, EdgeAnnotator, VertexLabelAnnotator, Color
from ultralytics import YOLO

# Initialize YOLO model for object detection
model = YOLO('yolov5s')

# Define video file path
SOURCE_VIDEO_FILE = "your_video_path.mp4"

# Initialize video frame generator
frame_generator = get_video_frames_generator(source_path=SOURCE_VIDEO_FILE, start=60, end=8 * 60)

# Initialize annotators
edge_annotator = EdgeAnnotator(color=Color.WHITE, thickness=2)
vertex_label_annotator = VertexLabelAnnotator(border_radius=5, text_color=Color.BLACK)

# Initialize video sink
video_info = VideoInfo.from_video_path(SOURCE_VIDEO_FILE)
with VideoSink("result.mp4", video_info=video_info) as sink:
    for frame in frame_generator:
        # Perform object detection
        result = model(frame, verbose=False)[0]
        keypoints = KeyPoints.from_ultralytics(result)

        # Annotate frames with detected keypoints
        annotated_frame = frame.copy()
        annotated_frame = edge_annotator.annotate(annotated_frame, keypoints)
        annotated_frame = vertex_label_annotator.annotate(annotated_frame, keypoints)

        # Write annotated frame to video
        sink.write_frame(annotated_frame)
```

## Features

- **Annotation Utilities**: Provides utilities for annotating labels, drawing bounding boxes, and labeling keypoints on images and videos.
- **Customization**: Offers customization options for annotator styles, including colors, text scales, and border radii.
- **Integration**: Seamlessly integrates with popular object detection frameworks like Ultralytics' YOLO for detecting objects and keypoints.


## Contribution

Contributions to Supervision are welcome! If you have any ideas for improvements, feature requests, or find any bugs, feel free to open an issue or submit a pull request.

