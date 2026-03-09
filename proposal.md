# Agentic Microscopy for Mapping DNA Repair Dependencies

**P.S. Kesavan**
CeMM Pre-ERC Postdoc Program — Interview Preparation

---

## The question hierarchy

```
 ┌────────────────────────────────────────────────────────────┐
 │  PERSONALIZED MEDICINE                                      │
 │                                                             │
 │  Patient has a tumor. Which drug will work?                 │
 │  Sequencing tells us which DDR genes are mutated.           │
 │  But mutations alone don't tell us which repair             │
 │  pathways the tumor is relying on to survive.               │
 │                                                             │
 │  Q: For this patient's mutation profile, which backup       │
 │     repair pathways keep the tumor alive — and how          │
 │     do we shut them down?                                   │
 └──────────────────────────┬─────────────────────────────────┘
                            │
                            │ requires
                            │
 ┌──────────────────────────▼─────────────────────────────────┐
 │  REPAIR DEPENDENCIES                                        │
 │                                                             │
 │  When pathway A is broken, the cell compensates with        │
 │  pathway B. That's synthetic lethality. We know a few       │
 │  pairs (BRCA + PARP). There should be hundreds.             │
 │                                                             │
 │  Q: What is the complete dependency map — if you lose X,    │
 │     what do you rely on? For every repair pathway, across   │
 │     every damage type?                                      │
 └──────────────────────────┬─────────────────────────────────┘
                            │
                            │ requires
                            │
 ┌──────────────────────────▼─────────────────────────────────┐
 │  NETWORK DYNAMICS                                           │
 │                                                             │
 │  DDR is not one pathway. HR, NHEJ, NER, MMR, BER,          │
 │  Fanconi, TLS all compete and cooperate. The parts          │
 │  list is known. The coordination — who moves first,         │
 │  who depends on whom, how this changes with damage          │
 │  type — is not. We've only ever watched one or two          │
 │  proteins at a time.                                        │
 │                                                             │
 │  Q: What does the collective repair response actually       │
 │     look like in living cells, at scale?                    │
 └──────────────────────────┬─────────────────────────────────┘
                            │
                            │ requires
                            │
 ┌──────────────────────────▼─────────────────────────────────┐
 │  THE EXPERIMENT                                             │
 │                                                             │
 │        ┌───────────────────┐                                │
 │        │  vpCells freezer   │                                │
 │        └────────┬──────────┘                                │
 │                 │                                           │
 │           select & pool                                     │
 │                 │                                           │
 │        ┌────────▼──────────┐                                │
 │        │    MICROSCOPE     │                                │
 │        └────────┬──────────┘                                │
 │                 │                                           │
 │   ┌─────────────▼─────────────┐                             │
 │   │          gently            │                             │
 │   │  OBSERVE ▶ REASON ▶ ACT   │                             │
 │   │     │         │            │                             │
 │   │   anton   CellWhisperer   │                             │
 │   └───────────────┬───────────┘                             │
 │                   │                                         │
 │                iterate                                      │
 └───────────────────┼────────────────────────────────────────┘
                     │
                     ▼
 ┌───────────────────────────────────────────────────────────┐
 │  ANSWERS FLOW UP                                           │
 │                                                            │
 │  EXPERIMENT → relocalization maps: 50+ proteins,           │
 │    multiple drugs, temporal resolution                     │
 │                          │                                 │
 │  NETWORK → protein A moves before B. C only moves          │
 │    if D is present. Etoposide activates {these},           │
 │    cisplatin activates {those}. First systems-level        │
 │    view of DDR coordination.                               │
 │                          │                                 │
 │  DEPENDENCIES → when pathway X is blocked, proteins        │
 │    from pathway Y compensate. New synthetic lethal          │
 │    pairs beyond BRCA + PARP.                               │
 │                          │                                 │
 │  MEDICINE → patient has mutation in X. The map says         │
 │    they rely on Y. Block Y. Testable prediction.           │
 └───────────────────────────────────────────────────────────┘
```

---

## The clinical problem

Most cancer therapies work by damaging tumor DNA — chemotherapy, radiation, PARP
inhibitors. The tumor fights back using its DNA damage response (DDR) machinery.
When repair succeeds, the tumor survives. When it fails, the tumor dies.

We can sequence a patient's tumor and find DDR gene mutations. But mutations alone
don't predict drug response reliably. A gene can be present but its protein may not
function correctly. A pathway can be intact genetically but silent transcriptionally.
What matters is not which genes the tumor has, but which repair pathways are actually
working — and which the tumor depends on for survival.

If we know the dependencies, we can block the backup. That's synthetic lethality.
BRCA + PARP is the proof of concept: BRCA-mutant tumors rely on PARP-mediated repair,
block PARP, tumor dies. But this is one pair. The DDR has seven major repair pathways
(HR, NHEJ, NER, MMR, BER, Fanconi, TLS) with hundreds of proteins. The complete
dependency map — which pathways compensate for which, under what damage conditions —
does not exist.

## Why the map doesn't exist

Because nobody has watched the full repair network respond at once. Classical DDR
research: damage cells, fix them, stain for one protein, count foci. One protein at
a time. One damage type at a time. One timepoint. This gives you pathway diagrams.
It doesn't give you the systems-level picture — the temporal ordering, the crosstalk,
the collective coordination of dozens of proteins in living cells.

## What makes it possible now

Three platforms at CeMM, built independently, that together close the loop:

**vpCells** (Kubicek lab, Nature Cell Biology 2024): A collection of 4,576 cryopreserved
clonal cell lines, each expressing two endogenously tagged fluorescent proteins. Custom
pools of up to 41 clones can be assembled from the freezer, imaged in pooled format,
and computationally demultiplexed. 137 DDR-related proteins are already in the collection
(Reactome-annotated), covering chromatin remodelers, cohesin, mismatch repair, NER,
DSB repair, ubiquitin ligases, and phase separation proteins. The drug screening
infrastructure (1,152 compounds, high-content microscopy, CellProfiler pipeline) is
operational.

**CellWhisperer** (Bock lab, Nature Biotechnology 2025): A multimodal embedding connecting
single-cell RNA profiles and natural language. Trained on over a million transcriptomes.
Enables zero-shot biological reasoning — you can query it in natural language about
transcriptomic context. This provides the semantic layer: what does a relocalization
event mean biologically?

**gently** (Kesavan, open-source): An agentic microscopy framework implementing the
observe-reason-act loop. Instrument- and organism-agnostic. Currently deployed on a
light-sheet microscope at Janelia for C. elegans developmental imaging. gently's plan
mode coordinates multi-day experimental workflows with access to lab resources — strains,
reagents, instrument schedules, and here: the vpCells cryopreserved clone collection.

## The experiment

1. Assemble a custom pool of 41 DDR-relevant clones from the vpCells freezer
2. Image the pool at baseline (steady state)
3. Treat with a DNA-damaging agent (etoposide, cisplatin, NCS, etc.)
4. gently observes: which proteins relocated?
5. gently reasons: queries CellWhisperer for transcriptomic context, checks the vpCells
   collection for related proteins not yet in the pool
6. gently acts: adjusts temporal resolution for responding clones, flags new clones for
   the next round, schedules follow-up timepoints
7. Iterate within the experiment (minutes to hours) and between experiments (days — thaw
   new clones, assemble refined pool)

Each iteration narrows the search. The first round is broad (41 clones, one drug). By
round three, the pool is focused on the proteins that matter for that damage type, with
temporal resolution matched to each protein's kinetics.

## The perception layer: anton

The current vpCells analysis pipeline uses CellProfiler (1,134 handcrafted features) and
a random forest classifier. Localization annotations are assigned manually by experts
into 12 categories. This works but is static — features are predefined, labels are
categorical, and there is no natural language interface.

anton is a VLM-based pipeline for building a phenotype ontology from microscopy images.
Fine-tuning strategy:

- **Stage 1**: Pre-train on the Human Protein Atlas (~13M images, ~13,000 proteins with
  expert subcellular localization labels)
- **Stage 2**: Domain adaptation on vpCells images (endogenous tagging, different microscope
  and cell lines)
- **Stage 3**: Perturbation learning on the vpCells drug screen data (~94,000 treatment
  conditions with before/after image pairs)

This gives gently a perception system that can detect relocalization and describe it in
natural language — "BRCA1 relocated from cytoplasmic vesicles to discrete nuclear foci"
— which CellWhisperer can then interpret biologically.

## What this project delivers

1. **A relocalization atlas**: 50+ DDR proteins imaged simultaneously across multiple
   DNA-damaging agents, with temporal resolution. The first systems-level view of DDR
   coordination in living cells.

2. **A dependency map**: Functional relationships between repair proteins — who requires
   whom, which pathways compensate for which, how this changes with damage type. New
   synthetic lethal candidates beyond BRCA + PARP.

3. **An agentic screening framework**: The software infrastructure (gently + anton +
   CellWhisperer integration) that makes iterative, hypothesis-driven screening possible.
   Generalizable beyond DDR to any biological question addressable by vpCells.

4. **Expanded vpCells collection**: Intron-tagging of key missing DDR proteins (PCNA,
   RAD51, 53BP1, H2AFX, MDC1, MRE11, RAD50, PARP1) as a concrete deliverable back
   to the Kubicek lab.

## The translation path

```
  this project                   future
  ──────────────────────────────────────────

  vpCells DDR network map        patient tumor
  (cell lines, all pathways,     sequenced +
   all damage types)             scRNA-seq
          │                          │
          └────────┬─────────────────┘
                   │
                   ▼
          network map + patient mutations
          + expression = prediction:
          "this tumor relies on pathway Y,
           block with drug Z"
```

The map built in cell lines becomes a lookup table. Patient genomics and transcriptomics
(via CellWhisperer) provide the input. The output is a drug prediction grounded in
functional protein dynamics, not just mutation status.

---

## Key resources

- vpCells paper: Reicher et al., Nature Cell Biology 26, 745–756 (2024)
- CellWhisperer paper: Schaefer et al., Nature Biotechnology (2025)
- gently: https://github.com/pskeshu/gently
- deepthought: https://github.com/pskeshu/deepthought
- anton: https://github.com/pskeshu/anton (prototype)
- DDR clone analysis: https://github.com/pskeshu/vpcells-ddr-screen
