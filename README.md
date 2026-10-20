# Unified Formal Audit Specification (UFAS) v1.0

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23440068.svg)](https://doi.org/10.5281/zenodo.23440068)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC_BY_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

Protocols for auditing analytical and computational manuscripts: two complementary audit modules and a stopping criterion for the revision cycle.

| Specification | Purpose | Files |
| --- | --- | --- |
| **UFAS-LC v1.0** — Logical Closure | Decides whether a manuscript is logically closed: every statement follows from explicitly stated premises under declared regularity and domains | [Markdown](UFAS_LC_v1.0.md) · [PDF](UFAS_LC_v1.0.pdf) |
| **UFAS-IC v1.0** — Implementation Correspondence | Checks whether the accompanying implementation does what the text asserts about it | [Markdown](UFAS_IC_v1.0.md) · [PDF](UFAS_IC_v1.0.pdf) |
| **Audit Stopping Criterion** | Determines when mandatory corrections end in the “verification → correction → new version” cycle | [Markdown](UFAS_Audit_Stopping_Criterion.md) · [PDF](UFAS_Audit_Stopping_Criterion.pdf) |

Each module is self-contained, is executed on its own and carries its own version number; neither supersedes the other. The modules address different questions: a manuscript may be logically closed while misdescribing its implementation, or describe its implementation faithfully while leaving a gap in a proof. The stopping criterion does not itself verify a manuscript; it governs the revision cycle in which the modules are applied.

## UFAS-LC — Logical Closure

The auditor acts as a formal mathematical referee: it verifies and does not rewrite, paraphrase or interpret. The module is aimed at analytical papers — analysis, probability, information geometry, mathematical physics — where typical defects include hidden regularity assumptions, circular dependencies and unnoticed changes of asymptotic order.

**Phases**

1. **Extraction** — an exhaustive inventory of definitions, assumptions, lemmas, propositions, theorems and corollaries, `\label{...}` labels, asymptotic statements, and explicit uses of derivatives, integrals, expectations and limits (K1–K6).
2. **Dependency graph** — an edge X → Y whenever Y uses X, checked by block D: acyclic, all dependencies explicit, no use before definition, forward references labeled and resolved.
3. **Atomic checks** — blocks S, R, A, E, L and C.
4. **Critical failure conditions** — CF1–CF7.
5. **Result classification** — each statement is marked **[F]** formally proven or **[H]** heuristic / interpretative.

**Boolean blocks**

| Block | Name | Items |
| --- | --- | --- |
| S | Structural Integrity | S1–S5 |
| D | Dependency Correctness | D1–D4 |
| R | Regularity Correctness | R1–R6 |
| A | Assumption Integrity | A1–A5 |
| E | Asymptotic Consistency | E1–E7 |
| L | Labeling Consistency | L1–L4 |
| C | Conclusion vs Premise Validity | C1–C4 |

An item passes only if it follows by formal logical necessity from the stated premises; any gap, ambiguity, missing condition or unjustified step is a FAIL.

**Critical failure conditions**

- **CF1** — a derivative is used that is not defined under the stated assumptions
- **CF2** — circular reasoning in any proof
- **CF3** — an asymptotic statement contradicts derived bounds
- **CF4** — an assumption contains a derived statement later used as independent
- **CF5** — an undefined object is used in a proof
- **CF6** — a dependency cycle
- **CF7** — regularity escalation: the required class strictly exceeds the assumed one

**Decision rule**

The manuscript is **PUBLISHABLE** ⇔ S ∧ D ∧ R ∧ A ∧ E ∧ L ∧ C = TRUE, and **NOT PUBLISHABLE** if any block is FALSE. Any critical failure forces NOT PUBLISHABLE and overrides every PASS. The mandatory output comprises the inventory of formal elements, the dependency graph, the table of checks, every FAIL item with its exact location, any triggered CF, the [F]/[H] classification and the final decision.

## UFAS-IC — Implementation Correspondence

The auditor checks whether the accompanying implementation — code, functions, procedures, algorithms, computational blocks, tables, protocols — does what the text asserts about it. The check concerns **structure, not numbers**: arguments and input data, the order of steps, dependencies between operations, conditions and branching, what is computed or verified and by what means, and whether the stated result corresponds to the procedure actually performed. Cross-checking numerical values against logs and output (“numeric-grep”) is a separate task and is out of scope.

**Steps**

1. **Inventory** — the passages that contain a verifiable description, selected by content rather than by literal markers, and the implementations they reference, each marked *present* or *ABSENT from the materials* (including files referenced by include commands but not supplied).
2. **Verification** — one table row per passage: the claim and its location, the implementation (`file:element:lines`), what the implementation actually does, the verdict, and whether it affects the stated conclusion.
3. **Summary** — verdict counts; DISCREPANCY and PARTIAL items with their impact on the conclusions; missing implementations; what remained unverified, and why.

**Verdicts** (exactly one per claim)

| Verdict | Meaning |
| --- | --- |
| **MATCH** | The implementation does what the text asserts. |
| **DISCREPANCY** | The implementation does something other than what the text asserts. |
| **PARTIAL** | The text omits or misrepresents a step, parameter or condition. |
| **NO IMPLEMENTATION** | The claim has no structural counterpart in the implementation. |
| **INCONCLUSIVE** | The material does not support a definite verdict. |

Every verdict except NO IMPLEMENTATION must be backed by a 1–2 line quotation from the implementation or a precise reference to the fragment; a verdict without one is recorded as INCONCLUSIVE. No counterpart is invented where none exists. The implementation is not run if a conclusive verdict can be reached without running it, and if it is run, the reason is stated. The auditor only reports: nothing is corrected or rewritten.

## Audit Stopping Criterion

Every comment on a version falls into one of two classes:

- **(A) Structural defect** — a violation that can be established as a yes-or-no matter against a fixed reference standard: the manuscript's premises and definitions, its implementation, formal rules. Any class-(A) defect, even a minor one, blocks stopping. It is eliminated by correction or by *restricting the statement*: adding hypotheses, narrowing the domain or regime, marking it as heuristic, or withdrawing it.
- **(B) Improvement** — a change the version does not require in order to be correct. Adoption is at the author's discretion.

Class (A) is unrelated to Block A of UFAS-LC. A dispute is settled by asking whether the current version contains, at the indicated location, a violation of class (A); the text is read literally, and any statement not explicitly marked as heuristic is taken to be presented as proven.

Class (A) is finite and converges; class (B) is unbounded, so the absence of class-(B) comments cannot serve as a stopping condition. The candidate version is frozen when all of the following hold:

1. every class-(A) comment has been resolved (the defect eliminated or the defect claim rejected) and the decision, with its justification, recorded;
2. an independent from-scratch verification (e.g., UFAS-LC and UFAS-IC) by a verifier who has made no corrections and does not rely on earlier verdicts has found no new class-(A) defects, or those found have been eliminated and the modified material, together with everything that depends on it, has been re-verified with no new class-(A) defects;
3. only class-(B) comments remain open.

## How to use

The modules are written for an auditor: a human referee, or a language model given the module as its audit prompt.

1. **Provide the manuscript** — the paper's files, e.g. `.tex` sources and any accompanying scripts or run outputs, attached to the chat or placed in a project.
2. **Run one module per task** — `UFAS_LC_v1.0.md` or `UFAS_IC_v1.0.md`. The modules are independent; neither needs the other's output.
3. **Revise and repeat** — eliminate class-(A) defects, issue a new version and re-verify until the stopping criterion holds. Then record the frozen version (number, DOI or checksums) together with the audit trail: verifications and their outcomes, eliminated defects, rejected defect claims with justifications, and open class-(B) comments marked as optional.

Copies of the specifications (`UFAS_*`) are not treated as part of the manuscript, so avoid this prefix for manuscript files.

## Scope and limitations

UFAS-LC ensures deterministic execution (a finite checklist) and soundness-oriented detection of structural defects, but it guarantees neither completeness of defect detection nor the correctness of the evaluator: PUBLISHABLE means that the checklist found no defect, not that the manuscript has been proven correct. UFAS-IC checks structure rather than numbers and makes no publishability decision. The stopping criterion is a risk-management rule, not a guarantee of freedom from error: the verifier may be mistaken.

## Citation

Version v1.0 (technical note, published 17 October 2026) is archived on Zenodo:

> Khomyakov, V. (2026). *Unified Formal Audit Specification (UFAS): Logical Closure (LC) and Implementation Correspondence (IC) Modules* (Version v1.0). Zenodo. https://doi.org/10.5281/zenodo.23440068

```bibtex
@misc{khomyakov2026ufas,
  author    = {Khomyakov, Vladimir},
  title     = {Unified Formal Audit Specification ({UFAS}): Logical Closure ({LC})
               and Implementation Correspondence ({IC}) Modules},
  year      = {2026},
  version   = {v1.0},
  publisher = {Zenodo},
  doi       = {10.5281/zenodo.23440068},
  url       = {https://doi.org/10.5281/zenodo.23440068}
}
```

To cite all versions, use the concept DOI [10.5281/zenodo.23440067](https://doi.org/10.5281/zenodo.23440067), which always resolves to the latest version.

## Author

**Vladimir Khomyakov**, Independent Researcher

- ORCID: [0009-0006-3074-9145](https://orcid.org/0009-0006-3074-9145)
- Project page: [digitalphysics.ru/ufas_v1.0.html](https://digitalphysics.ru/ufas_v1.0.html)
- Website: [digitalphysics.ru](https://digitalphysics.ru/)

## License

The specifications are licensed under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/), the same license as the Zenodo record. See [LICENSE](LICENSE).
