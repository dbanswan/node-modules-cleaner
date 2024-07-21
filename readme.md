# Node Modules Cleaner

A simple shell script which deletes all node_modules folders in the provided directory and all subdirectories. Handy for cleaning up old projects and getting some of that disk space back.

## Usage

```bash
./clean_node_modules.sh /path/to/directory
```

## Example

```bash
./clean_node_modules.sh /home/user/projects
```

In the end it generates a log file with the list of deleted node_modules folders along with the size of each folder.

Also it prints the list of deleted node_modules folders and the space freed in the terminal.

```bash
sh clean_node_modules.sh ./2024
Deleted: ./2024/proj1/node_modules
Space Freed: 316K

Deleted: ./2024/proj2/images/node_modules
Space Freed: 568K

Deleted: ./2024/proj3/audio/node_modules
Space Freed: 112M

Deleted: ./2024/proj4/app/node_modules
Space Freed: 341M

Deleted: ./2024/proj5/code-name/app/node_modules
Space Freed: 332M

Report generated at: node_modules_deletion_report.txt

```

# Complete Tutorial

Following doc made with the help of chatgpt-4o (helps greatly in covering edge cases while developing scripts ) gives a detailed explanation of the script. In case you are new to shell scripting and want to get the hang of it.

## Shell Script Explanation: Deleting `node_modules` Directories

This document aims to provide an easy-to-understand tutorial on how the provided shell script works. The script is designed to traverse a given directory and delete all folders named `node_modules` recursively, generating a report of the deletions and disk space reclaimed.

## Script Breakdown

### Shebang and Script Initialization

```bash
#!/bin/bash
```

- `#!/bin/bash`:
  - This line specifies the script interpreter. It tells the system to use the Bash shell to execute the script.

### Argument Check

```bash
# Check if the path is provided as an argument
if [ -z "$1" ]; then
  echo "Usage: $0 <path>"
  exit 1
fi

TARGET_PATH="$1"
```

- `if [ -z "$1" ]; then`:
  - Checks if the first argument (`$1`) is provided. `$1` represents the first command-line argument.
- `echo "Usage: $0 <path>"`:
  - If no argument is provided, prints a usage message showing how to use the script.
- `exit 1`:
  - Exits the script with a status of `1`, indicating an error.
- `TARGET_PATH="$1"`:
  - Assigns the first argument to the variable `TARGET_PATH`.

### Preparation of the Report File

```bash
REPORT_FILE="node_modules_deletion_report.txt"

# Prepare the report file
echo "Node Modules Deletion Report" > "$REPORT_FILE"
echo "============================" >> "$REPORT_FILE"
echo "" >> "$REPORT_FILE"
```

- `REPORT_FILE="node_modules_deletion_report.txt"`:
  - Defines the report file's name.
- The subsequent three `echo` commands:
  - Initialize the report file with a title and some formatting.

### Function Definition: `delete_node_modules`

```bash
# Function to delete node_modules and track disk usage
delete_node_modules() {
  local dir="$1"
  local size=$(du -sh "$dir/node_modules" 2>/dev/null | cut -f1)

  if [ -n "$size" ]; then
    rm -rf "$dir/node_modules"
    echo "Deleted: $dir/node_modules" | tee -a "$REPORT_FILE"
    echo "Space Freed: $size" | tee -a "$REPORT_FILE"
    echo "" | tee -a "$REPORT_FILE"
  fi
}
```

- `delete_node_modules() { ... }`:
  - Defines a function named `delete_node_modules`.
- `local dir="$1"`:
  - The `dir` variable is set to the function's first argument.
- `local size=$(du -sh "$dir/node_modules" 2>/dev/null | cut -f1)`:
  - Calculates the disk usage of the `node_modules` directory and captures its size.
  - `du -sh`: Displays the size in a human-readable format.
  - `2>/dev/null`: Suppresses error messages.
  - `cut -f1`: Extracts the size value.
- `if [ -n "$size" ]; then`:
  - Checks if size is non-empty.
- `rm -rf "$dir/node_modules"`:
  - Deletes the `node_modules` directory.
- The subsequent `echo` commands:
  - Append deletion information to both the console and the report file using `tee`.

### Exporting the Function

```bash
# Export the function to be used by 'find'
export -f delete_node_modules
export REPORT_FILE
```

- `export -f delete_node_modules`:
  - Exports the function so it can be invoked by subshells started by `find`.
- `export REPORT_FILE`:
  - Exports the report file variable to make it accessible to subshells.

### Find and Execute Deletion

```bash
# Find all node_modules folders and delete them
find "$TARGET_PATH" -name "node_modules" -type d -prune -exec bash -c 'delete_node_modules "$(dirname "{}")"' \;
```

- `find "$TARGET_PATH"`:
  - Starts a search within the `TARGET_PATH`.
- `-name "node_modules"`:
  - Looks for directories named `node_modules`.
- `-type d`:
  - Restricts the search to directories.
- `-prune`:
  - Avoids descending into directories that match the `-name` criteria.
- `-exec bash -c 'delete_node_modules "$(dirname "{}")"' \;`:
  - For each found `node_modules` directory, executes the `delete_node_modules` function in a new Bash shell, passing the parent directory as an argument.

### Final Report Notification

```bash
echo "Report generated at: $REPORT_FILE"
```

- Informs the user that the report has been generated and specifies the report file's location.

## Conclusion

By breaking down the script into these functional blocks, you can understand how each part contributes to the whole. This script checks for the argument, prepares a report file, defines a function to delete `node_modules` directories and record their sizes, and finally uses the `find` command to locate and delete these directories.
