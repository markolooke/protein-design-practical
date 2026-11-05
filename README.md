# Protein design practical

Course notebooks for the de novo protein design practical,
University of Tartu.

Both run entirely in Google Colab on a free T4 GPU — nothing to install.

| Notebook | What it does | Open |
|---|---|---|
| `boltzgen_binder_design.ipynb` | Design a binder to a real protein with BoltzGen, then read its scorecard — interface confidence, refold RMSD, and developability liabilities | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/markolooke/protein-design-practical/blob/main/boltzgen_binder_design.ipynb) |
| `protein_design_practical.ipynb` | Mask part of a real protein and let ESM3 fill it back in; measure what survived | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/markolooke/protein-design-practical/blob/main/protein_design_practical.ipynb) |

## Before the session

1. Open the notebook, click **Copy to Drive**
2. **Runtime → Change runtime type → T4 GPU**, Save
3. Run the **first cell only**. It downloads several GB and takes ~10 minutes.

Doing this at home avoids ten people downloading simultaneously on classroom wifi.

## Credits

BoltzGen: Stark et al., 2025 (MIT). ESM3: Hayes et al., *Science* 2025 (MIT).
