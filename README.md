# pogverse-ai

<img src="dragon.svg" alt="Pogverse AI — Pogona vitticeps pixel art mascot" width="100%" />

Shared Claude AI resources for the Whiteley Lab, University of Canberra.

---

## What is this?

This repo contains two types of Claude AI resources:

- **Skills** -- reusable prompt templates that configure Claude Code for a specific task. Invoke a skill by typing `/skill-name` at the start of a session.
- **Sandpits** -- self-contained project environments for complex, multi-step analytical workflows. Open a sandpit folder as your Claude Code project and the configuration loads automatically.

Together these mean you spend less time re-explaining who we are and what standards we follow, and more time doing the actual science.

---

## How Claude Code configuration works

This is important to understand before you set anything up.

### CLAUDE.md -- always loaded, every session

Claude Code automatically reads a file called `CLAUDE.md` from two places at the start of **every** session, before you type anything:

- **`~/.claude/CLAUDE.md`** (global) -- applies to every project on your machine
- **`CLAUDE.md` in your project folder** -- applies only when you open that project

You do not have to call it or remember it. It is always silently active.

This repo includes a `CLAUDE.md` file with the Whiteley Lab always-on rules (be critical and honest, never touch raw data, clarify before acting, etc.) plus RTK token-saving instructions. **Every team member should copy this file to `~/.claude/CLAUDE.md` on their own machine.** See [Getting started](#getting-started) below for how.

### Skills -- must be called explicitly

Everything in the `skills/` and `workflows/` folders, and files like `research-session.md`, are **skills** -- they do nothing until you invoke them. Once a skill file is in your `~/.claude/commands/` folder, you activate it by typing `/` followed by the filename at the start of a session:

```
/research-session
```

Think of it this way:
- `CLAUDE.md` = standing orders, always in effect
- Skills = specialist briefings you call in when you need them

For example: the always-on rules (honesty, raw data protection, audience calibration) live in `CLAUDE.md` and apply automatically. The detailed RNA-seq workflow, PBS job standards, and session summary requirements live in `/research-session` and `/rnaseq-pipeline` -- invoke those when you are actually starting that kind of work.

### Sandpits -- project environments for complex workflows

Some workflows are too complex and stateful for a single skill file. **Sandpits** are full Claude Code project directories. You open the sandpit folder in Claude Code (instead of your usual project folder), and the `.claude/CLAUDE.md` inside configures the session with the right tools, standards, and conventions for that workflow. The `prompts/` folder inside each sandpit contains orchestration templates you paste into the chat to start a multi-agent run.

Think of it this way:
- Skills = specialist briefings you call in for a single task
- Sandpits = a fully equipped lab bench you sit down at for a whole project

Sandpits are designed for long, structured workflows -- like producing a complete DGE report with literature context, statistical analysis, functional enrichment, and splicing -- not one-off questions.

---

## Resources in this repo

### Sandpits

| Sandpit | Folder | Description |
|---------|--------|-------------|
| DGE Exploration Report | [`dge-sandpit/`](dge-sandpit/) | Multi-agent pipeline for comprehensive first-pass DGE reports from bulk RNA-seq. Covers edgeR (QLF + TREAT), GO/GSEA with non-model caveats, and rMATS-turbo splicing. See [`dge-sandpit/README.md`](dge-sandpit/README.md) for setup and usage. |

### Lab skill (root folder)

| Skill | Command | Description |
|-------|---------|-------------|
| `research-session.md` | `/research-session` | Loads Whiteley Lab context -- research focus, Gadi infrastructure, coding standards, and session rules |

### RNA-seq workflow skills (`workflows/` folder)

Step-by-step guided workflows for the full RNA-seq analysis pipeline. Run these in order for each new experiment.

| Skill | Command | Description |
|-------|---------|-------------|
| `workflows/rnaseq-pipeline.md` | `/rnaseq-pipeline` | Configure and submit nf-core/rnaseq on Gadi -- samplesheet transfer, directory setup, PBS script generation, job submission and monitoring |
| `workflows/rnaseq-qc.md` | `/rnaseq-qc` | Post-run QC -- interpret MultiQC report, check alignment/mapping rates, review sample clustering, make pass/fail/exclude decisions before DGE |
| `workflows/rnaseq-dge.md` | `/rnaseq-dge` | Differential expression analysis -- R project setup, metadata configuration, run EdgeR scripts, assess PCA/MDS/BCV plots, interpret results, optional TPM visualisation and pooled analyses |

**Typical workflow:**
```
PogBase Sequencing Browser -> download samplesheet
  -> /rnaseq-pipeline   (on Gadi)
  -> /rnaseq-qc         (on Gadi, after job completes)
  -> /rnaseq-dge        (locally in RStudio)
```

### Scientific skills (`skills/` folder)

76 curated skills sourced from [K-Dense-AI/claude-scientific-skills](https://github.com/K-Dense-AI/claude-scientific-skills), organised by category:

#### Bioinformatics & Genomics (25 skills)
`alphafold-database` * `anndata` * `arboreto` * `biopython` * `biorxiv-database` * `bioservices` * `cellxgene-census` * `clinvar-database` * `deeptools` * `ena-database` * `ensembl-database` * `etetoolkit` * `gene-database` * `gget` * `gnomad-database` * `gtex-database` * `gwas-database` * `kegg-database` * `phylogenetics` * `pubchem-database` * `scanpy` * `scikit-bio` * `scvelo` * `scvi-tools` * `uniprot-database`

#### Data Analysis & Visualization (18 skills)
`astropy` * `dask` * `exploratory-data-analysis` * `matplotlib` * `networkx` * `plotly` * `polars` * `pydeseq2` * `pymc` * `scientific-schematics` * `scientific-visualization` * `scikit-learn` * `seaborn` * `shap` * `statistical-analysis` * `statsmodels` * `umap-learn` * `vaex`

#### Proteomics & Mass Spectrometry (8 skills)
`esm` * `interpro-database` * `matchms` * `molfeat` * `pathml` * `pdb-database` * `pyopenms` * `string-database`

#### Research Methodology (13 skills)
`arxiv-database` * `bgpt-paper-search` * `hypothesis-generation` * `literature-review` * `openalex-database` * `peer-review` * `perplexity-search` * `pubmed-database` * `research-grants` * `research-lookup` * `scholar-evaluation` * `scientific-brainstorming` * `scientific-critical-thinking`

#### Scientific Communication (13 skills)
`citation-management` * `docx` * `humanizer` * `infographics` * `latex-posters` * `markdown-mermaid-writing` * `pdf` * `pptx` * `pptx-posters` * `scientific-slides` * `scientific-writing` * `venue-templates` * `xlsx`

---

## Getting started

### 1. Install Claude Code

Claude Code is Anthropic's official CLI tool for working with Claude. You will need a Claude account (ask Sarah if you don't have one).

Install it via npm:

```bash
npm install -g @anthropic/claude-code
```

> **Note:** This requires Node.js. If you don't have it, download it from https://nodejs.org first.

### 2. Set up your CLAUDE.md (always-on lab rules)

This repo includes a `CLAUDE.md` file with the Whiteley Lab always-on rules. Copy it to your Claude Code config folder so it is automatically active in every session.

**Windows** (run in PowerShell):
```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude"
Copy-Item "path\to\pogverse-ai\CLAUDE.md" "$env:USERPROFILE\.claude\CLAUDE.md"
```

**Mac/Linux:**
```bash
mkdir -p ~/.claude
cp path/to/pogverse-ai/CLAUDE.md ~/.claude/CLAUDE.md
```

> If you already have a `~/.claude/CLAUDE.md` (e.g. from RTK setup), open both files and paste the Whiteley Lab section in -- do not overwrite the RTK block.

### 3. Create your commands folder

Claude Code looks for skills in a specific folder on your computer. Create it if it doesn't exist:

**Windows:**
```
C:\Users\YourName\.claude\commands\
```

**Mac/Linux:**
```
~/.claude/commands/
```

### 4. Get the skills

**Option A -- Download individual files (simplest):**
Browse to the file you want on GitHub, click it, then click the download icon. Save it into your commands folder from Step 2. You can use any filename -- the command name will match whatever you name the file (without the `.md`).

**Option B -- Clone the repo (recommended if you want updates):**
```bash
git clone https://github.com/sarahwhiteley/pogverse-ai.git
```
Then copy whichever skill files you need into your commands folder. When the repo is updated, run `git pull` in the cloned folder and re-copy any changed files.

---

## Saving tokens with RTK

RTK (Rust Token Killer) is a small tool that compresses noisy command output before Claude sees it, cutting token usage by 60-90% on common operations. This means longer sessions before hitting rate limits and a cleaner context for Claude.

See **[rtk-setup.md](rtk-setup.md)** for full installation and setup instructions.

Once installed and configured, RTK runs automatically via a hook in Claude Code. The only rule to remember when typing commands yourself in the Claude Code terminal:

> **Always prefix shell commands with `rtk`:**
> ```bash
> rtk git status    # not: git status
> rtk git diff      # not: git diff
> ```

Check your token savings any time:
```bash
rtk gain
```

---

## Using skills

Once a skill file is in your commands folder, open Claude Code and type `/` followed by the filename (without `.md`). For example:

```
/research-session
```
```
/literature-review
```
```
/scanpy
```

**Tip:** You can rename files when you copy them to your commands folder if you want shorter or more memorable command names.

---

## The research-session skill

This is our lab-specific skill. Once loaded, it:
- Asks clarifying questions before starting any analysis (don't skip this -- it keeps you on track)
- Writes all code to files, never just pastes it in chat
- Flags when something doesn't make biological or statistical sense
- Produces a summary document at the end of each session
- Escalates to Sarah or another senior researcher when appropriate

---

## Important rules when using Claude for research

- **Never modify raw data files.** Always work on copies.
- **All code goes to files.** If Claude pastes code into chat without saving it, ask it to save it properly.
- **Check the session summary** at the end -- it's your record of what was done and should be kept with your project files.
- **If something looks wrong, it probably is.** Claude is instructed to be critical and honest, but you should be too. Raise anything that looks odd with a senior lab member before proceeding.
- **Expensive compute jobs on Gadi** should be checked with Sarah or another CI before submission.

---

## Keeping skills up to date

Sarah manages this repo. When skills are updated, you'll be notified via the lab. To get the latest version:

- **If you downloaded directly:** re-download and replace the file in your commands folder
- **If you cloned the repo:** run `git pull` in the cloned folder, then copy any updated files across

---

## Questions?

Ask Sarah.
