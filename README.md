# GitHooks-Task
This repository contains the commit-msg file, which represents a Git hook script for enforcing a standardized commit message format.

## Required format
Required format that messages should have is <type>:<description> and message shouldn't be longer than 50 characters. 
- `<type>:<description>`
  - `<type>` should be one of the following:
    - `feature` (for new feature development)
    - `fix` (for bug fixes)
    - `refactor` (for refactoring code)
    - `wip` (for work in progress – commits to be pushed later)

  #### Example of a Valid Commit Message:    *fix:Fixed a critical bug*

## How It Works

The `commit-msg` hook script is executed automatically each time you make a commit in your local repository. Here's a detailed breakdown of how it works:

1. **Setting the Shell Interpreter:**
   - The first line of the script, `#!/bin/sh`, specifies the path to the shell interpreter that should be used to execute the script.

2. **Storing the Commit Message:**
   - `commit_message` is a variable that stores the content of the commit message.
   - `$(cat "$1")` is a command that reads the commit message from a file. `$1` represents the first argument passed to the script, which contains the path to the commit message file.
     
3. **Checking Message Length:**
   - `"${commit_message_length}" -gt 50` - This line checks if the commit message exceeds 50 characters in length.
   - If the message is longer than 50 characters, the script exits with an exit code of 1, signaling an error. An error message, "Commit message is too long!", is displayed, and the commit is not executed.
   - If the message fits within the length requirements, the script continues execution.


4. **Checking Message Format:**
   - `$commit_message" =~ $correct_commit_regex` - This line is responsible for verifying if the commit message is in correct format.
   
     - `$commit_message` contains the commit message to be checked.
     - `$correct_commit_regex` is a regular expression (regex) used to define the correct format.

   - `correct_commit_regex="^${commit_types}\:.+$"` - This regex defines the expected format of a commit message.
     - `^` ensures that the pattern starts at the beginning of the line.
     - `${commit_types}` contains the acceptable commit types: `feature`, `fix`, `refactor`, or `wip`.
     - `\:`, specifies that next character must be ':'.
     - `.+` indicates that there should be one or more characters following the ':', representing the description.

    - If the message is not correctly formated, the script exits with an exit code of 1, signaling an error. An error message,  "Format is not correct!", is displayed, and the commit is not executed.

## How to Use


1. **Download the Hook File:**
   - You should first download `commit-msg` file from this repository.

2. **Place the Hook File in Your Local Repository:**
   - In your local Git repository, locate the `.git/hooks/` directory.
   - Move the downloaded `commit-msg` hook file into the `.git/hooks/` directory of your local repository.

3. **Make the Hook File Executable (On Unix-Like Systems):**
   - Rename the `commit-msg.sample` file by removing the `.sample` extension. If this renaming step does not work as expected or if you encounter issues, follow the next step.
   - If you are using a Unix-like operating system (e.g., Linux or macOS), you may need to make the hook file executable. You can do this with the following command:

     ```shell
     chmod +x .git/hooks/commit-msg
     ```

5. **Commit with Enforced Commit Message Format:**
   - Now, every time you make a commit in your local repository, the `commit-msg` hook script will automatically validate your commit message against the specified rules.

   - If the commit message does not meet the required format or exceeds the character limit, the script will prevent the commit with an error message.



