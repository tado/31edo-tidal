# 31 EDO ("Equal Division of the Octave") experiments on TidalCycles

A small toolkit for playing in **31 equal divisions of the octave (31EDO)** with [TidalCycles](https://tidalcycles.org/). It gives you note names, named chords, arpeggios, and scales — all in 31EDO — using only plain Tidal, with no SuperDirt or synth-side configuration.

31EDO divides the octave into 31 equal steps (each ≈38.7 cents). It is close to quarter-comma meantone, so ordinary functional harmony works, while also providing near-just intervals that 12EDO lacks: the harmonic seventh (7/4), the neutral third (11/9), and many microtonal colors in between.

The core trick is one line:

```haskell
e31 p = p |* note (12/31)
```

## Files

- [edo31_example_en.tidal](edo31_example_en.tidal) — toolkit + commented examples (English)
- [edo31_example_ja.tidal](edo31_example_ja.tidal) — the same, with Japanese comments

Patterns are written in 31EDO *steps*, then scaled so that Tidal/SuperDirt's usual "12 notes per octave" pitch handling plays them at the right frequency. Everything else builds on this.

## Reference

### Note names — `n31`

`n31 "<c e g as ehf4>"` — root name + accidental + octave number (default octave is 5).

| Root | c | d | e | f | g | a | b |
|------|---|---|---|---|---|---|---|
| Steps | 0 | 5 | 10 | 13 | 18 | 23 | 28 |

| Accidental | `s` | `f` | `hs` | `hf` |
|------------|-----|-----|------|------|
| Meaning | sharp (+2) | flat (−2) | half-sharp (+1) | half-flat (−1) |

In 31EDO, sharps and flats are *not* enharmonic: `cs` ≠ `df`. `ehf` is a neutral third above C; `as` approximates the harmonic seventh of C.

### Chords — `ch31`

`ch31 "<c:maj f:harm7 g:dom7>"` — root name (accidentals allowed) + `:` (or `'`) + quality.

| Group | Qualities |
|-------|-----------|
| Highly consonant (near-just) | `maj` `harm7` `min` `submin` `harmdim` `submin7` |
| Moderately consonant | `supmaj` `neut` `aug` `maj7` `harm9` |
| Functional harmony (meantone) | `dim` `sus2` `sus4` `min7` `dom7` |
| Dissonant | `neut2` `wolf5f` `wolf5s` `clu3` `clu2` |
| Extremely dissonant (31EDO-only) | `clu1` |

Highlights: `harm7` is a 4:5:6:7 harmonic-seventh chord, `submin` uses the subminor third (7/6), `neut` the neutral third (11/9), and the `clu*` chords are microtonal clusters impossible in 12EDO.

### Arpeggios — `arpSteady`

`arpSteady 8 $ ch31 "..."` spreads each chord over `n` equal steps, cycling through the chord tones from the bottom. The rhythm stays steady even when triads and tetrads are mixed.

### Scales — `scale31` / `scale31on`

`scale31 "<scale name>" "<degrees>"` — degree 0 is the tonic; degrees beyond the scale size wrap up an octave. `scale31on` takes a root note name as its first argument.

| Group | Scales |
|-------|--------|
| Meantone family | `penta5` `major` `minor` `harmminor` `melminor` `chrom12` `enh19` |
| MOS scales unique to 31EDO | `mohajira` `orwell9` `miracle10` `blackjack` `myna4` `myna11` `wurschmidt7` `squares8` `valentine15` |
| World-scale approximations | `rast` `bayati` `slendro` `eq7` |
| Harmonic series | `harmonics` `otonal` |
| Symmetric scales | `wholetone` `dim8` |


