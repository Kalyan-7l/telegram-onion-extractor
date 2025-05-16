import os
import re
import json
import asyncio
from datetime import datetime, timezone
from telethon import TelegramClient
from telethon.tl.functions.messages import GetHistoryRequest

# Replace with your actual Telegram API credentials
api_id = 28428448 # ← Replace with your API ID
api_hash = '0634d83c6b75cb7c30bf7c9647b3f75a'  # ← Replace with your API Hash

# Target public Telegram channel
channel_username = 'toronionlinks'

# File to store last processed message ID
last_id_file = 'last_message_id.txt'

async def main():
    async with TelegramClient('anon', api_id, api_hash) as client:
        entity = await client.get_entity(channel_username)

        # Load last processed message ID (for deduplication)
        last_id = 0
        if os.path.exists(last_id_file):
            with open(last_id_file, 'r') as f:
                last_id = int(f.read().strip())

        # Fetch recent messages with required offset_date
        messages = await client(GetHistoryRequest(
            peer=entity,
            limit=100,
            offset_id=last_id,
            offset_date=None,  # Required by Telethon
            add_offset=0,
            max_id=0,
            min_id=0,
            hash=0
        ))

        new_last_id = last_id
        with open('onion_links.json', 'a', encoding='utf-8') as outfile:
            for msg in messages.messages:
                if msg.id > new_last_id:
                    new_last_id = msg.id

                if msg.message:
                    links = re.findall(r'http[s]?://[^\s]*\.onion', msg.message)
                    for link in links:
                        entry = {
                            "source": "telegram",
                            "url": link,
                            "discovered_at": datetime.now(timezone.utc).isoformat(),
                            "context": f"Found in Telegram channel @{channel_username}",
                            "status": "pending"
                        }
                        outfile.write(json.dumps(entry) + "\n")

        # Save the newest message ID for next time
        with open(last_id_file, 'w') as f:
            f.write(str(new_last_id))

# Entry point to run the async main
if __name__ == '__main__':
    asyncio.run(main())