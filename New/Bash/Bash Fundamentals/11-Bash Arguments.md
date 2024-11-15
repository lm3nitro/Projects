# Bash Arguments

In Bash, arguments can be passed to a script when running it from the command line. These arguments are accessible within the script using special variables.

### Accessing Arguments

- `$1`, `$2`, `$3`, etc.: These represent the first, second, third (and so on) arguments.
- `$0`: The name of the script itself.
- `$#`: The total number of arguments passed to the script.
- `$@`: All arguments as a list.
- `$*`: All arguments as a single word.

In the script, $1 can be used to reference the first argument that was specified. If second argument is passed, it would be available as $2 and so on.

Example 1:

```bash
#!/bin/bash
echo "Argument one is $1"
echo "Argument two is $2"
echo "Argument three is $3"
```

![Pasted image 20241018224918](https://github.com/user-attachments/assets/15bf61a3-5d48-49e0-9885-dd93d618b209)

Something to keep in mind is that $0 is used to reference the script itself. This is an excellent way to self-destruct the file if needed or simply get the name of the script.

Example 2:

```bash

#!/bin/bash
echo "The name of the file is: $0 and it is going to be self-
deleted."
rm -f $0
```

You need to be careful with the self deletion and ensure that you have your script backed up before you self-delete it.

![Pasted image 20241018225711](https://github.com/user-attachments/assets/767385ff-f644-4b75-a74a-efa0cf03974b)

> [!CAUTION]
> Precaution should be taken with self-deletion to ensure the script is backed up before it is deleted.

Example 3:

```bash
#!/bin/bash


#Printing the name of the script with $0

echo "Learn bash with lm3nitro: Name of the script is...." $0

#Printing the first argument with $1

echo -e "Who is learning bash today?\n"$1 # option -e add a new line
```

![Pasted image 20241018230549](https://github.com/user-attachments/assets/3c7eb05f-3282-4f78-83bb-f9f191bd7d56)

Example 4:

```bash
#!/bin/bash

# Check if at least one argument is provided
if [ $# -eq 0 ]; then
    echo "No arguments provided. Usage: $0 arg1 arg2 ..."
    exit 1
fi

# Print the name of the script
echo "Script name: $0"

# Print total number of arguments
echo "Total arguments: $#"

# Loop through all arguments
echo "Arguments received:"
for arg in "$@"; do
    echo "- $arg"
done

# Access specific arguments
echo "First argument: $1"
echo "Second argument: $2"

```

![Pasted image 20241018224709](https://github.com/user-attachments/assets/9278a515-7d5e-4e88-a085-b1af1f492c08)
