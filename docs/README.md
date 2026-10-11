# Benchmark cases

All 18 cases of the Protein–Modified-RNA Docking Benchmark. Click a PDB ID to open its directory.

| PDB ID | Mod. | Receptor | Protein chains | Protein res. | RNA chain | RNA length | Target resid | AIRs |
|---|---|---|---|---:|---|---:|---:|---:|
| [2MTV](../2MTV/) | 6MZ | monomer (NMR) | A | 156 | B | 6 | 3 | 35 |
| [4R3I](../4R3I/) | 6MZ | monomer | A | 164 | B | 5 | 3 | 36 |
| [4RCJ](../4RCJ/) | 6MZ | monomer | A | 188 | B | 5 | 3 | 28 |
| [6RT4](../6RT4/) | 6MZ | monomer | B | 160 | D | 3 | 1 | 28 |
| [6ZOT](../6ZOT/) | 6MZ | multi-subunit | A+B | 330 | C | 4 | 3 | 31 |
| [7PO6](../7PO6/) | 6MZ | monomer | A | 163 | E | 3 | 3 | 28 |
| [7V4G](../7V4G/) | 6MZ | monomer | B | 214 | E | 4 | 4 | 30 |
| [7Z26](../7Z26/) | 6MZ | monomer | A | 150 | D | 3 | 3 | 27 |
| [8COO](../8COO/) | 6MZ | monomer (NMR) | A | 191 | B | 7 | 5 | 22 |
| [5CCX](../5CCX/) | 1MA | monomer | B | 371 | N | 77 | 58 | 77 |
| [6A6J](../6A6J/) | 5MC | multi-subunit | A+C | 180 | D | 5 | 5 | 22 |
| [6KTC](../6KTC/) | 5MC | monomer | A | 74 | V | 4 | 10 | 18 |
| [7F3K](../7F3K/) | 5MC | monomer | A | 90 | B | 6 | 5 | 23 |
| [7DID](../7DID/) | A2M | monomer | A | 644 | C | 4 | 7 | 38 |
| [7JZ0](../7JZ0/) | A2M | monomer | C | 298 | F | 4 | 1 | 26 |
| [7L6R](../7L6R/) | A2M | multi-subunit | A+B | 409 | C | 5 | 1 | 26 |
| [9NHC](../9NHC/) | A2M | multi-subunit | A+E+F | 1155 | B | 25 | 25 | 114 |
| [9QRP](../9QRP/) | PSU | multi-subunit | A+B | 815 | T | 86 | 73 | 74 |

*Target resid* is the residue number of the modified nucleotide in the RNA chain. *AIRs* is the number of `assign` statements in the bound restraint file. A summary of all cases is in [`master_summary.tsv`](master_summary.tsv), and the full metadata for each case, including the RNA sequence, is in its `info.tsv`.

## Case-specific notes

- **7JZ0, 7L6R**: cap 2'-O-methyltransferases. The deposited RNA has a 7-methylguanosine (m7G) 5' cap. The forcefield does not support m7G, so the cap is left out of the prepared RNA. The `A2M` target nucleotide is kept.
- **9QRP**: seryl-tRNA synthetase. The tRNA binds across the homodimer, so both protein chains are kept.
- **5CCX**: m1A58 tRNA methyltransferase. Only the catalytic subunit (chain B) contacts the tRNA within 5 Å, so the receptor is that single chain.
- **2MTV, 8COO**: solution NMR structures. Model 1 is used for the default files, the docking configuration and the target. The full 20-model ensembles are also provided ([`2MTV`](../2MTV/), [`8COO`](../8COO/): `X_p_b_ens.pdb`, `X_r_b_ens.pdb`).

See also: [workflow](workflow.md) · [main README](../README.md)
