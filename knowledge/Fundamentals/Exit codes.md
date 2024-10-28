
| Exit Code | Description                                                                                              | Example Scenario                                                      |
| --------- | -------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| `0`       | Successful program termination (indicating no errors).                                                   | A program completes all tasks as expected.                            |
| `1`       | General error, often used for unspecified issues.                                                        | An error message is printed if a required file is not found.          |
| `2`       | Misuse of shell built-in commands.                                                                       | A script incorrectly uses a shell command or option.                  |
| `126`     | Command invoked cannot be executed.                                                                      | Attempting to run a command without execute permissions.              |
| `127`     | "Command not found" error in the shell.                                                                  | Typing a command name incorrectly in a script.                        |
| `128`     | Invalid argument to `exit()`.                                                                            | Calling `exit()` with an invalid argument (e.g., non-numeric values). |
| `128+N`   | Fatal error signaled by N, where N is a signal number (e.g., 130 for SIGINT, 131 for SIGQUIT).           | The program is interrupted by pressing Ctrl+C (SIGINT).               |
| `255`     | Exit status out of range. Typically seen if `exit(-1)` is used, which is equivalent to `exit(255)` in C. | Using an invalid exit status number.                                  |

In Unix systems, `exit()` codes help to diagnose program behavior, while certain signals (like `SIGINT`) often return 128+signal number to indicate specific termination reasons.