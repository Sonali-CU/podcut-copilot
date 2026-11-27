# podcut-copilot
Turn hour-long podcasts into viral-ready short blueprints—100% open-source multimodal AI for clip discovery, music ducking &amp; framing cues.
# 🎬 Multimodal Viral Clip Discovery & Editorial Intelligence Workbench
### *(AI Creative Director Co-Pilot)*

An open-source, locally executed creative decision-support workbench that analyzes long-form podcasts and interviews, discovers high-potential 30–60 second short-form highlights, and generates a transparent, timestamped editorial blueprint (`editorial_suggestions.csv`).

> **Project Metadata:** Final-Year BCA Capstone Project | **Author:** Sonali Gupta (`CU23BCA0060A`) | **Institution:** Chanakya University | **Guide:** Miss Vathsala Bhat

---

## ✨ Key Features

- **100% Open-Source & Local Inference:** Runs completely without paid LLM or cloud speech APIs using `faster-whisper`, `DeBERTa-v3`, `KeyBERT`, `Librosa`, and `OpenCV`.
- **Bandwidth-Saving Two-Pass Pipeline:**
  - **Pass 1:** Fetches lightweight audio only (`yt-dlp` + `FFmpeg` 16 kHz mono WAV), transcribes with Voice Activity Detection, groups sentences into coherent 30–60s windows, and ranks segments via NLP + acoustic dynamics.
  - **Pass 2:** Fetches only the top shortlisted intervals at low resolution (`<=360p`) for OpenCV framing inspection, then deletes temporary media immediately.
- **Explainable Multimodal Scoring:** Every candidate clip includes a transparent mathematical breakdown rather than a black-box score:
  \[\text{Pre-Score} = \min\left(0.60 \cdot S_{\text{sem}} + 0.25 \cdot S_{\text{ac}} + S_{\text{ling}},\, 1.0\right)\]
  \[\text{Final Score} = 0.90 \cdot \text{Pre-Score} + 0.10 \cdot S_{\text{vis}}\]
- **Editorial Mixing & Ducking Guide:** Generates a rule-based, per-second music intensity curve (`1–10`) using inverse vocal-energy ducking alongside timestamped hook, transition, and punch-in cues.
- **Privacy & Ephemeral Cleanup:** All temporary session directories and optional `cookies.txt` files are automatically purged in a `finally` block after each run.

---

## 🏗️ Repository Structure

```text
multimodal-viral-clip-workbench/
├── app.py                                              # Streamlit web interface & session lifecycle
├── src/
│   ├── __init__.py
│   ├── ingestion.py                                    # Two-pass yt-dlp retrieval & FFmpeg WAV normalization
│   ├── transcription.py                                # faster-whisper STT & sentence-boundary windowing
│   ├── analysis.py                                     # DeBERTa-v3 classification, Librosa acoustics & OpenCV framing
│   └── packaging.py                                    # Final scoring, KeyBERT keywords & CSV blueprint export
├── notebooks/
│   └── Multimodal_Viral_Clip_Discovery_Workbench.ipynb # Ready-to-run Google Colab T4 GPU notebook
├── docs/
│   └── sample_editorial_suggestions.csv                # Sample output blueprint from live evaluation
├── requirements.txt                                    # Python dependencies
├── Dockerfile                                          # Containerized deployment configuration
├── .gitignore                                          # Excludes cookies, temp media & caches
└── README.md
```

---

## 🚀 Quick Start

### Option A: Run in Google Colab (Recommended — Free T4 GPU)
1. Open `notebooks/Multimodal_Viral_Clip_Discovery_Workbench.ipynb` in [Google Colab](https://google.com).
2. Set **Runtime → Change runtime type → T4 GPU**.
3. Run all cells top-to-bottom and open the proxy link printed by the final cell.

### Option B: Run Locally
1. **Prerequisites:** Python 3.10+ and `FFmpeg` installed on your system path.
2. **Clone & Install:**
   ```bash
   git clone https://github.com<your-username>/multimodal-viral-clip-workbench.git
   cd multimodal-viral-clip-workbench
   pip install -r requirements.txt
   ```
3. **Launch Streamlit:**
   ```bash
   streamlit run app.py
   ```

---

## 📊 Sample Evaluation Output

Tested on Raj Shamani's *Figuring Out* podcast episode (*"The Neural Trick to Breaking ANY Habit in 60 Seconds"*):

| Rank | Timeline | Duration | Category | Score | Music Mood |
| :---: | :---: | :---: | :--- | :---: | :--- |
| **1** | `00:02:25 - 00:02:57` | `32.1s` | Bold Opinion | **0.872** | Tense, driving beat |
| **2** | `00:09:10 - 00:09:41` | `30.3s` | Personal Story | **0.872** | Warm, emotional piano/strings |
| **3** | `00:10:48 - 00:11:19` | `30.9s` | Counter-Intuitive Insight | **0.870** | Curious, suspenseful build |
| **4** | `00:00:00 - 00:00:31` | `30.6s` | Bold Opinion | **0.864** | Tense, driving beat |
| **5** | `00:07:25 - 00:08:01` | `36.5s` | Bold Opinion | **0.864** | Tense, driving beat |

---

## ⚠️ Known System Boundaries

- **Creative Decision-Support Only:** Ranks segments based on linguistic and acoustic cues; does not guarantee platform virality.
- **Approximate Visual Framing:** OpenCV Haar cascades detect face presence and shot geometry (single vs. two-shot), not active speaker identity.
- **Rule-Based Music Curve:** The `1–10` intensity curve is an editorial mixing guide rather than synthesized background audio.
- **Cloud IP Restrictions:** When running on shared cloud IPs (e.g., Google Colab), YouTube may require a temporary `cookies.txt` or local `.mp4`/`.mp3` upload fallback.

---

## 🛠️ Tech Stack

- **Frontend & Visualization:** Streamlit, Plotly, Pandas
- **Speech & NLP:** `faster-whisper`, Hugging Face `Transformers` (`MoritzLaurer/DeBERTa-v3-base-mnli-fever-anli`), `KeyBERT`
- **Audio & Vision:** `Librosa`, `OpenCV` (`opencv-python-headless`), `FFmpeg`, `yt-dlp`
