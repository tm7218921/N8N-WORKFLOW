# N8N-WORKFLOW
# Automated Ad Generator – Smart Water Bottle

This project uses **n8n + HeyGen + OpenAI** to create 3 short-form (9:16) ad videos from a single product description.

---

## 1. Project Contents

- `workflow.json` – n8n workflow blueprint (import this into your n8n instance).
- `scaling-plan.md` – how to scale the workflow to 100+ products.
- `video1_never-forget.mp4` – Ad variation 1 (problem/solution).
- `video2_track-sips.mp4` – Ad variation 2 (benefit-focused).
- `video3_health-starts.mp4` – Ad variation 3 (feature/demo).

---

## 2. One‑Click Import into n8n

### Option A – Import from File

1. Log in to your n8n instance.  
2. Click **“Workflows” → “New”** to open a blank canvas.  
3. In the top‑right, click the **three dots (⋯)** menu.  
4. Choose **“Import from File”**.  
5. Select `workflow.json` from this repo/folder.  
6. n8n will load the full workflow on the canvas – click **Save**.[web:50][web:51]

### Option B – Import from Clipboard (if you open JSON manually)

1. Open `workflow.json` in a text editor.  
2. Copy the entire JSON.  
3. In n8n editor, click the **⋯** menu → **“Import from Clipboard”**.  
4. Paste JSON → **Import** → **Save**.[web:52][web:78]

---

## 3. Required Credentials

After import, open the workflow and set:

- **OpenAI (or Groq) API key** in the “AI Copy” node.  
- **HeyGen API key** in the HTTP Request nodes calling `/v2/video/generate`.  
- (Optional) **Google Drive / Airtable / Google Sheets** credentials for storage nodes.

Each node with missing credentials will show a warning – open it and select or create credentials, then save.[web:52]

---

## 4. How the Workflow Works

1. **Trigger** – You provide a product name + description (manually, via webhook, or sheet row).  
2. **AI Copy Node** – Generates 3 hooks, headlines, and 20‑second scripts.  
3. **Loop** – The workflow iterates over each of the 3 variations.  
4. **HeyGen HTTP Node** – Sends the script and options (avatar, 9:16) to HeyGen API and gets a `video_id`.  
5. **Status Poll Node** – Polls HeyGen until the video is ready and gets a `video_url`.  
6. **Output Node** – Saves the video URL + ad text to your storage (Drive/Sheet/Airtable).

---

## 5. Running a Test

1. Import `workflow.json` into n8n.  
2. Open the workflow → click **Execute Workflow**.  
3. When prompted (or in the Trigger node), enter this sample product:

   - Product: `Smart Water Bottle`  
   - Description: `App-connected stainless steel bottle with LED hydration tracking, app reminders, and temperature display.`  

4. Wait until all nodes turn green.  
5. Check your output node (e.g., Google Drive or Sheet) for **3 video URLs**.  

Use those URLs or downloaded MP4s as the 3 required ad variations.
