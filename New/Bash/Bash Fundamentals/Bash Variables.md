Bash variables are essential for storing and manipulating data in scripts.


As in any other programming language, you can use variables in Bash
Scripting as well. However, there are no data types, and a variable in
Bash can contain numbers as well as characters.


To assign a value to a variable, all you need to do is use the = sign:

```
name="lm3nitro"
```


## NOTE:

you can not have spaces before and after the = sign.

# Variable common  use cases:


### Storing User Input:

You can prompt users for input and store their response in a variable.

```
read -p "Enter your name: " name
echo "Hello, $name!"

```

### Holding Configuration Values:


Variables can store configuration settings or constants used throughout your script.

```
db_name="my_database"
db_user="admin"
echo "Connecting to $db_name with user $db_user"

```

#### **File Paths**:

```
log_file="/var/log/myapp.log"
echo "Logs can be found at $log_file"

```

### **Command Output**:

Capture the output of a command in a variable for later use.

```
current_date=$(date)
echo "Today's date is $current_date"

```

After that, to access the variable, you have to use the $ and reference it
as shown below:

### Note:

Wrapping the variable name between curly brackets is not required, but
is considered a good practice, and I would advise you to use them
whenever you can:

```
echo ${name}
```

![Pasted image 20241018194732](https://github.com/user-attachments/assets/a93a8140-745d-44dc-a942-c74029599db6)

#### **Looping Through Lists**:

Use variables to iterate over a list of items. This is a great example of how to use arrays and loops in Bash!

```
fruits=("apple" "banana" "cherry")
for fruit in "${fruits[@]}"; do
    echo "I like $fruit"
done

```

Example:

![Pasted image 20241018194549](https://github.com/user-attachments/assets/7fcaa81f-5964-412d-bd47-329aa6f84f43)

#### **Arithmetic Operations**:

Perform calculations and store the result in a variable.

```
a=5
b=3
sum=$((a + b))
echo "The sum is $sum"

```

#### Conditional Checks:

Use variables in conditional statements to control script flow.

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


#### **Environment Variables**:

Set and use environment variables to configure the behavior of scripts and applications.


```bash
export PATH="$PATH:/usr/local/myapp/bin"

```


### command-line parameters:

In Bash scripts, you can read command-line parameters using special variables. The parameters are accessible using `$1`, `$2`, `$3`, etc., where `$1` is the first argument, `$2` is the second, and so on. You can also use `$@` to refer to all arguments and `$#` to get the count of arguments.

```bash
#!/bin/bash

echo " Hello, who is there?" $1
```

Example:

![Pasted image 20241018202255](https://github.com/user-attachments/assets/424133ee-4134-44b5-9dc7-fc50c568802f)


$1 is the ﬁrst input (lm3nitro) in the Command Line. Similarly, there could
be more inputs and they are all referenced to by the $ sign and their
respective order of input. This means that lm3nitro! is referenced to using
$2. Another useful method for reading variables is the $@ which reads
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
