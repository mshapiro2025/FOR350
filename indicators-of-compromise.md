# Indicators of Compromise

## Lecture Notes: Indicators of Compromise

### IOCs

* examples of IOCs
  * processes created
  * path of files running processes
  * strings
  * libraries loaded
  * network connections
  * file name
  * size
  * hash
  * DNS queries
  * IP addresses
* one of these on their own is not usually enough for accurate rules/threat hunting

### Yara

* text-based language to define a signature for malicious or suspicious processes or files
* yara \[options] \[rules file] \[target]
  * target can be file, folder, or running process

### Yara Rules

* rule \[rule name] : \[tag1], \[tag2] { }
  * sections:
    * meta: metadata for analysts or other people using the rules
      * fields can be whatever
        * syntax: \[field name] = \[field value]
    * strings:
      * strings to look for in the target
      * can set up variables here
      * can add modifiers like ascii and base64
      * $\[variable name] = {\[variable value]} / "\[variable value]"
        * defining a string in hex with {} means you don't have to worry about finding a match with ascii, base64, etc.
      * ? is a wildcard- can be used as a nibble in a hex character (ex. 0?, ?0, ??)
    * condition:
      * boolean conditions to search for certain equivalencies
      * $\[variable name] or $\[variable name 2]
* C-style notation, generally
  * /\* comment \*/
  * // commented line
* conditional operators
  * and
  * or
  * boolean/arithmetic (! >= etc.)

#### Example: Demo 1

```
rule demo1
{
    strings:
        $a = "_AVIRA_"
        $b = "NORTON" nocase
        $c = "McAfee" ascii wide
    condition:
        $a and $b and $c
}
```

#### Example: Demo 2

```
rule demo2
{
    condition:
        filesize == 53920
}
```

#### Example: Demo 3

```
rule demo3
{
    strings: 
        $s = "%PDF-"
    condition:
        $s at 0
}
```

#### Example: XOR

```
rule xor 
{
    strings:
        $xor_string = "This program cannot" xor
    condition:
        $xor_string
}
# used to search for strings with a single byte XOR applied
```

#### Example: PE Characteristics

```
import "pe"
rule upxpacked 
{
    condition:
    (pe.sections[0].name == ".UPX0") and
    (pe.number_of_sections >= 4)
}
# sections is an array- we are looking at section 0 in the array (the first section)
# indicates UPX-packed malware
```

#### Example: PE Imports

```
import "pe"
rule peimports
{
    pe.imports("kernel32.dll", "DeleteFileA") and
    pe.imports("kernel32.dll", "WriteFile")
}
```

#### Example: Entropy

```
import "math"
rule entropy 
{
    condition:
    (
        (math.entropy(0, filesize) >= 6.5)
    )
}
# can calculate entropy for a section by replacing the 0 and filesize with the beginning and end locations of your section
```

#### Example: ImpHash

```
import "pe"
rule check_imphash
{
    condition:
    (
        pe.imphash == "[file hash]"
    )
}
```
