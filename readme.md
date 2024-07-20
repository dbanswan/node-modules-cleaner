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

Happy cleaning!
