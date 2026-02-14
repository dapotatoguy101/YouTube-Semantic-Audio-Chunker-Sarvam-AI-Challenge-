# YouTube Semantic Audio Chunker (Sarvam AI Challenge)

This repository contains my solution for the **Sarvam AI Speech Team Hiring Challenge 2024**. 

The project is a robust pipeline that extracts audio from YouTube videos, transcribes it using OpenAI's Whisper model, and segments the text into semantically coherent chunks. Crucially, it enforces a strict time constraint (default: **15 seconds**) per chunk while maintaining sentence integrity.

##  Technical Approach

The core logic resides in the `RobustVideoChunker` class. The pipeline follows these steps:

1.  **Audio Extraction:** Downloads audio at 192kbps MP3 using `yt-dlp`.
2.  **Transcription:** Generates a transcript with specific start/end times for every single word.
3.  **Alignment (Sentence Mapping):**
    * The raw text is tokenized into sentences using NLTK.
    * The algorithm maps the Whisper word timestamps to these sentences to calculate the exact duration of every sentence.
4.  **Two-Pass Chunking Algorithm:**
    * **Pass 1 (Aggregation):** Greedily groups sentences together to form a chunk. It keeps adding sentences as long as the total duration is `< 15.0 seconds`.
    * **Pass 2 (Oversized Handling):** If a single sentence is naturally longer than 15 seconds, the algorithm performs a "forced split" at the word boundary closest to the 15-second mark to ensure the constraint is met.
  
##  Features

* **Robust YouTube Download:** Utilizes `yt-dlp` with cookie support to bypass "Sign in to confirm you’re not a bot" errors.
* **High-Precision Transcription:** Uses OpenAI's **Whisper** (configurable model size) to generate word-level timestamps.
* **Semantic Segmentation:** Uses `NLTK` to identify sentence boundaries, ensuring chunks don't cut off in the middle of a thought.
* **Time Constraint Enforcement:** Implements a two-pass algorithm to ensure no chunk exceeds the 15-second limit, splitting oversized sentences dynamically if necessary.
* **Interactive UI:** Includes a **Gradio** web interface for easy testing and visualization.

##  Installation

### Prerequisites
* Python 3.8+
* CUDA-enabled GPU (Recommended for Whisper speed)

### System Dependencies
Ensure the following system packages are installed on your machine (e.g., using `apt` or `brew`):
* **ffmpeg** (Required for audio processing)
* **nodejs**

### Python Dependencies
The project requires the following Python libraries:
* `yt-dlp`
* `openai-whisper`
* `nltk`
* `torch`
* `gradio`
* `pydub`

## 📂 Usage

### 1. The Cookies File (Crucial)
YouTube recently increased bot detection. To download audio successfully, you must export your YouTube cookies:
1.  Install a "Get cookies.txt" extension for your browser.
2.  Log into YouTube.
3.  Export the cookies as `cookies.txt`.
4.  Place this file in your project directory.

### 2. Running the Code
You can run the notebook directly or extract the script. To launch the Gradio Interface:

```python
# Ensure your cookies.txt is in the path
process_video("[https://www.youtube.com/watch?v=YOUR_VIDEO_ID](https://www.youtube.com/watch?v=YOUR_VIDEO_ID)", cookies_path="cookies.txt")

