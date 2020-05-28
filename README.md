# dBpoweramp-naming-help
A guide (/w tools) to help use the Dynamic Naming Tools in dBpoweramp.

## Intro
The dBpoweramp Dynamic Naming Code(DBPANC, pronounced DB Panic) tools provide power dynamic naming functionality to dBpoweramp.  It seems to be a cross beween a templating engine, and a functional scripting language.

#### Language Caveats
It has limited control flow, and some ability to nest statements; but, it does not seem to be a complete programing language.  The operators, functions, or control statements are in a pre-fix notation form -- similar to LISP.

Because DBPANC lacks the ability to create functions, store values for reuse or leverage modular code; code can get pretty complex and difficult to maintain.  I am writting this guide to correct, update and simplify the production of these scripts.

#### Example
Here is the CD Ripper Default Naming script
```
[IFVALUE]album artist,[album artist],[IFCOMP]Various Artists[][IF!COMP][artist][][]\[album]\[track] [artist] - [title]
```
## Function Summary
#### Compilation, Anthology, and Soundtrack Functions
For sound files that are properly tagged with the *Compilation Tag,* it's possible to change how file title or path is generated.  Some CD ripping and sound file production methods leave this tag off or, label it incorrectly.

|	Name		|	Description														|
|	---			|	--- 															|
|	IFCOMP		|	included if "Compilation tag" is set							|
|	IF!COMP		|	included if "Compilation tag" is not set						|

#### Multi Disc Functions
Multidisc sets identified can be simplified with these boolean control functions.  Similar functionality can be achived with the Logical Functions in the language.

|	Name		|	Description														|
|	---			|	--- 															|
|	IFMULTI		|	included if part of a multi CD / disc number is not null, > 1	|
|	IF!MULTI	|	included if not part of a multi CD / disc number is null || <=1	|

#### String Manipulation Functions
Manipulating Strings can be part of a final step in currating your file and path names.  Within dBpoweramp there are other methods to manipulate strings before processing them here.

|	Name		|	Description														|
|	---			|	--- 															|
|	UPPER		|	uppercases the string											|
|	LOWER		|	lowercases the string											|
|	RIGHT		|	uses last x characters from a string							|
|	GRAB		|	extracts a portion of string; if **`to`** is omitted, then string is grabbed to the end	|
|	TRIM		|	trims spaces from begining or end of string	(chars:` ` )			|
|	DEL			|	removes a portion of a string; opposite of **`GRAB`**; if **`to`** is omitted, then string is removed after **`from`**	|
|	SETLEN		|	gaurentees a string length, either pads or clips the string		|
| MAXLENGTH | clips a string if it's ove the maximum length(**`len`**) of the **`string`**
|	WORD		|	trims a string to **`wordcount`** limit.  (ie. `[WORD]A Quick Brown Fox,2[]` returns the string 'A Quick')	|
|	SPLIT		|	splits a **`string`** by **`\<letter or string\>`**(a character, or sub-string) and returns a sub-string at **`position`**. To splity by ","(comma) enter no **`string`**.
| REPLACE | **`string`** is searched by the **`search`** string and is substituted with the **`replacement`**. To search for ","(comma) enter no **`string`**. |

#### Logical Functions
Do basic comparisons and control flow.

|	Name		|	Description														|
|	---			|	--- 															|
|	IFEQUALS	|	if **`tag`** value == **`equals`** value, then include string		|
|	IF!EQUALS	|	if **`tag`** value != **`equals`** value, then include string		|
|	IF			|	compares a **`tag`** with a **`condition`**(`=, len\<len\>, len=\<len\> `), then includes either the **`stringmatch`** or **`stringnomatch`** string	|
|	IFVALUE		|	tests if **`tag`** is null, and includes **`strifvalue`** or **`strnovalue`**	|

#### File System / Folder Functions
Usefull mostly for batch jobs, as CD Ripping projects will not originate with a filename.

|	Name		|	Description														|
|	---			|	--- 															|
| TRIMFIRSTFOLDER | removes first folder form the path.  (ex. `[TRIMFIRSTFOLDER]f1\f2\f3[]` returns `f2\f3`) |
| TRIMLASTFOLDER  | removes last folder from path. (ex. `[TRIMLASTFOLDER]f1\f2[]` returns `f1\f2`)
| FRONTFOLDER | returns the folder in **`position`** from the begining of the path.   |
| BACKFOLDER  | returns the folder in **`position`** reletive from the end of the path.  |


## Reference Links
 * [illustrate's dBpoweramp Naming doc](https://www.dbpoweramp.com/Help/dMC/Naming.htm)
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
