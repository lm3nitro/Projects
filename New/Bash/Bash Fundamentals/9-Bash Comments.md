# Bash Comments

In Bash, comments are used to explain or annotate code, making it easier to understand. Comments are ignored by the shell during execution. Here’s how comments can be used in a Bash script:

1. Single-Line Comments:

To create a single-line comment by starting the line with a `#` symbol:

```bash
# This is a single-line comment
echo "Hello, World!"  # This comment follows a command
```

2, Multi-Line Comments:

While Bash does not have a built-in multi-line comment feature like some other programming languages, it can be similated using a combination of `: '...'` or `<< 'COMMENT' ... COMMENT` syntax.

Method 1: Using `:`

```bash
: '
This is a multi-line comment.
It will not be executed.
'
```

### Example Script with Comments:

```bash

#!/bin/bash

# This script greets the user

# Prompt for user input
read -p "Please enter your name: " name

: '
The following line will greet the user
and display the current date and time.
'
echo "Hello, $name! Today is $(date)."

# End of the script

```

![Pasted image 20241018224136](https://github.com/user-attachments/assets/754a837b-aa9a-42a4-8556-15535867279b)
