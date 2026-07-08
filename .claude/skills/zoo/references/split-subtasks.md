Split into subtasks:

- small but not trivial
- self-contained
- one reviewable commit each
- use multiple subtasks for cleanly separable incremental feature buildup

Red flags:
- unrelated features combined
- clean steps merged without good recorded reason
- micro-subtasks with no meaningful functionality step
- wrong order
- hidden deps
- missing browser-impact flags
- earlier subtasks relying on later outputs
- spec items uncovered

After split, run quick review agent to check split, spec coverage, ordering, browser-impact annotations.
