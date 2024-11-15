# Conditional Expressions

In Bash, conditional expressions are used to evaluate conditions and execute commands based on the result (true or false). 

Conditional expressions are used by the [[ compound command and the [built-in commands to test ﬁle attributes and perform string and arithmetic comparisons.

> [!NOTE]  
> Wrapping the variable name between curly brackets is not required, but is considered a good practice, and I would advise to use them whenever possible.

Benefits of Using Curly Braces:

+ Clarity: It clearly delineates the variable name from surrounding text, reducing ambiguity.
+ Avoids Ambiguity: When concatenating variables or adding text, braces make it explicit where the variable ends.
+ Parameter Expansion: Curly braces are required for certain parameter expansions, such as length or substring extraction.
+ String Manipulation: They are necessary for string replacements and other manipulations.

### Here is a list of the most popular Bash conditional expressions:

## File Expressions

File Tests:
- **-e**: True if the file exists.
- **-f**: True if the file is a regular file.
- **-d**: True if the file is a directory.
- **-r**: True if the file is readable.
- **-w**: True if the file is writable.
- **-x**: True if the file is executable.

1. True if ﬁle exists:

```bash
[[ -a ${file} ]]
```

2. True if ﬁle exists and is a block special ﬁle:

```bash
[[ -b ${file} ]]
```

3. True if ﬁle exists and is a character special ﬁle:
   
```bash
[[ -c ${file} ]]
```

4. True if ﬁle exists and is a directory:

```bash
[[ -d ${file} ]]
```

5. True if ﬁle exists:

```bash
[[ -e ${file} ]]
```

6. True if ﬁle exists and is a regular ﬁle:

```bash
[[ -f ${file} ]]
```

7. True if ﬁle exists and is a symbolic link:

```bash
[[ -h ${file} ]]
```

8. True if ﬁle exists and is readable:

```bash
[[ -r ${file} ]]
```

9. True if ﬁle exists and has a size greater than zero:

```bash
[[ -s ${file} ]]
```

10. True if ﬁle exists and is writable:

```bash 
[[ -w ${file} ]]
```

11. True if ﬁle exists and is executable:

```bash
[[ -x ${file} ]]
```

12. True if ﬁle exists and is a symbolic link:

```bash
[[ -L ${file} ]]
```

## Arithmetic Operators:

Numeric Tests:

- `-eq`: Equal.
- `-ne`: Not equal.
- `-lt`: Less than.
- `-le`: Less than or equal to.
- `-gt`: Greater than.
- `-ge`: Greater than or equal to.

1. Returns true if the numbers are equal:

```bash
[[ ${arg1} -eq ${arg2} ]]
```

2. Returns true if the numbers are not equal:

```bash
[[ ${arg1} -ne ${arg2} ]]
```

3. Returns true if arg1 is less than arg2:

```bash
[[ ${arg1} -lt ${arg2} ]]
```

4. Returns true if arg1 is less than or equal arg2:

```bash
[[ ${arg1} -le ${arg2} ]]
```

5. Returns true if arg1 is greater than arg2:

```bash
[[ ${arg1} -gt ${arg2} ]]
```

6. Returns true if arg1 is greater than or equal arg2:

```bash
[[ ${arg1} -ge ${arg2} ]]
```

Example 1:

``` bash
#!/bin/bash

num=100

if [[ $num -gt 10 ]]; then

echo "Number is greater than 10."

fi

```

![Pasted image 20241020123222](https://github.com/user-attachments/assets/3192719f-bf7a-46b7-81e9-aab9ca14c398)

> [!NOTE]  
> As a side note, arg1 and arg2 may be positive or negative integers. As with other programming languages AND & OR conditions can be used.
> Also `[[ ]]` is used for more advanced features, including pattern matching and logical operators:
> - `[[ condition1 && condition2 ]]`: Logical AND.
> - `[[ condition1 || condition2 ]]`: Logical OR.
> - `[[ string =~ regex ]]`: Regex match.
>
> ```
> [[ test_case_1 ]] && [[ test_case_2 ]] # And
> ```
>
> ```
> [[ test_case_1 ]] || [[ test_case_2 ]] # Or
> ```

Example 2:

``` bash

#!/bin/bash

# Define a file and a directory for testing
file="example.txt"
directory="example_dir"

# Test case 1: Check if the file exists
[[ -f $file ]] && echo "$file exists."

# Test case 2: Check if the directory exists
[[ -d $directory ]] && echo "$directory exists."

# Using AND
if [[ -f $file ]] && [[ -d $directory ]]; then
    echo "Both $file and $directory exist."
else
    echo "Either $file or $directory does not exist."
fi

# Using OR
if [[ -f $file ]] || [[ -d $directory ]]; then
    echo "At least one of $file or $directory exists."
else
    echo "Neither $file nor $directory exists."
fi

```


Example 3:

``` bash
#!/bin/bash


name=("lm3nitro" "bash")

if [[ $name == "lm3nitro" || $name == "bash" ]]; then

echo "Hello, lm3nitro or bash!"

fi

```

Example 4:

![Pasted image 20241020121629](https://github.com/user-attachments/assets/a736d69a-d0af-4c1b-9418-9391e3fad2e3)

## String expressions:

String Tests

- `-z string`: True if string is empty.
- `-n string`: True if string is not empty.
- `string1 = string2`: True if strings are equal.
- `string1 != string2`: True if strings are not equal.

Example 1:

``` bash
#!/bin/bash

my_var= # Empty variable:

if [[ -n "$my_var" ]]; then

    echo "Variable is not empty."
else
    echo "Variable is empty."
fi
```

![Pasted image 20241020123749](https://github.com/user-attachments/assets/e9b56872-a597-4869-b668-bb2c6fd61b1f)


1. True if the shell variable varname is set (has been assigned a
value):

```bash
[[ -v ${varname} ]]
```

2. True if the length of the string is zero:

```bash
[[ -z ${string} ]]
```

3. True if the length of the string is non-zero:

```bash
[[ -n ${string} ]]
```

4. True if the strings are equal.

> [!TIP]
> `=` should be used with the test command for POSIX conformance. When used with the [[ command, this performs pattern matching as described above (Compound Commands).

```bash
[[ ${string1} == ${string2} ]]
```

5. True if the strings are not equal:

```bash
[[ ${string1} != ${string2} ]]
```

6. True if string1 sorts before string2 lexicographically:

```bash
[[ ${string1} < ${string2} ]]
```

7. True if string1 sorts after string2 lexicographically:

```bash
[[ ${string1} > ${string2} ]]
```

## Exit status operators:

In Bash, exit statuses are integral for controlling the flow of a script based on the success or failure of commands. The exit status is a numeric value returned by a command when it finishes execution. By convention:

- `0` indicates success.
- Non-zero values indicate failure.

1. Returns true if the command was successful without any errors:

```bash
[[ $? -eq 0 ]]
```

Example:

```bash
#!/bin/bash

DOMAIN="google.com"

ping -c 1 ${DOMAIN}

# Returns true if the command was successful without any errors:

if [[ $? -eq 0 ]]; then

    echo "Server is open running"
else
    echo "Server is down"
fi

```

![Pasted image 20241020151254](https://github.com/user-attachments/assets/81b9f083-c08f-4860-8f71-c72f6a12674f)

2. Returns true if the command was not successful or had errors:

```bash
[[ $? -gt 0 ]]
```

Example:

``` bash
#!/bin/bash

DOMAIN="lm3nitro.bash.local"
LOG_FILE="ping_result.txt"

ping -c 1 ${DOMAIN} > "$LOG_FILE" 2>&1

# Returns true if the command was not successful or had errors:
if [[ $? -gt 0 ]]; then
    echo "Failed: Server is down"

else
    echo "Server is open running"
fi
```

![Pasted image 20241020151948](https://github.com/user-attachments/assets/b04ff199-5466-4d97-8551-42f0e36ec06e)
