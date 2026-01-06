## Arrhythmia Classification with LSTM

This repository contains a Raspberry Pi–ready pipeline for detecting arrhythmias from MAX30102 PPG data. The included inference script performs real-time signal filtering, beat detection, and an LSTM-based risk assessment with live matplotlib visualization. A Colab-friendly notebook is provided for training or retraining the model.

### Project Layout
- `main.py`: Real-time acquisition, preprocessing, peak detection, RR interval logic, and inference.
- `ppg_lib/`: Minimal MAX30102 driver used by the runtime script.
- `lstm_arrhythmia_model.h5`: Pretrained model used by `main.py`.
- `data/`: Example raw sessions and processed training features.
- `Training.ipynb`: Notebook for data prep and model training/evaluation.

### Requirements
Runtime (`main.py`) depends on TensorFlow, NumPy, SciPy, matplotlib, and smbus2 for I2C access. The training notebook additionally uses scikit-learn and wfdb. All required packages are listed in `requirements.txt`:
```
numpy>=1.21.0
tensorflow>=2.8.0
scipy>=1.7.0
matplotlib>=3.5.0
smbus2>=0.4.0
scikit-learn>=1.0.0
wfdb>=4.1.0
```

### Setup
1. Create and activate a virtual environment (recommended).
2. Install dependencies:
   ```
   pip install -r requirements.txt
   ```
3. Ensure I2C is enabled on the Raspberry Pi and the MAX30102 sensor is connected at address `0x57`.

### Running Real-Time Monitoring
1. Place `lstm_arrhythmia_model.h5` in the project root (already included).
2. Connect the MAX30102 sensor.
3. Start the monitor:
   ```
   python main.py
   ```
The script opens a live plot showing normalized PPG, RR intervals, heart rate, and status messages (locked/uncertain/resync and risk estimates).

### Training or Retraining
Use `Training.ipynb` (e.g., in Google Colab) to load data, train the LSTM, and export a new `.h5` model. The notebook imports `wfdb` for reading waveform data and `scikit-learn` for splitting, resampling, and reporting metrics. Replace the model file in the root to deploy updated weights.

### Data Notes
- `data/raw_sessions/`: Sample sensor captures.
- `data/processed/training_features.csv`: Example feature set for training.
Large or sensitive datasets should be stored outside version control.

### Troubleshooting
- If the sensor is not detected, verify I2C wiring and that `smbus2` can open the bus (`/dev/i2c-*`).
- If the GUI window does not update, ensure X forwarding/display is available or run within a desktop session on the Pi.

