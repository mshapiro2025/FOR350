# Managed Code (.NET)

## Lecture Notes: Managed Code

* managed code doesn't deal directly with memory
  * at runtime, the code is compiled, converted to native code, and executed on the OS
* managed code is readable
* managed code allocates memory for an object, and once that object is done, the "garbage collector" frees up that space
* examples:
  * Visual Basic
  * C#
  * Python

### Additional Notes

* if an EXE needs to run with admin perms, that may be defined in the manifest (available in the Resources)

### Lab Notes: dnSpy

* works as a decompiler
* can drag and drop a file
  * can expand sidebar to see PE headers and details
* right-click on file being analyzed in sidebar -> Go to Entry Point
  * takes you to where the execution will start- can see the code
  * can navigate through the code by clicking on functions
    * hit the backspace to go back
* .Run is the most important section, generally
