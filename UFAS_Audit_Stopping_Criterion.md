# UFAS — A Practical Audit Stopping Criterion

**Specification Author:**  
Vladimir Khomyakov / Independent Researcher  
**ORCID iD**: https://orcid.org/0009-0006-3074-9145  

**Related modules:** UFAS-LC — Logical Closure; UFAS-IC — Implementation Correspondence.  
This document does not itself verify a manuscript; it determines when mandatory corrections come to an end in the “verification → correction → new version” cycle.

---

## 1. Classes of Comments

A comment either asserts a defect — class (A) — or proposes a change without asserting a defect — class (B). Class (A) is unrelated to Block A of the UFAS-LC module.

**(A) Structural defect** — a violation that can be established as a yes-or-no matter against a fixed reference standard (the manuscript’s premises and definitions, its implementation, formal rules): a statement is false, unproven, or overstated (a missing hypothesis, an incorrect quantifier); an object is used without having been defined, outside its domain, or with higher regularity than has been assumed; a computational claim is not supported by the implementation, or is not supported by reproducible results; references or labels are broken, or the order in which results are established is violated. Any class-(A) defect, even a minor one, blocks stopping. Such a defect is eliminated either by correction or by **restricting the statement** to what has been established: by adding hypotheses, narrowing the domain or regime, marking it as “heuristic” ([H] in UFAS-LC), or withdrawing it.

Restriction requires explicit author approval; by default, augmentation is attempted first and the conflict is reported.

**(B) Improvement** — a change that the version does not require in order to be correct: exposition, explicitly caveated interpretations, development of the content beyond what is claimed. Its assessment is comparative and reader-dependent; adoption is at the author’s discretion.

## 2. The Distinguishing Test

A dispute is settled by asking: **is there, at the indicated location in the version as it currently stands, a violation from the list in (A)?** If so, the defect is eliminated; if not, the defect claim is rejected with a stated justification, and the proposal, if any, is treated as class (B). In adjudication:

* the text is read literally, and any statement not explicitly marked as heuristic is taken to be presented as proven; if even one admissible reading is false or unproven, the statement falls under class (A);
* a step that cannot be verified without nontrivial additional reasoning falls under class (A); matters of readability fall under class (B);
* a class-(A) comment must specify the location and the violation; general doubts do not block stopping;
* pending adjudication, a disputed comment is treated as class (A); once adjudicated, a comment is not reopened without new grounds (a counterexample, an error in the decision, an edit to that location).

## 3. Convergence of (A) and Divergence of (B)

**Class (A) is finite and converges.** A version has finitely many elements and there are finitely many kinds of violation; hence there are also finitely many class-(A) defects. A version free of them is attainable: restriction or a local edit eliminates any defect without introducing new unproven content, and if processing proceeds from premises to consequences (a dependency cycle is itself a class-(A) defect), each element is revisited at most once. The indicator of convergence is a round-by-round decrease in the number of newly confirmed class-(A) defects, down to zero; if this does not occur, the conflict between augmentation (new proofs, new code) and restriction is reported to the author, who decides (Section 1). The price of termination is the strength of the statements; only the author may agree to pay it.

**Class (B) is unbounded and diverges.** Readers’ preferences conflict, and in general there is no version that is best for all readers: for any version a new class-(B) comment can be found, and revisions may continue indefinitely or go in circles. Hence the requirement “no class-(B) comments” cannot be satisfied and cannot serve as a stopping condition.

## 4. The Criterion

Mandatory corrections cease and the candidate version is frozen if all of the following hold simultaneously:

1. every class-(A) comment has been resolved — the defect has been eliminated or the defect claim rejected under Section 2 — and the decision, together with its justification, has been recorded;
2. an independent from-scratch verification (e.g., UFAS-LC and UFAS-IC), performed by a verifier who has made no corrections and does not rely on earlier verdicts, has found no new class-(A) defects, or the defects found have been eliminated and the modified material, together with whatever depends on it, has likewise been re-verified with no new class-(A) defects;
3. only class-(B) comments remain open, however many there may be.

Otherwise, stopping is not permitted, and the blocking class-(A) comments are listed.

## 5. After Stopping

The following are recorded: the version (number, DOI, or checksums); the verifications performed and their outcomes; the class-(A) defects and how each was eliminated; the rejected defect claims, with justifications; and the open class-(B) comments, designated as optional.

The audit is resumed only on specific grounds: a localized class-(A) comment, a new result, or a revision of statements, proofs, definitions, or code — including one made in response to a class-(B) comment; whatever has been changed or added, and everything that depends on it, is then re-verified.

The criterion is a risk-management rule, not a guarantee of freedom from error: the verifier may be mistaken.

---

# END OF SPECIFICATION

---
