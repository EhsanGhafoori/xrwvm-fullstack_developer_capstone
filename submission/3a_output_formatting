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
