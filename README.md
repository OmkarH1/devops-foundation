# devops-foundation

1) How Linux executes a command?

When a commnad is executed the shell parses the command and checks if the command is builtin or external command, searches for binary using the PATH variable, and then create child process  using fork()
The child process call exec() to replace its memory with new program code. The kernel loads the binary into memory, schedules it on cup, and the process runs.
After execution, the process exits, and the parent shell collects the exit status using wait() and returns the prompt.

2) PID vs PPID
   
PID - Process ID

   -    Its unique idnetifier of a process

PPID - Parent Processs ID

   -   PID of the process that created it.
