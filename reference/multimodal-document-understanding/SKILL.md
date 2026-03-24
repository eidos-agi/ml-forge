---
name: multimodal-document-understanding
description: Process documents containing both text and images using multimodal AI techniques (CLIP, BERT, Vision Transformers). Extract signal from pitch decks, financial reports with charts, real estate listings with photos. Use when documents have visual content that carries signal alongside text.
---
# Multimodal Document Understanding
## When to Use This Skill
Use when processing documents where both text AND images carry signal: pitch decks with diagrams, financial reports with charts, real estate listings with photos, medical reports with scans, insurance claims with damage photos.

## Instructions
1. **Document Ingestion** — Extract text (PyPDF2, pdfplumber) and images (pdf2image) separately from PDFs/DOCX
2. **Text Processing** — BERT/sentence-transformers for text embeddings. Extract key entities, numbers, sentiment.
3. **Image Processing** — CLIP for image embeddings. Classify image type (chart, photo, diagram, logo). For charts: use chart-to-data extraction.
4. **Multimodal Fusion Strategies**
   - Early fusion: concatenate text + image embeddings, feed to classifier
   - Late fusion: separate text and image classifiers, combine predictions
   - Cross-attention: transformer that attends to both modalities (most powerful, most complex)
5. **Practical Pipeline (CLIP-based)**
   ```python
   from transformers import CLIPProcessor, CLIPModel
   model = CLIPModel.from_pretrained("openai/clip-vit-base-patch32")
   processor = CLIPProcessor.from_pretrained("openai/clip-vit-base-patch32")
   # Get image embedding
   inputs = processor(images=image, return_tensors="pt")
   image_features = model.get_image_features(**inputs)
   # Get text embedding
   inputs = processor(text=text, return_tensors="pt")
   text_features = model.get_text_features(**inputs)
   # Combine for downstream task
   combined = torch.cat([image_features, text_features], dim=-1)
   ```
6. **Applications**
   - Pitch deck scoring: text quality + slide design quality → deal score
   - Real estate: listing text + property photos → price prediction feature
   - Insurance: claim description + damage photos → severity classification
