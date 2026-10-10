# Identity Security Daily

**Season 1: Who Holds the Keys**

A five-minute daily cybersecurity briefing for client conversations. Each episode picks three to five stories from the past few days: breaches, market moves, buyer pressure, regulation, and what is happening in identity security. It explains what happened, why it matters, and what a customer might ask about it.

New episodes come out every morning.

- **Feed:** [`feed.xml`](https://raw.githubusercontent.com/root-access-audio/podcast/main/feed.xml)
- **Apple Podcasts:** search for *Identity Security Daily*

## Why this exists

This is a hobby project. I wanted to see how far local large language models can go on a real task, with no cloud and no API keys. The question was whether a small open model on one home server could read a day's security news, pick out what matters, and produce something a person would want to listen to.

The podcast is the test. It is also useful to me, because it is the kind of briefing I would want before a client call.

## Fully AI-produced

Every episode is generated automatically. No one writes, edits, or records it. A local language model analyzes the articles and writes the script. A local text-to-speech model reads it. The only human work is building and tuning the pipeline.

That means mistakes can happen. The pipeline is strict about evidence, but it is still an experiment. Before relying on a detail, check the linked source in the episode notes.

## How an episode is made

1. **Discover.** RSS feeds from independent security publishers are collected, plus targeted news searches for identity security, the security market, breaches, and buyer priorities.
2. **Filter.** Duplicates, stock-price chatter, low-context items, and direct competitor marketing posts are dropped. Candidates are ranked for relevance, freshness, and whether they would be useful in a customer conversation.
3. **Read.** Each candidate article is fetched and its text extracted. That text stays on the server and is kept for up to 21 days. It is never published here.
4. **Score sources.** Each source gets a trust score based on the publisher, article depth, author and date, promotional language, and whether other outlets report the same story.
5. **Analyze.** A local language model reads each article and returns a structured analysis: what happened, technical impact, business impact, market significance, and caveats. Every claim must cite a specific sentence in the source. Analyses whose evidence fails the check are rejected.
6. **Select.** At least three stories must clear the quality bar, or no episode is published that day. The app never pads a thin news day.
7. **Write.** The model turns the selected stories into one connected script of about 650 words, using only the verified analysis.
8. **Speak.** A local neural voice reads the script. The audio is adjusted to about five minutes and encoded as MP3.
9. **Publish.** The MP3, show notes, episode artwork, and RSS feed are committed to this repository. Apple Podcasts and other apps pick up the new episode from the feed.

## The stack

Everything runs on one Ubuntu virtual machine on a small home server, on CPU only: 12 vCPUs, 16 GB of RAM, and no GPU. Every tool is open source.

| Layer | Tool |
| --- | --- |
| Orchestration | [Node.js](https://nodejs.org) 22, plain JavaScript |
| Article extraction | [Mozilla Readability](https://github.com/mozilla/readability) and [jsdom](https://github.com/jsdom/jsdom) |
| Model runtime | [Ollama](https://ollama.com) |
| Language model | [Qwen3](https://github.com/QwenLM/Qwen3) 4B, an open-weight model run locally |
| Speech | [Kokoro](https://github.com/hexgrad/kokoro) 82M, with [Piper](https://github.com/rhasspy/piper) as a fallback |
| Audio | [FFmpeg](https://ffmpeg.org) |
| Artwork | [Pillow](https://python-pillow.org), applied to one generated base image |
| Hosting | Git and this public GitHub repository |
| Scheduling | cron |

No article text, prompt, or audio is sent to a hosted AI service.

## What I learned so far

- **A 4B model is enough if the pipeline does the checking.** Smaller models paraphrase and invent quotes. Having the model cite a sentence number, while the app copies the exact sentence, solved most of that.
- **CPU-only works, slowly.** One day's analysis takes from several minutes to around an hour, depending on how many articles need reading. The model handles articles one at a time, and saved analyses are reused on reruns.
- **Speech is the hard part to make sound human.** Kokoro sounds much more natural than older engines. Some words still need help: AI is spoken as "superintelligence," and vendor names get phonetic spellings.
- **Failing closed beats a bad episode.** If evidence is thin or the model cannot produce valid analysis, nothing is published that day.

## Sources and rights

Stories come from public reporting, including Dark Reading, BleepingComputer, Krebs on Security, The Register, The Hacker News, CISA, Help Net Security, SecurityWeek, CSO Online, Cybersecurity Dive, and Schneier on Security. Direct competitor marketing posts are excluded so no vendor content program can dominate the show. Google News search results are used only to discover articles. Each episode's show notes link to the original articles. Full article text is not stored or republished here.

## Disclaimer

This independent podcast is not associated with, sponsored by, or endorsed by any vendor mentioned in an episode. Company, vendor, and product names are used only for news reporting, commentary, and identification. Nothing here is investment advice.

## Repository layout

```
feed.xml                       podcast RSS feed
episodes/YYYY-MM-DD.mp3        episode audio
episodes/YYYY-MM-DD.md         show notes with source links
episodes/YYYY-MM-DD.json       episode metadata
artwork/show.jpg               show cover
artwork/episodes/YYYY-MM-DD.jpg  episode artwork
```

This repository is written by the pipeline. Manual changes are overwritten on the next publish.
