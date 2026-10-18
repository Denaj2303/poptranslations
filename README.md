# Languages and subtitles

Everything that makes up a language lives in one folder per language:
`Languages/<Name>/` beside `openjade.exe`. This covers translated text, voices,
movies and subtitles. The folder's name is the name the menus show.
`Languages/English/` ships with the game and holds the English subtitles.

**Options → Language** chooses every language the game uses. It saves the
choices in `settings.ini` beside `openjade.exe`. A row appears only when there
is something to choose.

| Setting | What it changes | `settings.ini` |
| --- | --- | --- |
| Text Language | Menus and in-game text. | `[game] text_language` |
| Voice Language | Spoken dialogue and the audio track of multi-language movies. | `[sound] voice_pack` |
| Show Subtitles | Off, Full or Cutscenes Only. Off also hides WW's and T2T's own subtitles. | `[game] subtitles_enabled`, `subtitle_mode` |
| Subtitle Language | The language folder whose subtitles show. | `[game] subtitle_language` (the folder name) |
| Speaker Names | Shows the speaker before each subtitle. | `[game] subtitle_speaker_names` |
| Subtitle Size | 75% to 150%. | `[game] subtitle_size` |
| Text Background | The box behind subtitles, tutorial hints, and WW's own subtitles in cutscenes and movies: Off, or 10% to 100% opacity (default 40%). | `[game] text_backdrop` |

Every change applies at once, without a restart.

- **Text Language** reloads the menus, fonts and menu images, and the page
  redraws in the new language. It is offered from the main menu only, and
  only when there are two or more languages. To change it during a game, quit
  to the main menu.
- **Show Subtitles** appears in SoT only when a language folder holds
  subtitles for the game.
- **Subtitle Language**, **Speaker Names** and **Subtitle Size** appear only
  when a language folder holds subtitles for the game.

## What follows the text language

Voices, subtitles and movies have no language of their own until you choose
one. Until then they follow the text language:

- **Voices**: the text language pack's own voices. Otherwise, the Retail
  voices of the text language, such as `sound/french/`. Otherwise, the first
  voices listed.
- **Subtitles**: the subtitles in the folder named like the text language.
  For a pack, that is the pack's own folder. For a Retail language, it is the
  folder named after the language in its own name or in English, such as
  `Français` or `French`. Otherwise, `English`.
- **Movies**: the game looks in three places, in this order:
  1. the `Video/` folder of the pack that holds the selected voices;
  2. the `Video/` folder of the text language's pack;
  3. the game's own `Video/` folder.

  A movie with several audio tracks plays the track of the voice language.

## Retail languages

The Text Language row lists the languages the game's own files already hold.

**SoT and WW**

- SoT has five language slots: English, French, Spanish, Italian and German.
  WW adds Dutch.
- A language is listed when `prince.bf` holds its in-game text.
- The menus come from `POPData.bf`, which holds only one language. Other
  languages therefore show their in-game text with those menus. To translate
  the menus as well, add that language's `POPData.bf` as a pack. See
  [Menus for a Retail language](#menus-for-a-retail-language).

**T2T**

- A language is listed when it has a `Menu/<LANGUAGE>/` folder that holds
  `.mgb` files and `Fonts/POP3_54.MFT`.
- The folder names are `ENGLISH`, `FRENCH`, `SPANISH`, `ITALIAN` and `GERMAN`.

**Voices**

- Each `sound/<language>/` folder inside `Sound/SoundLocal.FAT` and `.BIG` is
  listed under Voice Language.
- Multi-language editions hold several of these folders.

## Language packs

A pack adds a language from another edition of the same game: a localized
release, another region's edition, or a fan translation. Subtitles can be
added to any pack, or make up a pack on their own.

To install one, copy the edition's files into `Languages/<Name>/` beside
`openjade.exe`:

- Copy the files unchanged, in the same layout they have in that edition.
- The menus show the folder name as the language's name.
- File and folder names are not case-sensitive.

```
openjade.exe
Languages/
  Polski/
    POPData.bf           menu text (SoT: also menu fonts and images)
    prince.bf            in-game text and fonts (T2T: pop3.bf)
    Menu/                WW: *.MGM menu files; T2T: <LANGUAGE>/ folders
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
  - Holds the menu text. In SoT it also holds the menu fonts and images.
  - Declares the edition's language (`Language=` under `[General]`).
- **`prince.bf` / `pop3.bf`**
  - The game compares this archive with yours and uses only what it
    translates: the string tables of its language slot, plus the menu fonts and
    menu images that differ.
  - Everything else still comes from your own archive, including levels,
    campaigns and mods.
  - The game reads only the archive's index and those parts, so a large archive
    loads quickly.
- **`Menu/`**
  - WW reads its `.MGM` menu files from here.
  - T2T reads its `<LANGUAGE>/` folders from here, which hold menus and a font.
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
  - See [Subtitles](#subtitles).

Where a pack is listed depends on what it holds:

| The pack holds | Text Language | Voice Language | Subtitle Language |
| --- | --- | --- | --- |
| `POPData.bf`, `prince.bf` / `pop3.bf`, or `Menu/` | Listed | | |
| `Sound/` | | Listed | |
| `Subtitles/<game>.json` | | | Listed |
| Only `Video/` | Not listed: the pack is ignored | Not listed | Not listed |

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
| SoT | `POPData.bf`, `prince.bf`, `Sound/`, `Video/` |
| WW | `POPData.bf`, `prince.bf`, `Menu/`, `Video/`, and `Sound/` if the edition is dubbed |
| T2T | `POPData.bf`, `pop3.bf`, `Menu/`, `Sound/`, `Video/` |

Then choose the pack as the Text Language. Its voices, its movies, and its
`Subtitles/` follow automatically.

The Polish editions are laid out like this. The Polish WW edition is subtitled
only and has no `Sound/`.

#### Text only

Use this for a subtitled edition or a fan translation. Copy `POPData.bf` and
the archive. For WW and T2T, also copy `Menu/`. Your voices stay as they are.

#### Menus for a Retail language

This applies to SoT and WW, when your `prince.bf` already holds the language.

Copy only that edition's `POPData.bf`, and for WW its `Menu/`. For example:
`Languages/Français/POPData.bf`. The pack translates the menus and uses the
in-game text your archive already holds.

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

Add `Subtitles/<game>.json` to a new folder, such as
`Languages/Polski/Subtitles/sot.json`, or to an existing pack. See
[Adding a subtitle language](#adding-a-subtitle-language).

#### Movies

Put localized movies in the `Video/` folder of the pack whose voices or text
they match.

#### Mixing

Each row works on its own, so you can combine languages. For example: Polish
text, English voices and English subtitles.

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
- **Subtitles** use Unicode and their own font. See
  [Adding a subtitle language](#adding-a-subtitle-language).

### Troubleshooting

**A pack is missing from Text Language**

- Its files must sit directly in `Languages/<Name>/`, not in a subfolder.
- The row appears only in the main menu, and only when there are two or more
  languages.
- A pack that holds only `Sound/` appears under Voice Language instead.

**A pack is missing from Voice Language**

It needs both `Sound/SoundLocal.FAT` and `Sound/SoundLocal.BIG`.

**A language is missing from Subtitle Language**

- The file must be `Languages/<Name>/Subtitles/<game>.json`, where `<game>` is
  `sot`, `ww` or `t2t` for the game you are running.
- A file the game cannot read is skipped. `openjade.log` names the file and
  the problem, such as a JSON error and its position.

**The text stays in the old language**

Look in `openjade.log` beside the game for this line:

```
text language: <name>, ordinal <slot>, <count> translated resources
```

If the pack holds an archive, a count of 0 means the archive is the same as
yours, or comes from another game.

**Letters are wrong or missing**

- The text and its fonts come from different editions.
- A T2T font holds more than 32 characters beyond Latin-1.

**Menus stay in English with a Retail language (SoT, WW)**

This is expected. See
[Menus for a Retail language](#menus-for-a-retail-language).

**A movie plays in the wrong language**

- Put the movie in the `Video/` folder of the pack whose voices are selected.
- For movies with several audio tracks, the track follows the Voice Language.

## Subtitles

Each language's subtitles are `Languages/<Name>/Subtitles/<game>.json`, where
`<game>` is `sot`, `ww` or `t2t`.

- The game lists every language folder that holds its file under Subtitle
  Language, by folder name.
- Edit the files in any text editor, and save them as UTF-8.
- The game finds the files when it starts, and reads a file when you choose
  it. Restart the game after you add or edit a file.

WW already subtitles its cinematics, its movies and some level one-liners.
`Languages/English/Subtitles/ww.json` covers the gameplay dialogue that WW
leaves unsubtitled.

### Adding a subtitle language

1. Copy the game's file from `Languages/English/Subtitles/` into the language's
   folder, for example `Languages/Polski/Subtitles/sot.json`.
   - The folder name is the name the menu shows.
   - If the folder is also a pack, the subtitles follow its text
     automatically.
2. Translate every `"text"` and `"speaker"` value.
   - Leave `"sound"`, `"movie"`, `"start"` and `"end"` unchanged.
   - `"game"`, `"type"`, `"trigger"`, `"campaign"` and `"note"` give the translator
     context. They may stay in English.

**Fonts**

- Text can use any Unicode script.
- The default font is Palatino Linotype, which covers Latin, Greek and
  Cyrillic. Windows fonts fill in CJK, Hangul and Indic scripts.
- A file can name its own font, placed either beside the file (in
  `Subtitles/`) or in the Windows fonts folder:
  `"font": "NotoSans-Regular.ttf"`.

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
