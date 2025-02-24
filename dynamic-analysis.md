# Dynamic Analysis

## Lecture Notes: Process, Thread, Mutex

### Process

* any running program is a process
* a process provides the resources needed to execute a program
* OS provides a process the following:
  * virtual address space
  * security context
    * ex. user that started the process/perms the process is running with
  * unique process identifier (PID)
    * unique across the system
  * environment variables
  * priority class
  * minimum and maximum working set sizes
  * at least one thread of execution
    * each thread has a unique ID within the process as well
* Windows Task Manager
  * shows processes, username, image name (executable name), etc.

### Thread

* an entity which contains code for execution and is owned by a process
* a process can have multiple threads (called multithreading)
* every thread for a process has a unique identifier (TID)

### Mutex

* a mutex is a lockable object that is designed to signal when critical sections of code need exclusive access, preventing other threads with the same protection from executing concurrently and access the same memory locations
  * a mutex helps serialize access to a resource
* Win32 API
  * CreateMutex
  * OpenMutex
  * WaitForSingleObject (acquire/request access to mutex)
  * ReleaseMutex
* malware can create mutex objects as unique objects
* can view in WinObj (SysInternals tool)

## Lab Notes: Process Monitor (ProcMon)

* Wireshark running while performing dynamic analysis is a useful way to track network activity
* ProcMon -> Play button is for starting/stopping, erase button is clear
* Filter -> filter -> can select conditions
  * ex. Process Name is \[process name] then Include -> Apply -> OK
* legend of icons in the top: what icons mean registry activity, file system activity, etc.
* Tools -> Process Tree
  * can see running processes and sub-processes
  * PIDs, image path, process lifetime, username of owner, command, start and end time
  * right-click on process -> Go To Event takes you to the process start event in the main window
* if you've filtered by Process Name, you won't see sub-processes
* can right-click on the Path of Load Image events with DLLs and include or exclude them
  * also useful for files and registry keys
* additional filters:&#x20;
  * Path contains C:\Windows\SysWOW64 then Exclude
  * Path contains C:\Windows\System32 then Exclude
  * Path ends with .db then Exclude
  * Path ends with .cdb then Exclude
  * Path ends with .ini then Exclude
  * Operation is RegCreateKey then Include
* CreateFile event
  * the first thing your system does is check for a file with the same name
  * if there isn't, then the result of that operation will be NAME NOT FOUND
  * then, there will be an attempt to create that file and the OpenResult should be created
    * can filter for this with Detail contains OpenResult: Created then Include
    * the same thing happens in the RegCreateKey operation
* Process Tree -> right click on target process -> can add process and its children to the Include filter
  * may need to clear filters first
* can right click -> Exclude events before
  * can be used to exclude all events before a starting point
* can right click -> Jump To will open the file in File Explorer
* RegCreateKey, RegSetValue useful for looking at registry access and modification

## Lab Notes: Noriben and RegShot

* Noriben is a Python application that runs an executable alongside ProcMon and automatically applies filters
  * needs Python installed

```
# ADMIN COMMAND PROMPT
python Noriben.py --cmd C:\[directory for sample]
# once the sample has finished running, hit CTRL+C ONCE
```

* RegShot focuses on registry and file system activity
  * run before running malware- takes snapshot
  * run malware -> run RegShot again and take another snapshot
  * does a comparison between the snapshots
  * when setting up: enable Scan dir1 for C:\ and make output path the folder for the lab, then hit 1st shot -> Shot and save
  * don't run ProcMon and RegShot at the same time
  * once both shots have been taken, can use Compare -> Compare and output
