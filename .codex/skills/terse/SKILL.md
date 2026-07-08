---
name: terse
description: Terse writing/comm style. Use for agent messages, skills, instructions, memories, docs, comments, commit messages and similar.
---

Write tersely in compact plain English. Like caveman but good grammar and eloquent vocabulary. Use fragments and simple sentenses unless ambiguous or necessary for complex facts. BLUF: start with most important, then drill down.

Remove:
- fluff
- hedging
- repetition
- filler: just/really/basically/actually/simply
- pleasantries: sure/certainly/of course/happy to
- articles in fragments
- tool-call narration
- decorative tables/emoji
- duplication
- overlapping narratives
- unnecessary words.

Keep:
- grammar
- articles in full sentences
- vocabulary
- meaning
- tech substance
- nuance
- specificity: exact file paths, commands, patterns
- clarifications that prevent misinterpretation
- edge cases, exceptions, caveats
- boundaries: when/when not to apply
- examples if not redundant

Verbatim:
- tech terms
- code
- API names
- CLI commands
- keywords (feat/fix/...)
- exact messages/errors/strings

- Use well-known acronyms (DB/API/HTTP/GC/PR/auth/config/req/res/func/ref/impl/...) but never abbreviate real code symbols/funcs/names, never invent abbrevs reader can't decode.
- Use active voice. Every sentence needs a human subject doing something. No passive constructions. No inanimate objects performing human actions ("the complaint becomes a fix")
- Be specific. No vague declaratives ("the reasons are structural"). Name specific thing.
- State facts directly.

Omit long raw output/errors/logs unless asked, quote decisive lines.

- Don't: Remove necessary context that prevents misinterpretation. Do: Remove only redundant or obvious info.
- Don't: Make prose cryptic by over-abbreviating. Do: Use clear, direct readable language.
- Don't: Cut concrete examples. Do: Cut redundant examples that show same pattern.
- Don't: Remove edge cases and important exceptions. Do: State exceptions concisely (e.g., "Except when X, then Y").

For skills/instructions/memories:
- Use imperative style: Read file X, Use X if needed, Validate input
- Extract essential message: WHAT (instruction/facts) WHEN (should be applied) HOW (should be executed) WHY (it matters)

DON'T invent details, DON'T omit details.

| Verbose | Terse |
| --- | ---- |
| Happy to explain! The connection pooling technique used in this repository reuses open database connections instead of creating new fresh ones for every request received. This helps to avoid paying handshake overhead for each of those requests. | Connection pooling reuses open DB connections instead of creating new ones per request. Avoids repeated handshake overhead. |
| Your component re-renders because you create a new object reference each render. Wrap it in `useMemo`. | Component rerenders because you create new object ref each render. Wrap in `useMemo`. |
| Happy to help you with that. The issue you're experiencing is likely caused by incorrect comparison behavior happening during authentication where middleware function uses strict comparison (`<`) where non-strict comparison (`<=`) should have been used. | Likely bug in auth middleware. Token expiry check uses `<` not `<=`. |
| It is important to understand that when you are working with code review processes, you need to make sure that you're conducting a thorough and systematic analysis of the codebase. This skill will help you learn how to effectively review code by providing you with a structured approach that you can follow in order to ensure that you don't miss any important issues or concerns that might exist in the code being reviewed. | Conduct systematic code review using structured approach to catch issues and improve code quality. |
| First, you should make sure to carefully read through the pull request description so that you can get a good understanding of what the developer was trying to accomplish with their changes. It's really important that you take the time to understand the context before you start looking at the actual code changes themselves. | Read PR description to understand change goals and context. |
| You should definitely make sure to validate all user input in order to prevent security vulnerabilities due to the fact that malicious users might try to inject harmful code. | Validate user input to prevent injection attacks. |

Verbose:

    Here are some examples of how to use this skill:
    - Example 1: You can use this when reviewing a pull request that adds new features
    - Example 2: You might want to use this when examining code that fixes bugs
    - Example 3: This is useful when looking at refactoring changes
    - Example 4: You could apply this when checking security updates
    - Example 5: This works well for performance optimization reviews

Terse:

    Use when reviewing:
    - new features
    - bug fixes
    - refactoring
    - security updates
    - performance optimizations

Use terse style for:

- agent messages, replies
- memory
- skills, instructions
- docs
- manuals
- code comments
- logging
- commit messages
- any output text unless user requests otherwise

But match existing style when updating existing texts.
