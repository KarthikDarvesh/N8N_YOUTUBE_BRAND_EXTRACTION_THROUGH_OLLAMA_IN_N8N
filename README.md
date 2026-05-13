# 🧠 NER_YouTube_Brand_Extraction

---

## 📋 Overview

### Plug-and-Play AI Workflow for Extracting Brand Mentions from Influencer's YouTube Transcripts Using Local LLMs

---

## ❗ Problem

Manually identifying brand promotions inside influencer videos is difficult and time-consuming.

### How can brand promotions and sponsorship mentions be automatically detected??

This is useful for:
- Brand Monitoring
- Ad Intelligence

---

## ✅ Solution

This workflow helps brands, agencies, and marketing teams analyze large volumes of YouTube influencer transcripts to detect past brand promotions, sponsorship mentions, and collaboration patterns.

Instead of manually reviewing thousands or lakhs of influencer videos, the workflow uses a local LLM to understand the contextual and nuanced meaning of transcripts and extract brand mentions in structured JSON format.

This helps identify which influencer is suitable for which brand approach in future influencer marketing campaigns.

The AI model understands the complete contextual and nuanced meaning of YouTube transcripts and intelligently extracts brand mentions.

---

## 🚀 Workflow Process

- Processes YouTube transcripts
- Uses Local LLMs (OLLAMA)
- Understands transcript context
- Extracts brand mentions
- Returns structured JSON output

---

## 📝 Note

Provide your own YouTube transcript input, and the AI workflow will intelligently analyze the transcript and extract brand mentions and sponsorship references in structured JSON format.

---

# 🔧 Workflow Breakdown

---

# 🟢 Primary Workflow

<img width="2568" height="1372" alt="image" src="https://github.com/user-attachments/assets/70025ca1-d18a-4f93-8078-afedc6e01f1c" />

---

## 1. Trigger (Manual)

Starts the workflow when the user manually clicks **"Test Workflow"**.

---

## 2. PostgreSQL Query Node

Fetches enriched video and transcript data with necessary joins.

### Actions Performed:
- Performs an inner join across:
  - `video_details`
  - `video_transcripts`
  - `channel_details`

### Strategy:
- From a particular channel (`channel_details`)
- Fetch all associated videos (`video_details`)
- Retrieve their transcripts (`video_transcripts`)

### Query Logic:
- Selects videos with available transcripts
- Excludes already processed brand mentions
- Limits processing to 2000 videos per execution

---

## 3. Edit Fields Node

Transforms the data by:
- Adding fields
- Removing fields
- Renaming fields
- Rearranging fields
- Modifying fields

In this workflow, fields are rearranged for downstream processing.

---

## 4. Loop Over Items Node

The workflow divides the 2000 fetched items into batches of 1 item each.

### Purpose:
- Each transcript is processed individually
- Prevents model overload
- Improves workflow stability

Each item is then passed to the sub-workflow for AI processing.

---

## 5. Execute Workflow Node (Sub-Workflow Trigger)

Triggers the brand extraction sub-workflow for every individual transcript item.

---

## 6. Wait Node

Waits for 5 seconds after each sub-workflow execution.

### Purpose:
- Prevents local LLM overload
- Controls execution rate
- Improves workflow reliability

After waiting:
- The next transcript item is processed automatically

---

# 🟠 Secondary Workflow (Sub-Workflow)

<img width="1736" height="906" alt="image" src="https://github.com/user-attachments/assets/086aa0fc-bed0-4c5b-90a1-dcd158b45dd5" />

---

## 1. Execute Workflow Trigger Node

Entry point triggered by the parent workflow for each transcript item.

---

## 2. AI Agent Node (Ollama LLaMA 3.1)

Processes the transcript using the local LLM to identify brand mentions.

### Model:
- Ollama
- Llama 3.1:8B

### AI Capability:
- Understands transcript context
- Detects nuanced sponsorship mentions
- Extracts brand entities intelligently

---

## 3. Edit Fields Node

Merges:
- `video_id`
- `yt_video_id`

with:
- AI-generated brand extraction output

### Result:
A structured combined output containing:
- Video metadata
- Extracted brand mentions

---

## 4. Code Node (JavaScript)

Cleans and parses the LLM response into structured JSON format.

### Actions Performed:
- Removes newline characters
- Removes backticks
- Trims extra whitespace
- Formats clean JSON structure

---

## 5. PostgreSQL Node

Stores the cleaned structured brand mention entities into the:

```sql
brand_mention_entity
```

database table.

---

# ✅ End Result

The workflow automatically:

1. Reads YouTube transcripts
2. Processes them using a local LLM
3. Understands transcript context intelligently
4. Extracts brand mentions
5. Returns structured JSON output
6. Stores extracted entities for future analytics and business intelligence

---




  
