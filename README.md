# Protein design practical

Course notebooks for the de novo protein design practical,
University of Tartu.

Both run entirely in Google Colab on free hardware — nothing to install.

| Notebook | What it does | Open |
|---|---|---|
| `boltzgen_binder_design.ipynb` | Choose a target protein, a site on it and a binder type; BoltzGen designs the binder, folds it and scores it. Then read the scorecard — interface confidence, pLDDT, pAE and developability liabilities | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/markolooke/protein-design-practical/blob/main/boltzgen_binder_design.ipynb) |
| `protein_design_practical.ipynb` | Delete part of a real protein and let ESM3 invent replacements; measure how much of the protein survived | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/markolooke/protein-design-practical/blob/main/protein_design_practical.ipynb) |

## Run them both at once

Colab gives you **one GPU at a time**, but ESM3 is small enough to manage without
one. So open both notebooks in two browser tabs and give them different runtimes:

| Notebook | Runtime | A run takes |
|---|---|---|
| `boltzgen_binder_design.ipynb` | **T4 GPU** | ~20 min |
| `protein_design_practical.ipynb` | **CPU** | a minute or two |

**Start the binder notebook first, then spend its twenty minutes on the other
one.** That is the whole plan.

### In each tab

1. Open the notebook — it runs straight away, nothing to copy first
2. **Runtime → Change runtime type**, pick T4 GPU or CPU as above, Save
3. Fill in the choices cell at the top — all your choices are in that one cell
4. **Runtime → Run all**, then leave it alone

To try something different, change the top cell and Run all again.

Click **Copy to Drive** only if you want to keep your work. Without it the
notebook still runs normally, but your choices and results are gone when you
close the tab.

## Before the session

Run **the setup cell** of each once at home. Together they pull down about 11 GB
and take ten minutes or so. Doing it in advance avoids ten people downloading
simultaneously on classroom wifi.

Also worth five minutes of thought: which protein you want to work on, and why.

## Checking your result

Both notebooks end by printing sequences to paste into
[AlphaFold Server](https://alphafoldserver.com) (free, needs a Google account).
That is an independent check — the model that made your design is not a
neutral judge of it.

## If nothing works on the day

`sample_output/` holds real output from both notebooks, so the analysis still
happens even if Colab will not cooperate: a finished binder design with its
scorecard, an independent refold with a **full PAE matrix**, and two ESM3
redesigns. See [`sample_output/README.md`](sample_output/README.md) — it has the
ChimeraX commands for each file and the numbers to look for.

## The real structures

Every protein in both notebooks has been solved experimentally. Fetch one in
ChimeraX with `open <code>` and superimpose your design on it.

| Protein | PDB | Method | Used by |
|---|---|---|---|
| Trp-cage, 20 aa | `1L2Y` | NMR | ESM3 |
| Villin headpiece, 36 aa | `1VII` | NMR | ESM3 |
| Protein G B1 domain, 56 aa | `1PGA` | X-ray 2.07 Å | both |
| Ubiquitin, 76 aa | `1UBQ` | X-ray 1.80 Å | both |
| Lysozyme, 129 aa | `4LZT` | X-ray 0.95 Å | BoltzGen |
| GFP, 238 aa | `1GFL` | X-ray 1.90 Å | ESM3 |

Every sequence matches its entry residue for residue except GFP: the notebook
has wild-type avGFP and `1GFL` differs at two positions (M1A, Q80R). Trp-cage
and villin are NMR ensembles, so "the real structure" is already a spread of
models rather than one answer.

## Looking at the structures yourself

Download the `.cif` files from the notebooks (and the zip from AlphaFold Server),
then open them in [ChimeraX](https://www.cgl.ucsf.edu/chimerax/) — free, and it
reads all of it. A few commands worth knowing:

```
open 1UBQ                                    fetch the target from the PDB
open ~/Downloads/design.cif                  your design

color bfactor palette alphafold key true     AlphaFold models  (pLDDT 0-100)
color bfactor palette esmfold key true       BoltzGen .cif     (pLDDT 0-1)

alphafold pae #1 file fold_..._full_data_0.json    the PAE plot (ChimeraX 1.8+)
alphafold contacts #1/B toAtoms #1/A maxPae 5      only the confident contacts

matchmaker #2 to #1                          superimpose, RMSD goes to the Log
color #1/A gray; color #1/B orange           target grey, your binder orange
show #1/A:44 atoms; style #1/A:44 stick      pick out the site you asked for

save ~/Desktop/design.png width 2000 supersample 3
```

`#1` and `#2` are model numbers from the Models panel, `/A` is a chain and `:44`
a residue. The two palettes differ because BoltzGen stores pLDDT on a 0–1 scale
while AlphaFold uses 0–100 — `esmfold` has its stops in the right places for the
former.

## Credits

BoltzGen: Stark et al., 2025 (MIT). ESM3: Hayes et al., *Science* 2025 (MIT).
