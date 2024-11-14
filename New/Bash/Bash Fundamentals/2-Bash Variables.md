# Bash Variable

Bash variables are used to store and manipulate data in scripts. Unlike other programming languages, Bash doesn’t require specific data types, so a variable can hold both numbers and characters.

To assign a value to a variable, all that is needed is the = sign:

```bash
name="lm3nitro"
```

> [!TIP]
> Spaces are not needed before and after the = sign.

### Common Variable Use Cases

1. Storing User Input

Prompt users for input and store their response in a variable:

```bash
read -p "Enter your name: " name
echo "Hello, $name!"
```

2. Holding Configuration Values

Variables can store configuration settings or constants used throughout the script;

```bash
db_name="my_database"
db_user="admin"
echo "Connecting to $db_name with user $db_user"
```

3. File Paths

```bash
log_file="/var/log/myapp.log"
echo "Logs can be found at $log_file"
```

4. Command Output

Capture the output of a command in a variable for later use.

```bash
current_date=$(date)
echo "Today's date is $current_date"
```

Once captured, to access the variable, use the $ and reference it as shown below:

> [!NOTE]  
> Wrapping the variable name between curly brackets is not required, but is considered good practice, and I would advise you to use them
whenever you can.

```bash
echo ${name}
```

![Pasted image 20241018194732](https://github.com/user-attachments/assets/a93a8140-745d-44dc-a942-c74029599db6)

5. Looping Through Lists

Variable can be used to iterate over a list of items. This is a great example of how to use arrays and loops in Bash:

```bash
fruits=("apple" "banana" "cherry")
for fruit in "${fruits[@]}"; do
    echo "I like $fruit"
done
```

Example:

![Pasted image 20241018194549](https://github.com/user-attachments/assets/7fcaa81f-5964-412d-bd47-329aa6f84f43)

6. Arithmetic Operations

Perform calculations and store the result in a variable:

```bash
a=5
b=3
sum=$((a + b))
echo "The sum is $sum"
```

7. Conditional Checks

Use variables in conditional statements to control script flow:

```bash 

is_logged_in=true

if $is_logged_in; then
    echo "Welcome back!"
else
    echo "Please log in."
fi
```

Example:

![Pasted image 20241018194652](https://github.com/user-attachments/assets/dc446099-1db1-4a70-9f67-0d1cd4d14af2)


8. Environment Variables:

Set and use environment variables to configure the behavior of scripts and applications.

```bash
export PATH="$PATH:/usr/local/myapp/bin"

```

7. Command-line Parameters

In Bash scripts, command-line arguments can be accessed using special variables like `$1, $2, $3,` etc. where `$1` is the first argument, `$2` is the second, and so on. `$@` gives you all the arguments, and `$#` shows the total number of arguments.

```bash
#!/bin/bash

echo " Hello, who is there?" $1
```

Example:

![Pasted image 20241018202255](https://github.com/user-attachments/assets/424133ee-4134-44b5-9dc7-fc50c568802f)

`$1` is the ﬁrst input (lm3nitro) in the Command Line. Similarly, there could be more inputs and they are all referenced to by the `$` sign and their respective order of input. This means that lm3nitro! is referenced to using `$2`. Another useful method for reading variables is the `$@` which reads
all inputs.

```bash
#!/bin/bash

echo "I like" $1

# $1 : first parameter
echo "I would like" $2

# $2 : second parameter
echo "I'm in love with" $@

# $@ : all
```

Example:

![Pasted image 20241018202754](https://github.com/user-attachments/assets/dca23ae1-479a-4201-b317-c9513722855b)
