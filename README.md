# dBpoweramp-naming-help
A guide to use the Dynamic Naming Syntax and Tools in dBpoweramp.

[dBpoweramp Music Converter & CD Ripper](https://www.dbpoweramp.com/) - Is a powerful audio management application and utility to batch rip, tag, and convert audio, video, and images.  It supports all of the major codecs, has a rich plugin interface, and is scriptable.  A companion app, PerfectTUNES, can help add missing Album Art covers, edit ID Tags, remove duplicate tracks, and compare your CD audio rip to an online database.

## Intro
The dBpoweramp Dynamic Naming feature provides a powerful but limited scriptable naming interface.  It's language seems to be a cross between a templating engine, and a functional scripting language.  This documentation hopes to improve on the software's documentation and enable more users to create the audio ontologies they desire.

#### Language Caveats
It has limited control flow, and some ability to nest statements; but, it does not seem to be a complete programing language.  The operators, functions, or control statements are in a pre-fix notation form -- similar to LISP.

Because dBpoweramp Dynamic Naming feature lacks the ability to create functions, store values for reuse or leverage modular code; code can get pretty complex and difficult to maintain.  I am writing this guide to correct, update and simplify the production of these scripts.

#### Example
Here is the CD Ripper Default Naming script
```
[IFVALUE]album artist,[album artist],[IFCOMP]Various Artists[][IF!COMP][artist][][]\[album]\[track] [artist] - [title]
```
## Function Summary
### Compilation, Anthology, and Soundtrack Functions
For sound files that are properly tagged with the *Compilation Tag,* it's possible to change how file title or path are generated.  Some CD ripping and sound file production methods leave this tag off or, label it incorrectly.

|	Name		|   Params   |	Description	    													|
|	---			|   ---      |  --- 			    												|
|	IFCOMP  |	**`string`**  | included if "Compilation tag" is set    <br>`[IFCOMP]included if CompilationTag is set[]`              |
|	IF!COMP	|	**`string`**  |	included if "Compilation tag" is not set    <br>`[IF!COMP]included if CompilationTag is NOT set[]`     |

### Multi Disc Functions
Multidisc sets identified can be simplified with these boolean control functions.  Similar functionality can be achived with the Logical Functions in the language.

|	Name		|	Params  |   Description 														|
|	---			|	---     |   ---     															|
|	IFMULTI		|	**`string`**  | included if part of a multi disc (number is not null, > 1)  <br>`[IFMULTI]included if MultiDisc set[]`            |
|	IF!MULTI	|	**`string`**  | included if NOT part of a multi disc (number is null \|\| <=1)  <br>`[IF!MULTI]included if NOT MultiDisc set[]` |

### String Manipulation Functions
Manipulating Strings can be part of a final step in curating your file and path names.  Within dBpoweramp there are other methods to manipulate strings before processing them here.

|	Name		|	Params  |   Description														|
|	---			|   ---     |   --- 															|
|	UPPER		|	**`string`**  | upper-cases the string  <br>`[UPPER]string to upper[]`  returns `STRING TO UPPER`        |
|	LOWER		|	**`string`**  | lower-cases the string  <br>`[LOWER]String TO Lower[]`  returns `string to lowwer`       |
|	TRIM		|	**`string`**  | trims spaces from beginning or end of string (chars:` ` )   <br>`[TRIM] extra spaces []` returns `"extra spaces"`    |
|	RIGHT		|	**`count, string`**  | uses last x characters from a string <br>`[RIGHT]3,Best of the 1980s,[]` returns `80s`   |
|	GRAB		|   **`from, to, string`**  |   extracts a portion of string; if **`to`** is omitted, then string is grabbed to the end <br>`[GRAB]4,,The Beatles[]` returns `Beatles` <br>`[GRAB]1,3,The Beatles[]` returns `"The"`	|
|	DEL			|   **`from, to, string`**  |   removes a portion of a string; if **`to`** is omitted, then string is removed after **`from`**  <br>`[DEL]3,,The Beatles[]`	returns `"The"` <br>`[DEL]2,4,abcd123456789[]` returns `a123456789` |
|	SETLEN		|   **`count, pre, post, string`**  |   guarantees a string length, either pads or clips the string <br>`[SETLEN]4,,,abcdefg[]` returns `abcd`  <br>`[SETLEN]12,65,,abcdefg[]` returns `AAAAAabcdefg`   <br>`[SETLEN]12,,66,abcdefg[]`  returns `abcdefgBBBBB`  |
|   MAXLENGTH   |   **`len, string`**  |   clips a string if it's ove the maximum length(**`len`**) of the **`string`** <br>`[MAXLENGTH]14,long songtitle (15 year anniversary)[]` returns `long songtitle` |
|	WORD		|   **`string, wordcount`**  |	trims a string to **`wordcount`** limit.  <br>`[WORD]A Quick Brown Fox,2[]` returns `A Quick`	|
|	SPLIT		|   **`letter-or-string, string, position`**  |   splits a **`string`** by **`<letter or string>`**(a character, or sub-string) and returns a sub-string at **`position`**. To split by ","(comma) enter no **`string`**. <br>`[SPLIT];,Martha Stewart;Snoop Dog,2[]` returns `Snoop Dog`   |
|   REPLACE     |   **`search, replacement, string`**  |   **`string`** is searched by the **`search`** string and is substituted with the **`replacement`**. To search for ","(comma) enter no **`string`**. <br>`[REPLACE]feat.,;,Martha Stewart feat. Snoop Dog[]` returns `Martha Stewart ; Snoop Dog`  |
|   GROUP       |   **`count, string`**  |   returns a hash-group value for a **`string`**.  group size is defined by **`count`**. <br>**`count`** of size 4, uses the series \[a-d,e-h,...\], `[GROUP]4,Dua Lipa[]` returns `"a-d"` <br>**`count`** of size 3, uses the series \[a-c,d-f,...\], `[GROUP]3,Dua Lipa[]` returns `"d-f"` |

### Logical Functions
Do basic comparisons and control flow.

|	Name		|	Params  |   Description														|
|	---			|	---     | 	---     														|
|	IFEQUALS	|   **`tag, equals, string`**  |    if **`tag`** value == **`equals`** value, then include string		|
|	IF!EQUALS	|   **`tag, equals, string`**  |	if **`tag`** value != **`equals`** value, then include string		|
|	IF			|   **`tag, condition, match, stringmatch, stringnomatch`**  |compares a **`tag`** with a **`condition`**(`=, len\<len\>, len=\<len\> `), then includes either the **`stringmatch`** or **`stringnomatch`** string	|
|	IFVALUE		|   **`tag, strifvalue, strnovalue`**  |    tests if **`tag`** is null, and includes **`strifvalue`** or **`strnovalue`**	|

### File System / Folder Functions
Usefull mostly for batch jobs, as CD Ripping projects will not originate with a filename.

|	Name		|   Params   |	Description	    													|
|	---			|   ---      |  --- 			    												|
| TRIMFIRSTFOLDER |	**`string`**  | removes first folder form the path.  (ex. `[TRIMFIRSTFOLDER]f1\f2\f3[]` returns `f2\f3`) |
| TRIMLASTFOLDER  |	**`string`**  | removes last folder from path. (ex. `[TRIMLASTFOLDER]f1\f2[]` returns `f1\f2`)
| FRONTFOLDER |	**`position`**  | returns the folder in **`position`** from the beginning of the path.   |
| BACKFOLDER  |	**`position`**  | returns the folder in **`position`** relative from the end of the path.  |

### Custom Tags
Useful to access non-standard tags

|	Name		|   Params      |	Description	    													|
|	---			|   ---         |  --- 			    											        |
|   tag         |   `tagname`   |  access a custom **`tagname`** <br>`[TAG]tagname[]`                   |
|   tags        |   `key,delimiter` | `[TAGS]key,delimiter[]`                                           |

## Common Tag / Value / Variables

### dBpoweramp Audio Converter (dMC) Tags, Properties
for batch processing, converting, processing audio files

#### dBpoweramp Naming Tags
|	Name		    |	Description														|
|	---			    |	--- 															|
|   [artist]        |   track's artist or (artist list)                                 |
|   [album artist]  |   album's artist                                                  |
|   [album]         |   album title                                                     |
|   [title]         |   track title                                                     |
|   [disc]          |   disc number                                                     |
|   [disc_total]    |   total number of discs in **multi-disc**                         |
|   [band]          |                                                                   |
|   [composer]      |   album composer                                                  |
|   [conductor]     |   album conductor; normally used for classical music              |
|   [genre]         |   album or track genre, depending on settings                     |
|   [isrc]          |   International Standard Recording Code                           |
|   [label]         |                                                                   |
|   [mood]          |                                                                   |
|   [style]         |                                                                   |
|   [tempo]         |                                                                   |
|   [year]          |   YYYY MM DD, software configurable to YYYY                       |
|   [track]         |                                                                   |
|   [track_unpad]   |                                                                   |
|   [track_total]   |   total number of tracks in album/disc                            |
|   [track_total_unpad] |                                                               |
|   [upc]           |   album UPC code                                                  |
|   [tag]\<key\>[]    |   access custom or non-standard tags                              |
|   [tags]\<key\>,\<delimiter\>[]   |   access custom or non-standard tags                  |

#### dBpoweramp Naming Properties
|	Name		    |	Description														|
|	---			    |	--- 															|
|   [audio_quality] |   Audio Quality                                                   |
|   [bitrate]       |   Bitrate                                                         |
|   [k_bitrate]     |   Bitrate (Kbps)                                                  |
|   [bits_per_sample]   |   Bits per Sample                                             |
|   [bytes_per_sample]  |   Bytes per Sample                                            |
|   [channels]      |   Channels                                                        |
|   [channels_subone]   |   Channels -1                                                 |
|   [encoder]       |   name of the encoder (LAME, Nero, etc.)                          |
|   [encoder+]      |   detailed encoder info                                           |
|   [extension]     |   audio format extension (wav, mp3, flac, etc.)                   |
|   [frequency]     |   frequency                                                       |
|   [gapless]       |   gap-less track                                                  |
|   [id_tag]        |   ID Tag Type                                                     |
|   [length]        |   track length in milliseconds                                    |
|   [length_mmss]   |   track length in minutes and seconds (mm:ss)                     |
|   [length_hhmmss] |   track length in hours, minutes, and seconds (hh:mm:ss)          |
|   [protected]     |   protected                                                       |
|   [date]          |   current date                                                    |
|   [time]          |   current time                                                    |
|   [day_of_week]   |   current day of the week                                         |
|   [now_year]      |   current year                                                    |
|   [origpath]      |   source path                                                     |
|   [origfilename]  |   source filename                                                 |
|   [discunique]    |   disc unique number ( from where? )                              |
|   [unique]        |   track unique number ( from where? )                             |
|   [cddb_id]       |   unique disc ID from CDDB                                        |

#### dBpoweramp Video Converter (dMV) values
for batch processing video files

|	Name		    |	Description														|
|	---			    |	--- 															|
|   [author]        |                                                                   |
|   [title]         |                                                                   |
|   [genre]         |                                                                   |
|   [year]          |   YYYY MM DD, software configurable to YYYY                       |
|   [origpath]      |   the source file path (used for batch/ conversions, not CD Ripping   |
|   [origfilename]  |   the source filename ( minus extension, for batch / conversions, not CD Ripping)    |
|   [origdrive]     |   the source drive in the path, example `c:\`                     |
|   [bitrate]       |   overall bitrate                                                 |
|   [frame rate]    |                                                                   |
|   [frame count]   |   |
|   [aspect ratio]  |   |
|   [pixel format]  |   |
|   [color space]   |   |
|   [video bitrate] |   |
|   [width]         |   |
|   [height]        |   |
|   [sample rate]   |   |
|   [channels]      |   |
|   [sample size]   |   |
|   [audio bitrate] |   |

## File System Requirements / Limitations

An Example Set of Limits
 * 255 total path limit
 * example-1 for the path \\artist\\album\\artist - album - 01-01 - track title.ext
   * \\50\\50\\140 = 240
   * 50 chars for artist directory name
   * 50 chars for album directory name
   * 140 chars for file name
 * example-2 for the path \\artist\\album\\01-01 - track title.ext
   * \\80\\80\\80 = 240

### Windows
 * 255 characters, modern limit
 * [Microsoft Win32 Naming Files, Paths, and Namespaces](https://docs.microsoft.com/en-us/windows/win32/fileio/naming-a-file)
 * The following reserved characters:
   *  < (less than)
   *  \> (greater than)
   *  : (colon)
   *  " (double quote)
   *  / (forward slash)
   *  \\ (backslash)
   * | (vertical bar or pipe)
   * ? (question mark)
   * \* (asterisk)

### MacOS
 * 255 characters, OSX modern limit
### Linux

## Reference Links
 * [illustrate's dBpoweramp dMC Naming doc](https://www.dbpoweramp.com/Help/dMC/Naming.htm)
 * [illustrate's dBpoweramp dMV Naming doc](https://www.dbpoweramp.com/Help/dMV/Naming.htm)
 * [illustrate's ID Tag Processing doc](https://www.dbpoweramp.com/help/Codec/DSP/help.htm#ID_Tag_Processing)
 * [illustrate's dBpoweramp Codec Central: Utility Codecs](https://www.dbpoweramp.com/codec-central-utility.htm)
   * [Audio Info R2](https://www.dbpoweramp.com/codec-central-utility.htm) - extract and present audio details as a list, or spreadsheet compatible tab delimited table   \[Requires Reference\]
   * [Arrange Audio R5](https://www.dbpoweramp.com/codec-central-utility.htm) - arranges audio files based on ID Tags   \[Requires Reference\]
   * [Calculate Audio CRC R3](https://www.dbpoweramp.com/codec-central-utility.htm) - displays a CRC generated from audio data, check audio files are identical
   * [Channel Split R2](https://www.dbpoweramp.com/codec-central-utility.htm) - extract multi-channel audio to single files
   * [ID Tag Update R5](https://www.dbpoweramp.com/codec-central-utility.htm) - update ID Tag version, type, or manipulate tags
   * [Length Split R2](https://www.dbpoweramp.com/codec-central-utility.htm) - split larger files into smaller chunks
   * [Replay Gain R5](https://www.dbpoweramp.com/codec-central-utility.htm) - Calculates volume gain and writes to ID Tags
   * [Tag From Filename R3](https://www.dbpoweramp.com/codec-central-utility.htm) - creates ID Tags from the filename

 * [ASCII Character Chart](https://theasciicode.com.ar/ascii-printable-characters/number-zero-ascii-code-48.html)
