# Indicators of Compromise

## Lecture Notes: Indicators of Compromise

### IOCs

* indicator of compromise
  * specific criteria that can be used to distinguish whether or not a machine is compromised
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
* types of IOCs
  * OpenIOC (Mandiant)
  * Yara
  * threat intel
    * Structured Threat Information Expression (CTIX)
    * Trusted Automated Exchange of Intelligence Information (TAXII)
* one of these on their own is not usually enough for accurate rules/threat hunting

### Yara

* text-based language to define a signature for malicious or suspicious processes or files
* available on Windows, Linux, and Mac
* extensible via Python
* limited to scanning executables or processes
  * file header scanning (see example) can be a workaround for this
* yara \[options] \[rules file] \[target]
  * target can be file, folder, or running process

{% embed url="https://yara.readthedocs.io/en/v3.8.1/" %}

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
      * nocase means not case-sensitive
      * ascii means looking for words made of ASCII letters, wide means each character is 2 bytes
      * unicode strings (marked with U in DIE) must be marked as wide in the rule
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

### Examples

#### Example: Sample IOC

```
rule silent_banker : injector, ransomware
{
    meta:
            description = "example"
            threat_level = 3
            in_the_wild = true
    strings: 
            $a = {6A 40 68 ?? 30 00 00 6? 14 8D 91}
            $b = {8D 4D B0 2B C1 83 C0 27 99 6A 4E 59 F7 F9}
            $c = "UVODDFRYSIHLNWPEJXQZAKCBGMT"
    condition:
            ($a or $b) and $c
}
```

#### Example: Demo 1

```
// this will always be true, so it will always alert
rule demo1
{
    condition:
        true
}
```

#### Example: Strings

```
rule demo2
{
    strings:
        $a = "_AVIRA_"
        $b = "NORTON" nocase
        $c = "McAfee" ascii wide
    condition:
        $a and $b and $c
}
```

#### Example: File Size

```
rule demo3
{
    condition:
        filesize == 53920
}
// can add MB after to change size to from bytes to megabytes
```

#### Example: File Headers

```
rule demo4
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
        $xor_string = "This program cannot" xor base64
    condition:
        $xor_string
}
// used to search for strings with a single byte XOR applied
// will take a while
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
// "sections" is an array- we are looking at section 0 in the array (the first section)
// indicates UPX-packed malware
```

#### Example: PE Imports

```
import "pe"
rule peimports
{
    condition:
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
// first parameter is starting byte, second parameter is ending byte
// can calculate entropy for a section by replacing the 0 and filesize with the beginning and end locations of your section
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
// must be lowercase
// not an ideal method- must calculate the imphash of each file targeted
```
