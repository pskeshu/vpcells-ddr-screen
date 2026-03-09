# vpCells DDR Screen: Agentic Microscopy for DNA Damage Response

Companion repository to a CeMM Pre-ERC Postdoc Program application.

This repo explores how the [vpCells](https://vpcells.cemm.at/) clone collection
(Reicher et al., *Nature Cell Biology*, 2024) can be used for agentic screening of
DNA damage response (DDR) protein dynamics, orchestrated by
[gently](https://github.com/pskeshu/gently).

## What's here

```
data/
  vpcells_ddr_proteins.csv   53 DDR proteins found in the vpCells collection
  vpcells_ddr_clones.csv     855 usable clones containing DDR proteins

scripts/
  extract_ddr_clones.py      Reproduces the data extraction from vpCells Supp. Table 4

notebooks/
  ddr_screen_design.ipynb    Designing a 41-clone DDR pool + simulated gently reasoning loop
```

## Key findings

- **53 DDR-related proteins** are present in the vpCells collection (out of ~200 searched)
- **855 clones** have at least one DDR protein tagged, **404** are in the validated set
- Strong coverage of chromatin remodelers, mismatch repair, NER, cohesin, and phase
  separation proteins
- Classic DDR foci markers (PCNA, RAD51, 53BP1, gamma-H2AX) are **absent** — expanding
  the collection for DDR is a proposed deliverable

## The proposal

1. Make the vpCells infrastructure agentic using gently
2. Bridge imaging (vpCells) and transcriptomics (CellWhisperer) through gently's
   reasoning layer
3. Apply to DDR biology: assemble custom pools of DDR-relevant clones, treat with
   DNA damaging agents, let gently close the observe-reason-act loop

## Data source

Supplementary Table 4 from:

> Reicher, A., Reiniš, J., Ciobanu, M. et al. Pooled multicolour tagging for
> visualizing subcellular protein dynamics. *Nat Cell Biol* **26**, 745–756 (2024).
> https://doi.org/10.1038/s41556-024-01407-w

## Related

- [gently](https://github.com/pskeshu/gently) — agentic microscopy framework
- [deepthought](https://github.com/pskeshu/deepthought) — adaptive microscopy software
- [anton](https://github.com/pskeshu/anton) — VLM-based phenotype ontology (prototype)
- [CellWhisperer](https://cellwhisperer.bocklab.org) — multimodal AI for single-cell transcriptomics
