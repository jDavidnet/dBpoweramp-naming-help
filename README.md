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
### Functions
|	Name		|	Description														|
|	---			|	--- 															|
|	IFCOMP		|	included if "Compilation tag" is set							|
|	IF!COMP		|	included if "Compilation tag" is not set						|
|	IFMULTI		|	included if part of a multi CD / disc number is not null, > 1	|
|	IF!MULTI	|	included if not part of a multi CD / disc number is null || <=1	|

|	Name		|	Description														|
|	---			|	--- 															|
|	UPPER		|	uppercases the string											|
|	LOWER		|	lowercases the string											|
|	RIGHT		|	uses last x characters from a string							|
|	GRAB		|	extracts a portion of string; if **<code>to</code>** is omitted, then string is grabbed to the end	|
|	TRIM		|	trims spaces from begining or end of string	(chars:<code> </code> )			|
|	DEL			|	removes a portion of a string; opposite of **<code>GRAB</code>**; if **<code>to</code>** is omitted, then string is removed after **<code>from</code>**	|
|	SETLEN		|	gaurentees a string length, either pads or clips the string		|

|	Name		|	Description														|
|	---			|	--- 															|
|	IFEQUALS	|	if **<code>tag</code>** value == **<code>equals</code>** value, then include string		|
|	IF!EQUALS	|	if **<code>tag</code>** value != **<code>equals</code>** value, then include string		|
|	IF			|	compares a **<code>tag</code>** with a **<code>condition</code>**(<code>=, len\<len\>, len=\<len\> </code>), then includes either the **<code>stringmatch</code>** or **<code>stringnomatch</code>** string	|
|	IFVALUE		|	tests if **<code>tag</code>** is null, and includes **<code>strifvalue</code>** or **<code>strnovalue</code>**	|
