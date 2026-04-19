# FragranceDNA

FragranceDNA solves a practical problem: people new to perfumery often struggle to articulate what they like beyond naming specific fragrances that they enjoy.  This project addresses that in two ways – given a fragrance that the user enjoys, it returns a ranked list of similar fragrances, and it identifies which ‘archetypes’ best describe their taste, giving them a vocabulary to use when exploring on their own.

This project was based on coursework from DAT-325  Data Validation: Quality and Cleaning and MAT-350 Applied Linear Algebra, and was influenced by my own independent research, particularly Aggarwal’s Recommender Systems: The Textbook, from which I developed the approach of TF-IDF weighting and cosine similarity.  The nonnegative matrix factorization (NMF) aspect brings the project into machine learning territory, where the algorithm independently discovers 15 distinct fragrance ‘archetypes’, which I then interpreted and named based on my own knowledge of perfumery. 

**[Try it on my ePortfolio](https://zerobaseone.github.io/eportfolio/)**

---

## How It Works

The algorithm runs almost entirely client-side using Pyodide, (Python in the browser via WebAssembly):

1. Parses ~24,000 fragrances from a semicolon-delimited CSV
2. Builds a sparse fragrance-note matrix (~24k rows × ~1,600 columns) using a dictionary hash map for O(1) column lookup
3. Applies TF-IDF weighting to down-weight common notes (e.g. musk, which appears in over half the dataset) and surface more distinctive ones
4. Computes cosine similarity between the input fragrance and all others to generate recommendations

The NMF component does not run in bwrowser as sklearn proved too memory-intensive. The NMF W matrix is precomputed and served as JSON, where `W[i][j]` represents how strongly fragrance `i` corresponds to archetype `j`.

---

## The 15 Archetypes

Nonn-negative matrix factorization (NMF) independently discovers fragrance archetypes, producing a list of top notes that a human can interpret. The algorithm was run at several values of k; k=15 was selected as any larger values got too specific, and smaller k values grouped together larger categories that I feel should have been distinctive.  The categories are below:

| # | Archetype | Key Notes |
|---|-----------|-----------|
| 1 | Fruity Rose | peach, black currant, raspberry, plum, rose, vanilla |
| 2 | Aromatic Fougère | lavender, geranium, cardamom, vetiver, mint, sage |
| 3 | Oud | Cambodian/Hindi/Vietnamese oud, saffron, guaiac wood, ambergris |
| 4 | Vintage Floral | carnation, oakmoss, aldehydes, orris root, lily-of-the-valley, civet |
| 5 | White Floral | orange blossom, tuberose, ylang-ylang, gardenia, jasmine sambac |
| 6 | Gourmand | tonka bean, vanilla, almond, caramel, coffee, praline, cacao |
| 7 | Niche Citrus | Virginia cedar, Amalfi lemon, oakmoss, African orange flower, labdanum |
| 8 | Powdery Violet | iris, violet, mimosa, heliotrope, vetiver, musk |
| 9 | Hesperidic | neroli, petitgrain, bitter orange, bergamot, orange blossom |
| 10 | Fresh Citrus | lemon, orange, cedar, lime, bergamot, musk |
| 11 | Resinous Amber | labdanum, cinnamon, incense, benzoin, clove, myrrh |
| 12 | Fresh Floral | freesia, peony, pear, magnolia, lily-of-the-valley, musk |
| 13 | Fruity Aquatic | apple, pineapple, melon, birch, coconut |
| 14 | Modern Musk | pink pepper, white musk, jasmine sambac, ambroxan, cashmeran |
| 15 | Leather & Tobacco | leather, tobacco, artemisia, thyme, juniper berries, patchouli, rum |


---

## Dataset

~24,063 fragrances scraped from [Fragrantica](https://www.fragrantica.com), sourced via Kaggle. Each entry includes top, middle, and base notes across 1,671 unique note terms.

Data cleaning included 40 manual note corrections (misspellings, synonyms, regional variants) applied in-memory via a dictionary map, preserving the original CSV.

Generic descriptors (e.g. "woody notes," "spicy notes") are excluded from NMF input but retained in the weighted similarity matrix.

---

## Stack

- **Python** — NumPy, scikit-learn
- **Pyodide** — Python runtime in the browser via WebAssembly
- **JavaScript** — frontend, CSV parsing

---

## References

- Aggarwal, C.C. *Recommender Systems: The Textbook*. Springer, 2016.
