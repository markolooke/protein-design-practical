# Sample output — the backup set

Real output from both notebooks, committed here so the practical still works if
Colab will not cooperate. Download this folder and you can do the whole
analysis in [ChimeraX](https://www.cgl.ucsf.edu/chimerax/) without running
anything.

Everything below was generated on the University of Tartu cluster with the same
code the notebooks run.

---

## 1 · `boltzgen/` — a real binder design

A miniprotein designed against **ubiquitin (1UBQ chain A)**, aimed at the
**Ile44 hydrophobic patch** — the default choices in the notebook.

| File | What it is |
|---|---|
| `1_target_1UBQ_chainA.pdb` | the cleaned target, exactly what went in |
| `2_design_top.cif` | the best design: target **and** binder, with pLDDT in the B-factor column |
| `3_all_designs_metrics.csv` | the full scorecard for all designs |
| `4_sequences.txt` | both sequences, ready to paste into AlphaFold Server |

**How it scored:** ipTM **0.534** (weak), interface pAE **3.30 Å**,
liabilities: ProtTrypx8(pos6-45,sev10) (8 violations). The binder is 52 residues and
resembles the target **27%** — so it is its own protein.

### Two of the eight designs are copies of the target

This run is a good example of the shortcut these models take. Resemblance to the
target, across all eight designs:

| design | ipTM | resembles the target |
|---|---|---|
| `design_3` | 0.170 | **76%** |
| `design_0` | 0.143 | **66%** |
| `design_4` | 0.209 | **41%** |
| `design_5` | 0.534 | **27%** |
| `design_6` | 0.233 | **23%** |
| `design_7` | 0.293 | **22%** |
| `design_2` | 0.216 | **20%** |
| `design_1` | 0.514 | **18%** |

Unrelated proteins score 15–25% on this measure. `design_3` at
**76%** and `design_0` at **66%** are not binders
at all — they are near-copies of ubiquitin, which of course docks nicely
against ubiquitin. Both ranked near the bottom on ipTM, so the scoring caught
them, but a student sorting on sequence plausibility alone would not have.

```
open 2_design_top.cif
color bfactor palette esmfold key true
```

Use the **`esmfold`** palette, not `alphafold` — BoltzGen writes pLDDT on a
**0–1** scale and the `esmfold` palette has its stops at 0, 0.5, 0.7, 0.9, 1.0.
The `alphafold` palette expects 0–100 and would paint the whole model one
colour.

```
color /A gray; color /B orange
show /A:44 atoms; style /A:44 stick
```

Grey target, orange binder, and the residue you asked it to bind picked out in
sticks. Now look: **did the binder actually go there?**

---

## 2 · `refold_boltz2/` — the independent check, with a real PAE matrix

BoltzGen scores its own design, which is not an independent opinion. Normally
you would paste the two sequences into AlphaFold Server. This folder is the
same idea done in advance, so you have a PAE matrix to look at either way.

The complex was **refolded from sequence alone by Boltz-2** — a different model
that never saw the design process.

| File | What it is |
|---|---|
| `complex_predicted.cif` | Boltz-2's own structure of target + binder |
| `complex_pae.npz` | the full 128×128 PAE matrix — **use this one** |
| `complex_pae.json` | the same matrix in AlphaFold's json shape |
| `complex_confidence.json` | Boltz-2's own confidence scores |

> The PAE here is **Boltz-2's**, not AlphaFold's. ChimeraX reads Boltz `.npz`
> PAE files directly, which is why that is the one to use. The `.json` is the
> same matrix written in the shape AlphaFold Server produces, so the command
> from the slides works on it — if your ChimeraX will not open it, use the npz.

```
open complex_predicted.cif
alphafold pae #1 file complex_pae.npz
alphafold contacts #1/B toAtoms #1/A maxPae 5
```

Open the plot **before** running `contacts`. The second command draws only the
contacts the model is confident about, so an empty result is itself an answer.

**What to look for:** the off-diagonal block, where the target's residues meet
the binder's. Dark means the model knows where the two chains sit relative to
each other; pale means it is treating them as two unrelated objects that happen
to be in the same file.

In this matrix the best cross-chain PAE is **2.3 Å** and the average
across the whole interface block is **5.0 Å**. Under 5 Å would be
confident; over 10 Å means it does not really know.

### The two models do not agree, and that is the interesting part

| | ipTM |
|---|---|
| BoltzGen, scoring its own design | **0.534** |
| Boltz-2, refolding it from sequence alone | **0.773** |

Same complex, same scale, two different answers. Ask the students which one they
believe and why. Neither model is lying; they were trained differently and are
confident about different things. This is exactly why you check a design against
something that did not make it.

---

## 3 · `esm3/` — two redesigns, and why they differ

| File | What it is |
|---|---|
| `villin_native.pdb` | villin headpiece, folded by ESM3 |
| `villin_30pct_design.pdb` | the same protein with 30% of its sequence deleted and reinvented |
| `ubiquitin_native.pdb` | ubiquitin, folded by ESM3 |
| `ubiquitin_80pct_design.pdb` | ubiquitin with **80%** deleted and reinvented |
| `sequences.txt` | the numbers for both |

```
open villin_native.pdb
open villin_30pct_design.pdb
matchmaker #2 to #1
```

The RMSD goes to the Log. Then do the same for the ubiquitin pair.

```
open ubiquitin_native.pdb
open ubiquitin_80pct_design.pdb
matchmaker #2 to #1
```

### The result worth arguing about

```
villin_30pct: 10/36 masked, identity at masked positions 40%, top residue L 19% (original L 14%)
  original MLSDEDFKAVFGMTRSAFANLPLWKQQNLKKEKGLF
  design   MLSDEDFKALFGMTRSEFANLPLWEKINLLKEKGLF

ubiquitin_80pct: 60/76 masked, identity at masked positions 100%, top residue L 12% (original L 12%)
  original MQIFVKTLTGKTITLEVEPSDTIENVKAKIQDKEGIPPDQQRLIFAGKQLEDGRTLSDYNIQKESTLHLVLRLRGG
  design   MQIFVKTLTGKTITLEVEPSDTIENVKAKIQDKEGIPPDQQRLIFAGKQLEDGRTLSDYNIQKESTLHLVLRLRGG
```

Villin had 30% of its sequence deleted and the model put back something
genuinely different. Ubiquitin had **80%** deleted — far more — and came back
much closer to the original.

That is not the model being better at ubiquitin. Ubiquitin is one of the most
conserved proteins known and appears thousands of times in the training data,
so "fill in the blanks" is closer to a lookup than a design. **When the output
looks like a real protein, check whether the model is simply reproducing one.**

---

## Saving a picture

```
save ~/Desktop/figure.png width 2000 supersample 3 transparentBackground true
```

## A reminder on the syntax

`#1` and `#2` are model numbers from the Models panel, `/A` is a chain, `:44` a
residue. All of it is in the ChimeraX menus too — nobody has to memorise it.
