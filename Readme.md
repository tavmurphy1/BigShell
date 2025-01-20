# BigShell

**Author**: Tavner Murphy  
**Course**: Operating Systems I  
**Date**: December 1, 2024  

BigShell is an implementation of a POSIX (Portable Operating Systems Interface) shell. A shell is a command language interpreter program, primarily designed to parse the user’s command-line input and execute the extracted commands, either by forking separate processes or running built-in commands within the shell environment. BigShell supports core functionalities such as I/O redirection, signal handling, variable management, and job control, providing users with a robust command-line interface adhering closely to POSIX standards.

## Objectives
The project’s primary objectives were:
- Implementing built-in commands (`cd`, `exit`, `unset`).
- Enabling command word and variable expansions.
- Managing foreground/background processes using `fork`, `exec`, and `wait` system calls.
- Supporting I/O redirection and shell pipelines.
- Implementing signal handling to manage interrupts and process termination.

BigShell showcases a comprehensive understanding of UNIX operating system concepts, including process creation, inter-process communication, signal management, and shell behavior. Additionally, it demonstrates proficiency in C programming, particularly with pointers, structures, and library functions.

---

## Table of Contents
1. [Introduction](#introduction)  
2. [Program Structure](#program-structure)  
   - [Main Shell Loop](#main-shell-loop)  
   - [Command Parser](#command-parser)  
   - [Command Execution](#command-execution)  
   - [Parameters and Variables](#parameters-and-variables)  
   - [Signal Handling](#signal-handling)  
   - [Job Management](#job-management)  
3. [Implementation Details](#implementation-details)  

---

## Introduction

The UNIX operating system, developed at Bell Labs in the 1960s by Ken Thompson and Dennis Ritchie, became revolutionary due to its open architecture, which allowed users to customize and extend the system. This led to the development of a rich ecosystem of tools and applications built on top of UNIX. 

To ensure compatibility across UNIX-like systems, standards such as the Single UNIX Specification (SuS) and POSIX (Portable Operating System Interface) were introduced in the 1980s. These standards promote interoperability, portability, and standardization.

The POSIX shell is a powerful system interface for Unix-like operating systems. It allows users to directly input commands into the operating system using text, providing efficient navigation and operation execution. Over the years, popular implementations of POSIX-compliant shells, such as `bash`, `zsh`, `ksh`, and `fish`, have become integral to Linux, Windows, and macOS systems.

BigShell, loosely based on the POSIX.1 2008 shell command language specification, processes user input, parses commands, and executes them using features like pipelines, background execution, and control flow. This project demonstrates understanding of UNIX core concepts like process forking, shell language syntax, inter-process communication, job control, and signal handling.

---

## Program Structure

### Main Shell Loop (bigshell.c)
The main shell loop forms the core of BigShell, running continuously through a read-eval-print loop (REPL). Key steps include:
1. Initializing the parser and signal modules.
2. Prompting the user for input and parsing it into a command list.
3. Executing the parsed commands using the Command Execution module.
4. Handling exceptions, reporting errors, and restarting the loop as needed.

### Command Parser (parser.c, parser.h, expand.c, expand.h)
The parser interprets user input by:
- Breaking it into tokens (commands, arguments, operators).
- Handling word and variable expansion for special characters like `~`.
- Validating syntax and reporting errors to the shell loop.

### Command Execution (runner.c, runner.h)
The Command Execution module:
- Identifies whether commands are built-in, external, or pipelines.
- Forks child processes for external commands using `fork` and executes them with `exec`.
- Manages foreground and background processes with the help of the Jobs Management module.

### Parameters and Variables (params.c, vars.c)
This module manages:
- Environment variables (`$PATH`, `$HOME`) and user-defined variables.
- Variable storage, updates, and processing in compliance with POSIX standards.

### Signal Handling (signals.c, signals.h)
Handles UNIX signals to control running processes. This includes:
- Defining signal behavior for the shell and its children.
- Reporting unexpected behavior or errors to the main loop.

### Job Management (jobs.c, jobs.h, wait.c, wait.h)
Manages foreground and background jobs, including:
- Adding and tracking jobs on a jobs list.
- Handling process status updates and freeing resources for terminated jobs.

---

## Implementation Details

BigShell was implemented in **C**, leveraging C’s library functions and POSIX APIs. The following tools and techniques were used:

### Development Tools
- **Makefiles**: Automate compiling, defining dependencies, and reducing recompilation time.  
- **Git**: Used for version control, tracking changes, and efficient backtracking.  
- **VSCode with GitHub Codespaces**: Chosen for its compatibility with Makefiles, user-friendly UI, and plugin ecosystem.  

### Testing
Informal testing was conducted using:
- A debug executable with verbose output.
- Tests on built-in commands (`cd`, `exit`, `unset`), complex I/O redirections (`<`, `>`, pipes), and command expansions.

### Key Functions
1. **builtin_cd()**: Changes the current directory using `chdir()`. Defaults to `$HOME` if no path is provided.  
2. **builtin_unset()**: Unsets shell variables for cleanup.  
3. **builtin_exit()**: Terminates the shell after cleanup, using `strtol()` to handle exit arguments.  
4. **wait_on_fg_pgid()**: Manages foreground process groups, sending signals (`SIGCONT`) with `kill()` and waiting on processes with `waitpid()`.  
5. **run_command_list()**: Executes a list of commands based on their type (foreground, background, or pipeline). Handles word expansions, pipeline setup, and forking.  

### Libraries and System Calls
- **POSIX System Calls**: `fork()`, `exec()`, `waitpid()`, `dup2()`, `pipe()`.  
- **C Library Functions**: `strtol()`, `kill()`, `chdir()`.  

---

## Conclusion

BigShell is a robust implementation of a POSIX-compliant shell, showcasing advanced UNIX and C programming concepts. It is a practical demonstration of how operating systems manage processes, signals, and job control, adhering closely to POSIX principles. By building BigShell, a deeper understanding of system design and shell behavior has been achieved.

---

## References
1. Thompson, K., & Ritchie, D. (1974). UNIX Time-Sharing System.  
2. IEEE and The Open Group. POSIX and Single UNIX Specification.  
3. Advanced Programming in the UNIX Environment by W. Richard Stevens. 
