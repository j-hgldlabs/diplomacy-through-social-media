# Diplomacy Through Social Media

Empirical research on Chinese digital public diplomacy and state-linked information operations. The project combines five independent datasets — organic social media, paid advertising, expert democracy indices, fact-checking records, and public-diplomacy indicators — to test a common thesis: that Chinese online influence activity is **systematic, state-directed, and event-driven** (intensifying around geopolitical flashpoints such as Taiwan's 2018 and 2020 elections), rather than a diffuse global trend.

A core analytical strategy is **cross-dataset convergent validation**: a finding in one source (e.g. Xinjiang narrative defense dominating the Twitter takedown archive) is corroborated in a completely independent source (e.g. `topic_xinjiang_terror` across 175 countries of Facebook advertising in the same period).

Each directory below is a self-contained dataset + analysis unit with its own Jupyter notebook(s). Licensing and full citations for every dataset are in [`CITATIONS.md`](CITATIONS.md).

## Setup

```bash
source .venv/bin/activate           # Python 3.14, prompt "ccp-social-media"
```

Key libraries (already installed in `.venv`): `pandas`, `polars`, `numpy`, `scipy`, `statsmodels`, `scikit-learn`, `networkx`, `matplotlib`, `seaborn`, `transformers`, `sentence-transformers`, `torch`, `datasets`, `bertopic`, `openpyxl`.

A HuggingFace token is required for the datasets pulled from the Hub (Cofacts, and any `load_dataset` calls). It is read from `.env`:

```
HF_TOKEN=<your token>
```

Notebooks load it with `load_dotenv(os.path.join(os.path.dirname(os.getcwd()), '.env'))` — i.e. the `.env` at the repo root, one level above each dataset directory.

> **Data files are gitignored.** All `*.csv`, `*.xlsx`, `*.zip`, `*.pdf`, and `*.png` are excluded (see `.gitignore`). The repository tracks **analysis code only** — no dataset content is redistributed. To reproduce, obtain each dataset from the source in `CITATIONS.md` and place it in the corresponding directory.

---

## Datasets

### `chn-twitter-hashed/` — Twitter Information Operations Archive

State-linked accounts Twitter/X attributed to Chinese operations and removed for coordinated inauthentic behavior. User IDs are hashed; tweet content and metadata are preserved. Source: [Twitter Transparency Center](https://transparency.twitter.com/en/reports/moderation-research.html).

- **`China_Twitter_Hashed.csv`** — one row per tweet. Columns include `tweetid`, `userid`, `retweet_userid`, `follower_count`, `tweet_text`, `tweet_time`, `is_retweet`, `hashtags`, `user_mentions`. (Read `tweetid`/`retweet_userid` as `str` to avoid precision loss.)
- **`china-twitter-hashed-analysis.ipynb`** — builds a directed retweet graph with NetworkX (`userid → retweet_userid`), ranks accounts by in-degree and **PageRank** to surface amplification hubs, runs temporal volume analysis against the 2018/2020 election calendar, and applies **BERTopic** to English tweets. Two headline findings: Xinjiang narrative defense as the dominant topic (~11,721 tweets), and ~30% "fortune-cookie" filler content — itself documented as a known Spamouflage tactic.
- **`Full Spectrum Paper Replication.R`** — R replication referencing DiResta & Goldstein, *"Full-Spectrum Propaganda in the Social Media Era."*

### `50-cent-army/` — King, Pan & Roberts Replication (Weibo / gov posts)

Leaked posts from the Chinese government's astroturfing apparatus. Source: [Harvard Dataverse](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/QSZMPD) (King, Pan & Roberts 2017, *APSR*).

- **`posts_all.csv`** — the fabricated posts (Chinese-language `content`, `site`, `PostDate`, poster/organization fields).
- **`knownWeibos_zg.csv`** — Weibo posts with forwarding metadata (`account_id`, `f_account_id`, `forwarded`, like/comment/forward counts) used to build a repost network.
- **`50-cent-analysis.ipynb`** — translates Chinese posts to English via a **local Ollama server** (`qwen2.5-coder:3b` at `localhost:11434`), checkpointing to `translation_checkpoint.json` so runs are resumable. Then scores sentiment with `cardiffnlp/twitter-roberta-base-sentiment-latest` and runs a **one-sample t-test** (H₁: mean sentiment > 0) to test whether the 50-cent army acts as net-positive "cheerleaders" for CCP policy.
- **`network-analysis.ipynb`** — forwards graph from `knownWeibos_zg.csv` (`account_id → f_account_id`).
- `ReadMeBootResults*.csv`, `ReadMes_PackageVersions.rtf`, `ReplicationFile/` — original replication artifacts.

> Running `50-cent-analysis.ipynb` end-to-end requires a **local Ollama instance** serving `qwen2.5-coder:3b`. Sentiment cells operate on the translation checkpoint, not the full corpus.

### `dataverse_files 3/` — Chinese State Media Facebook Advertising

Panel of Chinese state-media Facebook ad activity: 175 countries × daily observations, 2018–2020. Source: Harvard Dataverse replication package.

- **`china_state_media_data.csv`** — one row per country-day. Columns: `date`, `iso`, `country`, `gdelt_sentiment`, `impressions`, six topic flags (`topic_huawei_safe`, `topic_poverty_victory`, `topic_xinjiang_terror`, `topic_bri_cooperation`, `topic_covid_recovery`, `topic_hong_kong_riot`), `events`, `diplomatic`, trade/FDI controls, and `impressions_with_labels` / `impressions_without_labels` (ad labels began Jun 4 2020 = day 886).
- **`fb ads.csv`** — 974 individual ad creatives (`ad_creative_body`, `translated_body`, `impressions_lower_bound`/`upper_bound`, ICEWS event linkage).
- **`chinese_state_media_analysis.ipynb`** — tests event-driven targeting via **90-day pre-election window comparisons** (2018 vs 2020), the BRI narrative concentration hypothesis (BRI vs non-BRI recipient countries), cross-dataset validation against the Twitter Xinjiang finding, and a regression of diplomatic outcomes on advertising intensity. Produces the publication figures (`*.png`).
- **`panelmatch_python.py`** — Python port of [PanelMatch](https://github.com/insongkim/PanelMatch) for causal inference on the panel, with custom missing-value handling (note: `"NA"` is the ISO code for Namibia and must **not** be parsed as null — see the `NANS` list).
- **`matched_pairs.xlsx`**, `src/xlsx-parsers-matched-pairs.ipynb` — matched treatment/control pairs and the parser that produces them.

### `taiwan-case-study/` — Cofacts Fact-Checking Platform (Taiwan)

Crowd-sourced fact-checks from Taiwan's Cofacts LINE bot — used as the "resilience" case study. Source: [Cofacts on HuggingFace](https://huggingface.co/datasets/Cofacts/line-msg-fact-check-tw), **CC BY-SA 4.0**.

- **`taiwan-hugging-face-data.ipynb`** — downloads the needed Cofacts tables (`articles`, `replies`, `article_replies`, `reply_requests`, `analytics`), joins them into one master frame (article text + flag count + verdict + response time), then measures (1) submission volume around the 2018/2020 elections and (2) whether **fact-checker response times got faster** between 2018 and 2020 — quantitative evidence of improving counter-disinformation infrastructure.
- **`cofacts_data/`** — the downloaded tables (large; `analytics.csv` ~1 GB). Regenerated by the notebook.
- **`LEGAL.md` / `LEGAL_EN.md` / `LICENSE`** — Cofacts CC BY-SA 4.0 terms. **Derivative findings from this data inherit CC BY-SA 4.0.**

### `V-Dem-CY-FullOthers-v16_csv/` — Varieties of Democracy v16

Expert-coded cross-national democracy indices, used as the macro-level information-environment baseline. Source: [V-Dem Institute](https://doi.org/10.23696/vdemds26).

- **`V-Dem-CY-Full+Others-v16.csv`** — country-year panel (large; loaded lazily with `polars.scan_csv`).
- **`v-dem-analysis.ipynb`** — compares disinformation indicators across five deliberately chosen countries: **China** (actor), **Taiwan** (primary target), **United States** (non-BRI democracy baseline), **Ethiopia** (BRI recipient), and **Hungary** (backsliding democracy, showing the pattern isn't China-unique). Key variables: `v2smgovdom` / `v2smgovab` (government false info, domestic/abroad), `v2smpardom` / `v2smparab` (party), `v2smfordom` (foreign govts targeting the country). **Scale caution: higher ≈ +4 means *less* disinformation, lower ≈ −4 means *more* — a downward trend is bad.** A central plot overlays China's outbound disinformation against Taiwan's received foreign interference.
- `codebook.pdf`, `cautionary_notes.pdf`, `whats_new.pdf`, `suggested_citation.pdf` — official V-Dem documentation.

> Also present: **`China's Global Public Diplomacy Indicators Dataset, Version 1.6 - Data Packet/`** (AidData) — global CI/Confucius-institute, student-exchange, and perception indicators, with an xlsx→csv parser in `src/`.

---

## Conventions

- **Notebook naming** — each dataset directory holds a `*-analysis.ipynb` (or similarly named) notebook; run it from within its own directory so the relative data paths and `.env` lookup resolve.
- **Reproducibility** — obtain raw data per `CITATIONS.md`, drop it in the matching directory, then run the notebook top to bottom. External dependencies to be aware of: a HuggingFace token (Cofacts), and a local Ollama server (50-cent translation).
- **Sentiment** — social-media text is scored with `cardiffnlp/twitter-roberta-base-sentiment-latest` (label map: `LABEL_0`=negative, `LABEL_1`=neutral, `LABEL_2`=positive), chosen over general-purpose/financial models because it is tuned on tweets.

## Citations & Licensing

See [`CITATIONS.md`](CITATIONS.md) for per-dataset citations and license terms. In brief: **V-Dem** and **AidData** are free for academic use with citation; the **Twitter IO archive** is governed by X's research ToS (no bulk redistribution); the **50-cent** data carries no explicit license (Harvard Dataverse defaults); **Cofacts** is **CC BY-SA 4.0** (share-alike applies to derivatives). Original analysis code in this repository is the authors' own academic work.
