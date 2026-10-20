# Dataset Citations and Licensing

This repository uses several datasets for academic research on Chinese digital diplomacy and information operations. Each dataset has its own licensing terms and attribution requirements documented below.

---

## 1. Cofacts Taiwan Fact-Checking Platform

**Source:** Cofacts/line-msg-fact-check-tw (HuggingFace)
**License:** CC BY-SA 4.0
**Directory:** `taiwan-case-study/cofacts_data/`

> This editorial data is sourced from the "Cofacts" message-reporting bot and fact-checking collaboration community, provided under CC BY-SA 4.0 license. To supplement information, please visit the Cofacts LINE bot at https://line.me/ti/p/@cofacts

**Citation:**
Cofacts Working Group. Cofacts LINE Message Fact-Check Dataset (Taiwan). Available at: https://huggingface.co/datasets/Cofacts/line-msg-fact-check-tw

**Full legal terms:** See `taiwan-case-study/LEGAL.md` and `taiwan-case-study/LEGAL_EN.md`

---

## 2. 50 Cent Army Replication Data (King, Pan, and Roberts)

**Source:** Harvard Dataverse
**License:** No explicit license set. Per the dataset record: *"This dataset is made available without information on how it can be used. You should communicate with the Contact(s) specified before use."* Harvard Dataverse's default terms apply (see below).
**Directory:** `50-cent-army/`

**Citation:**
King, Gary, Jennifer Pan, and Margaret E. Roberts. 2017. "How the Chinese Government Fabricates Social Media Posts for Strategic Distraction, Not Engaged Argument." *American Political Science Review* 111(3): 484-501. https://doi.org/10.1017/S0003055417000144

**Replication data:** https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/QSZMPD

**Note:** Raw data files are excluded from this repository (gitignored). Only analysis notebooks referencing this data are included. No dataset content is redistributed.

---

## 3. China Twitter Hashed (Twitter Information Operations Archive)

**Source:** Twitter Transparency Center — Information Operations
**License:** Twitter/X Terms of Service for research use
**Directory:** `chn-twitter-hashed/`

**Citation:**
DiResta, Renee, and Shelby Goldstein. "Full-Spectrum Propaganda in the Social Media Era: China's CCP Twitter Behavior." *Security Studies*.

**Data source:** https://transparency.twitter.com/en/reports/moderation-research.html

**Note:** User IDs are hashed for privacy. Redistribution of bulk tweet data is restricted under Twitter's Terms of Service.

---

## 4. V-Dem (Varieties of Democracy) Dataset v16

**Source:** V-Dem Institute, University of Gothenburg
**License:** Free for academic use with citation
**Directory:** `V-Dem-CY-FullOthers-v16_csv/`

**Citation:**
Coppedge, Michael, John Gerring, Carl Henrik Knutsen, Staffan I. Lindberg, Jan Teorell, David Altman, Fabio Angiolillo, Michael Bernhard, Cecilia Leal, Agnes Cornell, M. Steven Fish, Lisa Gastaldi, Haakon Gjerlow, Adam Glynn, Ana Good God, Sandra Grahn, Allen Hicken, Katrin Kinzelbach, Joshua Krusell, Kyle L. Marquardt, Kelly McMann, Valeriya Mechkova, Juraj Medzihorsky, Natalia Natsika, Anja Neundorf, Pamela Paxton, Daniel Pemstein, Josefine Pernes, Oskar Ryden, Johannes von Romer, Brigitte Seim, Rachel Sigman, Svend-Erik Skaaning, Jeffrey Staton, Aksel Sundstrom, Eitan Tzelgov, Yi-ting Wang, Tore Wig, Steven Wilson, and Daniel Ziblatt. 2026. "V-Dem [Country-Year/Country-Date] Dataset v16." Varieties of Democracy (V-Dem) Project. https://doi.org/10.23696/vdemds26

---

## 5. China Global Public Diplomacy Indicators Dataset v1.6

**Source:** AidData at William & Mary
**License:** Academic use with citation
**Directory:** `China's Global Public Diplomacy Indicators Dataset, Version 1.6 - Data Packet/`

**Citation:**
AidData. 2022. *China's Global Public Diplomacy Indicators Dataset, Version 1.6.* Williamsburg, VA: AidData at William & Mary.

---

## 6. Chinese State Media / Facebook Ads (Harvard Dataverse)

**Source:** Harvard Dataverse
**License:** Dataverse default terms (typically CC0 1.0, verify against specific record)
**Directory:** `dataverse_files 3/`

**Note:** Verify the specific Dataverse record for exact licensing terms applicable to this replication package.

---

## Licensing Note

The Cofacts dataset (Section 1) is licensed under **CC BY-SA 4.0**, which requires derivative works to be shared under the same license. Analysis notebooks in this repository that derive findings from Cofacts data are therefore subject to CC BY-SA 4.0 terms.

All other analysis code in this repository is original academic research work.
