# Scripts & External Tools Orchestration

Operations can execute Python scripts or external programs. The engine spawns each process, forwards arguments and captures the exit code.

For Python scripts, the path is relative to the project root and the interpreter can be overridden with the `python_executable` field. External tools may be invoked directly or through wrapper scripts.
