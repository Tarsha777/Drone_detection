# 🚁 Drone-Based Object Detection with YOLOv8  

This project uses **YOLOv8** to detect and classify objects (vehicles, buildings, garbage, flooded areas) in **aerial drone images**.  

---

## 📦 Installation  

```bash
# Clone repository
git clone https://github.com/<your_username>/<your_repo_name>.git
cd <your_repo_name>

# Install dependencies
pip install ultralytics roboflow opencv-python

```
## datset example:
```bash
path: dataset
train: train/images
val: val/images
test: test/images

names:
  0: vehicle
  1: building
  2: garbage
  3: flooded_area
```
📈 Current Results
```bash
Precision: 0.0030
Recall:    0.0099
mAP@0.5:   0.0017
mAP@0.5:0.95: 0.0005
```

⚠️ Current performance is low due to small dataset (90 images).

Planned improvements: more data, augmentation, larger YOLO models.
