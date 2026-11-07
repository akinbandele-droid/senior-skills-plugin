# Refactoring Legacy Code

Refactoring changes structure without changing behavior. In legacy code —
code without tests you trust, or that you don't fully understand — the danger is
silently changing behavior. Work in a way that makes that impossible to miss.

## First rule: behavior stays identical
A refactor that changes behavior is not a refactor; it's a change wearing a
disguise. Keep them separate. If you must fix a bug while refactoring, land the
behavior change as its own clearly-labeled commit.

## The safe loop
1. **Pin behavior with tests first.** Before changing legacy code, add
   *characterization tests* that capture what it currently does (even if that
   includes bugs). These are your safety net. If it's untestable as-is, make the
   smallest seam to get it under test.
2. **Refactor in tiny steps,** re-running tests after each. Each step keeps the
   code green. Small steps make the one that broke things obvious.
3. **Commit frequently** at green states so you can always retreat.
4. **Separate** mechanical/automated refactors (rename, extract, move) from
   hand-edits — review them apart.

## Getting untestable code under test (seams)
- Find a **seam**: a place you can alter behavior without editing in place —
  inject a dependency, extract a method/interface, or wrap a call.
- Break hard dependencies (clock, network, filesystem, globals, singletons) by
  extracting them behind a parameter or interface you can substitute in tests.
- Start with a characterization test that just asserts current output for real
  inputs; tighten from there.

## Common moves
- **Extract function/variable** to name a concept and shrink a big function.
- **Inline** needless indirection.
- **Rename** to reveal intent (use the tool/IDE so all refs update).
- **Introduce parameter object / replace magic number with constant.**
- **Replace conditional with polymorphism / lookup** where a switch sprawls.
- **Encapsulate** mutable state behind a clear interface.
Make one kind of move at a time.

## Judgment
- Refactor with a purpose: you're about to change this area, it's a hotspot, or
  it actively blocks work. Don't gold-plate stable code nobody touches.
- Follow the **boy-scout rule**: leave the code you touched a little cleaner —
  but keep it in scope and reviewable.
- Resist rewrites. A full rewrite loses hard-won edge-case knowledge; prefer
  incremental strangling of the old code behind a stable interface.
- Know when to stop: once it's clear enough to make the real change safely, stop.

## Verify
- The pinned tests still pass, unchanged.
- Behavior is observably identical (same outputs, same errors, same side effects).
- The diff is structural only; no logic quietly altered.
