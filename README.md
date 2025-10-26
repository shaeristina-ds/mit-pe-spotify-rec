# mit-pe-spotify-rec

---

# Music Recommendation System (MSD / Echo Nest – Spotify-style)

A reproducible notebook that builds and evaluates music recommenders (popularity baseline, collaborative filtering, and content-style variants) using the Echo Nest **Taste Profile Subset** of the **Million Song Dataset**.

> **Goal:** Recommend the **Top-10** songs a user is most likely to listen to, given historical user–song play counts and basic song metadata.

## 🔎 Project Overview

* **Why it matters:** With millions of tracks, manual discovery is hard. Recommenders improve user experience and engagement by surfacing relevant songs.
* **What this repo contains:** A single, end-to-end Jupyter notebook:

  * Data loading & cleaning
  * EDA on users, items, play counts
  * Baselines (global/popular)
  * Collaborative filtering (user–item signals)
  * Ranking Top-K per user
  * Offline evaluation and discussion (precision/recall@K, hit-rate style)

## 🗂️ Repository Structure

```
mit-pe-spotify-rec/
├── Music_Recommendation_System_Full_Code.ipynb   # main notebook
├── README.md                                     # this file
└── .gitattributes / .idea                        # repo configs
```

## 📦 Data

This project uses the **Echo Nest Taste Profile Subset** (user–song–playcount triplets) aligned to the **Million Song Dataset** (song metadata).

* Dataset home & docs: Taste Profile (MSD) ([millionsongdataset.com][1]), MSD overview ([millionsongdataset.com][2]).
* Kaggle mirrors/variants exist for convenience (MSD Challenge page, subsets) ([Kaggle][3]).

> **Note on large files:** GitHub limits files to 100 MB. Place raw data locally (e.g., `./data/`), which is **not** committed to the repo. If using LFS, configure it before adding big CSVs.

**Expected inputs (typical names):**

* `song_data.csv` → `song_id, title, release, artist_name, year`
* `count_data.csv` → `user_id, song_id, play_count`

## 🛠️ Setup

**Prereqs:** Python 3.9+ recommended, Jupyter.

Install the usual stack:

```bash
pip install numpy pandas scikit-learn scipy ipywidgets tqdm
```

> If you plan to try matrix-factorization/implicit feedback, also:
> `pip install implicit` (CPU) or follow project docs for GPU builds.

## ▶️ How to Run

1. Clone the repo and open the notebook.
2. Create a local `data/` folder and drop the CSVs there.
3. In the first data-loading cell, confirm the file paths (e.g., `./data/count_data.csv`, `./data/song_data.csv`).
4. Run all cells. The notebook will:

   * Load & clean data
   * Build baselines and CF models
   * Generate Top-10 recommendations
   * Print evaluation metrics and example outputs

## 🧠 Methods (high level)

* **Popularity baseline:** ranks globally most-played songs (performs surprisingly well with heavy-tail data).
* **Collaborative Filtering (CF):** learns from user–item co-listening signals; standard approach behind many DSP recommenders ([Spotify][4]).
* **(Optional) Content-style signals:** if you add audio/metadata features later, you can blend with CF to mitigate cold-start (a common issue in CF) ([Wikipedia][5]).

## 📏 Evaluation (offline)

The notebook demonstrates ranking evaluation with Top-K metrics such as **Precision@K**, **Recall@K**, and simple **hit-rate** on held-out interactions. (You can extend to MAP@K / NDCG@K as needed.)

## 🚧 Known Constraints & Notes

* **Sparsity & Cold Start:** User–item matrices are large and sparse; new users/songs need special handling (e.g., popularity priors, content features) ([Wikipedia][5]).
* **Data Hygiene:** The Taste Profile subset has documented quirks; see MSD notes and challenge paper for details and baselines ([Microsoft Learn][6]).
* **Scale:** For faster prototyping, start with a reduced slice of the data (the notebook includes guidance to work with smaller samples).

## 🔮 Roadmap Ideas

* Add **matrix factorization** (ALS via `implicit`) with confidence weighting.
* Compute **NDCG@K**, **MAP@K**, coverage, and diversity metrics.
* Build a **hybrid** model (CF + audio/metadata) to improve cold-start.
* Package the notebook into a **module** + CLI, and/or expose a simple **FastAPI** inference endpoint.

## 🙏 Acknowledgments

* **Million Song Dataset** & **Echo Nest Taste Profile Subset** (Thierry Bertin-Mahieux, Daniel P. W. Ellis, Brian Whitman, Paul Lamere) ([millionsongdataset.com][2]).
* MSD Challenge paper with baselines and task framing ([ee.columbia.edu][7]).
* Background explainers on Spotify-style recommenders and CF ([Stratoflow][8]).

## 📚 References

* Million Song Dataset (overview) ([millionsongdataset.com][2])
* Echo Nest Taste Profile Subset (details) ([millionsongdataset.com][1])
* MSD Challenge (task & baselines) ([ee.columbia.edu][7])
* Dataset citations & known issues (MS Docs sample page) ([Microsoft Learn][6])
* CF & Spotify context (articles/primers) ([Stratoflow][8])

---

Want me to tailor this to exactly what your notebook implements (e.g., the precise algorithms/metrics/outputs it prints) and add a “Results” section with example screenshots?

[1]: https://millionsongdataset.com/tasteprofile/?utm_source=chatgpt.com "The Echo Nest Taste Profile Subset"
[2]: https://millionsongdataset.com/?utm_source=chatgpt.com "Million Song Dataset: Welcome!"
[3]: https://www.kaggle.com/c/msdchallenge/data?utm_source=chatgpt.com "Million Song Dataset Challenge"
[4]: https://www.spotify.com/safetyandprivacy/understanding-recommendations?utm_source=chatgpt.com "safetyandprivacy/understanding-recommendations"
[5]: https://en.wikipedia.org/wiki/Collaborative_filtering?utm_source=chatgpt.com "Collaborative filtering"
[6]: https://learn.microsoft.com/en-us/samples/azure-samples/millionsongdataset-sql/millionsongdataset-sql/?utm_source=chatgpt.com "Million Song Dataset in Azure SQL DB / SQL Server"
[7]: https://www.ee.columbia.edu/~dpwe/pubs/McFeeBEL12-MSDC.pdf?utm_source=chatgpt.com "The million song dataset challenge"
[8]: https://stratoflow.com/spotify-recommendation-algorithm/?utm_source=chatgpt.com "Spotify Recommendation Algorithm: What's The Secret to ..."
