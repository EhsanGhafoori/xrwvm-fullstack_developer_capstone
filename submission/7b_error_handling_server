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
