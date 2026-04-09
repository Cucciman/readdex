# Readdex

**Live:** https://cucciman.github.io/readdex

I built this because I wanted in one place the websites I normally browse every morning for markets, business news, tech, etc. 

Readdex pulls live RSS feeds from the sources I actually read, organised by category, refreshed on demand. No accounts, no tracking, no noise. Just a single `index.html` file you can host for free on GitHub Pages.

If you find it useful and want to build your own version with your own sources, the instructions below will get you there in a few minutes.

---

## Sources included

| Category | Sources |
|---|---|
| Substack | AC Notebook |
| Markets | CNBC, Yahoo Finance, MarketWatch, Investing.com |
| Business | BBC Business, The Guardian, The Economist, Fortune |
| Strategy | SaaStr, a16z, Inc. |
| Tech | TechCrunch, Ars Technica, VentureBeat, Hacker News |

---

## How to add a source

All sources live in the `SOURCES` array near the top of the `<script>` block in `index.html`. Adding a source = adding one object. The guide panel at the bottom of the dashboard explains all of this too.

### Step 1 — Find the RSS URL

Most publications have a public RSS feed. Common patterns:

| Source type | Pattern |
|---|---|
| Substack | `https://yourpublication.substack.com/feed` |
| WordPress | `https://yoursite.com/feed` |
| Ghost | `https://yoursite.com/rss` |
| BBC sections | `https://feeds.bbci.co.uk/news/business/rss.xml` |
| The Guardian | `https://www.theguardian.com/[section]/rss` |
| TechCrunch | `https://techcrunch.com/feed/` |

Not sure? Append `/feed` or `/rss` to a site's homepage and see if XML loads. Or use the [RSS Validator](https://validator.w3.org/feed/) to check any URL.

### Step 2 — Wrap it through the proxy

Browsers block direct RSS requests (CORS). Prepend this to any RSS URL:

```
https://api.rss2json.com/v1/api.json?rss_url=YOUR_FEED_URL
```

Example:

```
https://api.rss2json.com/v1/api.json?rss_url=https://acnotebook.substack.com/feed
```

Free tier: 10,000 requests/day. If you hit the limit, get a free API key at [rss2json.com](https://rss2json.com) and append `&api_key=YOUR_KEY`.

### Step 3 — Add the object to `SOURCES[]`

Open `index.html`, find `const SOURCES = [`, paste a new entry:

```javascript
{
  id:       "my-source",     // unique, no spaces
  label:    "Source Name",   // shown in card header
  category: "Markets",       // tab it appears under — reuse existing or create new
  color:    "#c8102e",       // dot color in the card header (any hex)
  rssUrl:   "https://api.rss2json.com/v1/api.json?rss_url=YOUR_FEED_URL"
},
```

Save and reload. Done.

### Adding your own Substack or newsletter

```javascript
{
  id:       "my-newsletter",
  label:    "My Newsletter",
  category: "Substack",
  color:    "#2c5f8a",
  rssUrl:   "https://api.rss2json.com/v1/api.json?rss_url=https://YOURNAME.substack.com/feed"
},
```

Works the same for Beehiiv, Ghost, ConvertKit, WordPress — any platform that produces RSS.

### Removing a source

Delete its object from `SOURCES[]`. If it was the only source in that category, the tab disappears automatically.

---

## Hosting on GitHub Pages

1. Create a GitHub repository
2. Upload `index.html` (and this `README.md`) to the root
3. Go to **Settings → Pages → Source** → Deploy from branch → `main` / `root`
4. Live at `https://yourusername.github.io/readdex` in ~30 seconds

To update sources later: edit `index.html` directly in GitHub's web editor, commit, done.

---

## Troubleshooting

**Feed shows "unavailable"**

Some sources block the rss2json proxy — this is common with paywalled publications (FT, WSJ, Bloomberg, Reuters). Try:
- A different feed URL for the same source
- A different publication in the same category
- Testing the raw RSS URL in your browser first

**rss2json rate limit**

Register for a free API key at [rss2json.com](https://rss2json.com) and append `&api_key=YOUR_KEY` to each `rssUrl`.

---

## Useful links

- [rss2json.com](https://rss2json.com) — proxy used by this dashboard
- [aboutfeeds.com](https://aboutfeeds.com) — guide to finding RSS on any site
- [RSS Validator](https://validator.w3.org/feed/) — check if a feed URL is valid
- [FeedSpot directory](https://rss.feedspot.com/business_news_rss_feeds/) — curated list of business RSS feeds
- [GitHub Pages](https://pages.github.com) — free hosting

---

## License

MIT.
