# SuperScraper

## Overview

SuperScraper is a robust n8n workflow designed to scrape websites via Telegram. By sending a message to the bot, users can extract web page content and convert it into cleanly formatted Markdown files. The workflow handles individual URLs, bulk text lists (.txt, .md, .csv), and XML sitemaps.

## Features

- **Intelligent Input Detection:** The workflow automatically determines if the user sent a single URL, a sitemap file, or a text-based list of URLs.
- **Single URL Scraping:** When provided with a single link, the bot fetches the HTML, strips out non-content blocks (like scripts and styles), converts the text to Markdown, and sends the `.md` file back to the chat.
- **Bulk Processing:** Users can upload `.txt`, `.md`, `.csv`, or `sitemap.xml` files. The bot will parse the URLs, filter out duplicates, and process them sequentially.
- **Rate Limiting:** To prevent overwhelming target servers, bulk scraping includes a mandatory 2-second delay between requests.
- **Live Progress Updates:** During batch jobs, the bot calculates elapsed time and sends periodic progress updates to the chat.
- **State Management:** The workflow uses global static data to track per-chat scraping states, preventing concurrent jobs from colliding.
- **Error Handling:** The bot notifies the user if an invalid file is sent, if a sitemap index is provided instead of a standard sitemap, or if specific URLs fail to load.

## How It Works

1. **Telegram Trigger:** The bot listens for incoming text messages or document uploads.
2. **Detection & Routing:** A custom JavaScript node analyzes the payload to determine the processing mode (single, sitemap, urllist, no_url, or unsupported_file).
3. **Execution:**

    - For single URLs, the bot fetches the page, converts the HTML to Markdown, and replies with the attached file and character count.
    - For files, the bot downloads the document, extracts the URLs, and enters a batch processing loop.

4. **Batch Tracking:** Each processed URL increments success or failure counters. Once complete, the bot delivers a final summary with elapsed time and a list of failed URLs.

## Installation

1. Download the `SuperScraper.json` file to your local machine.
2. Open your n8n workspace.
3. Navigate to **Workflows** and select **Add Workflow**.
4. Click the options menu in the top right corner and select **Import from File**.
5. Upload the `SuperScraper.json` file.
6. Update all Telegram nodes with your active Telegram Bot credentials.
7. Save the workflow and toggle it to **Active**.
