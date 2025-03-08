# System

This repository contains system files for use with Javelin's firmware builder.

File names are <System>-<Orthography>.yaml.

# Schema

```
  name: String
  keys: [KeyDefinition]
  undo: Stroke
  layout: [Layout]
  orthography: Orthography
```

## KeyDefinition

```
  name: String   # single character
  mask: int
  absentMask: int?
  presentMask: int?
```

- Normal keys are defined with `mask != 0`.
  - Keys with only a single bit defined are used to format a stroke.
  - Keys with multiple bits defined are used to parse a stroke.
- Separators are defined with `mask == 0`, and `absentMask` and `presentMask` defined.
  - `absentMask` defines what bits mean the separator does not need to be shown. For example, in WSI, `-` does not need to be shown if any of `AO*EU` are present.
  - `presentMask` defines what bits need to be defined for the separator to be shown. For example, in WSI, the enders `FRPBLGTSDZ` will require the separator.

## Layout

Layout is used to define what is shown in the Steno Layout web tools

```
  mask: int
  name: String
  center: {x: double, y: double}
  path: [PathCommands]
```

PathCommands are:

```
  c(ommand): CommandType
  p(arameters): [double]
```

Valid commands are:

- `m`: Move to x, y
- `rm`: Relative move to offset dx, dy
- `l`: Line to x, y
- `rl`: Relative line to offset dx, dy
- `q`: Cubic to x1, y1, x2, y2, x3, y3
- `rq`: Relative cubic to offset dx1, dy1, dx2, dy2, dx3, dy3
- `c`: Close path

Relative offsets are compared to the last point in the previous command.

# Orthography

```
  rules: [Rule]
  aliases: [Alias]
  auto-suffix: [AutoSuffix]
  reverse-auto-suffix: [ReverseAutoSuffix]
```

## Rule

```
  pattern: String
  replacement: String
```

## Alias

```
  suffix: String
  alias: String
```

## AutoSuffix

```
  key: Stroke
  suffix: String
```

## ReverseAutoSuffix

```
  key: Stroke
  suppressMask: String
  pattern: String
  replacement: String
```
