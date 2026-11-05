# Protein design practical

Course notebooks for the de novo protein design practical,
University of Tartu.

Both run entirely in Google Colab on a free T4 GPU — nothing to install.

| Notebook | What it does | Open |
|---|---|---|
| `boltzgen_binder_design.ipynb` | Choose a target protein, a site on it and a binder type; BoltzGen designs the binder, folds it and scores it. Then read the scorecard — interface confidence, pLDDT, pAE and developability liabilities | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/markolooke/protein-design-practical/blob/main/boltzgen_binder_design.ipynb) |
| `protein_design_practical.ipynb` | Delete part of a real protein and let ESM3 invent replacements; measure how much of the protein survived | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/markolooke/protein-design-practical/blob/main/protein_design_practical.ipynb) |

## How to run either one

Both notebooks work the same way. **All your choices are in one cell at the
top** — nothing below it asks you anything.

1. Open the notebook, click **Copy to Drive**
2. **Runtime → Change runtime type → T4 GPU**, Save
3. Fill in the choices cell at the top
4. **Runtime → Run all**, then leave it alone

To try something different, change the top cell and Run all again.

## Before the session

Run **the setup cell** once at home. It downloads 5–6 GB and takes about ten
minutes. Doing it in advance avoids ten people downloading simultaneously on
classroom wifi.

Also worth five minutes of thought: which protein you want to work on, and why.

## Checking your result

Both notebooks end by printing sequences to paste into
[AlphaFold Server](https://alphafoldserver.com) (free, needs a Google account).
That is an independent check — the model that made your design is not a
neutral judge of it.

## Credits

BoltzGen: Stark et al., 2025 (MIT). ESM3: Hayes et al., *Science* 2025 (MIT).
