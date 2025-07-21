# 📚 AI Story Generator, Translator & Narrator (All-in-One)

This is a **Streamlit app** that:
- Generates AI-based stories using GPT-2 🤖
- Translates them into Indian languages 🇮🇳
- Converts them into audio using gTTS 🔊
- Lets you download the story as a PDF 📄
- Runs locally or online via ngrok 🌐

---

## 💻 Full Code (`app.py`)

```python
import streamlit as st
from transformers import pipeline, set_seed
from gtts import gTTS
from fpdf import FPDF
from io import BytesIO

# Title
st.title("📚 AI Story Generator, Translator & Narrator")

# Story Generator
st.subheader("📝 Enter Prompt for Story")
prompt = st.text_input("Enter starting text (e.g., 'Once upon a time')")

# Select language
language = st.selectbox("🌐 Choose language", ["English", "Hindi", "Telugu", "Tamil", "Kannada", "Malayalam", "Marathi"])

# Generate Story
if st.button("✍️ Generate Story"):
    generator = pipeline('text-generation', model='gpt2')
    set_seed(42)
    story = generator(prompt, max_length=200, num_return_sequences=1)[0]['generated_text']
    st.success("Generated Story:")
    st.write(story)

    # Translate
    translations = {
        "Hindi": "hi",
        "Telugu": "te",
        "Tamil": "ta",
        "Kannada": "kn",
        "Malayalam": "ml",
        "Marathi": "mr"
    }

    if language != "English":
        from googletrans import Translator
        translator = Translator()
        translated = translator.translate(story, dest=translations[language])
        story = translated.text
        st.info("Translated Story:")
        st.write(story)

    # Audio
    if st.button("🔊 Speak Story"):
        tts = gTTS(text=story, lang='en' if language == "English" else translations[language])
        audio_bytes = BytesIO()
        tts.write_to_fp(audio_bytes)
        st.audio(audio_bytes.getvalue(), format="audio/mp3")

    # PDF
    pdf = FPDF()
    pdf.add_page()
    pdf.set_font("Arial", size=12)
    for line in story.split('\n'):
        pdf.multi_cell(0, 10, line)
    pdf_buffer = BytesIO()
    pdf.output(pdf_buffer)
    pdf_buffer.seek(0)
    st.download_button("📄 Download PDF", pdf_buffer, file_name="story.pdf")





<img src="img_girl.jpg" alt="Girl in a jacket" width="500" height="600">
