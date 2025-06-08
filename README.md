# flicked_hackthon
🌟 Project Title:

Flicked Fashion AI - Detect, Match & Classify Fashion in Reels

📚 Overview:

This project is a smart fashion analysis pipeline that takes Instagram-style reels (short videos), detects fashion products worn by people in the video frames, and:

Matches them with a provided product catalog using CLIP + FAISS.

Classifies the overall vibe of the outfit.

Outputs per-video structured JSON data containing the matched product information and vibes.

💡 Key Features:

YOLOv8n for product detection from each frame.

CLIP (ViT-B/32) for embedding both catalog images and cropped products.

FAISS to find exact/similar matches from the catalog.

NLP-based Vibe Classifier that assigns relevant fashion aesthetics (e.g., "Y2K", "Clean Girl", etc.).

Structured per-reel outputs.

🔄 Pipeline Steps:

1. Frame Extraction:

Videos are placed in /videos/reel/

Frames are extracted using OpenCV (1 frame per second).

2. Fashion Product Detection:

YOLOv8n model (models/yolov8n.pt) detects items like tops, trousers, dresses.

Detected bounding boxes are cropped and saved in flickd_ai_project/data/cropped_items.

3. Catalog Embedding:

Product catalog (from catalog.csv) is processed.

Images are downloaded from URLs.

CLIP encodes them into 512-d embeddings.

FAISS index (catalog.index) + metadata (catalog_metadata.pkl) saved in data/catalog/.

4. Matching Cropped Items:

Each cropped frame is passed through CLIP.

Compared to the catalog using FAISS.

Top-1 result selected if similarity > 0.75.

Output saved in match_results.json.

5. Per-Video JSON Output:

Items are grouped by reel.

Output saved as /outputs/reel_XXX.json.

Each contains:
{
  "video_id": "reel_001",
  "vibes": ["Clean Girl", "Boho"],
  "products": [
    {
      "type": "Top",
      "color": "Pink",
      "matched_product_id": "15592",
      "match_type": "similar",
      "confidence": 0.78
    }
  ]
}

6. Vibe Classification:

Keywords are extracted from title + description + tags.

Matched to vibe keywords in vibes_list.json.



 Folder Structure:
 submission/
├── videos/                # Input videos & cropped visuals
├── catalog.csv            # Provided fashion catalog
├── vibes_list.json        # Fashion aesthetics keyword dictionary
├── outputs/               # Final result JSONs per video
│   ├── reel_001.json
│   └── match_results
├── models/                # Trained detection model + CLIP code
│   ├── yolov8n.pt
│   └── flickd_code.ipynb
├── README.md              # You're here!
├── requirements.txt       # All dependencies
└── demo.mp4 / Loom link   # Demo video of working pipeline
![image](https://github.com/user-attachments/assets/76a40f69-ba92-460b-94a7-3c2a78628e88)

⚙️ Setup Instructions:

1. Clone the Repo:
   git clone <your-repo-url>
   cd submission/

2. Create Environment:
   pip install -r requirements.txt

3. Run Catalog Embedding:
   python embed_catalog.py #(builds catalog.index)

4. Process Videos:
   python detect_and_crop.py
   python match_with_catalog.py
   python classify_vibes.py

    Credits:

Built by Vinayak
