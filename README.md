# Image-Caption-Generation
 Use image and text datasets to develop a neural model that generates descriptive  captions for images.
# Gemini Image Caption Generator (Gemini 2.5 Pro)
A Streamlit app that captions images using Google's Gemini 2.5 Pro.

- **Features**: Upload image, choose style (Neutral, Poetic, Product, Alt-Text), single concise caption.
- **Requirements**: Windows, Python 3.13+, Google AI Studio API key.
- **Get API key**: https://aistudio.google.com/app/apikey

## Setup
```
py -3.13 -m venv .\.venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

## Run
```
.\.venv\Scripts\python.exe -m streamlit run app.py --server.port=8502
```
Then open http://localhost:8502

## Use
- Enter API key in the sidebar.
- Upload an image (jpg/png).
- Select style and click Generate.

## Notes
- Model: `gemini-2.5-pro` via `google-generativeai`.
- No extra scripts; uses local venv at `.\.venv`.
