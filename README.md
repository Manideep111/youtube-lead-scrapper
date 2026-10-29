# YouTube Lead Scraper

Find YouTube channels to pitch, filtered by:

- **Niche**: any search term (long-form channels only; Shorts-focused
  channels are left out)
- **Channel subscribers** (default 50K–100K): Under 1K, 1K–10K, 10K–50K, 50K–100K, 100K–500K, 500K–1M, 1M+
- **Last uploaded video**: last 2 days, week, 2 weeks, month, 3 months, 6 months
- **Region**: 24 countries or worldwide

Each result shows subscribers, last upload, video and view counts, the latest
video, and any email addresses found in the channel's About text. You can sort,
filter to channels with an email, copy all emails, or export to CSV.

## Setup

1. In [Google Cloud Console](https://console.cloud.google.com/), enable
   **YouTube Data API v3** and create an API key under **Credentials**.
2. Copy `.env.example` to `.env.local` and paste the key:

   ```
   YOUTUBE_API_KEY=your_key_here
   ```

3. Install and run:

   ```bash
   npm install
   npm run dev
   ```

4. Open http://localhost:3000.

## How it works

The scraper looks for long-form channels, not Shorts channels. YouTube's
search can't filter by subscriber count or upload date, so `/api/leads`
searches the niche for 4–20 minute videos (only recent ones when an upload
window is set), then checks each channel's real subscriber count and its last
10 uploads. A channel is kept only when at least 60% of those uploads are
longer than 3 minutes (the Shorts limit), and "last upload" means its newest
long-form video. "Scan more channels" continues from where the last search
stopped.

Region biases the search toward that country and drops channels registered
in a different one; channels with no country set are kept.

## Quota

The free YouTube API quota is 10,000 units a day. A search uses up to about
600, so plan on roughly 15 searches a day. If you deploy this publicly, anyone
with the URL can spend your quota.
