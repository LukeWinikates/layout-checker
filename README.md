# layout-checker

What if there was something like a static type system or a linter, but for identifying potential UI layout problems: "you have a container of variable width with a child positioned relative to it's left edge and another positioned relative to the right. If the container width is unusually narrow, the child elements will overlap. How do you want to solve this? a) set a min width that prevents clipping b) I'm a monster, just let them overlap." I *think* that's possible in theory. Layout constraints in iOS can specify rules like that but I don't think there's tooling that detects potentially fragile relationships between UI elements.

Possible implementation style, elm-ish pseudocode:

``` elm
top : LayoutRule
top = {
  top: n px
  , bottom: FillContainer ScrollContents | FixedHeight ApplyMaxHeightToParent | FixedHeight PushesNeighborsDown | AllowClip
  , left: Nothing
  , right: Nothing
}

```

```
fill - 100% height, 100% width minus margins
```

Not sure if this is sufficiently expressive, for example
Do we allow clipping by default in cases where something else with a top constraint) -> are other elements allowed to have an overlapping top position? Will it be possible to check that at build-time?

MVP version here could define a top-level container type and check constraints on elements positioned within that
Could transpile down to CSS, maybe?
