# dBpoweramp-naming-help
A guide (&amp; tools) to help with the Dynamic Naming Tools in dBpoweramp

## Intro
The dBpoweramp Dynamic Naming tools seem to be a cross beween a templating engine, and a functional scripting language.

It has limited control flow, some ability to nest statements, but, it does not seem to be a complete programing language.  The operators, functions, or control statements are in a pre-fix notation form, similar to LISP.

It lacks the ability to create functions, store values for reuse or other useful means of reusing code segments.

#### Example
Here is the CD Ripper Default Naming script
```
[IFVALUE]album artist,[album artist],[IFCOMP]Various Artists[][IF!COMP][artist][][]\[album]\[track] [artist] - [title]
```
### Function Summary
#### Compilation, Anthology, and Soundtrack Functions
|	Name		|	Description														|
|	---			|	--- 															|
|	IFCOMP		|	included if "Compilation tag" is set							|
|	IF!COMP		|	included if "Compilation tag" is not set						|

#### Multi Disc Functions
|	Name		|	Description														|
|	---			|	--- 															|
|	IFMULTI		|	included if part of a multi CD / disc number is not null, > 1	|
|	IF!MULTI	|	included if not part of a multi CD / disc number is null || <=1	|

#### String Manipulation Functions
|	Name		|	Description														|
|	---			|	--- 															|
|	UPPER		|	uppercases the string											|
|	LOWER		|	lowercases the string											|
|	RIGHT		|	uses last x characters from a string							|
|	GRAB		|	extracts a portion of string; if **<code>to</code>** is omitted, then string is grabbed to the end	|
|	TRIM		|	trims spaces from begining or end of string	(chars:<code> </code> )			|
|	DEL			|	removes a portion of a string; opposite of **<code>GRAB</code>**; if **<code>to</code>** is omitted, then string is removed after **<code>from</code>**	|
|	SETLEN		|	gaurentees a string length, either pads or clips the string		|
| MAXLENGTH | clips a string if it's ove the maximum length(**<code>len</code>**) of the **<code>string</code>**
|	WORD		|	trims a string to **<code>wordcount</code>** limit.  (ie. <code>[WORD]A Quick Brown Fox,2[]</code> returns the string 'A Quick')	|
|	SPLIT		|	splits a **<code>string</code>** by **<code>\<letter or string\></code>**(a character, or sub-string) and returns a sub-string at **<code>position</code>**. To splity by ","(comma) enter no **<code>string</code>**.
| REPLACE | **<code>string</code>** is searched by the **<code>search</code>** string and is substituted with the **<code>replacement</code>**. To search for ","(comma) enter no **<code>string</code>**. |

#### Logical Functions
|	Name		|	Description														|
|	---			|	--- 															|
|	IFEQUALS	|	if **<code>tag</code>** value == **<code>equals</code>** value, then include string		|
|	IF!EQUALS	|	if **<code>tag</code>** value != **<code>equals</code>** value, then include string		|
|	IF			|	compares a **<code>tag</code>** with a **<code>condition</code>**(<code>=, len\<len\>, len=\<len\> </code>), then includes either the **<code>stringmatch</code>** or **<code>stringnomatch</code>** string	|
|	IFVALUE		|	tests if **<code>tag</code>** is null, and includes **<code>strifvalue</code>** or **<code>strnovalue</code>**	|

#### File System / Folder Functions
|	Name		|	Description														|
|	---			|	--- 															|
| TRIMFIRSTFOLDER | removes first folder form the path.  (ex. <code>[TRIMFIRSTFOLDER]f1\f2\f3[]</code> returns <code>f2\f3</code>) |
| TRIMLASTFOLDER  | removes last folder from path. (ex. <code>[TRIMLASTFOLDER]f1\f2[]</code> returns <code>f1\f2</code>)
| FRONTFOLDER | returns the folder in **<code>position</code>** from the begining of the path.   |
| BACKFOLDER  | returns the folder in **<code>position</code>** reletive from the end of the path.  |
