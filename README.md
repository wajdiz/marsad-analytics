# MARSAD Analytics

A social media and text analysis dashboard that runs entirely in the browser. Load a file, or collect data
straight from the page, and get 104 analyses over it without installing anything and without sending your data
anywhere.

**Live: https://wajdiz.github.io/marsad-analytics**

Built for the MARSAD Observatory at Northwestern University in Qatar, for research on Arabic social media
discourse. It works on any language, with extra handling for Arabic throughout.

## What it does

Open the page and you have three ways to get data in.

**Upload your own.** CSV, Excel, JSON or NDJSON. Exports from CrowdTangle, Meta Content Library, Meltwater,
Communalytic, Zeeschuimer and the common API dumps are recognised and mapped automatically, so you do not have
to rename a single column. A file it does not recognise is read generically and it tells you which columns it
matched to what.

**Collect in the page.** Seventeen sources, grouped into eight categories, called directly from your browser.
Nothing is proxied through a server.

| Category | What runs here |
| --- | --- |
| Social media | Bluesky search and one account, Mastodon by hashtag, by account and by server, Reddit, YouTube videos and comments |
| News and media | GDELT, The Guardian |
| Wikipedia and encyclopedias | Wikipedia in seven modes, Wikidata |
| Websites, blogs and forums | Reddit, Hacker News, Stack Exchange |
| Academic research | OpenAlex, Crossref |
| Government and public documents | Nothing yet, and the page says so rather than offering a dead button |
| Video, podcasts and transcripts | YouTube videos and comments |
| Open data and public datasets | Hugging Face datasets |

Most need no key at all. YouTube and The Guardian take a free key of your own, which is used for that one search
and is never written into the collected file.

**Record while you browse.** TikTok, Instagram, Threads and X allow no search from a web page. For those, the
[Zeeschuimer](https://github.com/digitalmethodsinitiative/zeeschuimer) browser extension records what you scroll
past, and MARSAD reads what it saves.

## The analyses

105 entries in eleven groups: 104 analyses and one export action.

| Group | Entries | The question |
| --- | --- | --- |
| Check my data | 7 | What is in my file, and is it clean? |
| When | 8 | When did it happen? |
| Who | 10 | Who is posting? |
| Words | 11 | Which words, in which contexts? |
| Topics | 3 | What is it about? |
| Feelings and tone | 20 | How is it said? |
| Reactions | 10 | How did people respond? |
| Spread and facts | 13 | How did it spread, and was it coordinated? |
| Society | 17 | What does it say about society? |
| Dialects | 3 | Which regional Arabic words occur? |
| My report | 3 | How do I use this in a paper? (two cards and the export action) |

Every card states its method in plain words, lists what it cannot tell you, and links back to the posts behind
any number. Results download as Excel, CSV, PNG, SVG or an interactive HTML chart.

### Documents, not just posts

Wikipedia articles, papers and page histories do not fit the one-row-per-post shape the rest assumes. Four cards
read them directly: how a page grew, shrank and was reverted over its edit history; how one subject reads across
language editions; readership beside coverage, with the lag between them; and papers by year, venue, author and
citation count. Long documents can also be split into their sections or paragraphs, so the word, topic and tone
cards have units to work on instead of one row per whole article.

## Nothing leaves your device

The analysis runs in your browser through [Pyodide](https://pyodide.org), which is CPython compiled to
WebAssembly. Your file is read into the tab and stays there. There is no server, no account, no upload, no
analytics and no third-party script on the page.

This is also why the page is large on first visit: the Python runtime, pandas, scikit-learn, the word models and
the lexicons all come down once and are then cached. A cold first load is about 60 MB over 27 requests. After
that the browser serves them from its cache and the page opens in seconds.

The one thing that does leave your browser is a search you type into the collector, which goes to the source
you picked and nowhere else.

## Arabic

The whole interface, including every card's method and caveats, is available in Arabic, right to left. Arabic
text is normalised before analysis (alef and hamza variants, taa marbuta, alef maqsura, diacritics, tatweel,
elongation), dialect and Arabizi are detected, and the word models for sentiment, emotion and offensive language
were trained on Arabic data. Cards that only work on Arabic are hidden when a file has none, rather than
returning numbers from accidental matches.

## Using it

Nothing to install. Open the link, drag a file in, and start with **Check my data**. Five sample files ship with
the page if you want to try it before using your own.

For scripted or repeatable work, the same analysis code is a Python package, so a notebook and the dashboard
produce identical numbers.

## Updating this site

The build produces six files that make up the application:

```
index.html        the whole dashboard, one file
marsad.zip.wasm   the analysis package
docs.json         the help centre content
docs.html         a static copy of the help, for linking and search engines
help.js
help.css
```

Replace those six in the repository and the site updates on the next Pages build. The rest of the repository
(`pyodide/`, `plotly.min.js`, the sample files and the two large data bundles) only changes when the runtime or
the word lists change.

## Licensing and attribution

**Your collected data carries its own terms.** Every row MARSAD collects records the licence it came with, and
the dashboard repeats it above the numbers. Collecting text is not permission to republish it. Wikipedia and
Stack Exchange text is CC BY-SA and must keep its attribution and licence; Wikidata and the scholarly metadata
are CC0; each Hugging Face dataset carries its own licence; and posts from the social platforms are covered by
each platform's terms, which generally do not allow redistributing the content. Analyse them and report on them,
but publish ids and links rather than a copy of the corpus.

**Research word lists.** Two of the lexicons used here (MADAR and NileULex) are licensed for research use but
not for redistribution. The browser build carries them sealed: keyed hashes of the terms and none of the words,
so the analyses run and nobody can download the lexicon from this site. See `NOTICE_THIRD_PARTY.md` for the full
list of resources and what each one allows.

**The code.** No licence is declared for this repository yet. Until one is added, treat the code as all rights
reserved.

## Citing it

If you use MARSAD Analytics in published work, please cite it as software and say which version you used, which
source you collected from, which search, and on which date. The collected file records the exact collection time
in its `collected_at` column, and the licence in its `rights` column, so both are there when you write the
method section.

> Zaghouani, W. MARSAD Analytics (version 1.5) [software]. MARSAD Observatory, Northwestern University in Qatar.
> https://wajdiz.github.io/marsad-analytics

## Limits worth knowing

Each collection is one query at one moment and none of them is a complete sample of anything. GDELT indexes the
outlets it indexes. A Mastodon server sees the part of the network it federates with. Bluesky and YouTube rank
before they return. A Wikipedia article is the version that existed when you collected it. Word lists and word
models are approximations with known error rates, and every card that uses one says so on its own face.

A browser can only call a site that agrees to be called from another site. Some sources agree, some do not, and
a few change their minds. When one refuses, the page says so plainly and collects nothing, rather than failing
silently.

## Contact

Wajdi Zaghouani, Northwestern University in Qatar.
[ORCID 0000-0003-1521-5568](https://orcid.org/0000-0003-1521-5568)
