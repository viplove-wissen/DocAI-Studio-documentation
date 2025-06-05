# Doc AI Studio Documentation

## Overview

**Doc AI Studio** is a comprehensive document processing system tailored for enterprise-level applications. It enables advanced text extraction, table detection, visual question answering (VQA), and large language model (LLM) evaluation, with a focus on scalability and observability. Leveraging state-of-the-art models like YOLOv10, Microsoft's Table Transformer (TATR), and PaddleNLP, it handles various document types, including PDFs and images, to extract structured and unstructured data efficiently. The system includes robust preprocessing tools, layout detection, data extraction, LLM evaluation metrics, and OpenTelemetry-based observability, making it ideal for complex document workflows.

[TOC]

<a id="quickstart"></a>
## Quickstart

**Doc AI Studio** is a powerful document processing system designed for enterprise-level applications, enabling text extraction, table detection, visual question answering, and more with scalability and observability. Below are the steps to get started.

<a id="prerequisites"></a>
### Prerequisites
- **Python 3.10**: Required for compatibility with the library.
- **System Requirements**: Linux-based system (e.g., Ubuntu) with internet access for dependency installation.
- **Docker**: Required for setting up the observability dashboard.
- **Git**: For cloning the repository.

<a id="installation"></a>
### Installation
1. **Install Python 3.10**:
   ```bash
   sudo apt update
   sudo apt upgrade
   sudo apt autoremove
   sudo add-apt-repository ppa:deadsnakes/ppa
   sudo apt update
   sudo apt install python3.10
   ```

2. **Clone the Repository**:
   ```bash
   git clone <repository-url>
   cd AI_Studio_CoE
   ```

3. **Set Up a Virtual Environment**:
   ```bash
   python3.10 -m venv .doc_env
   source .doc_env/bin/activate
   ```

4. **Install Dependencies**:
   ```bash
   pip install -r req.txt
   ```

5. **Install YOLOv10**:
   ```bash
   git clone https://github.com/THU-MIG/yolov10.git
   cd yolov10
   pip install .
   cd ..
   ```
<a id="getting-started"></a>
## Getting Started with pipeline

A typical document AI appplication consists of certain elements in its pipeline. These elements are as follows:
#### 1. Pre-Processing: 
Pre-processing is done to obtain the information of file type, language and reducing the anomalies using image processing.

#### 2. Identification:
Then the layout and structure detection of processed file is carried out in identification.

#### 3. Extraction:
After detecting the layout and structure, information extraction is done only from the selected elements of the layout.

#### 4. Post processing:
The information extracted is either used in more complex downstream extraction or displayed to the user.

#### 5. Observability:
After the application is made, observability is used to keep an eye on the system resources and api calls.

#### 6. Storage:
The vectorization/indexing of the document is needed in many extraction tasks. These indexes are saved in databases for future queries.
 
<a id="core-functionalities"></a>
## Core Functionalities

Doc AI Studio provides a comprehensive set of features for document processing, data extraction, evaluation, and observability, leveraging advanced models like YOLOv10, Microsoft's Table Transformer (TATR), and PaddleNLP.

<a id="preprocess"></a>
### [Preprocess](html/wissenai.preprocess.html)
The preprocessing module prepares documents and images for analysis with tools for text extraction, PDF conversion, and image enhancement.

<a id="text--bounding-box-extractor"></a>
#### [Text & Bounding Box Extractor](html/wissenai.preprocess.html)
- Extracts text and precise bounding box coordinates from PDFs using `pdfminer.six`.
- Transforms coordinates to a top-left origin system for consistency.
- Outputs a dictionary with text and bounding box data for structured processing.

<a id="create-searchable-pdf"></a>
#### [Create Searchable PDF](html/wissenai.preprocess.html)
- Converts scanned or non-searchable PDFs into searchable PDFs using OCR.
- Preserves original layout while enabling text selection.
- Includes utilities to:
  - Extract images from PDFs as a list of tuples (`List[(PIL_Image, page_number)]`).
  - Verify if a PDF is searchable, returning a boolean.

<a id="image-preprocessing"></a>
#### [Image Preprocessing](html\wissenai.preprocess.html)
- Offers a suite of operations to enhance document images:
  - **Resolution Enhancement**: Increases DPI (`enhance_dpi`).
  - **Image Enhancement**: Increases sharpness and contrast (`enhance_image`).
  - **Derasterization**: Removes watermarks, gridlines and stamps (`derasterize`).
  - **Thresholding**: Removes shadows and lighting inconsistencies (`adaptive_threshold`, `threshold_advanced`).
  - **Noise Removal**: Eliminates artifacts (`noise_removal`,`denoise_advanced`).
  - **Deskewing**: Corrects image skew (`deskew`).
  - **Contrast Enhancement**: Improves readability (`enhance_contrast`).
  - **Image Resizing**: Adjusts dimensions (`resize_image`).
  - **Edge Detection**: Identifies structural edges (`edge_detection`).
  - **Border Removal**: Removes unwanted borders (`remove_border`).
  - **Thinning & Skeletonization**: Simplifies image structure (`thinning`, `skeletonization`).
  - **Rotation**: Corrects orientation (`rotate`).
- Supports PIL Images, NumPy arrays, or image paths, returning processed PIL Images.

<a id="document-layout-detection--table-extraction"></a>
### [Document Layout Detection & Table Extraction](html/wissenai.identify.html)
- Detects document layouts (e.g., tables, headers) in PDFs or images using YOLOv10.
- Extracts bounding boxes and exports tables to Excel files.
- Provides annotated images, structured layout data, and cropped images for specified elements (e.g., tables).

<a id="extraction"></a>
### [Extraction](html/wissenai.extract.html)
The extraction module focuses on extracting structured and unstructured data from documents.

<a id="table-detection"></a>
#### [Table Detection](html\wissenai.extract.tables.html)
- Identifies table regions, including rotated tables, using Microsoft's Table Transformer (TATR).
- Returns cropped table images and metadata (confidence scores, bounding boxes).
- Configurable with class thresholds and crop padding.

<a id="table-structure-recognition-tsr"></a>
#### [Table Structure Recognition (TSR)](html\wissenai.extract.tables.html)
- Extracts table structures from images using TATR and saves them as CSV files.
- Supports visualization of table structures and customizable image resizing.

<a id="document-visual-question-answering-vqa"></a>
#### [Document Visual Question Answering (VQA)](html\wissenai.extract.vqa.html)
- Answers natural language questions about document content using PaddleNLP's Document Intelligence Taskflow.
- Supports single-document queries, batch processing, and direct chat-style QA.

<a id="document-qa-service"></a>
#### [Document QA Service](html\wissenai.extract.vqa.html)
- Integrates with database to track extraction progress and manage active sessions.
- Retrieves model training questions for ongoing sessions.

<a id="structured-chunking"></a>
#### [Structured Chunking](html\wissenai.extract.structured_chunking.html)
- Uses YOLOv10 for layout detection to identify tables, headers, and other elements.
- Extracts tables and filters them using filtering_table_pipeline.
- Processes headers with main_pipeline_create_put_table_headers and organizes content hierarchically.
- Merges multi-page tables and maps them to headers.
- Extracts table of contents (TOC) data with customised_toc_extraction_pipeline.

<a id="llm-evaluation"></a>
### [LLM Evaluation](html\wissenai.eval.html)
Evaluates Retrieval-Augmented Generation (RAG) pipelines and LLM outputs for accuracy and reliability.

<a id="rag-pipeline-evaluation"></a>
#### RAG Pipeline Evaluation
- Measures performance with metrics like:
  - **Faithfulness**: Alignment with provided context.
  - **Relevancy**: Relevance to the query.
  - **Precision and Recall**: Accuracy with or without ground truth.
- Uses RAGAS and DeepEval frameworks.

<a id="risk-assessment"></a>
#### Risk Assessment
- Detects issues in LLM outputs:
  - **Hallucination**: Fabricated or unsupported information.
  - **Bias**: Biased content detection.
  - **Toxicity**: Harmful or inappropriate language.

<a id="summarization-evaluation"></a>
#### Summarization Evaluation
- Assesses summary quality using:
  - **Semantic Similarity**: BERTScore for contextual accuracy.
  - **N-gram Metrics**: BLEU and ROUGE for word-level evaluation.

<a id="post-process"></a>
### [Post-Process](html\wissenai.postprocess.html)
Provides tools for extracted data to be either displayed to user or use it in some downstream task

<a id="citation"></a>
#### Citation
- Extracts text from searchable and scanned PDFs using PyMuPDF and OCR engines (Tesseract, PaddleOCR, EasyOCR).
- Matches fields and values with fuzzywuzzy for robust handling of text variations.
- Groups text blocks into paragraphs for long-form text matching.
- Uses a spatial graph and Dijkstra’s algorithm to pair fields and values by proximity.
- Saves cropped images of matched regions and optional spatial graph visualizations.
- Configurable OCR engine priority for optimal scanned PDF processing.
- Adjustable settings for OCR confidence, paragraph grouping, and spatial distances.
- Returns JSON with citations, including bounding boxes, confidence scores, and matched text.

<a id="semantic-matching"></a>
#### Semantic Matching 
- Matches predefined query sentences (clauses) against PDF text passages using semantic similarity with the all-mpnet-base-v2 sentence transformer model.
- Extracts interest or discounting charge percentages from matched clauses using a DistilBERT question-answering model.
- Cleans extracted numerical values and saves results, including similarity scores and interest values, to a CSV file.

<a id="observability"></a>
### [Observability](html\wissenai.observability.html)
- Provides OpenTelemetry-based observability for FastAPI or Flask applications:
  - **Tracing**: Instruments endpoints with OpenTelemetry Tracer and supports custom spans via `@traced`.
  - **Metrics**: Exports service metrics to external backends.
  - **Logging**: Logs to console and file (`/var/log/app.log`) with customizable formats.

<a id="observability-dashboard"></a>
## Observability Dashboard
Run the following bash command to go in side observability and up the docker image.
```bash
# bash
cd wissenai/observability/docker-config
sudo docker compose up -d
```
- Powered by Grafana, accessible at `localhost:3000` after Docker setup.
- Displays tracing, metrics, and logging data for application monitoring.
- Login with username: `admin` and password: `admin`.

<a id="demo-pipeline"></a>
## Demo Pipeline
We will be creating a pipeline which takes a noisy pdf as an input, converts it into image, do some image processing to make it readable and then perform a VQA on the resulting image.

For this demo a 1 page invoice was selected
```python
# python
## Converting the pdf to image
from wissenai.preprocess.searchable_pdf import *

images = extract_images_from_pdf('/path/to/pdf')
```
images is a list of tuples in the form of List[(PIL_Image, page_number)]

The image of first page is images[0][0]

```python
# python
## To resolve the rotation present in the image
from wissenai.preprocess.utils import de_skew

deskewed = de_skew(image = images[0][0])
```
deskewed is an opencv image.

Now we see that the image coming out of pdf is unresonably large in size. It is rendered with 600 dpi which might be too much for the image processing.

```python
# python
## To resize the deskewed to a reasonable size

resized = cv2.resize(deskewed, (deskewed.shape[0]//10,deskewed.shape[1]//10))
```
resized is a much resonably sized image

Now there are some shadows/non-uniform lighting in image which can cause problem in VQA.

```python
# python
## To resolve shadows/non-uniform lighting we will use adaptive thresholding
from wissenai.preprocess.utils import adaptive_threshold

thresh_image = adaptive_threshold(image = resized)

```
thresh_image is an opencv image.

Thresholding removes the shadows but introduces some artifacts and noise. We will try to denoise it.

```python
# python
## To resolve the noise we will use noise_removal function
from wissenai.preprocess.utils import noise_removal

denoised = noise_removal(image = thresh_image)
```
denoised is PIL image

```python
# python
## Saving the final image

denoised.save("denoised.jpg")
```
Now we will do the VQA

```python
# python
## For VQA we will use the docvqa chat function
from wissenai.vqa.docvqa import chat

answer = chat(_path = "denoised.jpg", question = "What is the Invoice Number?")
```
answer will be a string

This pipeline leverages preprocessing, image processing, and VQA features to deliver accurate results from noisy inputs.

## Models Used
- **YOLOv10**: For layout detection and table extraction.
- **Microsoft Table Transformer (TATR)**: For table detection and structure recognition.
- **PaddleNLP**: For document VQA.
- **RAGAS, DeepEval, BERTScore, BLEU, ROUGE**: For LLM evaluation.
