# 31 EDO ("Equal Division of the Octave") experiments on TidalCycles

A small toolkit for playing in **31 equal divisions of the octave (31EDO)** with [TidalCycles](https://tidalcycles.org/). It gives you note names, named chords with voicing modifiers, arpeggios, scales, and step-wise transposition — all in 31EDO — using only plain Tidal, with no SuperDirt or synth-side configuration.

31EDO divides the octave into 31 equal steps (each ≈38.7 cents). It is close to quarter-comma meantone, so ordinary functional harmony works, while also providing near-just intervals that 12EDO lacks: the harmonic seventh (7/4), the neutral third (11/9), and many microtonal colors in between.

The core trick is one line:

```haskell
st31 p = p |* (12/31)
```

Patterns are written in 31EDO *steps*, then scaled so that Tidal/SuperDirt's usual "12 notes per octave" pitch handling plays them at the right frequency. Everything else builds on this.

## Files

- [edo31_example_en.tidal](edo31_example_en.tidal) — toolkit + commented examples (English)
- [edo31_example_ja.tidal](edo31_example_ja.tidal) — the same, with Japanese comments

## Quick reference

| Function | Example | What it does |
|----------|---------|--------------|
| `n31` | `n31 "c ehf as3"` | note names → single notes |
| `ch31` | `ch31 "<c:maj f4:harm7:i>"` | chord names → chords (plain note names mix in freely) |
| `sc31` | `sc31 "major" "0 .. 7"` | scale name + degrees → melody |
| `sc31on` | `sc31on "c" "major" "0 .. 7"` | scale with a named root |
| `up31` | `up31 18 $ ...` | transpose in 31EDO steps (18 = fifth) |
| `arpSteady` | `arpSteady 8 $ ch31 "..."` | n-slot arpeggio over any chord |
| `chordList31` / `scaleList31` | | evaluate to list all chord / scale names |

Low-level functions returning raw 31EDO steps as `Pattern Note`: `nt31` (note names), `chd31` (chord names), `scale31` / `scale31on` (scales). `st31` converts steps to `note` values; `e31` is the `ControlPattern` version kept for legacy compatibility.

## Reference

### Note names — `n31`

`n31 "<c e g as ehf4>"` — root name + accidental + octave number (default octave is 5).

| Root | c | d | e | f | g | a | b |
|------|---|---|---|---|---|---|---|
| Steps | 0 | 5 | 10 | 13 | 18 | 23 | 28 |

| Accidental | `s` | `f` | `hs` | `hf` | `ss` | `ff` |
|------------|-----|-----|------|------|------|------|
| Meaning | sharp (+2) | flat (−2) | half-sharp (+1) | half-flat (−1) | double-sharp (+4) | double-flat (−4) |

In 31EDO, sharps and flats are *not* enharmonic: `cs` ≠ `df`. `ehf` is a neutral third above C; `as` approximates the harmonic seventh of C.

### Chords — `ch31`

`ch31 "<c:maj f4:harm7 g:dom7>"` — note name (accidentals and octave numbers allowed) + `:` + quality, optionally followed by `:`-separated voicing modifiers. Plain note names can be mixed into the same pattern: `ch31 "c:maj e g"`.

| Group | Qualities |
|-------|-----------|
| Highly consonant (near-just) | `maj` `harm7` `min` `submin` `harmdim` `submin7` `5` |
| Moderately consonant | `supmaj` `neut` `neut7` `aug` `maj7` `harm9` `harm11` |
| Functional harmony (meantone) | `dim` `dim7` `halfdim` `sus2` `sus4` `maj6` `min6` `min7` `dom7` `dom9` `min9` |
| Dissonant | `neut2` `wolf5f` `wolf5s` `clu3` `clu2` |
| Extremely dissonant (31EDO-only) | `clu1` |

Highlights: `harm7` is a 4:5:6:7 harmonic-seventh chord, `harm11` extends it to the full 4:5:6:7:9:11 otonality, `submin` uses the subminor third (7/6), `neut` the neutral third (11/9), and the `clu*` chords are microtonal clusters impossible in 12EDO. Evaluate `chordList31` to see every name.

### Voicing modifiers

Chain any number of modifiers after the quality, all separated by `:` — e.g. `"f4:maj:i2"`, `"g:dom7:d"`, `"c:maj:6"`.

| Modifier | Meaning |
|----------|---------|
| `i` / `iN` | invert once / N times |
| `o` | open voicing (2nd-from-bottom note up an octave) |
| `d` / `dN` | drop the 2nd / Nth-from-top note an octave (plain `d` = drop-2) |
| number `N` | extend or truncate the chord to N notes, continuing up the octaves |

Combining inversions with root octaves (e.g. `"<c:maj f4:maj:i2 g4:dom7:i c:maj>"`) gives progressions with minimal voice movement.

### Arpeggios — `arpSteady`

`arpSteady 8 $ ch31 "..."` spreads each chord over `n` equal slots, cycling through the chord tones from the bottom. The rhythm stays steady even when chords of different sizes are mixed; extending chords with `:6` or `:8` makes the arpeggio climb across octaves.

### Scales — `sc31` / `sc31on`

`sc31 "<scale name>" "<degrees>"` — degree 0 is the tonic; degrees beyond the scale size wrap up an octave (negative degrees wrap downward). `sc31on` takes a root note name as its first argument. (`scale31` / `scale31on` are the low-level versions returning raw steps.)

| Group | Scales |
|-------|--------|
| Meantone family | `penta5` `major` `minor` `harmminor` `melminor` `dorian` `phrygian` `lydian` `mixolydian` `chrom12` `enh19` |
| MOS scales unique to 31EDO | `mohajira` `orwell9` `miracle10` `blackjack` `myna4` `myna11` `wurschmidt7` `squares8` `valentine15` |
| World-scale approximations | `rast` `bayati` `hijaz` `slendro` `eq7` |
| Harmonic series | `harmonics` `otonal` |
| Symmetric scales | `wholetone` `dim8` |

Evaluate `scaleList31` to see every name.

### Transposition — `up31` / `st31`

`up31 18 $ ...` transposes a `ControlPattern` up 18 steps (a 31EDO fifth, ≈697 cents); combined with `off` it makes step-accurate echoes. `st31` converts step counts to `note` values, so `|+ note (st31 "<0 8 10 5>")` drifts a whole phrase in parallel on the 31EDO grid. Unlike 12EDO-unit shifts such as `|+ note 7`, these stay on the 31EDO grid (only octave shifts like `|+ note 12` are equivalent).
