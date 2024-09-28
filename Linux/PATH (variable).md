**PATH** is an [environment variable](https://en.wikipedia.org/wiki/Environment_variable "Environment variable") on [Unix-like](https://en.wikipedia.org/wiki/Unix-like "Unix-like") [operating systems](https://en.wikipedia.org/wiki/Operating_system "Operating system"), [DOS](https://en.wikipedia.org/wiki/DOS "DOS"), [OS/2](https://en.wikipedia.org/wiki/OS/2 "OS/2"), and [Microsoft Windows](https://en.wikipedia.org/wiki/Microsoft_Windows "Microsoft Windows"), specifying a set of [directories](https://en.wikipedia.org/wiki/Directory_(file_systems) "Directory (file systems)") where executable programs are located. In general, each executing [process](https://en.wikipedia.org/wiki/Process_(computing) "Process (computing)") or [user session](https://en.wikipedia.org/wiki/Login_session "Login session") has its own PATH setting.

## Contents

-   [1History](https://en.wikipedia.org/wiki/PATH_(variable)#History)
-   [2Unix and Unix-like](https://en.wikipedia.org/wiki/PATH_(variable)#Unix_and_Unix-like)
-   [3DOS, OS/2, and Windows](https://en.wikipedia.org/wiki/PATH_(variable)#DOS,_OS/2,_and_Windows)
-   [4References](https://en.wikipedia.org/wiki/PATH_(variable)#References)

## History[[edit](https://en.wikipedia.org/w/index.php?title=PATH_(variable)&action=edit&section=1 "Edit section: History")]

[Multics](https://en.wikipedia.org/wiki/Multics "Multics") originated the idea of a search path. The early [Unix shell](https://en.wikipedia.org/wiki/Unix_shell "Unix shell") only looked for program names in `/bin`, but by [Version 3 Unix](https://en.wikipedia.org/wiki/Version_3_Unix "Version 3 Unix") the directory was too large and `/usr/bin`, and a search path, became part of the operating system.[[1]](https://en.wikipedia.org/wiki/PATH_(variable)#cite_note-reader-1)

## Unix and Unix-like[[edit](https://en.wikipedia.org/w/index.php?title=PATH_(variable)&action=edit&section=2 "Edit section: Unix and Unix-like")]

On [POSIX](https://en.wikipedia.org/wiki/POSIX "POSIX") and Unix-like operating systems, the **`$PATH`** variable is specified as a list of one or more directory names separated by colon (`:`) characters.[[2]](https://en.wikipedia.org/wiki/PATH_(variable)#cite_note-2)[[3]](https://en.wikipedia.org/wiki/PATH_(variable)#cite_note-3) Directories in the `PATH`-string are not meant to be escaped, making it impossible to have directories with `:` in their name. [[4]](https://en.wikipedia.org/wiki/PATH_(variable)#cite_note-4)

The `/bin`, `/usr/bin`, and `/usr/local/bin` directories are typically included in most users' `$PATH` setting (although this varies from implementation to implementation). The [superuser](https://en.wikipedia.org/wiki/Superuser "Superuser") also typically has `/sbin` and `/usr/sbin` entries for easily executing [system administration](https://en.wikipedia.org/wiki/System_administration "System administration") commands. The current directory (`.`) is sometimes included by users as well, allowing programs residing in the [current working directory](https://en.wikipedia.org/wiki/Current_working_directory "Current working directory") to be executed directly. System administrators as a rule do _not_ include it in `$PATH` in order to prevent the accidental execution of scripts residing in the current directory, such as may be placed there by a malicious [tarbomb](https://en.wikipedia.org/wiki/Tarbomb "Tarbomb"). In that case, executing such a program requires specifying an absolute (`/home/userjoe/bin/script.sh`) or relative path (`./script.sh`) on the command line.

When a command name is specified by the user or an [exec](https://en.wikipedia.org/wiki/Exec_(operating_system) "Exec (operating system)") call is made from a program, the system searches through `$PATH`, examining each directory from left to right in the list, looking for a [filename](https://en.wikipedia.org/wiki/Filename "Filename") that matches the command name. Once found, the program is executed as a [child process](https://en.wikipedia.org/wiki/Child_process "Child process") of the command shell or program that issued the command.

## DOS, OS/2, and Windows[[edit](https://en.wikipedia.org/w/index.php?title=PATH_(variable)&action=edit&section=3 "Edit section: DOS, OS/2, and Windows")]

On DOS, OS/2, and Windows operating systems, the **`%PATH%`** variable is specified as a list of one or more directory names separated by semicolon (`;`) characters.[[5]](https://en.wikipedia.org/wiki/PATH_(variable)#cite_note-5)

The Windows system directory (typically `C:\WINDOWS\system32`) is typically the first directory in the path, followed by many (but not all) of the directories for installed software packages. Many programs do not appear in the path as they are not designed to be executed from a [command window](https://en.wikipedia.org/wiki/Command_shell "Command shell"), but rather from a [Graphical User Interface](https://en.wikipedia.org/wiki/GUI "GUI"). Some programs may add their directory to the front of the PATH variable's content during installation, to speed up the search process and/or override OS commands. In the DOS era, it was customary to add a `PATH {program directory};%PATH%` or `SET PATH={program directory};%PATH%` line to [AUTOEXEC.BAT](https://en.wikipedia.org/wiki/AUTOEXEC.BAT "AUTOEXEC.BAT").

When a command is entered in a command shell or a system call is made by a program to execute a program, the system first searches the [current working directory](https://en.wikipedia.org/wiki/Current_working_directory "Current working directory") and then searches the path, examining each directory from left to right, looking for an [executable](https://en.wikipedia.org/wiki/Executable "Executable") filename that matches the command name given. Executable programs have [filename extensions](https://en.wikipedia.org/wiki/Filename_extension "Filename extension") of `[EXE](https://en.wikipedia.org/wiki/EXE "EXE")` or `[COM](https://en.wikipedia.org/wiki/COM_file "COM file")`, and batch scripts have extensions of `[BAT](https://en.wikipedia.org/wiki/Batch_file "Batch file")` or `[CMD](https://en.wikipedia.org/wiki/CMD_file_(CP/M) "CMD file (CP/M)")`. Other executable filename extensions can be registered with the system as well.

Once a matching executable file is found, the system [spawns](https://en.wikipedia.org/wiki/Spawn_(computing) "Spawn (computing)") a new process which runs it.

The PATH variable makes it easy to run commonly used programs located in their own folders. If used unwisely, however, the value of the PATH variable can slow down the operating system by searching too many locations, or invalid locations.

Invalid locations can also _stop_ services from running altogether, especially the 'Server' service which is usually a dependency for other services within a Windows Server environment.

## References[[edit](https://en.wikipedia.org/w/index.php?title=PATH_(variable)&action=edit&section=4 "Edit section: References")]

1.  **[^](https://en.wikipedia.org/wiki/PATH_(variable)#cite_ref-reader_1-0 "Jump up")** [McIlroy, M. D.](https://en.wikipedia.org/wiki/Doug_McIlroy "Doug McIlroy") (1987). [_A Research Unix reader: annotated excerpts from the Programmer's Manual, 1971–1986_](http://www.cs.dartmouth.edu/~doug/reader.pdf) (PDF) (Technical report). CSTR. Bell Labs. 139.
2.  **[^](https://en.wikipedia.org/wiki/PATH_(variable)#cite_ref-2 "Jump up")** [Open Group Unix Specification, Environment Variables](http://www.opengroup.org/onlinepubs/000095399/basedefs/xbd_chap08.html#tag_08_03)
3.  **[^](https://en.wikipedia.org/wiki/PATH_(variable)#cite_ref-3 "Jump up")** [Open Group Unix Specification, execve() function](http://www.opengroup.org/onlinepubs/000095399/functions/execve.html)
4.  **[^](https://en.wikipedia.org/wiki/PATH_(variable)#cite_ref-4 "Jump up")** [Dash exec.c as an example of an implementation of a PATH-string parser](https://git.kernel.org/pub/scm/utils/dash/dash.git/tree/src/exec.c?h=v0.5.9.1&id=afe0e0152e4dc12d84be3c02d6d62b0456d68580#n173)
5.  **[^](https://en.wikipedia.org/wiki/PATH_(variable)#cite_ref-5 "Jump up")** [Microsoft.com, PATH command](http://msdn.microsoft.com/en-us/library/aa922003.aspx)

[Categories](https://en.wikipedia.org/wiki/Help:Category "Help:Category"): 

-   [Computer file systems](https://en.wikipedia.org/wiki/Category:Computer_file_systems "Category:Computer file systems")
-   [Environment variables](https://en.wikipedia.org/wiki/Category:Environment_variables "Category:Environment variables")