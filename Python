from flask import Flask, request, send_file, render_template
from pytube import YouTube
from moviepy.editor import AudioFileClip
import os

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/convert', methods=['POST'])
def convert():
    url = request.form['url']
    try:
        # Download the YouTube video
        yt = YouTube(url)
        video = yt.streams.filter(only_audio=True).first()
        out_file = video.download(output_path='downloads')

        # Convert to MP3
        base, ext = os.path.splitext(out_file)
        mp3_file = base + '.mp3'
        audio_clip = AudioFileClip(out_file)
        audio_clip.write_audiofile(mp3_file)
        audio_clip.close()

        # Remove the original video file
        os.remove(out_file)

        return send_file(mp3_file, as_attachment=True)

    except Exception as e:
        return str(e)

if __name__ == '__main__':
    if not os.path.exists('downloads'):
        os.makedirs('downloads')
    app.run(debug=True)
