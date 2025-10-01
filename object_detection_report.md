
# Object Detection Model Training Report

## Model Choice

For this object detection task, YOLOv8 (specifically the YOLOv8s model) was chosen. YOLO (You Only Look Once) models are known for their speed and accuracy in real-time object detection, making them suitable for analyzing aerial drone imagery. YOLOv8 is the latest iteration in the YOLO series, offering improved performance and ease of use. Its architecture is well-suited for identifying multiple objects within images, which aligns with the goal of detecting various classes like vehicles, buildings, garbage, and flooded areas in the dataset.

## Dataset Usage and Preprocessing

The dataset used for training and evaluation was obtained from a Google Drive folder located at `/content/drive/MyDrive/flood_dataset`. The initial labeling of this dataset was performed using a Roboflow workflow ("find-garbage-vehicles-floodeds-and-buildings") via the Roboflow Inference HTTP Client. This process provided bounding box annotations and class labels for objects in each image.

After obtaining the labeling results, the dataset was split into training and testing sets with an 80% to 20% ratio using `sklearn.model_selection.train_test_split`. The image filenames and their corresponding labeling results were split to maintain the association between images and their annotations.

To prepare the data for YOLOv8 training, the images and their annotations were saved to temporary directories (`/content/yolov8_data/train/images`, `/content/yolov8_data/train/labels`, `/content/yolov8_data/valid/images`, `/content/yolov8_data/valid/labels`). The annotations, originally in a dictionary format with x, y, width, and height, were converted to the YOLO format (class_id center_x center_y width height, normalized) and saved as text files corresponding to each image. A `data.yaml` file was created and updated to point to these temporary directories and define the class names and counts.

## Training Process

The YOLOv8s model was trained using the prepared dataset. The training was configured with the following parameters:
- **Image Size (`imgsz`):** 640 pixels
- **Batch Size (`batch`):** 16
- **Epochs (`epochs`):** 50
- **Model Configuration (`cfg`):** yolov8s.yaml
- **Data Configuration (`data`):** data.yaml (pointing to the temporary training and validation data)
- **Cache:** Enabled for faster training

The training process involved iterating over the training data for 50 epochs, with the test set being used for validation at the end of each epoch to monitor performance and prevent overfitting.

## Evaluation Results

The trained model was evaluated on the testing dataset. The following evaluation metrics were obtained:
- **Precision (P):** 0.0041
- **Recall (R):** 0.0147
- **mAP@0.5:** 0.0043
- **mAP@0.5:0.95:** 0.0012

These metrics indicate that the model's performance on the testing set is very low. A low Precision suggests that when the model detects an object, it is often incorrect. A low Recall indicates that the model is missing a large percentage of the actual objects present in the images. The low mAP scores, especially mAP@0.5:0.95, confirm that the model is not effectively identifying and localizing objects across different Intersection over Union (IoU) thresholds. This poor performance could be attributed to several factors, including the small size of the dataset, the complexity of the objects and scenes in aerial imagery, or the need for more extensive hyperparameter tuning and potentially a larger model or longer training duration.

## Sample Predictions

Sample predictions were generated on a few images from the testing set using the trained model with a confidence threshold of 0.25. For the selected sample images visualized, the model did not detect any objects at this confidence level. This observation is consistent with the low evaluation metrics, suggesting that the model has low confidence in its predictions on unseen data. Further analysis with a lower confidence threshold might show some detections, but their reliability would likely be low based on the overall evaluation scores. The visualization process successfully displayed the original images alongside the annotated images, although the annotated images appeared identical to the originals due to the absence of detections.

