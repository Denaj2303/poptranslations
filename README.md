# Subtitles

`subtitles/<game>/<language>.json` beside `openjade.exe`, where `<game>` is
`sot`, `ww` or `t2t`. The game lists every `.json` file in its folder under
**Game Options → Subtitle Language**, showing the file's `language` name; the
same page sets Subtitles, Show Subtitles (Full or Cutscenes Only), Speaker
Names and Subtitle Size. Edit
the files with any text editor and save them as UTF-8; the game reads them at
startup, so restart it after a change.

## Adding a language

1. Copy `en.json` to a new file named after the language code, e.g. `pl.json`.
2. Set `"language"` to the name the menu shows, e.g. `"Polski"`.
3. Translate every `"text"` and `"speaker"` value. Leave `"sound"`, `"start"`
   and `"end"` unchanged; `"type"`, `"trigger"`, `"campaign"` and `"note"` are
   context for the translator and may stay in English.

Text can use any Unicode script. The default font is Palatino Linotype (Latin,
Greek, Cyrillic) with Windows fallbacks for CJK, Hangul and Indic scripts. A
file can name its own font, either beside the file or in the Windows fonts
folder: `"font": "NotoSans-Regular.ttf"`. The menu uses the game's 8-bit font,
so a language name outside Latin-1 appears there as its file code (`RU`).

## Lines

```json
{
  "sound": "1100B0",
  "speaker": "Farah",
  "type": "dialogue",
  "trigger": ["1101_Hall: 1101_Hall_SoundBank (hash_E641B292[1], event 1100F9)"],
  "text": "Prince, wait!"
}
```

- `sound`: the DARE sample that plays the line (hexadecimal). The subtitle
  shows while that sample plays and disappears when it stops.
- `text`: one caption for the whole sample. `\n` forces a line break.
- `cues`: instead of `text`, captions timed in seconds from the start of the
  sample: `{"start": 8.9, "end": 11.9, "speaker": "Prince", "text": "..."}`.
  A cue without `end` lasts until the next cue or the end of the sample.
- `speaker`: shown before the text when **Speaker Names** is on; a cue's own
  `speaker` overrides the line's.
- `type`: `cutscene` lines are the only ones shown with **Show Subtitles:
  Cutscenes Only**. `dialogue`, `narration` and `combat` show in Full mode.

CGI cinematics use `movie` instead of `sound`: the movie file name without its
extension, in any case (`"movie": "cine_010"`, `"movie": "P3C_010"`). Their cue
times count from the first frame, and they show in both modes.
