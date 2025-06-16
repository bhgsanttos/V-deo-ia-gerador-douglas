import openai
import moviepy.editor as mp
from gtts import gTTS
from PIL import Image
import requests
from io import BytesIO
import os

# CONFIGURAÇÕES
openai.api_key = "SUA_API_KEY_OPENAI"
idioma = "pt"
tema = "O que acontece se cair num buraco negro?"  # Altere aqui
tipo = "curiosidade"  # ou "asmr"

def gerar_roteiro(tema):
    prompt = f"Crie um roteiro curto para um vídeo viral no Instagram sobre: {tema}. Linguagem informal, instigante e com suspense."
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}],
        temperature=0.8,
        max_tokens=300
    )
    return response['choices'][0]['message']['content']

def baixar_imagem(tema, caminho="assets/fundo.jpg"):
    url = "https://source.unsplash.com/1080x1920/?" + tema.replace(" ", ",")
    response = requests.get(url)
    img = Image.open(BytesIO(response.content))
    img.save(caminho)

def gerar_audio(texto, caminho="outputs/audio.mp3"):
    tts = gTTS(texto, lang=idioma)
    tts.save(caminho)

def criar_video(roteiro, imagem="assets/fundo.jpg", audio_path="outputs/audio.mp3", saida="outputs/video_final.mp4"):
    audio = mp.AudioFileClip(audio_path)
    imagem_fundo = mp.ImageClip(imagem).set_duration(audio.duration).resize((1080, 1920)).set_audio(audio)
    txt_clip = mp.TextClip(roteiro, fontsize=50, color='white', size=(1000, None), method='caption')
    txt_clip = txt_clip.set_position('center').set_duration(audio.duration)
    video_final = mp.CompositeVideoClip([imagem_fundo, txt_clip])
    video_final.write_videofile(saida, fps=24)

if __name__ == "__main__":
    roteiro = gerar_roteiro(tema)
    baixar_imagem(tema)
    gerar_audio(roteiro)
    criar_video(roteiro)
    print("✅ Vídeo gerado com sucesso! Confira na pasta 'outputs'.")
