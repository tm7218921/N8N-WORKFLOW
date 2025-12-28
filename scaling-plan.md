# Scaling Plan for 100 Products

## Input
- Source: Google Sheet or CSV with 100 rows.
- Columns: product_name, description, image_url (optional).

## Workflow
1. n8n Cron trigger runs every hour.
2. Google Sheets node reads all rows with status = "NEW".
3. For each row:
   - Send description to OpenAI node to generate 3 hooks, headlines, and 20s scripts.
   - Loop over 3 variations and call HeyGen `/v2/video/generate` via HTTP node.
   - Poll status until video is ready and store video_url + headline + hook.
4. Save outputs to:
   - Google Drive (MP4 files) and
   - Sheet columns: headline_1, video_url_1, headline_2, video_url_2, headline_3, video_url_3.
5. Update row status to "DONE" to avoid re-processing.

## Rate Limits and Costs
- Process max 5–10 products in parallel to respect HeyGen API limits.
- If average video creation time is 2–5 minutes, 100 products × 3 videos can finish in a few hours.
- Monitor failed calls and retry with an Error Trigger workflow.

## Summary
The same blueprint runs in batch, only the input list of products changes. No manual work is needed per product once the workflow is active.
