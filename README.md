# Zephyr Project Helper CLI

A powerful, plug-and-play command-line utility for streamlining Zephyr RTOS project management, building, flashing, and debugging. Built with tight Visual Studio Code integration.

## Features

- **Interactive Project Creation**: Create minimal Zephyr projects from scratch with interactive board selection.
- **Project Duplication**: Easily duplicate existing projects (with or without current board selection) for quick prototyping.
- **VS Code Integration**: Automatically generates `.vscode/tasks.json` and `.vscode/launch.json` tailored specifically for your Zephyr environment.
- **Dynamic GDB Resolution**: Automatically finds your `arm-zephyr-eabi-gdb` path and Zephyr SDK regardless of version or installation directory.
- **Bash Auto-Completion**: Includes a robust auto-completion system for project names and commands.
- **Safe Deletion**: Easily clean up development environments with the built-in safe delete function.

## Installation

Download the script and make it executable:
```bash
wget https://raw.githubusercontent.com/emirarkali/Zephyr-Project-Manager/main/zephyrproject
chmod +x zephyrproject
```

Then, move it to a directory in your PATH (e.g., `~/.local/bin/`):
```bash
mv zephyrproject ~/.local/bin/
```

### Install Auto-Completion (Recommended)
To enable the interactive bash auto-completion features, simply run:
```bash
zephyrproject -install-completion
```
Then restart your terminal or run `source ~/.bashrc`.

## Usage

Create a new project:
```bash
zephyrproject -newproject <project-name>
```

Open a project in VS Code (auto-triggers a clean build):
```bash
zephyrproject -openproject <project-name>
```

Copy an existing project:
```bash
zephyrproject -copy <source-project> <new-project>
```

Build, flash, and debug:
```bash
zephyrproject -build
zephyrproject -flash
zephyrproject -debug
```

List all available projects:
```bash
zephyrproject -show
```

Delete a project safely:
```bash
zephyrproject -delete <project-name>
```

## Directory Structure Assumptions
By default, the script looks for your workspace at `~/zephyrproject` and applications at `~/zephyrproject/applications`. 
You can override these using environment variables:
- `ZEPHYR_WORKSPACE`
- `ZEPHYR_APPLICATIONS_DIR`
- `ZEPHYR_VENV`

## License

MIT License
