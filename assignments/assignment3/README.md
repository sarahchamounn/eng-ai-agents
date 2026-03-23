Sarah Chamoun
Assignment 3
Date of Submission - 3/22

Project Overview

In this project, I built a system to detect and track a drone in videos. The goal is to follow the drone across frames, even when it is not detected for a short time.

I used a YOLO model (yolo11n.pt) to detect the drone in each frame. The model gives a bounding box, a confidence score, and the center position of the drone. For each frame, I used the detection with the highest confidence.

To improve tracking, I used a Kalman filter from the filterpy library. The state is defined as [x, y, vx, vy], where (x, y) is the position of the drone and (vx, vy) is its velocity. For each frame, the filter first predicts where the drone will be, then updates the position using the detection if one is available.

If the detector does not find the drone, the Kalman filter continues predicting the position for a few frames. This is controlled using a missed_frames counter. This allows the tracker to keep following the drone even if it disappears briefly. If too many frames are missed, the tracker resets and waits for a new detection.

For each input video, one output video is created. The output videos include only the frames where the drone is detected or actively tracked. Each frame shows a bounding box, a center point, and a trajectory line that shows the path of the drone. OpenCV is used to process and draw on the frames, and ffmpeg is used to create the final video files.

The detection results are also saved as a dataset in Parquet format. The dataset includes the video name, frame number, bounding box coordinates, center position, confidence score, and the number of missed frames. This dataset is uploaded to Hugging Face for sharing and reuse.

Links

YouTube Video 1: https://www.youtube.com/watch?v=Y3shHgQSkWQ
YouTube Video 2: https://www.youtube.com/watch?v=FveZUMc3n0Q
Hugging Face Dataset: https://huggingface.co/datasets/sarahchamoun/eng-ai-agents/blob/main/detections%20(1).parquet 
