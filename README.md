# 1-Word SRT Generator

1-Word SRT Generator is a simple desktop application that converts MP3, WAV, or M4A audio files into SRT subtitle files with one word per timestamp. Built with Python, Whisper, SRT, and Tkinter for demonstration and learning purposes only — not intended for production use.

## Features
- Convert audio files to word-level SRT subtitles  
- Supports MP3, WAV, and M4A formats  
- GUI for selecting audio files and output folder  
- Cleans words by removing non-alphanumeric characters  
- Lightweight, runs entirely locally with no backend required  

## Installation
1. Install Python 3.x and required libraries (`whisper`, `srt`, optionally `pydub`)  
2. Download or clone the repository  
3. Run `python script_name.py`  
4. Use the interface to select audio and generate SRT  

## Notes
- Designed as a learning tool and transcription experiment  
- Accuracy depends on the Whisper model and audio quality  
- One-word-per-timestamp format is for educational and visualization purposes only  

## License
MIT License
