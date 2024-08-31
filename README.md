# AudioTranscribe-Windows
A comprehensive guide and toolset for capturing and transcribing live audio on Windows using FFmpeg and Whisper by OpenAI. This repository provides step-by-step instructions to set up the environment, configure Voicemeeter Banana for audio routing.

# Audio Capture and Transcription Using FFmpeg and Whisper on Windows

This repository provides a step-by-step guide to setting up a system for capturing audio from your computer and transcribing it using FFmpeg and the Whisper model from Hugging Face Transformers. This setup is particularly useful for recording and transcribing audio from online courses or videos.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## Prerequisites

Before you begin, ensure you have the following installed on your Windows machine:

- [Python 3.9](https://www.python.org/downloads/release/python-390/)
- [FFmpeg](https://ffmpeg.org/download.html)
- [VB-Audio Virtual Cable](https://vb-audio.com/Cable/)

## Installation

1. **Install Python 3.9:**
   - Download and install Python 3.9 from the official website.
   - Ensure to check the box "Add Python to PATH" during installation.

2. **Install FFmpeg:**
   - Download FFmpeg from the official website.
   - Extract the files and add the `bin` directory to your system's PATH.

3. **Install VB-Audio Virtual Cable:**
   - Download and install VB-Audio Virtual Cable.
   - This will allow you to route audio output from your applications to be captured by FFmpeg.

4. **Install Required Python Packages:**
   - Open a Command Prompt or PowerShell and run the following commands:
     ```bash
     pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
     pip install transformers
     ```

## Configuration

1. **Configure VB-Audio Virtual Cable:**
   - Set `VB-Audio Virtual Cable` as the output device in the application from which you want to capture audio.
   - Open Voicemeeter Banana, set `A1` to your speaker output and `B1` to `CABLE Input (VB-Audio Virtual Cable)`.

2. **Create the Script:**
   - In your `Documents` folder, create a file named `whisper_hf.py` with the following content:
     ```python
     from transformers import pipeline

     # Set up the transcription pipeline with the Whisper model
     transcriber = pipeline("automatic-speech-recognition", model="openai/whisper-medium")

     # Transcribe the audio file
     result = transcriber("output.wav")

     # Save the transcription to a text file
     with open("transcricao.txt", "w", encoding="utf-8") as f:
         f.write(result["text"])
     ```

3. **Adjust Buffer Size for FFmpeg:**
   - To avoid dropped frames during audio capture, increase the buffer size in FFmpeg using the `-rtbufsize` option.

## Usage

1. **Navigate to Your Working Directory:**
   - Open Command Prompt or PowerShell and navigate to your `Documents` folder. Replace `YourUsername` with your actual Windows username:
     ```bash
     cd C:\Users\YourUsername\Documents
     ```

2. **Capture the Audio:**
   - Run the following command to capture the audio for up to 1 hour:
     ```bash
     ffmpeg -f dshow -i audio="CABLE Output (VB-Audio Virtual Cable)" -t 3600 -ac 1 -ar 16000 -rtbufsize 512M output.wav
     ```

3. **Transcribe the Audio:**
   - After capturing the audio, transcribe it by running:
     ```bash
     C:\Users\YourUsername\AppData\Local\Programs\Python\Python39\python.exe whisper_hf.py
     ```
   - The transcription will be saved in `transcricao.txt` in the same directory.

## Troubleshooting

- **Audio Not Capturing:**
  - Ensure `VB-Audio Virtual Cable` is set as the output device for the application.
  - Check if Voicemeeter Banana is configured correctly.

- **Low-Quality Transcriptions:**
  - Ensure the audio quality is high enough (use a sampling rate of at least 24000 Hz).
  - Consider using a higher-quality Whisper model, but note that this may require more resources.

- **Buffer Overflows in FFmpeg:**
  - Increase the buffer size further using the `-rtbufsize` option in FFmpeg if you encounter frame drops.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request with your improvements.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
