#Volatility
## task 1
It's a free memory forensics tool developed and maintained by Voltality Foundation.
Used by blue team or malware and SOC analysts as detection and monitoring solutions.
Written in python with pytohn plugins and modules.
## task 2
"Volatility is the world's most widely used framework for extracting digital artifacts from volatile memory (RAM) samples. The extraction techniques are performed completely independent of the system being investigated but offer visibility into the runtime state of the system. The framework is intended to introduce people to the techniques and complexities associated with extracting digital artifacts from volatile memory samples and provide a platform for further work into this exciting area of research."
### Start
identify image type
[Velocity3](https://github.com/volatilityfoundation/volatility3)
## task3

IP: whatever they give you -> 10.10.153.167
Username: thmanalyst
Password: infected
## task4
### Bare-Metal
Listed below are a few of the techniques and tools that can be used to extract a memory from a bare-metal machine.

    - FTK Imager
    - Redline
    - DumpIt.exe
    - win32dd.exe / win64dd.exe
    - Memoryze
    - FastDump
When using on bare-metal host, it can take a lot of time.
Will output .raw file with some exceptions like Redline that can use its own agent and session structure.
### VM
few of the hypervisor virtual memory files you may encounter.

    - VMWare - .vmem
    - Hyper-V - .bin
    - Parallels - .mem
    - VirtualBox - .sav file *this is only a partial memory file*

## task 6
Default, Volatility comes with existing Windows profiles XP - 10
If you don't know the version of the machine, *imageinfo* plugin can come in handy. takes the provided memory dump and assing it a list of the best possible OS proiles. *May not always be correct*

Example for information about host memory dump
*python3 vol.py -f <file.vmem> (windows|linux|mac).info*

## task 7
Five different plugins within Volatility allow you to dump processes and network connections, each with varying techniques used.

### pslist
*pslist* plugin lists processes from the doubly-linked list that keeps track of processes in memeory equivalent to process list in task manager. output includes current processes and terminated processes with exit times.

*python3 vol.py -f <file.vmem> windows.pslist*

### psscan
*psscan* will locate processes by finding data structures that match *_EPROCESS*. While it can evade countermeasures, it can cause false positives

*python3 vol.py -f <file.vmem> windows.psscan*

### pstree
*pstree* list all processes based on their parent process ID.
uses same method as *pslist*
useful for analayst to get full story of processes and what may have occured at time of extraction.

*python3 vol.py -f <file.vmem> windows.pstree*

### netstat

*netstat* will attempt to id all memory structures with a network connection

*python3 vol.py -f <file.vmem> windows.netstat*

This command in volatility3 can be very unstable, particulary old Windows builds.

### dlllist

*dllist* lists all DLLs associated with processes at time of extration.
Useful once you have done further analysis and can filter output to a specific DLL that might be indicator for specific type of malware you believe to be present on system.

*python3 vol.py -f <file.vmem> windows.dllist*

## task 8

Most useful plugin when hunting or code injection is *malfind*
attempts to id injected processes and their PIDs along with offset address and Hex, Ascii, and Disassembly view of infected area. Works by scanning heap an did processes that have executable bit set *RWE or RW* and/or no memeory-mapped file on disk (file-less malware).

*python3 vol.py -f <file.vmem> windows.malfind*

*yarascan* will search for strings, patterns, and compound rules against a rule set.

*python3 vol.py -f <file.vmem> windows.yarascan*

## task 9
The first evasion technique we will be hunting is hooking; there are five methods of hooking employed by adversaries, outlined below:

    - SSDT Hooks
    - IRP Hooks
    - IAT Hooks
    - EAT Hooks
    - Inline Hooks

### SSDT
this plugin will search fo rhooking and output its result.
*SYSTEM Service Descriptor Table*
Windows Kernel uses this table to look up system functions.
Hundreds of tables entries ssdt will dump
Use after investigating inital compromise and working off it as part of lead investigation.

*python3 vol.py -f <file.vmem> windows.ssdt*

### modules
dump a list of loaded kernel modules.
useful to id active malware.
best used once further investigating and found potential indicators to use as input for searching and filtering
*python3 vol.py -f <file.vmem> windows.modules*

### driverscan
scan for drivers present on system at time of extraction.
*python3 vol.py -f <file.vmem> windows.driverscan*

### other plugins

	- modscan
	- driverirp
	- callbacks
	- idt
	- apihooks
	- moddump
	- handles

## task 10

## task 11
[Volacitiy wiki](https://github.com/volatilityfoundation/volatility/wiki)
