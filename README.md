# Real-Time Safety Monitoring System

A computer vision system for real-time workplace safety hazard detection using BLIP vision-language model with automated alert mechanisms.

![System Demo](docs/demo_screenshots/system_overview.png)

## 🎯 Overview

This project implements an end-to-end safety monitoring system that analyzes webcam feeds in real-time to detect workplace hazards such as electrical risks, trip hazards, blocked pathways, and unsafe conditions.

**Key Features:**
- Real-time webcam monitoring at 30 FPS
- Automated hazard detection and classification
- Sound alerts and visual notifications
- Auto-screenshot capture on hazard detection
- Comprehensive alert logging system
- Performance evaluation on 121-image benchmark

## 📊 Performance Metrics

**Baseline System (BLIP + Keyword Classification):**
- Overall Accuracy: 68.6%
- Hazard Detection F1: 81%
- Hazard Recall: 94% (catches 94% of real hazards)
- Safe Detection F1: 17%

**Key Insight:** The system prioritizes safety by using conservative classification - it catches 94% of hazards with an acceptable false alarm rate, which is appropriate for safety-critical applications.

## 🛠️ Tech Stack

- **Vision Model:** BLIP (Salesforce/blip-image-captioning-base)
- **Framework:** PyTorch with MPS backend (Apple Silicon optimization)
- **Computer Vision:** OpenCV
- **ML Libraries:** Transformers (Hugging Face), scikit-learn
- **Data Processing:** Pandas, NumPy
- **Visualization:** Matplotlib, Seaborn

## 📁 Project Structure

```
RealTimeSafetyMonitor/
├── notebooks/
│   ├── 01_baseline_evaluation.ipynb      # Baseline BLIP testing
│   ├── 02_data_preparation.ipynb         # Data collection & annotation
│   ├── 03_performance_evaluation.ipynb   # Metrics calculation
│   └── 04_realtime_monitoring.ipynb      # Main webcam system
│
├── data/
│   └── training/
│       ├── images/                        # 118 safety-annotated images
│       └── annotations2.csv               # Ground truth labels
│
├── results/
│   ├── confusion_matrix.png
│   ├── evaluation_report.txt
│   └── evaluation_results.csv
│
└── README.md
```

## 🚀 Quick Start

### Prerequisites

- Python 3.11+
- macOS (for alert notifications) or Linux
- Webcam access
- 8GB+ RAM recommended

### Installation

1. Clone the repository:
```bash
git clone https://github.com/YOUR_USERNAME/RealTimeSafetyMonitor.git
cd RealTimeSafetyMonitor
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Run the real-time monitoring system:
```bash
jupyter notebook notebooks/04_realtime_monitoring.ipynb
```

Then run all cells and the webcam monitor will start.

## 📖 Usage

### Real-Time Monitoring

1. Open `notebooks/04_realtime_monitoring.ipynb`
2. Run all cells sequentially
3. The webcam window will open showing:
   - Live video feed
   - Hazard detection status
   - Scene description
   - Alert statistics

**Controls:**
- `SPACE` - Force immediate caption update
- `S` - Take manual screenshot
- `A` - Toggle alerts on/off
- `Q` - Quit and save session logs

### Alert System

When a hazard is detected:
- 🔊 System sound alert plays
- 💬 Popup dialog appears (center of screen)
- 📸 Screenshot auto-saved to `alerts/` folder
- 📝 Alert logged to `logs/alert_log_TIMESTAMP.json`

### Evaluation

Run `notebooks/03_performance_evaluation.ipynb` to:
- Test system on benchmark dataset
- Calculate precision, recall, F1 scores
- Generate confusion matrix
- Analyze misclassifications

## 🎯 System Architecture

### Hazard Detection Pipeline

```
Webcam Frame → BLIP Caption Generation → Keyword Classification → Alert System
     ↓                    ↓                         ↓                  ↓
  640x480            "cables on floor"         hazard: trip      🔊 Sound
  30 FPS             ~0.3s latency            severity: high     💬 Popup
                                                                📸 Screenshot
```

### Classification Logic

The system uses keyword-based classification with the following categories:
- **Electrical:** wire, cable, cord, plug, socket, electrical, power
- **Trip:** floor, lying, scattered, clutter, mess, pile
- **Slip:** wet, water, liquid, spill, puddle
- **Fire:** fire, flame, smoke, burning, hot
- **Sharp:** glass, broken, sharp, shattered, crack
- **Blocked:** blocked, obstruct, narrow, crowded

**Conservative Approach:** Assumes potential hazard unless clear safe indicators present (prioritizes safety over precision).

## 📊 Dataset

**Training/Evaluation Set:**
- 118 workplace images
- Categories: electrical hazards, trip hazards, slip risks, blocked pathways, safe scenes
- Manual annotations with safety-focused captions
- Sources: Google Images, personal photography, COCO dataset

**Annotation Format:**
```csv
filename,scene_type,has_hazard,hazard_type,severity,caption
Google_broken_socket.jpg,office,yes,electrical,high,"burned electrical plug with charred contacts"
```

## 🔬 Experimental Results

### Confusion Matrix

|               | Predicted Hazard | Predicted Safe |
|---------------|------------------|----------------|
| Actual Hazard | 79               | 5              |
| Actual Safe   | 33               | 4              |

### Analysis

**Strengths:**
- High hazard recall (94%) - catches most real hazards
- Fast inference (~300ms per frame on M4 Mac)
- Real-time performance (30 FPS display)
- Reliable alert system with minimal latency

**Limitations:**
- High false alarm rate on safe scenes (89%)
- Generic BLIP captions lack safety-specific vocabulary
- Keyword-based classification is rule-based, not learned

## 🚧 Future Improvements

**Short-term:**
1. Collect 500-1000 safety-specific training images
2. Fine-tune BLIP on domain-specific dataset
3. Add confidence thresholding for alert sensitivity
4. Implement temporal filtering (reduce false alarms in video)

**Long-term:**
1. Train custom vision-language model with safety focus
2. Add object detection for specific hazard types (cables, spills, etc.)
3. Build web dashboard with historical analytics
4. Deploy as REST API for multi-camera monitoring
5. Add user feedback loop for continuous improvement

## 📚 Lessons Learned

### Data Quality Matters
Initial fine-tuning attempts with 118 images failed - model generated repetitive generic phrases. Discovered that:
- Small datasets (< 500 images) lead to overfitting
- Caption diversity is critical for generalization
- Baseline keyword matching can be effective starting point

### Engineering vs. ML Performance
Focused on building robust end-to-end system rather than chasing marginal metric improvements. Real-world deployment requires:
- Reliable alert mechanisms
- Fast inference speed
- User-friendly interface
- Comprehensive logging

## 🤝 Contributing

Contributions welcome! Areas for improvement:
- Expanding training dataset
- Testing on different hardware
- Adding new hazard categories
- Improving classification accuracy

## 📄 License

MIT License - see LICENSE file for details

## 👤 Author

Samia Diba
- GitHub: [ssdiba](https://github.com/ssdiba)


## 🙏 Acknowledgments

- BLIP model: Salesforce Research
- Transformers library: Hugging Face
- Dataset images: Google Images, COCO Dataset

---

**Status:** ✅ Baseline system complete | 🔄 Fine-tuning in progress | 📊 Evaluation complete
