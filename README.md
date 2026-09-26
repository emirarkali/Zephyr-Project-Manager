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

This tool is designed around a simple, interactive workflow. You don't need to memorize complex `west` or `cmake` commands.

### 1. Creating a New Project
```bash
zephyrproject -newproject <project-name>
```
**What it does:** 
- Creates a new Zephyr application folder under `~/zephyrproject/applications/`.
- Generates a template `CMakeLists.txt`, `prj.conf`, and `src/main.c`.
- **Interactive Board Selection:** It will prompt you to type your target board name (e.g., `nucleo_f303re`). It validates the board against the official Zephyr list to prevent typos!
- Configures `.vscode/launch.json` dynamically, automatically locating your local `arm-zephyr-eabi-gdb` path.

### 2. Copying/Duplicating a Project
```bash
zephyrproject -copy <source-project> <new-project>
```
**What it does:** 
- Perfect for prototyping! Safely duplicates an existing project folder.
- Automatically excludes the heavy `build/` directory from the copy.
- Prompts you to either keep the original board (`k`), change it, or leave it blank (`q`).
- Opens the new project in VS Code automatically.

### 3. Opening a Project in VS Code
```bash
zephyrproject -openproject <project-name>
```
**What it does:** 
- Opens the specified project directly in Visual Studio Code.
- If the project has a valid board selected, it automatically performs a clean `west build` to ensure intellisense and the build environment are up to date.

### 4. Building and Flashing
Navigate to your project directory or use these commands directly via VS Code Tasks:

- `zephyrproject -build` : Performs a standard `west build`.
- `zephyrproject -clean` : Performs a pristine build (`west build -p always`).
- `zephyrproject -flash` : Builds the project and flashes it to your connected board.
- `zephyrproject -clean-flash` : Pristine build + flash.
- `zephyrproject -flash-only` : Skips the build process and directly flashes the existing `zephyr.elf`.

### 5. Debugging
```bash
zephyrproject -debug
```
**What it does:** 
- Builds the project and starts the OpenOCD GDB server in your terminal.
- Perfectly integrates with the VS Code `Cortex-Debug` extension (configured via the generated `launch.json`).

### 6. Managing Projects
```bash
zephyrproject -show
```
Lists all your active Zephyr projects located in the applications directory.

```bash
zephyrproject -delete <project-name>
```
Safely deletes a project folder. Includes a `[y/N]` confirmation prompt to prevent accidental data loss.

## Directory Structure Assumptions
By default, the script looks for your workspace at `~/zephyrproject` and applications at `~/zephyrproject/applications`. 
You can override these using environment variables:
- `ZEPHYR_WORKSPACE`
- `ZEPHYR_APPLICATIONS_DIR`
- `ZEPHYR_VENV`

## License

MIT License
