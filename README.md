# RG-UARG: Reference Workflow

This repository outlines the claim-amendment workflow at pseudocode level.
Selected components are intentionally omitted, including knowledge retrieval,
feature construction, scoring, guidance selection policy, and prompt content.
The code illustrates the information flow and is not a reproduction package.

## Algorithm

```text
INPUT: claim text C, rejection reasons J, user settings S

entities   <- extract key terms from C and link them to the knowledge graph
subgraph   <- expand outgoing edges from entities using S.graph_hops
structures <- encode directional propagation from the topic entities
query      <- encode C

FOR each candidate (head, relation, tail) in subgraph:
    features <- concatenate(
        query,
        encode(head), encode(relation), encode(tail),
        structures[head], structures[tail]
    )
    score <- user-supplied triple scorer(features)

ranked     <- sort candidates by decreasing score
high       <- select the high-ranked fraction specified by S
protected  <- select the leading portion of high specified by S
refinement <- select the remaining portion of high
forbidden  <- select the low-ranked fraction specified by S

prompt     <- user renderer for P_CLAIM_AMENDMENT(C, J, guidance groups)
revision   <- user language model(prompt, S.model_options)
RETURN revision
```
