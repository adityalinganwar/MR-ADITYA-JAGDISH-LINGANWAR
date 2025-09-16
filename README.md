from fastapi import FastAPI, File, UploadFile
import shutil, os
from vai.frame_extractor import extract_frames
from vai.image_detector import analyze_frame

app = FastAPI(title="VAI Tool API")

@app.post("/analyze")
async def analyze_video(file: UploadFile = File(...)):
    # Save uploaded video
    upload_path = f"temp_{file.filename}"
    with open(upload_path, "wb") as buffer:
        shutil.copyfileobj(file.file, buffer)
    
    # Extract frames
    frames = extract_frames(upload_path)
    
    # Analyze frames
    results = []
    for f in frames:
        score = analyze_frame(f)
        results.append({"frame": f, "score": score})
    
    # Cleanup
    os.remove(upload_path)
    
    # Return results
    return {"video": file.filename, "frame_analysis": results}

import cv2, os

def extract_frames(video_path, rate=1):
    """
    Extract frames every `rate` seconds
    """
    vidcap = cv2.VideoCapture(video_path)
    frames = []
    count = 0
    success, image = vidcap.read()
    fps = int(vidcap.get(cv2.CAP_PROP_FPS))
    while success:
        if count % (fps*rate) == 0:
            frame_file = f"frame_{count}.jpg"
            cv2.imwrite(frame_file, image)
            frames.append(frame_file)
        success, image = vidcap.read()
        count += 1
    vidcap.release()
    return frames
# placeholder for real CNN + ELA code
def analyze_frame(frame_path):
    """
    Returns fake_score (0=real, 1=fake)
    """
    # integrate FakeImageDetector's analyze function here
    # for now random placeholder
    import random
    return round(random.uniform(0, 1), 2)
fastapi
uvicorn
opencv-python
numpy
Pillow
notebook
torch
tensorflow
cd path/to/Vai-Tool
git init
git add .
git commit -m "Initial commit — VAI Tool starter repo"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/Vai-Tool.git
git push -u origin main
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
uvicorn vai.api:app --reload
