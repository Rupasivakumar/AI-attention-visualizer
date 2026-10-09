# AI Attention Visualizer

An interactive Streamlit web application that takes an image containing text, extracts the text using OCR, converts it into numerical embeddings, computes attention scores between the words, and displays how strongly each word attends to every other word.

The project is designed as a learning tool to help understand the attention mechanism that powers modern transformer-based AI models.

---

## Table of Contents

1. Overview
2. Features
3. How It Works
4. Project Structure
5. Technology Stack
6. Requirements
7. Installation
8. Running the Application
9. Usage Guide
10. Module Reference
11. Troubleshooting
12. Known Limitations
13. Future Improvements
14. Author

---

## 1. Overview

Attention is the core idea behind transformer models such as the ones used in modern language models. For each word in a sentence, attention decides how much focus to place on every other word. This project makes that process visible.

The user uploads an image of text. The app reads the text, builds a numerical representation of each word, calculates attention weights, and presents the result visually so the relationships between words can be inspected.

---

## 2. Features

- Image upload through a browser interface
- Automatic text extraction from images using OCR
- Conversion of words into numerical embeddings
- Calculation of attention scores between all words
- Visual display of the attention matrix
- Runs locally with no external server required

---

## 3. How It Works

The application follows a four-stage pipeline:

1. **Image input.** The user uploads an image through the Streamlit interface. The image is opened with Pillow (PIL).
2. **Text extraction (OCR).** The `extract_text` function in `ocr.py` reads the image and returns the text it contains.
3. **Embedding.** The `create_embeddings` function in `embedding.py` converts the extracted words into numeric vectors.
4. **Attention calculation.** The `calculate_attention` function in `attention.py` compares the vectors with each other and produces an attention weight matrix. Each row shows how much one word attends to all other words, and each row sums to 1.

The core attention idea used is scaled dot-product attention:

```
scores  = (Q x K^T) / sqrt(d)
weights = softmax(scores)
```

where Q and K are the word vectors, and d is the vector dimension.

---

## 4. Project Structure

```
Ai attention visualizer/
|
|-- app.py            Main Streamlit application (user interface and pipeline control)
|-- ocr.py            Text extraction from images
|-- embedding.py      Conversion of text into numerical embeddings
|-- attention.py      Attention score calculation
|-- requirements.txt  List of Python dependencies
|-- README.md         Project documentation
```

---

## 5. Technology Stack

| Component | Purpose |
|-----------|---------|
| Python 3.12 | Programming language |
| Streamlit | Web interface |
| NumPy | Numerical computation |
| Pillow (PIL) | Image loading and handling |
| pytesseract and Tesseract OCR | Text extraction from images |

Note: add the embedding library you use in `embedding.py` here (for example, sentence-transformers or a custom approach).

---

## 6. Requirements

- Windows, macOS, or Linux
- Python 3.10 or newer (developed on Python 3.12)
- Tesseract OCR installed on the system (required for pytesseract)
- An internet connection for the first installation of packages

---

## 7. Installation

### Step 1: Get the project

Place the project folder on your computer, or clone it:

```powershell
git clone <your-repository-url>
cd "Ai attention visualizer"
```

### Step 2: Create a virtual environment (recommended)

```powershell
python -m venv venv
venv\Scripts\activate
```

On macOS or Linux:

```bash
python3 -m venv venv
source venv/bin/activate
```

### Step 3: Install the Python packages

```powershell
python -m pip install streamlit numpy pillow pytesseract
```

Or, if a `requirements.txt` file is present:

```powershell
python -m pip install -r requirements.txt
```

### Step 4: Install Tesseract OCR

On Windows, download the installer from the UB Mannheim Tesseract builds and install it. The default location is:

```
C:\Program Files\Tesseract-OCR\tesseract.exe
```

If Python cannot find Tesseract, add this line to `ocr.py` below the imports:

```python
pytesseract.pytesseract.tesseract_cmd = r"C:\Program Files\Tesseract-OCR\tesseract.exe"
```

On macOS: `brew install tesseract`
On Ubuntu or Debian: `sudo apt install tesseract-ocr`

---

## 8. Running the Application

This is a Streamlit app, so it must be started with the Streamlit command. Running `python app.py` directly will not work correctly.

From inside the project folder:

```powershell
python -m streamlit run app.py
```

The terminal will print a local address:

```
Local URL: http://localhost:8501
```

Your browser should open automatically. If it does not, copy that address into your browser. To stop the app, press `Ctrl+C` in the terminal.

---

## 9. Usage Guide

1. Start the app using the command above.
2. Upload an image that contains clear, readable text (PNG or JPG).
3. Wait for the text to be extracted and displayed.
4. View the generated attention matrix.
5. Read the matrix: a higher value between two words means the first word pays more attention to the second.

Tips for best results:

- Use high-resolution images with dark text on a light background.
- Avoid handwriting, heavy blur, or very small fonts.
- Keep the text short (one sentence to a short paragraph) so the attention matrix stays readable.

---

## 10. Module Reference

### app.py

The entry point. It imports the three processing modules, builds the Streamlit interface, accepts the uploaded image, and runs the pipeline in order: OCR, embeddings, attention, display.

### ocr.py

```python
def extract_text(image) -> str
```

Takes a PIL image and returns the text found in it as a string.

### embedding.py

```python
def create_embeddings(text) -> numpy.ndarray
```

Takes the extracted text and returns a numeric vector for each word.

### attention.py

```python
def calculate_attention(embeddings) -> numpy.ndarray
```

Takes the embeddings and returns a square matrix of attention weights between all words.

---

## 11. Troubleshooting

| Problem | Cause | Fix |
|---------|-------|-----|
| `ImportError: cannot import name 'extract_text' from 'ocr'` | `ocr.py` does not define a function named exactly `extract_text`, or the file was not saved | Open `ocr.py`, make sure it contains `def extract_text(...)`, save with Ctrl+S, and run again |
| `No module named streamlit` | Streamlit is not installed | `python -m pip install streamlit` |
| `TesseractNotFoundError` | Tesseract program is not installed or not on the path | Install Tesseract and set `tesseract_cmd` in `ocr.py` |
| App prints nothing when run with `python app.py` | Streamlit apps must be launched through Streamlit | Use `python -m streamlit run app.py` |
| `No matching distribution found for ...` during pip install | Typo in the package name | Re-type the package name carefully |
| Page does not open in the browser | Browser did not launch automatically | Open `http://localhost:8501` manually |

---

## 12. Known Limitations

- OCR accuracy depends heavily on image quality.
- The attention shown is computed from the embeddings directly. It illustrates the mechanism but is not extracted from a trained production transformer, so the values should be treated as educational.
- Very long text produces large matrices that are hard to read.

---

## 13. Future Improvements

- Heatmap visualization with word labels on both axes
- Support for multiple attention heads
- Option to type or paste text instead of uploading an image
- Integration of a pretrained transformer model for real attention weights
- Export of results as an image or CSV file

---

## 14. Author

Rupasivakumar
B.Sc Computer Science with Artificial Intelligence

---

## License

Add your preferred license here (for example, MIT) or remove this section if not needed.
