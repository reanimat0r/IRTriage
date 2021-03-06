##### Incident Response Triage:
Scripted collection of system information valuable to a Forensic Analyst. 
IRTriage will automatically "Run As ADMINISTRATOR" in all Windows versions except WinXP.

The original source was [Triage-ir v0.851](https://code.google.com/p/triage-ir/) an Autoit script written by Michael Ahrendt.
Unfortunately Michael's last changes were [posted](http://mikeahrendt.blogspot.ca/2012/01/automated-triage-utility.html) on 9th November 2012

I let Michael [know](http://mikeahrendt.blogspot.com/2012/01/automated-triage-utility.html?showComment=1455628200788#c6111030418808145121) that I have forked his project:
I am pleased to anounce that he gave me his blessing to fork his source code, long live Open Source!)

###### What if having a full disk image is not an option during an incident?
Imagine that you are investigating a dozen or more possibly infected or compromised systems.
Can you spend 2-8 hours making a forensic copy of the hard drives on those computers?
In such situation fast forensics\"Triage" is the solution for such a situation.
Instead of copying everything, collecting some key files can solve this issue.

IRTriage will collect:
- system information
- network information
- registry hives
- disk information, and
- dump memory.

One of the powerful capabilities of IRTriage is collecting information from "Volume Shadow Copy" which can defeat many anti-forensics techniques.

The IRTriage is itself just an autoit script that depend on other tools such as:
- Win32|64dd (free from Moonsols) or FDpro *(HBGary's commercial product)
- Sysinternals Suite
- The Sleuth Kit
- Regripper
- NirSoft => MFTDump and WinPrefetchView
- md5deep and sha1deep
- CSVFileView
- 7zip
- and some windows built-in commands.

In case of an incident, you want to make minimal changes to the "evidence machine", therefore I would suggest you copy IRTriage to a USB drive, the only issue here is if you are planning to dump the memory, the USB drive must be larger than the physical ram installed in the computer.

Once you launch the GUI application you can select what information you would like to collect.
Each category is in a separate tab.
All the collected information will be dumped into a new folder labled with [hostname-date-time].

NEWS:
Changes from triage-ir v0.851
   - Renamed project to IRTriage
   - Versioning has changed to v2.[YY.MM.DD] for easier identification of last changes.
   - Updated the project to currently available tools.
   - Fixed the "commands executed" logging errors
   - Changed "Incident Log.txt" to "IncidentLog.csv" (TAB delimited)
   - Changed Compile time tools folder to ".\Compile\Tools\" (Local to script)
   - Fixed ini file open dialog to open in local script directory

Version 2016.02.24 IRTriage is now truly compatible with the following versions of Windows:
   - Windows Workstations "WIN_10", "WIN_81", "WIN_8", "WIN_7", "WIN_VISTA", "WIN_XP", "WIN_XPe",
   - Windows Servers: "WIN_2016", "WIN_2012R2", "WIN_2012", "WIN_2008R2", "WIN_2008", "WIN_2003".

Version 2016.02.26
   *Started to add new funtions:

	*Processes()
		- tcpvcon -anc -accepteula > Process2PortMap.csv
		- tasklist /SVC /FO CSV > Processe2exeMap.csv
		- wmic /output:ProcessesCmd.csv process get Caption,Commandline,Processid,ParentProcessId,SessionId /format:csv

	*SystemInfo()
		- wmic /output:InstallList.csv product get /format:csv
		- wmic /output:InstallHotfix.csv qfe get caption,csname,description,hotfixid,installedby,installedon /format:csv
		
	*Prefetch
		**WinPrefetchView /Folder Prefetch /stab Prefetch.csv 
		
	*Options()
		- mftdump.exe /l /m ComputerName /o ComputerName-MFT_Dump.csv $MFTcopy
		
	TriageGUI()
		- CSVFileView.exe IncidentLog.csv ;Added Checkbox to view IncidentLog after Acquisition
		- cmd.exe                         ;Added Checkbox to open IRTriage commandline after Acquisition
		
Version 2016.03.08
   - added a custom compiled version of ReactOS's "cmd.exe" based on v0.4.0
   - +it can now use Linux equivalent commands:
     - clear = cls
     - cp = copy
     - df = free
     - env = set
     - ln = mklink
     - ls = dir
     - mv = move
     - pwd = cd, chdir
     - rm = delete, del, erase
     - sleep = pause
     - uname = ver, version
     - vmstat = memory, mem

Version 2016.03.08
   - Started to cleanup the code, trying to make it easier to modualarize.
   - Added the option at compile time to use HBGary's FDpro (Commercial) or [Moonsol's (Free)](http://www.moonsols.com/downloads/1) memory acquisition software.
     - If you have HBGary's FDpro place it under the .\Compile\Tools folder in place of the "Zero byte" size file, is easy to switch back to Moonsol's memory acquisition software by replacing the FDpro.exe with a "less than 100 byte" sized file:-)

Version 2016.03.10
   - Continued cleanup of the code, removed unused Function CommandROSLOG()
   - Added $MFT parce to CSV
   - Added ability to view IncidentLog.csv after acquisition completed.

Version 2016.03.11
   - Updated cmd.exe
   - Added ability to open IRTriage's cmd.exe after acquisition completed.
    
Version 2016.03.14
   - Added Prefetch parce to CSV
   
Version 2016.03.24
   - Added IRTriage Update in tools menu (Update buttons mixed up)
   
Version 2016.03.28
   - Fixed IRTriage Update (Yes=Download Update, No=Display Update Info, Cancel=Cancel Update)
  
Version 2016.03.29
   - Integrate [Didier Stevens](http://blog.didierstevens.com/2015/12/13/windows-backup-privilege-cmd-exe/)'s new commands: privilege and info into the latest version of ReactOS's "cmd.exe".
     Both new commands are invaluable for a Forensic Analyst.
   - Source for [IRTriage command processor.](https://github.com/AJMartel/IRTriageCMD)
    
Version 2016.03.30
   - Fixed Volume Shadow Copy Functions
   - Minor Update to cmd.exe ver 4.1

Future Updates\Features will be based on this report: [On-scene Triage open source forensic tool chests are they effective.](http://www.researchgate.net/profile/Stavros_Shiaeles/publication/236681282_On-scene_Triage_open_source_forensic_tool_chests_Are_they_effective/links/00b4953ac91d0d0086000000.pdf?inViewer=true&pdfJsDownload=true&disableCoverPage=true&origin=publication_detail)
