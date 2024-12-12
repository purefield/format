# README.md

## Yet another automation tool - yaat

The motivation for this script was to have something to remove clutter and make scripts more scannable and grep-able while adding some flow control.
You can quickly see what your script does and generate documentation. My inspiration came from jupyter notebooks where the code and documentation join in an execution environment without markup language clouding the content.

## Shell Script Formatter and Flow Control Tool

This script provides utilities to improve the readability, usability, and flow control of shell scripts. It includes functions for formatted output, handling user inputs, command execution with error handling, and dynamic flow control. These utilities are particularly helpful in complex environments.

---

## Table of Contents
1. [Getting Started](#getting-started)
2. [Features and Commands](#features-and-commands)
   - [Formatted Output (`__`or `_msg`)](#formatted-output-__)
   - [Pause or Delay (`___` or `_wait`)](#pause-or-delay-___)
   - [Execute Commands (`_:` or `cmd`)](#execute-commands-cmd)
   - [Prompt for Input (`_?` or `_ask`)](#prompt-for-input-__)
   - [Flow Control (`oo` or `_loop`)](#flow-control-oo)
3. [Examples and Use Cases](#examples-and-use-cases)
   - [Synthetic Example Script](#synthetic-example-script)
4. [Missing Features](#missing-features)
5. [Contributing](#contributing)

---

## Getting Started

### Prerequisites
Ensure you have the following:
- A Unix-based shell environment
- Basic knowledge of shell scripting

### Installation
1. Copy the script to your working directory and name it `format.sh`.
2. Source the script to use its functions:
   ```bash
   source format.sh
   ```

---

## Features and Commands

### Formatted Output (`__` or `_msg`)
The `__` function prints messages with customizable styles to improve script readability.

#### Syntax:
```bash
__ "<message>" <format_code>
```

#### Format Codes:
- `1`: Large double-bordered box
- `2`: Single-bordered box
- `3`: Header with tilde borders
- `4`: Small dot-borders
- `5`: Simple bullet point
- `6`: Asterisk bullet point
- `sameline`: Print on the same line
- `error`: Prefix message with red 'error'
- `cmd`: Prefix message with >
- `ok`: Prefix message with green 'ok'

#### Example:
```bash
__ "Starting the script..." 1
__ "Loading data" 2
__ "Information with dot border" 4
__ "This is a bullet point" 5
```

### Pause or Delay (`___` or `_wait`)
The `___` function pauses the script or delays execution for a specified time.

#### Syntax:
```bash
___ "<message>" <seconds>
```

#### Example:
```bash
___ "Processing, please wait..." 5
___ "Press any key to continue..."
```

### Execute Commands (`_:` or `cmd`)
The `cmd` function runs shell commands with error handling and formatted output. The output is exported into $OUTPUT.

#### Syntax:
```bash
cmd "<command>"
```

#### Example:
```bash
cmd "ls -al"
cmd "mkdir test_directory"
```

If a command fails, the script will prompt the user to press a key to continue.

### Prompt for Input (`_?` or `_ask`)
The `_?` function prompts the user for input and stores it in a variable.

#### Syntax:
```bash
_? "<message>" <variable_name> <default_value> [optional_direct_value]
```

#### Example:
```bash
_? "Enter the file name" FILE_NAME "default.txt"
_? "Enter the directory name" DIR_NAME "" "/tmp/example_dir"
_? "Enter the directory name" DIR_NAME "" "/tmp/example_dir" $DIR_NAME
```

This will prompt the user and store the input in the environment variable unless already set.

### Flow Control (`oo` or `_loop`)
The `oo` function executes a command repeatedly until a specific condition is met or interrupted.

#### Syntax:
```bash
oo <target_value> "<command>"
```

#### Example:
```bash
oo 5 "ls | wc -l"
oo 10 "find . -type f | wc -l"
```

This example waits until the specified condition is satisfied. You can interrupt with ctrl+c and the loop will end with a user prompt to continue.

---

## Examples and Use Cases

### Synthetic Example Script
```bash
#!/bin/bash

# Load formatting and flow control tools
source format.sh

# Example: Formatted output and command execution
__ "Starting the synthetic example script" 1

# Prompt for input with default value
__ "Gathering user input" 2
_? "Enter your name" USER_NAME "Guest"
__ "Hello, $USER_NAME!" 5

# Prompt for input with direct value
_? "Enter the directory name" DIR_NAME "" "/tmp/example_dir"
__ "You have chosen: $DIR_NAME" 4

# Execute a series of commands
__ "Executing system checks" 2
cmd "mkdir -p $DIR_NAME"
cmd "touch $DIR_NAME/sample_file.txt"

# Check if file creation was successful
if [ -f "$DIR_NAME/sample_file.txt" ]; then
  __ "File created successfully" 3
else
  __ "File creation failed" 3
fi

# Demonstrate flow control
__ "Monitoring file count in directory" 2
oo 3 "ls $DIR_NAME | wc -l"

# Pause for user confirmation
___ "Press any key to continue to advanced formatting examples..."

# Demonstrating various formatting levels
__ "Header with double border" 1
__ "Sub-header with single border" 2
__ "Informational header with tilde border" 3
__ "Details with small dot border" 4
__ "Simple bullet point" 5

# Pause for user input with timer
___ "Waiting for 5 seconds..." 5

# Clean up temporary resources
__ "Cleaning up resources" 2
cmd "rm -rf $DIR_NAME"
___ "Cleanup complete. Goodbye, $USER_NAME!"
```

---

## Use Script for documentation
**Create a document tree**
```bash
egrep '__|_msg' script.sh
```

**Show all input questions**
```bash
egrep '_\?|_ask' script.sh
```

**Show all commands**
```bash
egrep '_\:|cmd' script.sh
```

**Show all H1-H3**
```bash
egrep '^\s*(__|_msg).*?[123]$' script.sh 
```

---

## Missing Features
1. **Enhanced Error Reporting:**
   - Include detailed logs for failed commands.
   - Add an option to retry commands automatically.

2. **Parallel Command Execution:**
   - Support running multiple commands concurrently with progress tracking.

3. **Dynamic Input Validation:**
   - Allow validation of user inputs against custom criteria (e.g., regex).

4. **Configuration File Support:**
   - Enable loading variables and settings from a configuration file.

5. **Modular Architecture:**
   - Break the script into smaller, reusable modules for scalability.

---

## Contributing
To contribute to this project:
1. Fork the repository.
2. Create a feature branch.
3. Submit a pull request with your changes.

---

## License
This script is open-source and free to use. Feel free to customize it for your needs.

