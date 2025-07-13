# Bulk File Operations Script

This Bash script provides an interactive interface for performing bulk file operations (rename, copy, move, and delete) using `fzf` for file selection. It allows users to efficiently manage multiple files or directories with a terminal-based fuzzy finder and preview capabilities.

## Features
- **Interactive File Selection**: Uses `fzf` to select multiple files or directories with a preview of file contents (via `bat`) or directory listings (via `eza`).
- **Bulk Operations**:
  - **Rename**: Edit filenames in bulk using your preferred text editor.
  - **Copy**: Copy selected files or directories to a specified destination.
  - **Move**: Move selected files or directories to a specified destination.
  - **Delete**: Move selected files or directories to the system trash.
- **Recursive Mode**: Supports recursive file selection for operations on nested directories.
- **Safety Checks**: Includes confirmation prompts and validation to prevent errors like overwriting files or mismatched rename entries.
- **Colorized Output**: Provides clear feedback with color-coded success, warning, and error messages.

## Dependencies
The script requires the following tools to be installed:
- `fzf`: For interactive file selection.
- `fd`: For fast file searching.
- `eza`: For enhanced directory listings with icons and colors.
- `bat`: For syntax-highlighted file previews.

Install these dependencies on a Debian-based system (e.g., Ubuntu) with:
```bash
sudo apt update
sudo apt install fzf fd-find eza bat
```

Install these dependencies on NixOS with:
```bash
nix-shell -p fzf fd eza bat
```

On other systems, refer to the respective package managers or official documentation for installation instructions.

## Installation
1. Save the script to a file, e.g., `fzf-bulk.sh`.
2. Make it executable:
   ```bash
   chmod +x fzf-bulk.sh
   ```
3. Optionally, move it to a directory in your `$PATH` (e.g., `/usr/local/bin`) for global access:
   ```bash
   sudo mv fzf-bulk.sh /usr/local/bin/fzf-bulk
   ```

## Usage
Run the script with one of the following subcommands:

```bash
bulk_ops <command> [path]
```

### Commands
- `r` or `R`: Bulk rename selected entries. Use `R` for recursive selection.
- `c` or `C <path>`: Copy selected entries to `<path>`. Use `C` for recursive selection.
- `m` or `M <path>`: Move selected entries to `<path>`. Use `M` for recursive selection.
- `d` or `D`: Move selected entries to the system trash (`~/.local/share/Trash/files`). Use `D` for recursive selection.

### Examples
- Rename files in the current directory:
  ```bash
  fzf-bulk r
  ```
- Recursively copy files to `/tmp/destination`:
  ```bash
  fzf-bulk C /tmp/destination
  ```
- Move files to `/home/user/archive`:
  ```bash
  fzf-bulk m /home/user/archive
  ```
- Delete files by moving them to trash:
  ```bash
  fzf-bulk d
  ```

### Controls
- **Tab**: Move down in the `fzf` selection list.
- **Shift+Tab**: Move up in the `fzf` selection list.
- **Ctrl+N**: Select an entry (supports multiple selections).
- **Enter**: Confirm selection and proceed.

## Configuration
The script includes the following customizable settings:
- `LIST_CMD`: Command for directory listing previews (default: `eza -al --group-directories-first --icons --color=always`).
- `TRASH_DIR`: Directory for deleted files (default: `${HOME}/.local/share/Trash/files`).
- `TEMPFILE`: Temporary file for renaming (default: `/tmp/bulk_rename_$$`).
- `EDITOR`: Text editor for renaming (defaults to `nano` if not set).

You can modify these variables at the top of the script or set the `EDITOR` environment variable before running the script:
```bash
export EDITOR=vim
bulk_ops r
```

## Notes
- Ensure the destination path exists for copy and move operations.
- The rename operation opens a temporary file in your editor with the selected filenames. Edit each name on its respective line, save, and exit to proceed.
- The script skips renaming if a target file already exists or if the new name is empty.
- Recursive mode (`R`, `C`, `D`) includes files in subdirectories up to a maximum depth of 999.

## License
This script is provided under the MIT License. Feel free to modify and distribute it as needed.
