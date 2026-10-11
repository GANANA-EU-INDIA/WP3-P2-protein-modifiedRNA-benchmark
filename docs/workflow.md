# Workflow

This document describes how each case of the Protein–Modified-RNA Docking Benchmark was prepared from its deposited PDB entry.

## Overview

All cases use a **bound–bound** setup: the protein and RNA structures are both extracted from the experimentally determined complex. Every case has passed HADDOCK3 topology generation with the modified residue built correctly. The benchmark is intended to facilitate the testing, validation and comparison of docking workflows for modified RNA.

| Code | Modification | Common name | Cases |
|---|---|---|---:|
| `6MZ` | N6-methyladenosine | m6A | 9 |
| `1MA` | 1-methyladenosine | m1A | 1 |
| `5MC` | 5-methylcytidine | m5C | 3 |
| `A2M` | 2'-O-methyladenosine | Am | 4 |
| `PSU` | Pseudouridine | Ψ | 1 |
| | | **Total** | **18** |

For the per-case details (chains, residue counts, RNA length, target residue, AIRs) and case-specific notes, see [`README.md`](README.md). A summary of all cases is in [`master_summary.tsv`](master_summary.tsv), and the full metadata for each case, including the RNA sequence, is in its `info.tsv`.

## Selection criteria

Candidate structures were retrieved from the RCSB PDB and kept only if they met all of the following criteria:

1. **Protein–RNA complex.** The entry contains at least one protein chain and at least one RNA chain in direct contact.
2. **Supported modification.** The RNA carries at least one of the five modified nucleotides parametrized in the force-field extension: N6-methyladenosine (`6MZ`), 1-methyladenosine (`1MA`), 5-methylcytidine (`5MC`), 2'-O-methyladenosine (`A2M`) or pseudouridine (`PSU`).
3. **Modification at the interface.** At least one atom of the modified nucleotide lies near the interface, so that docking tests the modified chemistry at the binding site rather than at a distal position.
4. **Tractable size.** The complex is of a size suitable for docking. Megadalton assemblies such as ribosomes, spliceosomes and mitoribosomes, where the modification is buried in rRNA rather than at a discrete protein–RNA interface, are excluded.
5. **One interface per entry.** Where an entry contains several copies of the complex, only the protein–RNA pair whose modified nucleotide is closest to a protein is used. Crystallographic neighbours are not included.

## Preparation steps

Preparation has four stages: atom-level cleaning, receptor selection, restraint generation and target construction, followed by HADDOCK3 topology validation.

### 1. Atom-level cleaning

From the deposited mmCIF, for the selected protein chain(s) and the RNA chain:

- hydrogens are removed;
- alternate locations are resolved (the highest-occupancy conformer is kept);
- waters, ions and non-polymer ligands are removed;
- selenomethionine `MSE` is converted to `MET` (atom `SE` → `SD`, element `S`), so that it is a standard methionine rather than a hetero-atom that `pdb_delhetatm` would delete;
- the modified nucleotide is kept with its force-field residue name (`6MZ`, `1MA`, `5MC`, `A2M`, `PSU`). For `6MZ` the methyl carbon is renamed `C9` → `C10` to match the force-field template.

Stock `pdb_delhetatm` would strip both the modified nucleotide and selenomethionine, so this stage is done at the atom level rather than with the standard pdb-tools cleaning pipeline.

### 2. Receptor selection

The RNA copy whose target modification lies closest to a protein is selected. Every protein chain with any atom within 5.0 Å of that RNA chain is kept as the receptor and written to a single file, `X_p_b.pdb`, so that HADDOCK treats it as one body. A monomeric reader that appears as several crystallographic copies therefore reduces to the one chain bound to the modification. Assemblies in which the RNA bridges several chains keep all of those chains: 6ZOT, 6A6J, 7L6R, 9NHC and 9QRP.

The RNA chain is written to `X_r_b.pdb`. Protein and RNA keep their original chain identifiers and residue numbering from the deposited entry, and the chain ID is also used as the segid. Chain identifiers never overlap between protein and RNA within a case.

### 3. NMR ensembles

For the solution-NMR entries (2MTV, 8COO), model 1 is used as the single bound structure (`X_p_b.pdb`, `X_r_b.pdb`). All 20 deposited models are also provided as multi-model ensembles (`X_p_b_ens.pdb`, `X_r_b_ens.pdb`). Each model takes its coordinates from the deposited mmCIF and keeps the atom set, order and naming of the processed model-1 files.

### 4. Restraints

The ambiguous interaction restraints (`X_b_ambig.tbl`) are the true bound interface: every protein and RNA residue with any atom within 5.0 Å of the partner molecule is an active residue. Assign statements are written in both directions (protein → RNA and RNA → protein), with a target distance of 2.0 Å and bounds 2.0/0.0 Å. `contacts.dat` lists the underlying residue–residue contacts.

### 5. Target complex

`X_target.pdb` is rebuilt from the processed bound protein and RNA, and is the reference for `caprieval`.

### 6. Validation

Every case passes HADDOCK3 topology generation (`topoaa`, exit code 0, no warnings) with the modified residue built correctly.
