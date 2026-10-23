# Languages and subtitles

Everything that makes up a language lives in one folder per language:
`Languages/<Name>/` beside `openjade.exe`. This covers translated text, voices,
movies and subtitles. The folder's name is the name the menus show.
`Languages/English/`, `Languages/Deutsch/`, `Languages/Français/` and
`Languages/Русский/` ship subtitles for all three games. `Languages/Polski/`
holds the Polish SoT and T2T subtitles. Español, Italiano and Nederlands also
ship the original WW scene and movie subtitles.

**Options → Language** chooses every language the game uses. It saves the
choices in `settings.ini` beside `openjade.exe`. A row appears only when there
is something to choose.

| Setting | What it changes | `settings.ini` |
| --- | --- | --- |
| Text Language | All text: menus, in-game text, tutorial hints and subtitles. Includes WW cutscene, movie and level subtitles from JSON. | `[game] text_language` |
| Voice Language | Spoken dialogue and the audio track of multi-language movies. | `[sound] voice_pack` |
| Show Subtitles | Off, Full or Cutscenes Only. Off hides every subtitle. | `[game] subtitles_enabled`, `subtitle_mode` |
| Speaker Names | Shows the speaker before each subtitle. | `[game] subtitle_speaker_names` |
| Text Size | The size of subtitles, tutorial hints and the games' own captions, including WW subtitles: 75% to 150%. Text the game lays out itself grows only as far as the screen width allows. | `[game] text_size` |
| Text Backdrops | Which text gets a box behind it: All Text (default), Subtitle Text Only (subtitles, including WW captions in cutscenes and movies), Tutorial Text Only (tutorial hints), or Off. | `[game] text_backdrops` (`all`, `subtitles`, `tutorials`, `off`) |
| Text Background Opacity | How dark that box is: Off, or 10% to 100% opacity (default 40%). | `[game] text_backdrop` |

Every change applies at once, without a restart.

All subtitles follow the text language. WW reads every caption from
`Languages/<Name>/Subtitles/ww.json`: original scene and movie subtitles,
plus the added gameplay dialogue. Text and timing never fall back to BF
subtitle tables. A missing file or entry produces no caption.

- **Text Language** reloads the menus, fonts, menu images and subtitles, and
  the page redraws in the new language. It is offered from the main menu only,
  and only when there are two or more languages. To change it during a game,
  quit to the main menu.
- **Show Subtitles** appears in SoT only when a language folder holds
  subtitles for the game.
- **Speaker Names** appears only when a language folder holds subtitles for
  the game.

## What follows the text language

Subtitles always follow the text language. Voices and movies follow it until
you choose a voice language:

- **Subtitles**: the subtitles in the folder named like the text language.
  For a pack, that is the pack's own folder. For a Retail language, it is the
  folder named after the language in its own name or in English, such as
  `Français` or `French`. Without one, no subtitles show.
- **Voices**: the text language pack's own voices. Otherwise, the Retail
  voices of the text language, such as `sound/french/`. Otherwise, the first
  voices listed.
- **Movies**: the game looks in three places, in this order:
  1. the `Video/` folder of the pack that holds the selected voices;
  2. the `Video/` folder of the text language's pack;
  3. the game's own `Video/` folder.

  A movie with several audio tracks plays the track of the voice language.
  A pack's movies may hold their language on any track: the Polish SoT
  movies put Polish last, the Polish T2T movies first. The game finds it by
  comparing the pack's movie with its own copy: the track the game's copy
  lacks is the pack's language. With other voices, a pack's movie plays the
  same track the game's copy would, or the game's copy plays instead.

## Non-English installations

openjade works from any edition's folder, not just an English one. The
folder's own language is the default language:

- Its `POPData.bf` names the language. If the name has no slot of its own, the
  menus show that name. For example, the Polish SoT edition shows "Polski".
- Its own voices carry the same name under Voice Language.
- Some editions name English even though they are translated, such as the
  Polish WW and T2T editions. Those show as "English".

English, German, French and Russian subtitles ship for all three games, with
Polish subtitles for SoT. Spanish, Italian and Dutch include original WW
subtitles. Other languages need a subtitle file in the folder
named after the language, in its own name or in English, such as
`Languages/Polski/Subtitles/sot.json`.

UI translations are available from the shipped `UI/<game>.json` catalogs.
To also replace the edition's game resources, install that edition as a pack.
For example, put the English edition's `POPData.bf` and archive into
`Languages/English/`, next to its `UI/` and `Subtitles/` folders.

## Retail languages

The Text Language row lists the installed edition, every language with a
`Languages/<Name>/UI/<game>.json` or `Tutorials/<game>.json` catalog, and optional packs of translated
game resources. `<game>` is `sot`, `ww` or `t2t`. A catalog for another game,
`common.json` alone, or subtitles alone do not add a text language.

UI translations come from JSON. A catalog needs no `POPData.bf`, `Menu/`,
MGM/MGB files or font files to appear in the list. Without matching resources,
it keeps the installed edition's script-text slot, fonts and images. Its
catalog does not duplicate the default entry when both name the same language.

**SoT and WW**

- SoT has five language slots: English, French, Spanish, Italian and German.
  WW adds Dutch.
- `POPData.bf` identifies the installed edition's default resource language.
- `prince.bf` often holds the in-game text of the other languages too.
  An edition's `POPData.bf` identifies its language and supplies SoT's menu
  fonts and images. Install it as a pack with the matching UI catalogs. See
  [Another Retail language](#another-retail-language-sot-ww).

**T2T**

- Optional game resources still use `Menu/<LANGUAGE>/` for the font in
  `Fonts/POP3_54.MFT`. MGB files are no longer read. Font presence does not
  determine which UI catalogs are listed.
- A resource pack's folder name selects its resource-language slot: `ENGLISH`,
  `FRENCH`, `SPANISH`, `ITALIAN` or `GERMAN`.
- When a catalog matches one of these languages and its font is already
  installed in the game's `Menu/`, it uses that resource slot and font.

**Voices**

- Each `sound/<language>/` folder inside `Sound/SoundLocal.FAT` and `.BIG` is
  listed under Voice Language.
- Multi-language editions hold several of these folders.

## Language packs

A pack adds a language from another edition of the same game: a localized
release, another region's edition, or a fan translation. Subtitles can be
added to any pack.

To install one, copy the edition's files into `Languages/<Name>/` beside
`openjade.exe`:

- Copy the files unchanged, in the same layout they have in that edition.
- The menus show the folder name as the language's name.
- File and folder names are not case-sensitive.

```
openjade.exe
Languages/
  Polski/
    POPData.bf           edition identity (SoT: also menu fonts and images)
    prince.bf            in-game text and fonts (T2T: pop3.bf)
    Menu/                T2T: script string packages and fonts
    UI/
      common.json        shared engine menu text
      sot.json           game-specific engine menu text
      ww.json
      t2t.json
    Sound/
      SoundLocal.FAT     voices
      SoundLocal.BIG
    Video/               movies
    Subtitles/
      sot.json           subtitles for each game (see Subtitles)
      ww.json
      t2t.json
```

Every file is optional: copy what the edition translates.

- **`POPData.bf`**
  - Supplies SoT's menu fonts and images. Its menu strings are not loaded.
  - Declares the edition's language (`Language=` under `[General]`).
- **`prince.bf` / `pop3.bf`**
  - The game compares this archive with yours and uses only what it
    translates: the string tables of its language slot, plus the menu
    resources (fonts, font pages, menu images and their materials).
  - Resources that differ from yours replace them. Resources that only the
    pack holds are added, in the pack's order. For example, the Polish T2T
    edition draws its fonts on pages of its own, which its materials point to.
    Any edition that brings its own fonts, pages or images works the same way.
  - Everything else still comes from your own archive, including levels,
    campaigns and mods.
  - The game reads only the archive's index and those parts, so a large archive
    loads quickly.
  - Mods stay in your own archive. If a mod changed a string table, font or
    menu image that the pack also holds, the pack's version is used while the
    pack is selected.
- **`Menu/`**
  - T2T reads fonts from its `<LANGUAGE>/` folders.
  - Legacy `.MGM` menus and all `.MGB` packages are not loaded.
- **`UI/`**
  - `common.json` and `<game>.json` hold the engine's own menu entries. See
    [Menu text](#menu-text). These JSON files do not require subtitles.
- **`Sound/SoundLocal.FAT` and `.BIG`**
  - Each `sound/<folder>/` inside becomes a voice language named after the
    pack: "Polski".
  - If the archive holds several folders, each name also includes the folder:
    "Polski English".
- **`Video/`**
  - Holds the movies. They play while the pack's voices or its text are
    selected.
  - The pack can hold only some of the movies. The others come from the game's
    own `Video/`.
- **`Subtitles/<game>.json`**
  - The language's subtitles for `sot`, `ww` or `t2t`. A game uses only its own
    file, so one folder can serve all three games.
  - They show while the folder's language is the text language.
  - See [Subtitles](#subtitles).

Where a pack is listed depends on what it holds:

| The pack holds | Text Language | Voice Language |
| --- | --- | --- |
| `UI/<game>.json` | Listed; uses installed resources unless the pack replaces them | |
| `POPData.bf`, `prince.bf` / `pop3.bf`, or `Menu/` | Listed | |
| `Sound/` | | Listed |
| `Subtitles/<game>.json` | Not listed: shown with the folder's text language | |
| Only `Video/` | Not listed: the pack is ignored | Not listed |

Put movies in the same pack as the voices they match.

### Language slots

The games store text in numbered slots: English, French, Spanish, Italian,
German and, in WW, Dutch. An edition in another language reuses one of these
slots. For example, the Polish editions of all three games replace the English
slot.

The game works out a pack's slot by itself. It uses the first rule that
applies:

1. **T2T**: the first of `ENGLISH`, `FRENCH`, `SPANISH`, `ITALIAN` and `GERMAN`
   found in the pack's `Menu/`.
2. The language that the pack's `POPData.bf` declares, if the archive
   translates that slot or the pack has no archive.
3. The first slot whose string tables the archive changes.
4. The language of your own `POPData.bf`.

The slot decides which string tables load. It also decides which voices and
movie audio track the text language picks when you have not chosen a voice
language.

A multi-language edition needs one pack per language:

- **SoT, WW**: copy the archive into each pack, and give each pack a
  `POPData.bf` that declares its language.
- **T2T**: copy `pop3.bf` and only that language's `Menu/<LANGUAGE>/` folder
  into each pack.

### Recipes

#### A full localized edition

This covers text, voices and movies.

| Game | Copy into `Languages/<Name>/` |
| --- | --- |
| SoT | `POPData.bf`, `prince.bf`, `UI/`, `Sound/`, `Video/` |
| WW | `POPData.bf`, `prince.bf`, `UI/`, `Video/`, and `Sound/` if the edition is dubbed |
| T2T | `POPData.bf`, `pop3.bf`, `Menu/`, `UI/`, `Sound/`, `Video/` |

Use the shipped `UI/` catalogs or translate them as described under
[Menu text](#menu-text); original game editions do not contain these JSON files.

Then choose the pack as the Text Language. Its voices, its movies, and its
`Subtitles/` follow automatically.

The Polish editions are laid out like this. The Polish WW edition is subtitled
only and has no `Sound/`.

#### Text only

Use this for a subtitled edition or a fan translation. Copy `POPData.bf`,
the archive and the matching `UI/` catalogs. For T2T, also copy `Menu/`.
Your voices stay as they are.

#### Another Retail language (SoT, WW)

Use this when your `prince.bf` already holds the language's in-game text, as
multi-language editions do.

Copy that edition's `POPData.bf`, for example `Languages/Français/POPData.bf`,
beside its `UI/` catalogs. The pack selects the in-game text your archive
already holds; SoT also uses its menu fonts and images.

#### Another T2T language

- If your `pop3.bf` already holds the language's text (multi-language
  editions), copy the edition's `Menu/<LANGUAGE>/` into your own `Menu/`.
- Otherwise, make a pack with that folder under `Menu/` and the edition's
  `pop3.bf`.

#### Voices only

Use this for a dub without translated text:

1. Copy `Sound/SoundLocal.FAT` and `.BIG` into the pack.
2. Add `Video/` if the dub has its own movies.
3. Choose the pack under Voice Language.

#### Subtitles only

Add `Subtitles/<game>.json` to the text language's folder: a pack's folder,
or for a Retail language a folder with its own or English name, such as
`Languages/Polski/` for the Polish SoT edition. See
[Adding a subtitle language](#adding-a-subtitle-language).

#### Movies

Put localized movies in the `Video/` folder of the pack whose voices or text
they match.

#### Mixing

Text and voices work on their own, so you can combine them. For example:
Polish text and subtitles with English voices.

You can install as many packs as you like. Each one is listed by its folder
name.

#### Fan translations

Install them like an edition: copy only the files the translation changes.

A translation that patches your game folder in place becomes your Retail
language. A pack keeps the original language available as well.

#### Upgrading from older builds

Older builds used different locations. Move the files as follows, then
delete the old ones:

| Older location | New location |
| --- | --- |
| `Sound/soundlocal_<Name>.FAT` and `.BIG` | `Languages/<Name>/Sound/SoundLocal.FAT` and `.BIG` |
| `subtitles/<game>/<code>.json` | `Languages/<Name>/Subtitles/<game>.json` |

Remove the `"language"` field from moved subtitle files. The folder name now
names the language.

### Letters and fonts

- **SoT and WW** use 8-bit fonts in their edition's code page. The Polish
  editions use Windows-1250.
  - Install the text and its fonts from the same edition, or letters show
    wrongly.
  - SoT keeps its menu fonts in `POPData.bf`, and its in-game fonts in the
    archive. WW keeps its fonts in the archive.
  - Button prompts work in every code page.
- **T2T** menus use Unicode text, drawn with the edition's font.
  - The font draws Latin-1, plus up to 32 further characters held in
    `Fonts/POP3_54.MFT`, such as Polish letters and typographic quotes.
  - Russian editions keep their Cyrillic font.
- **Language names** in the menus use these same fonts.
  - Name language folders with letters the font has, for example
    `Languages/Russkiy/`. Other letters show as `?`.
- **Subtitles** use Unicode and their own font. See [Fonts](#fonts).
- Any language can draw its menus, tutorial hints and subtitles in a font of
  your choice. See [Fonts](#fonts).

### Fonts

The configuration initializes Polish with the bundled fonts below
and writes all six path/size settings into `settings.ini` on the first save.
The paths are relative to the game folder.

| Game | Menu, tutorial and subtitle font | Menu size | Tutorial size | Subtitle size |
| --- | --- | --- | --- | --- |
| Sands of Time | `Fonts/AtlantisMF.ttf` | `1.12` | `1.0` | `1.0` |
| Warrior Within | `Fonts/Cinzel-Regular.ttf` | `1.4` | `1.0` | `1.0` |
| The Two Thrones | `Fonts/Cinzel-Regular.ttf` | `1.4` | `1.0` | `1.0` |

These presets apply only to Polish, including its language-pack folder.
All other languages use their existing fonts when available. If a game's
bitmap font or atlas cannot load, missing menu and tutorial faces fall back
to the bundled font above. Subtitles also use it before system fallback.
This recovery does not change the selected language or save font overrides;
switching languages tries that language's own font again. Usable custom
fonts keep their configured files and sizes.
The complete `assets/Fonts` directory is copied into runtime folders and the
release ZIP as `Fonts`, including `Cinzel-OFL.txt`.

Edit the generated `[fonts.Polski]` section to change these choices, or add a
`[fonts.<language>]` section for another language. Existing
sections keep their names and values; saving fills in missing defaults. Font
selection reads this loaded configuration without a separate game/language preset.

```ini
[fonts.Polski]
menu = Lato-Regular.ttf
menu_size = 1.0
tutorial = Lato-Regular.ttf
tutorial_size = 1.2
subtitles = Lato-Regular.ttf
subtitles_size = 1.1
```

- **The section name** is the language's folder name (`Polski`, `English`),
  or for a Retail language its own or English name (`Français` or
  `French`). Letter case does not matter.
- **`menu`, `tutorial`, `subtitles`** name TrueType or OpenType files. The game
  looks for each file in `Languages/<Name>/`, then the game folder, then the
  system's fonts (on Windows, macOS, Linux and Android). A full path works
  too. A key left out keeps the default font; an explicitly empty file name
  disables that custom font and uses the game's usual font selection.
- **`menu`** draws all menu text: pages, buttons, saved games and prompts.
- **`tutorial`** draws tutorial hints. In SoT and WW, it redraws the hints the
  game shows in the scene, where the game puts them, with their colour,
  button icons and the Text Size. In T2T, it draws the hint panel.
- **`subtitles`** draws all JSON subtitles, including original WW captions
  imported for cutscenes, movies and level lines.
- **`menu_size`, `tutorial_size`, `subtitles_size`** independently multiply
  the size of the corresponding custom font: `1.0`
  keeps their current size, `1.2` makes them 20% larger, and `0.8` makes them
  20% smaller. Each scales glyphs, inline icons, line spacing and text measurements
  together, preserving the relative sizes of headings and ordinary text.
  These multipliers do not change the distance between glyphs and their shadows.
  Tutorials and subtitles also retain the game's Text Size adjustment.
  Each defaults to `1.0`, except for the menu presets above; positive values
  are limited to `0.25` through `4.0`. Invalid or nonpositive values keep the
  default for that role. Roles without a custom font keep
  their original size.
- **Other defaults:** menus and tutorial hints use the game's or the language
  pack's own fonts. Subtitles use Arial, or Liberation Sans, Roboto or
  DejaVu Sans where Arial is not installed.
  - In WW and T2T, subtitles use the game's own font instead (WW's cutscene
    font, T2T's menu font), when it can draw every letter of the subtitle
    file. This is the text language's own font, so a pack's font, such as the
    Polish one, draws its letters.
- Text encoding is automatic, including with the original BF fonts and no
  `[fonts.*]` section. `codepage` and `bytes` are no longer settings; old
  entries are ignored and removed on the next settings save.
- Subtitle JSON text is UTF-8: write the actual letters, such as `Р` and `Ч`,
  directly. The engine maps these letters back to the edition's bitmap
  glyphs when using a BF font; TrueType fonts receive Unicode directly.
- Characters a custom font lacks come from the default fonts, including the
  system's Chinese, Japanese, Korean and Indic fonts.
- Restart the game after you edit these sections.

### Text encodings

The engine and [`extract_ww.py`](../../tools/subtitles/README.md#original-ww-subtitles)
share the fixed [language dictionary](../../src/TextLanguages/Encodings.inc).
Native and English language names are accepted, regardless of case.

| Language / folder | Code page | WW byte overrides |
| --- | --- | --- |
| English | 1252 | None |
| French / Français | 1252 | None |
| Spanish / Español | 1252 | None |
| Italian / Italiano | 1252 | None |
| German / Deutsch | 1252 | None |
| Dutch / Nederlands | 1252 | None |
| Polish / Polski | 1250 in SoT/WW; 1252 in T2T's host menu | `B0 → ą`, `B4 → ł` |
| Russian / Русский | 1251 | `8A → Р`, `8C → Ч` |

The moved bytes apply only to WW. SoT uses the standard code-page positions.
T2T's menu text is Unicode: its loaded extended character map takes precedence.
Its host menu keeps Latin-1 bytes, including in Polish; Cyrillic uses the standard
Windows-1251 positions. The engine falls back to
Windows-1252 for unknown language names. A new 8-bit localization needs a verified
entry in the dictionary; the subtitle extractor rejects unknown languages.

### Troubleshooting

**A pack is missing from Text Language**

- Its files must sit directly in `Languages/<Name>/`, not in a subfolder.
- The row appears only in the main menu, and only when there are two or more
  languages.
- A pack that holds only `Sound/` appears under Voice Language instead.

**A pack is missing from Voice Language**

It needs both `Sound/SoundLocal.FAT` and `Sound/SoundLocal.BIG`.

**Subtitles do not show**

- The file must be `Languages/<Name>/Subtitles/<game>.json`, where `<game>` is
  `sot`, `ww` or `t2t` for the game you are running.
- `<Name>` must be the text language's folder: a pack's own, or for a Retail
  language its own or English name.
- Show Subtitles must be on. Cutscenes Only shows only cutscene lines.
- A file the game cannot read is skipped. `openjade.log` names the file and
  the problem, such as a JSON error and its position.

**The text stays in the old language**

Look in `openjade.log` beside the game for this line:

```
text language: <name>, ordinal <slot>, <count> translated resources
```

If the pack holds an archive, a count of 0 means the archive is the same as
yours, or comes from another game.

**A pack switches back to the folder's own language**

The pack's menu fonts or images could not be loaded, so the game used the
folder's own language instead of starting without menus. `openjade.log` names
the pack:

```
ui: the language pack '<name>' has unusable menu fonts or images; using the game folder's own language
```

**Letters are wrong or missing**

- The text and its fonts come from different editions.
- A T2T font holds more than 32 characters beyond Latin-1.

**French, German or another Retail language is missing (SoT, WW)**

This is expected: the game lists only languages whose menus are installed.
See [Another Retail language](#another-retail-language-sot-ww).

**A movie plays in the wrong language**

- Put the movie in the `Video/` folder of the pack whose voices are selected.
- For movies with several audio tracks, the track follows the Voice Language.

## Subtitles

Each language's subtitles are `Languages/<Name>/Subtitles/<game>.json`, where
`<game>` is `sot`, `ww` or `t2t`.

- The text language's folder supplies them: a pack's own folder, or for a
  Retail language the folder with its own or English name.
- Edit the files in any text editor, and save them as UTF-8.
- The game reads the text language's file when it starts and when you change
  the text language. Restart the game after you add or edit a file.

WW files contain both its original cinematics, movies and level one-liners,
and the added gameplay dialogue. They are the only subtitle text and timing
source at runtime.

### Adding a subtitle language

1. Copy the game's file from `Languages/English/Subtitles/` into the text
   language's folder, for example `Languages/Polski/Subtitles/sot.json`.
   - For a pack, that is the pack's folder. For a Retail language, name the
     folder after the language in its own name or in English.
2. Translate every `"text"` and `"speaker"` value.
   - Leave `"sound"`, `"movie"`, `"table"`, `"start"`, `"end"` and `"duration"` unchanged.
   - `"game"`, `"type"`, `"trigger"`, `"campaign"` and `"note"` give the translator
     context. They may stay in English.

**Fonts**

- Text can use any Unicode script.
- Russian and Polish in SoT and WW use the bundled [font presets](#fonts).
- Otherwise the default font is Arial; the system's fonts fill in CJK, Hangul and
  Indic scripts. In WW the default is the game's cutscene font, and in T2T
  the game's menu font, when it can draw every letter of the file.
- To choose another font, set `subtitles` in the language's `[fonts.<Name>]`
  section. See [Fonts](#fonts).

### Menu text

The host UI's text lives in `Languages/<Name>/UI/`, independently of subtitles.
`common.json` holds shared phrases; `sot.json`, `ww.json` and `t2t.json` hold
entries for each game, including the Retail labels requested by the host UI.
Both use the same `Localization::Get` facade. No English dictionary is compiled
into the executable.

Resource keys use `Section.Element`, for example `P_ControlsMenu.TitleKeyboard`.
Repeated elements append `[1]`, `[2]`, etc.; identifier-only MGB elements use
`0x` followed by eight hexadecimal digits. Only keys requested by the host are
published. See [UI catalog validation](../../tools/localization/README.md).

To translate them, copy the relevant JSON files from `Languages/English/UI/`
to your language's `UI/` folder and edit the values. For example,
`Languages/Polski/UI/common.json`:

```json
{
  "Language.Title": "Język",
  "Language.Text": "Język tekstu"
}
```

- Loading merges English and selected-language `common.json` and `<game>.json`
  into one dictionary. Translated values replace English, and game-specific
  values replace common ones. UI lookups read only this dictionary; POPData,
  MGM and MGB are never consulted for missing UI entries. An empty string
  intentionally hides the text. A key absent from JSON returns `!MISSING!`.
- English `common.json` and the current game's English file are required.
  Translation files are optional. Invalid translation files are reported
  and skipped.
- Codes in braces, such as `{B6}`, are controller buttons. Keep them.
- Keep placeholders such as `{button}` unchanged too.
- Use `\n` for a line break and `\n\n` for a blank line.
- The menus draw this text in the language's menu font using its automatic
  [text encoding](#text-encodings). Letters the encoding or font lacks show as `?`.
- A folder containing only these JSON files does not add a selectable text
  language: the game's own translated text still comes from its Retail files.
- A translation may contain a requested key absent from the English edition.

### Lines

```json
{
  "sound": "1100B0",
  "speaker": "Farah",
  "type": "dialogue",
  "trigger": ["1101_Hall: 1101_Hall_SoundBank (hash_E641B292[1], event 1100F9)"],
  "text": "Prince, wait!"
}
```

- **`sound`**: the DARE sample that plays the line, in hexadecimal. The
  subtitle shows while that sample plays, and disappears when it stops.
- **`text`**: one subtitle for the whole sample. `\n` forces a line break.
- **`cues`**: use this instead of `text` for subtitles timed in seconds from
  the start of the sample:
  `{"start": 8.9, "end": 11.9, "speaker": "Prince", "text": "..."}`.
  A cue without `end` lasts until the next cue, or until the sample ends.
- **`speaker`**: shown before the text when Speaker Names is on. A cue's own
  `speaker` replaces the line's.
- **`type`**: lines of type `cutscene` are the only ones shown with **Show
  Subtitles: Cutscenes Only**. Types `dialogue`, `narration` and `combat` show
  in Full mode.

**CGI cinematics**

- These use `movie` instead of `sound`: the movie's file name without its
  extension, in any letter case. For example, `"movie": "cine_010"` or
  `"movie": "P3C_010"`.
- Their cue times count from the first frame.
- They show in both Full and Cutscenes Only modes.

### WW scripted scenes

Original WW scenes use `table` instead of `sound`. Keep the table ID and cue
order: the game's scripts select a table and page, pause its clock and control
its fade. Text is UTF-8 and times are seconds:

```json
{
  "table": "2F008F11",
  "type": "cutscene",
  "cues": [
    {"start": 5.0, "duration": 3.0, "text": "You will never reach our shores alive!"}
  ]
}
```

`duration` is the time before fading out; zero holds until the next page and a
negative value fades immediately. Empty text clears the current caption.
Tables with an empty `cues` array can be omitted; missing tables behave the
same way. Keep individual cues with empty text, since they clear a caption.
Missing pages end the sequence. These captions show in Full and Cutscenes Only
modes. WW movies use the usual `movie` format, for example `"cine0100"`.

To import the original localized text, see
[`tools/subtitles/README.md`](../../tools/subtitles/README.md#original-ww-subtitles).

## Saved-game names

The current save caption and the save/load lists use only `UI/<game>.json`.
`SaveNames.Checkpoint.<index>` contains the checkpoint names for that game;
WW additionally uses `SaveNames.Chapter.<index>` for the first line of each
saved-game caption. These entries belong in `sot.json`, `ww.json` or `t2t.json`,
never in `common.json`.

Keep the numeric suffixes: existing save files and scripts select those indices.
Edit Unicode text and use `\n` for line breaks. Names follow Text Language with
English fallback, independently of the installed game's BF/MGB resources.
An explicit empty entry stays empty; an unknown index produces no caption.

Extraction is documented in
[tools/localization](../../tools/localization/README.md#saved-game-names).

## WW reward-video text

`UI/ww.json` groups reward captions under `VideoOverlay.PowerUp.<name>` and
`VideoOverlay.Weapon.<name>`, for example `VideoOverlay.Weapon.WaterSword`.
Each has `Header`, `Title` and `Description` strings. `VideoOverlay.Continue`
is the shared continue prompt.

Use `\n` between description lines. Leading and internal blank lines preserve
vertical gaps; spaces are retained. Trailing empty lines can be omitted.
The game displays the first **ten lines**, counting blank lines, in the original
positions and ignores any additional lines. Text is not automatically wrapped
or resized. Each field falls back to English when missing; an explicit empty
`Description` hides the entire description.

## T2T video-overlay text

`UI/t2t.json` supplies `VideoOverlay.SavePrompt` and `VideoOverlay.QuitWarning`
to the game's video-overlay scripts. The gallery's Back instruction shares
`Common.Back`. These labels and the script-visible tutorial tables come from
JSON; the game no longer opens MGB packages.

## Tutorial hints

Tutorials use only `Languages/<Name>/Tutorials/<game>.json`, where `<game>` is
`sot`, `ww` or `t2t`. They follow Text Language and need no translated game archive
or MGB package. English fills missing translations; an unknown entry or an explicit
empty string clears the hint.

Shipped hints cover English, French, German, Polish and Russian in all three
games, Spanish and Italian in SoT and WW, and Dutch in WW. Each file contains
only that game's tutorial tables; SoT also includes the dictionary labels its
binding substitutions use.

Edit the Unicode JSON values while keeping the keys, for example `5000FBA2.29`.
Write line breaks as `\n`. Preserve WW/T2T action tokens such as `{BB}` and SoT
substitutions such as `\\p5\\A` and `\\d2\\`; the game substitutes current
bindings when displaying the hint. T2T's dictionary phrases are already expanded
in the catalog. Extraction and validation are documented in
[tools/localization](../../tools/localization/README.md#tutorial-catalogs).
