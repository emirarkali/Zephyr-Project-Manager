# Zephyr Project Helper (`zephyrproject`)

A command-line workflow tool for managing, scaffolding, building, flashing, and debugging Zephyr RTOS applications. 

This update introduces **AI-powered STM32CubeIDE to Zephyr RTOS migration** powered by Google Gemini Flash models.

---

## 🚀 What's New in this Update: `-import-cube`

You can now automatically convert existing **STM32Cube (HAL / LL / FreeRTOS)** projects into fully functional **Zephyr RTOS** applications with a single command.

```bash
zephyrproject -import-cube <cube-project-folder-or-name> [new-zephyr-project-name]
```
*(Aliases: `-cube`, `--import-cube`)*

### 🧠 Key Features of the Converter:

1. **Pre-flight Connectivity Check:**
   * Verifies network availability before making calls. If offline, exits cleanly with:
     `"No internet connection. Couldnt reach out to AI model for conversion."`

2. **Secure, Zero-Leak API Key Handling:**
   * **No hardcoded secrets:** Safe to push to public repositories.
   * Reads from `~/.config/zephyrproject/gemini_api_key.txt` or `$GEMINI_API_KEY`.
   * On first run, interactively prompts the user for their free key from [Google AI Studio](https://aistudio.google.com/) and saves it locally with restricted permissions (`chmod 600`).

3. **Selectable AI Reasoning / Thinking Levels:**
   * **1) Low / Fast:** Minimal thinking budget; ideal for simple GPIO, blink loops, and basic delays.
   * **2) Medium (Default):** Balanced reasoning budget; ideal for timers, interrupts, and UART.
   * **3) High / Deep:** Extended thinking budget; ideal for complex RTOS migrations, multi-task synchronization, DMA, and multi-bus systems (SPI/I2C).

4. **Smart STM32Cube Project Scanner:**
   * Parses **`*.ioc`** files to extract the exact target MCU part number (e.g. `STM32F446RETx`), clock configurations, and pin muxing.
   * Extracts **`Core/Src/main.c`**, **`Core/Inc/main.h`**, and secondary source files while automatically filtering out heavy ST HAL boilerplate libraries.

5. **Complete Zephyr Scaffolding:**
   The model doesn't just convert C code; it generates a complete Zephyr project:
   * **`src/main.c`**: Modern Zephyr APIs (`gpio_dt_spec`, `k_msleep`, `gpio_init_callback`, etc.).
   * **`prj.conf`**: Automatically enables needed Kconfig subsystems (`CONFIG_GPIO=y`, `CONFIG_SERIAL=y`, etc.).
   * **`app.overlay`**: Devicetree overlay if custom pin/node overrides are necessary.
   * **`CMakeLists.txt` & `.gitignore`**: Standard Zephyr build definitions.
   * **`board.txt`**: Automatically identifies and validates the closest Zephyr board target (e.g., `nucleo_f446re`, `nucleo_f303re`).
   * **`PROJECT_INFO.md`**: Conversion documentation explaining architectural mappings and design decisions.
   * **VS Code Integration**: Scaffolds `.vscode/tasks.json` and `.vscode/launch.json` for one-click OpenOCD and GDB debugging.

6. **Interactive Build Verification:**
   * Prompts to run `zephyrproject -build` immediately after conversion to test compilation.

7. **Tab Auto-completion:**
   * Full path completion for `-import-cube` when pressing `Tab` in Bash.

---

## 🛠 Prerequisites

* **Zephyr Development Environment:** Installed and initialized via `west`.
* **Python 3:** Included in your Zephyr virtual environment (`$ZEPHYR_WORKSPACE/.venv`).
* **Gemini API Key:** A free API key from [Google AI Studio](https://aistudio.google.com/).

---

## 📖 Quick Start

### 1. Install & Setup Completion
```bash
# Make script executable
chmod +x zephyrproject

# Copy to local path
cp zephyrproject ~/.local/bin/

# Install tab completion
zephyrproject -install-completion
source ~/.bashrc
```

### 2. Convert an STM32Cube Project
```bash
# Convert by passing path directly:
zephyrproject -import-cube ~/STM32CubeIDE/workspace/f446_blink my_zephyr_blink

# Or run interactively:
zephyrproject -import-cube
```

### 3. Build & Flash
```bash
cd ~/zephyrproject/applications/my_zephyr_blink

# Normal build
zephyrproject -build

# Build and flash to connected board
zephyrproject -flash
```

---

## 📋 Full Command Reference

| Command | Description |
| :--- | :--- |
| `zephyrproject -newproject <name>` | Creates a minimal Zephyr project from scratch |
| `zephyrproject -import-cube [path] [name]` | **(New)** Converts an STM32Cube project using Gemini AI |
| `zephyrproject -copy <src> <dest>` | Duplicates an existing Zephyr project |
| `zephyrproject -openproject <name>` | Opens project in VS Code with clean build |
| `zephyrproject -show` | Lists all applications in workspace |
| `zephyrproject -build` | Compiles the current application |
| `zephyrproject -clean` | Performs a pristine rebuild (`--pristine`) |
| `zephyrproject -flash` | Builds and flashes binary to the target board |
| `zephyrproject -clean-flash` | Pristine rebuild and flash |
| `zephyrproject -flash-only` | Flashes existing binary without rebuilding |
| `zephyrproject -debug` | Starts GDB debugging session |
| `zephyrproject -debugserver` | Starts OpenOCD GDB server |
| `zephyrproject -menuconfig` | Launches interactive Kconfig menu |
| `zephyrproject -pristine` | Deletes the `build/` directory |
| `zephyrproject -status` | Displays project, board, and toolchain info |
| `zephyrproject -delete <name>` | Permanently removes a project |
| `zephyrproject -install-completion` | Installs Bash tab auto-completion |
| `zephyrproject -help` | Displays help message |

---

## 🔒 Security & Privacy

* Your Google Gemini API key is stored locally in `~/.config/zephyrproject/gemini_api_key.txt` with `0600` permissions.
* The script contains no hardcoded keys, passwords, or personal user paths.
