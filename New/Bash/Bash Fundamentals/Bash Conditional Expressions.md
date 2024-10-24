

In Bash, conditional expressions are used to evaluate conditions and execute commands based on the result (true or false). 

Conditional expressions are used by the [[ compound command and the [built-in commands to test ﬁle attributes and perform string and arithmetic comparisons.


### NOTE:

Wrapping the variable name between curly brackets is not required, but is considered a good practice, and I would advise you to use them whenever you can:


## Benefits of Using Curly Braces:

**Clarity**: It clearly delineates the variable name from surrounding text, reducing ambiguity.

**Avoids Ambiguity**: When concatenating variables or adding text, braces make it explicit where the variable ends.

**Parameter Expansion**: Curly braces are required for certain parameter expansions, such as length or substring extraction.

**String Manipulation**: They are necessary for string replacements and other manipulations.


# Here is a list of the most popular Bash  conditional expressions:


## File expressions:


#### File Tests

- **-e**: True if the file exists.
- **-f**: True if the file is a regular file.
- **-d**: True if the file is a directory.
- **-r**: True if the file is readable.
- **-w**: True if the file is writable.
- **-x**: True if the file is executable.
#### True if ﬁle exists:


```
[[ -a ${file} ]]
```


#### True if ﬁle exists and is a block special ﬁle:

```
[[ -b ${file} ]]
```

#### True if ﬁle exists and is a character special ﬁle:
```
[[ -c ${file} ]]
```

#### True if ﬁle exists and is a directory:

```
[[ -d ${file} ]]
```

#### True if ﬁle exists:

```
[[ -e ${file} ]]
```

### True if ﬁle exists and is a regular ﬁle:

```
[[ -f ${file} ]]
```
### True if ﬁle exists and is a symbolic link:

```
[[ -h ${file} ]]
```
### True if ﬁle exists and is readable:

```
[[ -r ${file} ]]
```

### True if ﬁle exists and has a size greater than zero:

```
[[ -s ${file} ]]
```

#### True if ﬁle exists and is writable:

```
[[ -w ${file} ]]
```
### True if ﬁle exists and is executable:

```
[[ -x ${file} ]]
```
### True if ﬁle exists and is a symbolic link:

```
[[ -L ${file} ]]
```



# Arithmetic operators:

#### Numeric Tests:

You can compare numbers:

- `-eq`: Equal.
- `-ne`: Not equal.
- `-lt`: Less than.
- `-le`: Less than or equal to.
- `-gt`: Greater than.
- `-ge`: Greater than or equal to.


#### Returns true if the numbers are equal:



```bash
[[ ${arg1} -eq ${arg2} ]]
```

### Returns true if the numbers are not equal:

```bash
[[ ${arg1} -ne ${arg2} ]]
```

#### Returns true if arg1 is less than arg2:

```bash
[[ ${arg1} -lt ${arg2} ]]
```


#### Returns true if arg1 is less than or equal arg2:

```bash
[[ ${arg1} -le ${arg2} ]]
```


#### Returns true if arg1 is greater than arg2:

```bash
[[ ${arg1} -gt ${arg2} ]]
```

#### Returns true if arg1 is greater than or equal arg2:

```bash
[[ ${arg1} -ge ${arg2} ]]
```


Example:

``` bash
#!/bin/bash

num=100

if [[ $num -gt 10 ]]; then

echo "Number is greater than 10."

fi

```

![Pasted image 20241020123222](https://github.com/user-attachments/assets/3192719f-bf7a-46b7-81e9-aab9ca14c398)




# Note:

As a side note, arg1 and arg2 may be positive or negative integers. As with other programming languages you can use AND & OR conditions:

Also `[[ ]]` is used  for more advanced features, including pattern matching and logical operators:

- `[[ condition1 && condition2 ]]`: Logical AND.
- `[[ condition1 || condition2 ]]`: Logical OR.
- `[[ string =~ regex ]]`: Regex match.


```
[[ test_case_1 ]] && [[ test_case_2 ]] # And
```

```
[[ test_case_1 ]] || [[ test_case_2 ]] # Or
```

Example 1:

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


Example 2:

``` bash
#!/bin/bash


name=("lm3nitro" "bash")

if [[ $name == "lm3nitro" || $name == "bash" ]]; then

echo "Hello, lm3nitro or bash!"

fi

```

Example:

![Pasted image 20241020121629](https://github.com/user-attachments/assets/a736d69a-d0af-4c1b-9418-9391e3fad2e3)


# String expressions:


#### String Tests


You can compare strings:

- `-z string`: True if string is empty.
- `-n string`: True if string is not empty.
- `string1 = string2`: True if strings are equal.
- `string1 != string2`: True if strings are not equal.

Example:


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


### True if the shell variable varname is set (has been assigned a
value):

```
[[ -v ${varname} ]]
```

#### True if the length of the string is zero:

```
[[ -z ${string} ]]
```

#### True if the length of the string is non-zero:

```
[[ -n ${string} ]]
```


#### True if the strings are equal. = should be used with the test
command for POSIX conformance. When used with the [[ command, this performs pattern matching as described above (Compound Commands).:


```
[[ ${string1} == ${string2} ]]
```


#### True if the strings are not equal:

```
[[ ${string1} != ${string2} ]]
```

### True if string1 sorts before string2 lexicographically:

```
[[ ${string1} < ${string2} ]]
```

#### True if string1 sorts after string2 lexicographically:


```
[[ ${string1} > ${string2} ]]
```


### Exit status operators:

In Bash, exit statuses are integral for controlling the flow of a script based on the success or failure of commands. The exit status is a numeric value returned by a command when it finishes execution. By convention:

- `0` indicates success.
- Non-zero values indicate failure.


#### Returns true if the command was successful without any errors:

```
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

#### Returns true if the command was not successful or had errors:

```
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
