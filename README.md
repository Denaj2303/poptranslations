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
| Text Language | All text: menus, in-game text, tutorial hints and subtitles. In WW, also its own cutscene, movie and level subtitles. | `[game] text_language` |
| Voice Language | Spoken dialogue and the audio track of multi-language movies. | `[sound] voice_pack` |
| Show Subtitles | Off, Full or Cutscenes Only. Off also hides WW's and T2T's own subtitles. | `[game] subtitles_enabled`, `subtitle_mode` |
| Speaker Names | Shows the speaker before each subtitle. | `[game] subtitle_speaker_names` |
| Text Size | The size of subtitles, tutorial hints and the games' own captions, including WW's own subtitles: 75% to 150%. Text the game lays out itself grows only as far as the screen width allows. | `[game] text_size` |
| Text Background Opacity | The box behind subtitles, tutorial hints, and WW's own subtitles in cutscenes and movies: Off, or 10% to 100% opacity (default 40%). | `[game] text_backdrop` |

Every change applies at once, without a restart.

All text is in one language, the text language. WW subtitles its own
cutscenes, movies and some level lines in it, and the subtitle files add the
gameplay dialogue that WW leaves out. Show Subtitles, Text Size and Text
Background Opacity apply to both.

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

## Non-English installations

openjade works from any edition's folder, not just an English one. The
folder's own language is the default language:

- Its `POPData.bf` names the language. If the name has no slot of its own, the
  menus show that name. For example, the Polish SoT edition shows "Polski".
- Its own voices carry the same name under Voice Language.
- Some editions name English even though they are translated, such as the
  Polish WW and T2T editions. Those show as "English".

The shipped subtitles are English. On a non-English installation, none show
until its language has a subtitle file. Add one in the folder named after the
language, in its own name or in English, such as
`Languages/Polski/Subtitles/sot.json`.

To add English, or any other language, install that edition as a pack. For
example, put the English edition's `POPData.bf` and archive into
`Languages/English/`, next to its `Subtitles/` folder.

## Retail languages

The Text Language row lists the languages the game's own files hold
completely: menus as well as in-game text.

**SoT and WW**

- SoT has five language slots: English, French, Spanish, Italian and German.
  WW adds Dutch.
- Only the language of your `POPData.bf` is listed.
- `prince.bf` often holds the in-game text of the other languages too. Their
  menus, however, come only with their own edition's `POPData.bf`. To add one
  of them, install that `POPData.bf` as a pack. See
  [Another Retail language](#another-retail-language-sot-ww).

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
  - They show while the folder's language is the text language. The file can
    also translate the menu entries the game adds.
  - See [Subtitles](#subtitles) and [Menu text](#menu-text).

Where a pack is listed depends on what it holds:

| The pack holds | Text Language | Voice Language |
| --- | --- | --- |
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

#### Another Retail language (SoT, WW)

Use this when your `prince.bf` already holds the language's in-game text, as
multi-language editions do.

Copy only that edition's `POPData.bf`, and for WW its `Menu/`. For example:
`Languages/Français/POPData.bf`. The pack brings the menus and uses the
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

Each language can name its own fonts in `settings.ini`, in a section you add
by hand. The game reads these sections and keeps them as you wrote them.

```ini
[fonts.Polski]
menu = Lato-Regular.ttf
tutorial = Lato-Regular.ttf
subtitles = Lato-Regular.ttf
codepage = 1250
bytes = B0:ą B4:ł
```

- **The section name** is the language's folder name (`Polski`, `English`),
  or for a Retail language its own or English name (`Français` or
  `French`). Letter case does not matter.
- **`menu`, `tutorial`, `subtitles`** name TrueType or OpenType files. The game
  looks for each file in `Languages/<Name>/`, then the game folder, then the
  system's fonts (on Windows, macOS, Linux and Android). A full path works
  too. A key left out keeps the default font.
- **`menu`** draws all menu text: pages, buttons, saved games and prompts.
- **`tutorial`** draws tutorial hints. In SoT and WW, it redraws the hints the
  game shows in the scene, where the game puts them, with their colour,
  button icons and the Text Size. In T2T, it draws the hint panel.
- **`subtitles`** draws the authored subtitles. In WW, it also redraws WW's own
  subtitles (cutscenes, movies and level lines), so both look alike.
- **Defaults:** menus and tutorial hints use the game's or the language
  pack's own fonts. Subtitles use Arial, or Liberation Sans, Roboto or
  DejaVu Sans where Arial is not installed.
  - In WW, subtitles use the game's cutscene font instead, when it can draw
    every letter of the subtitle file. This is the text language's own font,
    so a pack's font, such as the Polish one, draws its letters.
- **`codepage`** tells the game which letter each byte of the language's text
  is, so a TrueType font draws the right letters, and how to write
  [menu text](#menu-text) translations and language names. Windows code pages 1250 to
  1258 are built in; the default is 1252 (Western). Examples: 1250 for
  Czech, Hungarian, Polish or Slovak, 1251 for Russian, 1253 for Greek,
  1254 for Turkish.
  - SoT and WW text needs the edition's code page.
  - T2T menus are Unicode. Russian needs 1251, because T2T stores its
    Cyrillic letters on those bytes. Other T2T languages need none.
- **`bytes`** corrects single bytes where an edition moved letters away from
  its code page: each byte in hexadecimal, a colon and the letter (or
  `U+hhhh`). The Polish WW edition moved `ą` and `ł` to B0 and B4,
  because WW uses their code-page bytes for controller buttons.
- Characters a custom font lacks come from the default fonts, including the
  system's Chinese, Japanese, Korean and Indic fonts.
- Restart the game after you edit these sections.

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

WW already subtitles its cinematics, its movies and some level one-liners.
`Languages/English/Subtitles/ww.json` covers the gameplay dialogue that WW
leaves unsubtitled.

### Adding a subtitle language

1. Copy the game's file from `Languages/English/Subtitles/` into the text
   language's folder, for example `Languages/Polski/Subtitles/sot.json`.
   - For a pack, that is the pack's folder. For a Retail language, name the
     folder after the language in its own name or in English.
2. Translate every `"text"` and `"speaker"` value.
   - Leave `"sound"`, `"movie"`, `"start"` and `"end"` unchanged.
   - `"game"`, `"type"`, `"trigger"`, `"campaign"` and `"note"` give the translator
     context. They may stay in English.
3. Translate the `"menu"` values. See [Menu text](#menu-text).

**Fonts**

- Text can use any Unicode script.
- The default font is Arial; the system's fonts fill in CJK, Hangul and
  Indic scripts. In WW the default is the game's cutscene font when it can draw
  every letter of the file.
- To choose another font, set `subtitles` in the language's `[fonts.<Name>]`
  section. See [Fonts](#fonts).

### Menu text

The game adds menu entries that the games' own text lacks, such as this
Language page. The `"menu"` object of a subtitle file translates them into
its language:

```json
"menu": {
  "Language.Title": "Język",
  "Language.Text": "Język tekstu",
  "Multiplayer.CameraHint": "Naciśnij {B6}, aby pokazać wyjście."
}
```

- The English files list every entry. Entries you leave out or leave in
  English stay English.
- Codes in braces, such as `{B6}`, are controller buttons. Keep them.
- The menus draw this text in the language's menu font. In SoT and WW, set the
  language's `codepage` (see [Fonts](#fonts)), or letters outside Western
  European ones show as `?`. Letters the font lacks show as `?` too.
- A file can hold `"menu"` alone, without `"lines"`, to translate only the
  menus.
- `openjade.log` names any entry the game does not know.

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
