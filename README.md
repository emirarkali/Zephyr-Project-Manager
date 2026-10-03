# Zephyr Project Helper CLI

A powerful, plug-and-play command-line utility for streamlining Zephyr RTOS project management, building, flashing, and debugging. Built with tight Visual Studio Code integration.

## Features

- **Interactive Project Creation**: Create minimal Zephyr projects from scratch with interactive board selection.
- **Project Duplication**: Easily duplicate existing projects (with or without current board selection) for quick prototyping.
- **VS Code Integration**: Automatically generates `.vscode/tasks.json` and `.vscode/launch.json` tailored specifically for your Zephyr environment.
- **Dynamic GDB Resolution**: Automatically finds your `arm-zephyr-eabi-gdb` path and Zephyr SDK regardless of version or installation directory.
- **Bash Auto-Completion**: Includes a robust auto-completion system for project names and commands.
- **Safe Deletion**: Easily clean up development environments with the built-in safe delete function.
- **Smart Runner Fallback**: Automatically attempts flashing with `pyocd` if `openocd` fails or is unsupported, including a one-click pyOCD installation prompt.
- **Environment Validation**: Actively checks for the existence of your `ZEPHYR_WORKSPACE` before executing commands, providing clear instructions for setting environment variables if misplaced.

## Installation

Download the script directly into your local binary folder and make it executable:
```bash
wget -O ~/.local/bin/zephyrproject https://raw.githubusercontent.com/emirarkali/Zephyr-Project-Manager/main/zephyrproject
chmod +x ~/.local/bin/zephyrproject
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
zephyrproject -newproject [project-name] [-sample]
```
**What it does:**
- Creates a new multi-app Zephyr workspace under `~/zephyrproject/applications/`.
- If no `[project-name]` is provided, it launches an interactive wizard to ask for it.
- Automatically asks for the name of your first application (e.g. `main_node`, `traction`).
- Sets up a robust directory structure (`apps/app_name/`) including auto-generated `include/` and `Kconfig` files.
- **Interactive Board Selection:** It will prompt you to type your target board name (e.g., `nucleo_f303re`). It validates the board against the official Zephyr list to prevent typos!
- Configures `.vscode/launch.json` dynamically, automatically locating your local `arm-zephyr-eabi-gdb` path.

### 2. Adding a New App to a Project
```bash
zephyrproject -addapp <app-name> [-sample]
```
**What it does:**
- Run this inside your project workspace to scaffold a brand new Zephyr app.
- Creates `apps/<app-name>/` with its own dedicated `CMakeLists.txt`, `board.txt`, `prj.conf`, `Kconfig` and `include/` directory.
- Interactively asks for the board selection for this specific app.

### 3. Copying/Duplicating a Project
```bash
zephyrproject -copy <source-project> <new-project>
```
**What it does:** 
- Perfect for prototyping! Safely duplicates an existing project folder.
- Automatically excludes the heavy `build/` directory from the copy.
- Prompts you to either keep the original board (`k`), change it, or leave it blank (`q`).
- Opens the new project in VS Code automatically.

### 4. Opening a Project in VS Code
```bash
zephyrproject -openproject <project-name>
```
**What it does:** 
- Opens the specified project directly in Visual Studio Code.
- If the project has a valid board selected, it automatically performs a clean `west build` to ensure intellisense and the build environment are up to date.

### 5. Building and Flashing
Navigate to your project directory or use these commands directly via VS Code Tasks:

- `zephyrproject -build [app-name]` : Performs a standard `west build` for the specified app.
- `zephyrproject -clean [app-name]` : Performs a pristine build (`west build -p always`).
- `zephyrproject -flash [app-name]` : Builds the project and flashes it to your connected board.
- `zephyrproject -clean-flash [app-name]` : Pristine build + flash.
- `zephyrproject -flash-only [app-name]` : Skips the build process and directly flashes the existing `zephyr.elf`.

*Note: If you have a multi-app workspace and omit `[app-name]`, the CLI will interactively ask you which app to build/flash.*

### 6. Debugging
```bash
zephyrproject -debug
```
**What it does:** 
- Builds the project and starts the OpenOCD GDB server in your terminal.
- Perfectly integrates with the VS Code `Cortex-Debug` extension (configured via the generated `launch.json`).

### 7. Managing Projects
```bash
zephyrproject -show
```
Lists all your active Zephyr projects located in the applications directory.

```bash
zephyrproject -delete <project-name>
```
Safely deletes a project folder. Includes a `[y/N]` confirmation prompt to prevent accidental data loss.

### 8. Serial Port Monitor
```bash
zephyrproject -monitor
```
**What it does:**
- Automatically detects connected boards (e.g., `/dev/ttyACM0` or `/dev/ttyUSB0`).
- Connects to the board's serial output at `115200` baud using `picocom`. (Requires `picocom` to be installed: `sudo apt-get install picocom`).

### 9. Exporting/Packaging a Project
```bash
zephyrproject -export <project-name>
```
**What it does:**
- Compresses the specified project into a `.tar.gz` archive.
- Automatically excludes the massive `build/` folder and IDE cache files, reducing the size from gigabytes to kilobytes! Perfect for sharing your code with others.

## Directory Structure Assumptions
By default, the script looks for your workspace at `~/zephyrproject` and applications at `~/zephyrproject/applications`. 
You can override these using environment variables:
- `ZEPHYR_WORKSPACE`
- `ZEPHYR_APPLICATIONS_DIR`
- `ZEPHYR_VENV`

## License

MIT License
