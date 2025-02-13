# Malware Analysis

## Lecture Notes: Intro to Malware Analysis

### Types of Malware

* downloader/dropper
  * types of malware downloads and installs other malware
  * downloader vs. dropper: downloader pulls from an external source whereas a dropper pulls from the EXE itself
* rootkit
  * malware that conceals the existence of its code/process/resources (usually by manipulating the OS code)
* worm
  * malware that can create copies of itself and infect other computers in an automated fashion
* botnet
  * a malware infested network (or group of computers) that can take orders/instructions and execute them
* ransomware
  * malware that encrypts your files and then asks the computer owner to pay a fee to decrypt them
* scareware
  * malware that can frighten a user into buying a product they don't need
* keylogger
  * malware that steals your keystrokes
* trojan horse
  * malware that creates a backdoor on your computer/network
* backdoor
  * enables a remote attacker to access your computer
* hacktool/RAT
  * admin tools or programs that may be used by hackers to attack computer systems/networks
* adware
  * shows you advertisements to generate revenue for the malware owner

### Analysis Techniques

* static
  * inspect binary for raw data
  * decompile to examine code
* dynamic
  * run, trace, and analyze process
  * debug

#### Static Analysis

* pros
  * get an idea of basic functionality without running the malware
  * no risk of infecting your analysis machine
* cons
  * decompiled code is in Assembly language
  * does not work for complex malware that used obfuscation/packing

#### Dynamic Analysis

* pros
  * packed code can be analyzed
  * analysis is quicker than static mode
* cons
  * run time conditions must be met precisely
  * anti-debugging code can make it difficult
  * you're infecting a machine

## Lab Notes: Analyzing Suspicious File Samples Based on File Metadata

### File Metadata

* file types
* strings
  * information in the code like file names, registry keys, etc. are saved in the file's strings and can be viewed and analyzed
  * different types: ASCII (1 byte/character), Unicode (2 bytes/character)
    * Unicode when opened in a hex editor will have "." between characters
  * most tools look for 5 sequential characters- may have to change this
* file hashes

### Sysinternals

* File Explorer -> This PC -> Properties -> Advanced system settings -> Environment Variables -> Add Sysinternals directory path -> Move Up to top

### Strings

```
strings [filename.bin] > [filename.txt]
```

* .pdb files: debugging database
  * useful for threat intelligence

### File Hashes

* HashMyFiles
  * File -> Add Files

### Detect It Easy

* ... -> Add File -> check Advanced box
  * Strings -> can see strings, what entry # it is, (absolute) offset, address, section, size, type (ASCII vs Unicode), etc.
  * when compiling an EXE, the compiler will create directories (ex. i386)

## Lab Notes: Malware Classification Using Fuzzy Hashing

### ssdeep

{% embed url="https://github.com/ssdeep-project/ssdeep/releases" %}

* ssdeep is a fuzzy hashing tool, which allows for partial matches with file hashes

<pre><code><strong># -b: get bare name of files and info
</strong>ssdeep.exe -b [filename- can wildcard for several] >> hashes.txt
# -m: runs against hash database (hash file just created) and gives similarity percentage
ssdeep.exe -bm hashes.txt [executable for lab 1]
</code></pre>
