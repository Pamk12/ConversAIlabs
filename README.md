
# 🎧 Sales Call Analysis Dashboard

A **complete pipeline** for analyzing sales calls using **AI-powered transcription (Whisper), sentiment analysis (Hugging Face), and visual dashboards (Plotly/Matplotlib)**.
This project helps sales teams extract insights from call recordings in **under 30 seconds** on **Google Colab (Free Tier)**.

---

## 🚀 Features

1. **Talk-Time Ratio** → % of time each speaker spoke.
2. **Number of Questions Asked** → Helps assess sales rep engagement.
3. **Longest Monologue Duration** → Detects dominance in conversation.
4. **Call Sentiment** → Overall call tone (Positive / Neutral / Negative).
5. **One Actionable Insight** → Practical suggestion for the sales rep.
6. **Dashboard** → Interactive charts & executive-friendly insights.

---

## ⚙️ Tech Stack

* **Speech-to-Text** → [OpenAI Whisper](https://github.com/openai/whisper) (tiny model for speed).
* **Sentiment Analysis** → Hugging Face `transformers` (`distilbert-base-uncased-finetuned-sst-2-english`).
* **Visualization** → Plotly (interactive pie, gauge charts).
* **Environment** → Google Colab (free tier, <30 sec runtime).
* **Audio Conversion** → `ffmpeg-python`.

---

## 📂 Project Workflow

### 1. Install Dependencies

```bash
!pip install -q openai-whisper transformers torch tqdm ffmpeg-python plotly
```

### 2. Load & Convert Audio

* Upload your call recording (MP3, M4A, WAV).
* Convert to **mono 16kHz WAV** for Whisper:

```python
import ffmpeg
input_file = '/content/Sales Call.mp3'
output_file = 'call.wav'
(
    ffmpeg.input(input_file)
          .output(output_file, ar=16000, ac=1)
          .overwrite_output()
          .run(quiet=True)
)
```

### 3. Transcribe Call

```python
import whisper
whisper_model = whisper.load_model("tiny")  # fast & free
raw_result = whisper_model.transcribe("call.wav", verbose=False)
segments = raw_result["segments"]
text = raw_result["text"]
```

* **Segments** contain timestamps → useful for talk-time & monologue analysis.
* **Text** → used for sentiment & insights.

### 4. Assign Speakers

(Simplified heuristic: alternate speakers)

```python
for i, seg in enumerate(segments):
    seg["speaker"] = "sales_rep" if i % 2 == 0 else "customer"
```

### 5. Analysis Functions

* **Talk-Time Ratio** (pie chart)
* **Questions Count** (count `?`)
* **Longest Monologue** (max segment length)
* **Sentiment** (via Hugging Face)
* **Actionable Insight** (simple rules)

### 6. Dashboard (Sexy Visuals)

* 🥧 **Donut Pie Chart** → Talk-Time
* ❓ **Gauge Chart** → Questions
* 🎤 **Monologue Highlight** → Longest duration
* 💡 **Sentiment Gauge** → Positive/Neutral/Negative
* 📌 **Actionable Insight** → Markdown summary

---

## 📊 Example Output

* **Talk-Time Ratio:**

  * Sales Rep: 65%
  * Customer: 35%
* **Questions Asked:** 4
* **Longest Monologue:** 1 min 12s (Sales Rep)
* **Call Sentiment:** Positive ✅
* **Insight:** Encourage the sales rep to ask more open-ended questions.

---

## 🖼️ Dashboard Preview

* **Donut chart** for speaker balance.
* **Gauge chart** for questions asked.
* **Color-coded sentiment gauge.**
* **Executive summary block for actionable insight.**

---

## 🔮 Future Improvements

* Real speaker diarization (not just alternating).
* Topic detection (pricing, timeline, objections).
* Multiple call comparison (rep performance trends).
* Export insights as **PDF report**.

---

## 🏁 How to Run

1. Open [Google Colab](https://colab.research.google.com/).
2. Copy-paste the notebook cells from this repo.
3. Upload your `.mp3` or `.wav` file to `/content/`.
4. Run the notebook.
5. Enjoy your **sales dashboard** 🚀

