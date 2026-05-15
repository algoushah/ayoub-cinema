# AYOUB CINEMA - Cloudflare Pages

Movie streaming website with IPTV support & Telegram visitor notifications.

## Features
- Browse popular movies via TMDB API
- IPTV channels (HLS, YouTube, custom channels)
- Telegram notification on each visitor
- Dark/Light theme
- AR/EN language support
- Custom channel management (saved in browser)

## Setup

### 1. Install Wrangler
```bash
npm install -g wrangler
```

### 2. Set Telegram Secrets
Get a bot token from [@BotFather](https://t.me/BotFather) and your chat ID from [@userinfobot](https://t.me/userinfobot).

```bash
echo "YOUR_BOT_TOKEN" | wrangler pages secret put TELEGRAM_BOT_TOKEN
echo "YOUR_CHAT_ID" | wrangler pages secret put TELEGRAM_CHAT_ID
```

### 3. Set TMDB API Key (optional)
The default key is already in the code, but you can set your own:
```bash
echo "your_tmdb_key" | wrangler pages secret put TMDB_API_KEY
```

### 4. Deploy
```bash
cd ayoub-cinema
wrangler pages deploy .
```

### 5. Cloudflare Dashboard (Alternative)
1. Go to Cloudflare Dashboard > Pages > Create a project
2. Connect your Git repo or upload files manually
3. Go to Project Settings > Environment Variables > Add:
   - `TELEGRAM_BOT_TOKEN` (secret)
   - `TELEGRAM_CHAT_ID` (secret)
   - `TMDB_API_KEY` (optional)

## Project Structure
```
ayoub-cinema/
  index.html           # Main website
  functions/api/
    notify.js          # Telegram notification endpoint
    tmdb.js            # TMDB API proxy
  wrangler.toml        # Deploy config
```

## Adding IPTV Channels
Edit the `IPTV_CHANNELS` array in `index.html`, or use the "Add Custom Channel" form on the website (channels are saved in browser localStorage).

## Channel Types
- `hls`: Direct HLS stream URL (uses HLS.js player)
- `yt`: YouTube video URL (embeds YouTube player)
- `embed`: Any URL to open in new tab

## Telegram Notification
Each unique visitor (per browser session) triggers a Telegram message with:
- Country & city (via Cloudflare)
- Referrer & page URL
- Browser language & screen size
- Timestamp
