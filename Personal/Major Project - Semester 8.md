
*Resource*
Medium - https://medium.com/axinc-ai/lipgan-machine-learning-model-for-generating-lip-sync-videos-d04884061cfa
LIP-GAN model is predecessor of Wav2Lip
Wav2Lip - https://github.com/snehitvaddi/Deepfake-using-Wave2Lip

**What we have done..**
- Uses <span style="color:rgb(102, 207, 255)">AssemblyAI</span> to transcribe speech from the provided video.
- `transcript.text` contains the transcribed English text.
- Uses <span style="color:rgb(102, 207, 255)">GoogleTranslator</span> to translate the English transcription into Spanish.
- Initializes a TTS engine using <span style="color:rgb(102, 207, 255)">pyttsx3</span> (offline, platform-independent).
- Converts the translated Spanish text into speech and saves it as an MP3 file.
- Logs the speech rate, volume, and selected voice.

Eleven Labs TTS API Key - 
`sk_419fd2c3465b3e332671a4f1748a5ae1632d8871fe42740a`   _expired_
`sk_8393ff5771271956f07be461593db3362f65ddb00be66a08`
N-grok API Key - 
`2wWQxNaeADQuwQeFE0X0kZn6S0b_6U9KneM4gBVq79GeLRPwM`
Sync Labs API Key - 
`sk-rUdn4nVaROC1dBbietYLag.h0FIXOBrBxcwndbJYKdgkGbSInk5RTpa`

> [!Script] Script:
```python
from transformers import pipeline
import torch
import assemblyai as aai
from deep_translator import GoogleTranslator
import pyttsx3

# "2c73bd86bdab48a7954bd7ae15ca8a30"
video_path = "video.mp4"
aai.settings.api_key = "2c73bd86bdab48a7954bd7ae15ca8a30" # sarthak key
transcriber = aai.Transcriber()
transcript = transcriber.transcribe(video_path)

if transcript.status == aai.TranscriptStatus.error:
    print(transcript.error)
else:
    print(transcript.text) # successfully printed 

translation = GoogleTranslator(source="auto", target="es").translate(transcript.text)
print(translation)

engine = pyttsx3.init()
rate = engine.getProperty('rate')
print(rate) #200

volume = engine.getProperty('volume')
print(volume) #1.0

voice = engine.getProperty('voice')
print(voice)

audio_file = "audio.mp3"
engine.save_to_file(translation, audio_file)
engine.runAndWait()
```


### Suggested Improvements (To Make It a 5/5)

1. **Replace `pyttsx3` with High-Quality TTS**:
    - Use **ElevenLabs**, **Amazon Polly**, or **Google Cloud TTS** for more **natural, expressive voices**.
2. **Time-Aligned Subtitles or Voice Mapping**:
    - Use **timestamped transcripts** from AssemblyAI and align translated text with original speech duration.
3. **Video Editing (Superimpose Dubbing)**:
    - Use **`moviepy`** or **`ffmpeg`** to replace the audio track in the video:
        from moviepy.editor import VideoFileClip, AudioFileClip 
        video = VideoFileClip("video.mp4") 
        dubbed_audio = AudioFileClip("audio.mp3") 
        final_video = video.set_audio(dubbed_audio) 
        final_video.write_videofile("dubbed_video.mp4")
4. **Speaker Detection & Voice Assignment**:
    - Identify different speakers and assign different voices using AI speaker diarization and multi-voice TTS.
5. **GUI for Better UX**:
    - Add a simple **Tkinter or PyQt** GUI so users can upload a video and choose the target language more easily.
6. **Multi-language Support**:
    - Allow users to select any language for dubbing with dynamic language codes and localized TTS voices.