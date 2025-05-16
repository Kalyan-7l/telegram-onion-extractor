# Telegram .onion Link Extractor

## Objective
Extract `.onion` URLs from messages in a public Telegram channel using the Telethon API.

## Requirements
- Python 3.8+
- Telethon library (`pip install telethon`)
- Telegram API credentials (from https://my.telegram.org)

## Setup Instructions
1. Clone or download this repository.
2. Install dependencies:
pip install telethon


3. Update `extract_onion_links.py` with your `api_id` and `api_hash`.
4. Run the script:
python extract_onion_links.py

5. Enter your Telegram phone number and login code when prompted.

## Output
Extracted `.onion` URLs are saved in `onion_links.json` with this format:

```json
{
"source": "telegram",
"url": "http://example.onion",
"discovered_at": "2025-05-17T12:30:00+00:00",
"context": "Found in Telegram channel @toronionlinks",
"status": "pending"
}
