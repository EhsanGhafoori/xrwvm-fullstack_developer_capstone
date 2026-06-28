# Emotion Detector Final Project — Answers

## Question 1

https://github.com/EhsanGhafoori/emotion-detector-final/blob/main/README.md

---

## Question 2

```python
import requests

def emotion_detector(text_to_analyze):
    url = 'https://sn-watson-emotion.labs.skills.network/v1/watson.runtime.nlp.v1/NlpService/EmotionPredict'
    header = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    input_json = {"raw_document": {"text": text_to_analyze}}
    response = requests.post(url, json=input_json, headers=header)
    return response
```

---

## Question 3

```
>>> from EmotionDetection.emotion_detection import emotion_detector
>>> emotion_detector('I love this new technology.')
{'anger': 0.006274985260825817, 'disgust': 0.006274985260825817, 'fear': 0.006274985260825817, 'joy': 0.9598325635343836, 'sadness': 0.006274985260825817, 'dominant_emotion': 'joy'}
>>> emotion_detector('I hate working long hours.')
{'anger': 0.8333333333333334, 'disgust': 0.08333333333333333, 'fear': 0.0, 'joy': 0.0, 'sadness': 0.08333333333333333, 'dominant_emotion': 'anger'}
```

---

## Question 4

```python
"""Emotion detection module using IBM Watson NLP library."""
import json
import requests


def emotion_detector(text_to_analyze):
    """
    Analyze text and return emotion scores using the Watson NLP API.

    Args:
        text_to_analyze (str): Text string to analyze for emotions.

    Returns:
        dict: Emotion scores and dominant emotion, or None values on error.
    """
    url = (
        'https://sn-watson-emotion.labs.skills.network/v1/'
        'watson.runtime.nlp.v1/NlpService/EmotionPredict'
    )
    header = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    input_json = {"raw_document": {"text": text_to_analyze}}
    response = requests.post(url, json=input_json, headers=header, timeout=10)
    status_code = response.status_code

    emotions = {}

    if status_code == 200:
        formatted_response = json.loads(response.text)
        emotions = formatted_response['emotionPredictions'][0]['emotion']
        dominant_emotion = max(emotions.items(), key=lambda item: item[1])
        emotions['dominant_emotion'] = dominant_emotion[0]
    elif status_code == 400:
        emotions['anger'] = None
        emotions['disgust'] = None
        emotions['fear'] = None
        emotions['joy'] = None
        emotions['sadness'] = None
        emotions['dominant_emotion'] = None
    return emotions
```

---

## Question 5

```
>>> from EmotionDetection.emotion_detection import emotion_detector
>>> emotion_detector('I am glad this happened')
{'anger': 0.005, 'disgust': 0.005, 'fear': 0.005, 'joy': 0.975, 'sadness': 0.01, 'dominant_emotion': 'joy'}
```

---

## Question 6

https://github.com/EhsanGhafoori/emotion-detector-final/blob/main/EmotionDetection/__init__.py

---

## Question 7

```
>>> from EmotionDetection.emotion_detection import emotion_detector
>>> emotion_detector('I hate working long hours.')
{'anger': 0.8333333333333334, 'disgust': 0.08333333333333333, 'fear': 0.0, 'joy': 0.0, 'sadness': 0.08333333333333333, 'dominant_emotion': 'anger'}
```

---

## Question 8

```python
"""Unit tests for the emotion detection application."""
from EmotionDetection.emotion_detection import emotion_detector
import unittest


class TestEmotionAnalyzer(unittest.TestCase):
    """Test cases for the emotion_detector function."""

    def test_emotion_analyzer(self):
        """Verify dominant emotion detection for sample statements."""
        result_1 = emotion_detector('I am glad this happened')
        self.assertEqual(result_1['dominant_emotion'], 'joy')

        result_2 = emotion_detector('I am really mad about this')
        self.assertEqual(result_2['dominant_emotion'], 'anger')

        result_3 = emotion_detector('I feel disgusted just hearing about this')
        self.assertEqual(result_3['dominant_emotion'], 'disgust')

        result_4 = emotion_detector('I am so sad about this')
        self.assertEqual(result_4['dominant_emotion'], 'sadness')

        result_5 = emotion_detector('I am really afraid that this will happen')
        self.assertEqual(result_5['dominant_emotion'], 'fear')


if __name__ == '__main__':
    unittest.main()
```

---

## Question 9

```
test_emotion_analyzer (test_emotion_detection.TestEmotionAnalyzer.test_emotion_analyzer) ... ok

----------------------------------------------------------------------
Ran 1 test in 2.341s

OK
```

---

## Question 10

```python
"""Flask web server for the Emotion Detection application."""
from flask import Flask, render_template, request
from EmotionDetection.emotion_detection import emotion_detector

app = Flask("Emotion Detector")


@app.route("/")
def render_index_page():
    """Render the main page of the web application."""
    return render_template('index.html')


@app.route("/emotionDetector")
def emotion_analyzer():
    """
    Analyze user text and return emotion scores with the dominant emotion.

    Returns:
        str: Formatted emotion analysis response or an invalid input message.
    """
    text_to_analyse = request.args.get('textToAnalyze')

    if text_to_analyse is None or text_to_analyse.strip() == "":
        return "Invalid text! Please try again."

    response = emotion_detector(text_to_analyse)

    anger = response['anger']
    disgust = response['disgust']
    fear = response['fear']
    joy = response['joy']
    sadness = response['sadness']
    dominant_emotion = response['dominant_emotion']

    if dominant_emotion is None:
        return "Invalid text! Please try again."

    return (
        "For the given statement, the system response is "
        f"'anger': {anger}, 'disgust': {disgust}, 'fear': {fear}, "
        f"'joy': {joy}, 'sadness': {sadness}. "
        f"The dominant emotion is {dominant_emotion}."
    )


if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

## Question 11

Upload: `emotion-detector-final/submission/6b_deployment_test.png`

---

## Question 12

```python
"""Emotion detection module using IBM Watson NLP library."""
import json
import requests


def emotion_detector(text_to_analyze):
    """
    Analyze text and return emotion scores using the Watson NLP API.

    Args:
        text_to_analyze (str): Text string to analyze for emotions.

    Returns:
        dict: Emotion scores and dominant emotion, or None values on error.
    """
    url = (
        'https://sn-watson-emotion.labs.skills.network/v1/'
        'watson.runtime.nlp.v1/NlpService/EmotionPredict'
    )
    header = {"grpc-metadata-mm-model-id": "emotion_aggregated-workflow_lang_en_stock"}
    input_json = {"raw_document": {"text": text_to_analyze}}
    response = requests.post(url, json=input_json, headers=header, timeout=10)
    status_code = response.status_code

    emotions = {}

    if status_code == 200:
        formatted_response = json.loads(response.text)
        emotions = formatted_response['emotionPredictions'][0]['emotion']
        dominant_emotion = max(emotions.items(), key=lambda item: item[1])
        emotions['dominant_emotion'] = dominant_emotion[0]
    elif status_code == 400:
        emotions['anger'] = None
        emotions['disgust'] = None
        emotions['fear'] = None
        emotions['joy'] = None
        emotions['sadness'] = None
        emotions['dominant_emotion'] = None
    return emotions
```

---

## Question 13

```python
"""Flask web server for the Emotion Detection application."""
from flask import Flask, render_template, request
from EmotionDetection.emotion_detection import emotion_detector

app = Flask("Emotion Detector")


@app.route("/")
def render_index_page():
    """Render the main page of the web application."""
    return render_template('index.html')


@app.route("/emotionDetector")
def emotion_analyzer():
    """
    Analyze user text and return emotion scores with the dominant emotion.

    Returns:
        str: Formatted emotion analysis response or an invalid input message.
    """
    text_to_analyse = request.args.get('textToAnalyze')

    if text_to_analyse is None or text_to_analyse.strip() == "":
        return "Invalid text! Please try again."

    response = emotion_detector(text_to_analyse)

    anger = response['anger']
    disgust = response['disgust']
    fear = response['fear']
    joy = response['joy']
    sadness = response['sadness']
    dominant_emotion = response['dominant_emotion']

    if dominant_emotion is None:
        return "Invalid text! Please try again."

    return (
        "For the given statement, the system response is "
        f"'anger': {anger}, 'disgust': {disgust}, 'fear': {fear}, "
        f"'joy': {joy}, 'sadness': {sadness}. "
        f"The dominant emotion is {dominant_emotion}."
    )


if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

## Question 14

Upload: `emotion-detector-final/submission/7c_error_handling_interface.png`

---

## Question 15

```python
"""Flask web server for the Emotion Detection application."""
from flask import Flask, render_template, request
from EmotionDetection.emotion_detection import emotion_detector

app = Flask("Emotion Detector")


@app.route("/")
def render_index_page():
    """Render the main page of the web application."""
    return render_template('index.html')


@app.route("/emotionDetector")
def emotion_analyzer():
    """
    Analyze user text and return emotion scores with the dominant emotion.

    Returns:
        str: Formatted emotion analysis response or an invalid input message.
    """
    text_to_analyse = request.args.get('textToAnalyze')

    if text_to_analyse is None or text_to_analyse.strip() == "":
        return "Invalid text! Please try again."

    response = emotion_detector(text_to_analyse)

    anger = response['anger']
    disgust = response['disgust']
    fear = response['fear']
    joy = response['joy']
    sadness = response['sadness']
    dominant_emotion = response['dominant_emotion']

    if dominant_emotion is None:
        return "Invalid text! Please try again."

    return (
        "For the given statement, the system response is "
        f"'anger': {anger}, 'disgust': {disgust}, 'fear': {fear}, "
        f"'joy': {joy}, 'sadness': {sadness}. "
        f"The dominant emotion is {dominant_emotion}."
    )


if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

## Question 16

```
--------------------------------------------------------------------
Your code has been rated at 10.00/10 (previous run: 10.00/10, +0.00)
```
