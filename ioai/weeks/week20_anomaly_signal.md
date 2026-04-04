# Week 20 — Anomaly Detection & Signal Processing
## Phase 3: Advanced Topics + Competition Hardening (Week 20 of 30)

**IOAI relevance**: IOAI 2025 Day 1 Task 1 "Radar" was a signal/anomaly detection task.

---

## Theory (4h)

- **Anomaly detection**: Isolation Forest, Local Outlier Factor, Autoencoder-based
- **One-class classification** vs. binary classification for anomaly tasks
- **Signal processing basics**: Fourier transform, spectrogram, wavelet transform

---

## Anomaly Detection Approaches
```python
from sklearn.ensemble import IsolationForest
from sklearn.neighbors import LocalOutlierFactor

# Isolation Forest (fast, good for high-dimensional data)
iso = IsolationForest(contamination=0.05, random_state=42)
pred = iso.fit_predict(X)   # -1 = anomaly, 1 = normal

# Local Outlier Factor
lof = LocalOutlierFactor(n_neighbors=20, contamination=0.05)
pred = lof.fit_predict(X)

# Autoencoder-based (learns normal distribution, flags high reconstruction error)
class AnomalyAE(nn.Module):
    def __init__(self, in_dim, latent=32):
        super().__init__()
        self.enc = nn.Sequential(nn.Linear(in_dim, 64), nn.ReLU(), nn.Linear(64, latent))
        self.dec = nn.Sequential(nn.Linear(latent, 64), nn.ReLU(), nn.Linear(64, in_dim))
    def forward(self, x):
        z = self.enc(x)
        return self.dec(z)

# Train only on normal data; anomalies will have high reconstruction error
recon_error = ((model(X) - X)**2).mean(dim=1)
threshold = np.percentile(recon_error.detach().numpy(), 95)
anomalies = recon_error > threshold
```

## Spectrogram for audio/signal classification
```python
import librosa
import librosa.display

# Load audio
y, sr = librosa.load('audio.wav', sr=22050)

# Mel spectrogram (frequency × time → image)
mel_spec = librosa.feature.melspectrogram(y=y, sr=sr, n_mels=128)
log_mel = librosa.power_to_db(mel_spec, ref=np.max)

# Feed into CNN as 2D "image"
# shape: (1, 128, time_frames) → standard image CNN pipeline
```

---

## Practice (8h)

- Implement Isolation Forest and autoencoder-based anomaly detection; compare on same dataset
- Practice [IOAI 2025 "Radar" task](https://github.com/IOAI-official/IOAI-2025)
- Build a classifier on audio spectrograms (use RAVDESS or UrbanSound dataset)

---

## Deliverable

`week20_anomaly_signal.ipynb` — IF vs. AE comparison + mel spectrogram audio classifier

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 3: Advanced Topics*
