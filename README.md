# Identity Security Daily

A local application running in a VM on a home server. It discovers reporting about the cybersecurity market, breaches, buyer priorities, SailPoint, IAM, IGA, identity security, competitors, and automated access; reads the public publisher pages; and produces an evidence-grounded daily sales-enablement audio.

Ollama and Qwen3 perform the analysis locally. Kokoro 82M performs natural speech synthesis locally, with Piper as a lightweight fallback. No article text, prompt, or audio is sent to a hosted AI API, everything runs locally (out of curiosity).

The public show is an experiment in end-to-end local AI production. Source analysis, editorial synthesis, writing, and narration happen on a small home server with open-source tools. Collection, evidence validation, audio assembly, and publishing are automated by the application rather than a generative model.

## How the editorial pipeline works

1. RSS and Google News are discovery inputs, not the briefing itself. Vendor and identity blogs that publish the full post in the feed are kept as articles immediately.
2. Other candidates are resolved to publisher pages and parsed with Mozilla Readability. Full articles are saved in a local knowledge base and can be recalled for 21 days, so a thin news morning still has source material.
3. Each source receives a transparent trust score based on publisher type, author/date metadata, article depth, promotional language, extraction quality, and corroborating coverage.
4. Qwen3 produces a structured analysis with exact evidence snippets, technical impact, business impact, caveats, and IAM-market significance.
5. Duplicate coverage is clustered. The algorithm selects three to five stories by relevance, consequence, novelty, usefulness, trust, recency, and topic diversity.
6. A second local-model pass builds one coherent thesis and a deeper narrative across the selected stories.

Vendor material is allowed, but it is labeled as vendor material and promotional claims reduce confidence. The model is instructed to use only supplied evidence. Exact evidence snippets are checked against the source text before an analysis is accepted.

## What it produces

Each run writes a folder under `output/YYYY-MM-DD/`:

- `identity-security-daily.mp3` — the episode, aimed at 5 minutes
- `briefing.txt` — the exact words that were spoken
- `show-notes.md` — thesis, source assessment, key analysis, caveats, and links
- `sources.json` — provenance, trust scores, evidence snippets, model/version, and feed status
- `analysis-diagnostics.json` — written when deep analysis is partial or fails

Full article text is stored only in the local knowledge base at `~/.local/share/identity-briefing/knowledge` (override with `BRIEFING_KNOWLEDGE`). Entries expire after 21 days. That text is not copied into show notes, diagnostics, or the podcast repository.

## Feeds

Focused Google News searches cover SailPoint, IGA competitors (Saviynt, Omada, One Identity, Ping Identity, ForgeRock, EmpowerID), adjacent platforms (Okta, CyberArk, Microsoft Entra, BeyondTrust, Delinea, Silverfort, Veza), automated identity topics, cybersecurity funding and acquisitions, CISO priorities and spending, and material enterprise breaches.

Publisher feeds include Dark Reading, BleepingComputer, Krebs on Security, The Register, The Hacker News, CISA advisories, Help Net Security, SecurityWeek, CSO Online, Cybersecurity Dive, and Schneier on Security. Direct identity-vendor feeds include Saviynt, Veza, Silverfort, Delinea, Omada, Microsoft Security, Auth0, and Okta security advisories. SailPoint has no public article feed, so SailPoint coverage still comes from Google News and from articles already saved in the knowledge base. SailPoint forum threads are left out. If SailPoint itself is quiet for two days, that section reaches back up to two weeks so the company is still covered.

Stock-price chatter and low-context threat notices are dropped. General security reporting is kept when it can support a useful client conversation about buyer pressure, business exposure, spending, regulation, market structure, or a material breach. A thin news day produces fewer stories rather than padding the episode.

Google News RSS is included for personal, non-commercial use, which is the limit stated on that feed. Publisher feeds are read directly.
