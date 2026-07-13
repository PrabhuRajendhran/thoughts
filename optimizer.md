Here's the consolidated workflow I'd recommend for your taxonomy optimization project.

Scope

Optimize one team at a time.

Optimize one hierarchy level at a time (Grandparent → Parent → Child).

Treat each sibling set (~10 categories) as one optimization unit.



---

Optimization Loop

Current sibling taxonomy
        ↓
Analyze overlaps & confusion
        ↓
Generate candidate mutations
        ↓
Offline evaluation
        ↓
Select best candidate(s)
        ↓
Human review
        ↓
Approve & deploy
        ↓
Repeat until no meaningful improvement


---

Mutation Strategies

Phase 1: Full-set mutation

Rewrite all sibling definitions together.

Goal: Reduce overlap and improve mutual exclusivity.

Repeat until improvements plateau.


Phase 2: Pairwise mutation

Use the confusion matrix.

Optimize only the most confused category pairs.

Goal: Sharpen decision boundaries.


Phase 3: Error-driven mutation

Use actual misclassified emails.

Make minimal targeted changes.

Goal: Eliminate remaining errors.



---

Evaluation

Evaluate the entire sibling set, not individual categories.

Track:

Overall Accuracy/F1

Per-category Precision/Recall/F1

Confusion matrix

Confidence (optional)



---

Acceptance Criteria

Accept a mutation only if:

Overall metric improves.

No major regression in another category.

Improvement holds on a held-out validation set.



---

Human Review

For every accepted candidate, present:

Definitions before/after

Categories changed

Reason for change

Metrics before/after

Confusions reduced

New regressions (if any)


Humans make the final decision.


---

Guards

Regression guard

Held-out validation guard

Audit trail of every iteration

Stop after no meaningful improvement for N iterations



---

After Taxonomy Stabilizes

Once the taxonomy is clean:

1. Freeze the taxonomy.


2. Use DSPy to optimize the classification prompt.


3. Benchmark different models if needed.




---

Overall Pipeline

Current Taxonomy
        ↓
Full-set optimization
        ↓
Evaluate
        ↓
Pairwise optimization
        ↓
Evaluate
        ↓
Error-driven optimization
        ↓
Evaluate
        ↓
Human review
        ↓
Freeze taxonomy
        ↓
DSPy prompt optimization
        ↓
Production

This sequence separates knowledge optimization (taxonomy) from prompt optimization (DSPy), making it easier to measure where improvements come from and yielding a more maintainable system.
