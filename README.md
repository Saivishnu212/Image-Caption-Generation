import streamlit as st
import google.generativeai as genai
from PIL import Image
import io

st.set_page_config(layout="wide", page_title="Gemini Image Captioner")

def generate_caption(api_key, image, style):
    """Generates a caption for the image using the Gemini API."""
    try:
        genai.configure(api_key=api_key)
    except Exception as e:
        return f"Error configuring API: {e}"

    model = genai.GenerativeModel('gemini-2.5-pro')

    prompt_instructions = f"""
    You are an assistant integrated into a Streamlit app. Your role is to generate a single, descriptive caption for the uploaded image.
    The caption must be accurate, concise, and grounded in the visible content of the image.

    Guidelines:
    - Output only one caption sentence, 8–18 words.
    - Mention salient objects, attributes, and actions.
    - Adapt the tone to be '{style}' while remaining factual.
    - Avoid hallucinations, brand names, or subjective judgments.
    - If the image is unclear, respond with: "Unclear image; unable to generate a reliable caption."
    - Do not include any extra commentary, lists, hashtags, or emojis.
    """

    try:
        response = model.generate_content([prompt_instructions, image])
        return response.text.strip()
    except Exception as e:
        return f"An error occurred during caption generation: {e}"

# --- Streamlit App UI ---
st.title("🖼️ Gemini Image Caption Generator")
st.write("Upload an image and select a style to generate a descriptive caption using Google's Gemini 2.5 Pro.")

# API Key Input in Sidebar
with st.sidebar:
    st.header("Configuration")
    google_api_key = st.text_input("Enter your Google Gemini API Key", type="password")
    st.markdown("Get your API key [here](https://aistudio.google.com/app/apikey).")

# Main content area
col1, col2 = st.columns(2)

with col1:
    st.header("Upload Image")
    uploaded_file = st.file_uploader("Choose an image...", type=["jpg", "jpeg", "png"])

    if uploaded_file is not None:
        try:
            image = Image.open(uploaded_file)
            st.image(image, caption='Uploaded Image.', use_column_width=True)
        except Exception as e:
            st.error(f"Error opening image file: {e}")
            uploaded_file = None # Reset to prevent further processing

with col2:
    st.header("Generate Caption")
    caption_style = st.selectbox(
        'Select a caption style:',
        ('Neutral', 'Poetic', 'Product', 'Alt-Text')
    )

    generate_button = st.button("Generate Caption", type="primary")

    if generate_button:
        if not google_api_key:
            st.error("Please enter your Google Gemini API Key in the sidebar.")
        elif uploaded_file is None:
            st.warning("Please upload an image first.")
        else:
            with st.spinner('Generating caption...'):
                # The image object is already a PIL Image from the check above
                caption = generate_caption(google_api_key, image, caption_style)
                st.subheader("Generated Caption")
                st.markdown(f"> {caption}")
