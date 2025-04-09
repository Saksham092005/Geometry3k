<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">

</head>
<body>
  <h1>Geometry Problem Solver using Vision-Language Models</h1>

  <h2>1. Introduction</h2>
  <p>
    The objective of this project is to develop a deep learning model capable of interpreting and solving geometry-based mathematical problems presented in image format with accompanying multiple-choice answers. This task requires integrating visual understanding and natural language processing, making it an ideal problem for Vision-Language Models (VLMs).
  </p>

  <h2>2. Dataset Exploration and Understanding</h2>
  <h3>Dataset Structure:</h3>
  <ul>
    <li>The dataset consists of image-based geometry questions.</li>
    <li>Each entry is a dictionary with keys: <code>images</code>, <code>problem</code>, <code>answer</code>, <code>id</code>, <code>choices</code>, and <code>ground_truth</code>.</li>
  </ul>
  <h3>Exploratory Data Analysis (EDA):</h3>
  <ul>
    <li>Samples were visualized to observe layout styles, handwriting/text types, and diagram complexity.</li>
    <li>Class distribution and image clarity were analyzed.</li>
    <li>Identified challenges included inconsistent diagram clarity and presence of redundant or complementary text in the problem field.</li>
  </ul>

  <h2>3. Model Development Journey</h2>
  <h3>A. Pretrained VLM Experiments</h3>
  <ol>
    <li>
      <strong>BLIP-2 (Salesforce):</strong>
      <ul>
        <li>Tried as a zero-shot and fine-tuned model.</li>
        <li>Ran into GPU memory constraints even with 8-bit quantization via bitsandbytes on Kaggle and Colab.</li>
      </ul>
    </li>
    <li>
      <strong>SigLIP (Google):</strong>
      <ul>
        <li><strong>siglip1:</strong> Initial implementation suffered from severe overfitting.</li>
        <li><strong>siglip2:</strong> Incorporated regularization techniques (Dropout, Weight Decay) and image augmentations (flips, rotation). Also experimented with freezing early layers. Performance improved but remained suboptimal.</li>
      </ul>
    </li>
  </ol>

  <h2>4. Custom VLM Pipeline Design</h2>
  <p>
    Given the limitations of off-the-shelf models, a custom pipeline was designed to gain more control over both modalities and model architecture.
  </p>
  <h3>Step 1: Image Preprocessing</h3>
  <ul>
    <li>Images converted to tensors, resized uniformly.</li>
    <li>Normalized using standard vision model values.</li>
    <li>Data augmentation: rotation, contrast, Gaussian noise, horizontal flip.</li>
  </ul>
  <h3>Step 2: Text Preprocessing</h3>
  <ul>
    <li>Tokenized problem field using BERT tokenizer.</li>
    <li>Removed placeholder tokens and cleaned noisy data.</li>
    <li>Plan to add OCR support for extracting embedded text from diagrams.</li>
  </ul>
  <h3>Step 3: Feature Extraction</h3>
  <ul>
    <li><strong>Visual Backbone:</strong> ResNet-50 pretrained on ImageNet.</li>
    <li><strong>Text Encoder:</strong> BERT-base with uncased vocabulary.</li>
  </ul>
  <h3>Step 4: Modality Fusion</h3>
  <ul>
    <li>Concatenated final embeddings from both modalities.</li>
    <li>Used a fully connected fusion MLP with dropout.</li>
  </ul>
  <h3>Step 5: Reasoning Module</h3>
  <ul>
    <li>Initial implementation includes a 2-layer MLP classifier.</li>
    <li>Output is a softmax over the multiple-choice options.</li>
  </ul>

  <h2>5. Challenges Faced</h2>
  <ul>
    <li><strong>Memory Limitations:</strong> BLIP-2 failed to run within the GPU limits.</li>
    <li><strong>Overfitting:</strong> SigLIP models showed poor generalization initially.</li>
    <li><strong>Text-Image Alignment:</strong> Some images contain crucial diagram-specific text not reflected in the problem field.</li>
    <li><strong>OCR Noise:</strong> Need to integrate math-specific OCR and properly format LaTeX-style tokens.</li>
  </ul>

  <h2>6. Performance & Future Work</h2>
  <p>
    Accuracy metrics currently hover around baseline (to be finalized). Error analysis shows difficulty in problems with complex diagrams.
  </p>
  <p>
    <strong>Plan to:</strong>
  </p>
  <ul>
    <li>Add OCR and re-rank choices based on extracted text.</li>
    <li>Explore attention-based fusion instead of simple concatenation.</li>
    <li>Try multimodal ensembling for diverse problem types.</li>
    <li>Introduce curriculum learning for step-by-step reasoning.</li>
  </ul>

  <h2>7. Deliverables Summary</h2>
  <ul>
    <li>
      <strong>Codebase:</strong> Submitted as a collection of structured notebooks:
      <ul>
        <li><code>blip2.ipynb</code></li>
        <li><code>siglip1.ipynb</code></li>
        <li><code>siglip2.ipynb</code></li>
        <li><code>custom-pipeline.ipynb</code></li>
      </ul>
    </li>
    <li>
      <strong>This Report:</strong> Contains methodology, architectural decisions, and analysis.
    </li>
  </ul>

  <h2>8. Conclusion</h2>
  <p>
    This project involved iterative experimentation with both pretrained and custom-built vision-language models to solve geometry-based image questions. A clear progression from off-the-shelf models to a custom pipeline allowed for finer control and better generalization. The groundwork laid out sets the stage for deeper integration of reasoning and OCR components in the next phase.
  </p>
  <p>
    <strong>Team Name:</strong> Neuronauts
  </p>
  <p>
    <strong>Team Members:</strong><br>
    Rajat Bansal<br>
    Saksham Kankaria<br>
    Aditya Gupta
  </p>

  <h2>Per-class Metrics</h2>
  <ul class="metrics">
    <li><strong>Option A:</strong> Precision = 0.0000, Recall = 0.0000, F1 = 0.0000</li>
    <li><strong>Option B:</strong> Precision = 0.3116, Recall = 0.5617, F1 = 0.4009</li>
    <li><strong>Option C:</strong> Precision = 0.3014, Recall = 0.4468, F1 = 0.3600</li>
    <li><strong>Option D:</strong> Precision = 0.0000, Recall = 0.0000, F1 = 0.0000</li>
  </ul>
</body>
</html>
