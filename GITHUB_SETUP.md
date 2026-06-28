# GitHub Repository Setup

The assignment requires **public GitHub URLs** for Questions 1 and 6.

## Step 1: Create a public repository on GitHub

1. Go to https://github.com/new
2. Repository name: `emotion-detector-final` (or any name)
3. Set visibility to **Public**
4. Do NOT initialize with README (we already have one)
5. Click **Create repository**

## Step 2: Push this project

Run these commands in PowerShell (replace YOUR_USERNAME):

```powershell
cd "d:\Documents\Projects\courses\17.04\test1\emotion-detector-final"
git remote remove origin 2>$null
git remote add origin https://github.com/YOUR_USERNAME/emotion-detector-final.git
git add .
git commit -m "Emotion Detector final project submission"
git branch -M main
git push -u origin main
```

## Step 3: Use these URLs in Coursera

**Question 1 (README.md URL):**
```
https://github.com/YOUR_USERNAME/emotion-detector-final/blob/main/README.md
```

**Question 6 (__init__.py URL):**
```
https://github.com/YOUR_USERNAME/emotion-detector-final/blob/main/EmotionDetection/__init__.py
```

Replace `YOUR_USERNAME` with your actual GitHub username.

## Important note

The Watson NLP API (`sn-watson-emotion.labs.skills.network`) only works inside the **IBM Skills Network Cloud IDE**. If you need to re-run live tests or capture fresh terminal output, open the project in the Coursera lab environment and run:

```bash
python3.11 -m unittest test_emotion_detection.py -v
python3.11 -m pylint server.py
```
