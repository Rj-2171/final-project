"""
This module implements a Flask API for the Emotion Detection application,
providing an endpoint to analyze the emotions expressed in a given text.
"""

from flask import Flask, render_template, request
from EmotionDetection.emotion_detection import emotion_detector

app = Flask("Emotion Detector")

@app.route("/emotionDetector")
def emot_detector():
    """
    This function receives text from the client and passes it to the
    emotion_detector() function to get the emotion analysis response.
    """
    text_to_analyze = request.args.get('textToAnalyze')
    response = emotion_detector(text_to_analyze)

    if response['dominant_emotion'] is None:
        return "Invalid text! Please try again!"

    return "For the given statement, the system response is 'anger': " + \
        str(response['anger']) + ", 'disgust': " + str(response['disgust']) + \
        ", 'fear': " + str(response['fear']) + ", 'joy': " + str(response['joy']) + \
        " and 'sadness': " + str(response['sadness']) + \
        ". The dominant emotion is " + response['dominant_emotion'] + "."

@app.route("/")
def render_index_page():
    """
    This function initiates the rendering of the main application
    page over the Flask channel.
    """
    return render_template('index.html')

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)