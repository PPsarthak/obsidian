#project #placement-preparation 
**Basic Intro about the Project**
I worked on this project from Feb.2024 - April 2024 in 6th Semester 
This was a Course Project based on "GenAI and OpenCV" course of our semester
This project can be used to analyze the *presenter's confidence and communication* during a seminar. 

**Flow Diagram**
![[CV CP Flow Diagram.png|850]]

**Parameter used for analysis -** 
Face tracking
Eye Contact
Stutter Detection
Pause Detection
Grammatical Mistakes
Body language - through hand gestures

**Constraints of Video**
- plain background 
- only 1 face, not more than that
- well-lit background and foreground
- clear audio, no background noise
- good portion of body visible - till waist??
- 30 fps of video

**Facial Feature Extraction**
The first part is facial feature extraction - we used *OpenCV* to detect face in the given video (*we could have used haarcascade as well*). The goal is to identify face from the video. Now we whether the presenter's face is completely visible throughout the video
For eye detection, we have used *dlib* which is used to detect important facial landmarks of a face as below - 
![[DLIB Facial Landmarks.png]]
We analyzed the key points in and around the eye to monitor eye detection. We believed that monitoring eye blinking is important to analyze the presenter's skill. A very high count of eye blinking and a very low count (almost like a stare) can be indication of poor presentation

**Audio Analysis**
Next up, we analyzed the audio related features of the video like stutters and pauses in the speech of video
For this, we converted the video to audio, for which we used *ffmpeg* at bitrate: 160k, sampling rate: 44100 Hz
To analyze stutters and pauses, we used *librosa* and *MFCC*

**Speech-to-Text**
To analyze grammar, it is important to convert the speech to text - for this we explored a wide range of independent APIs like *OpenAI Whisper, IBM Watson, Assembly AI*
After extracting text, we used *python language tool* to analyze grammar of the presenter

**Hand movement detection**
We are using hand-movement detection to analyze the body language, particularly *Mediapipe* which identifies a region of interest and tracks the hand movement
![[CV CP Hand movement detection.png]]

**Limitations**
it is acknowledged that there is no definitive or perfect benchmark against which presentations can be evaluated, rendering all generated results inherently subjective.
Nonetheless, we believe we have achieved a substantial level of reliability in assessment process

**Future**
- potential for live video analysis, 
- incorporation of built-in noise cancellation, 
- and integration of additional supportive features