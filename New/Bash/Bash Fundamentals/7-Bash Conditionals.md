# Bash Conditionals

In Bash, conditionals are used to perform different actions based on whether a specified condition is true or false. The main types of conditionals are `if`, `elif`, `else`, and `case`. 

### Comparison Operators

When using `[` (test), common comparison operators include:

+ `-eq`: equal
+ `-ne`: not equal
+ `-lt`: less than
+ `-le`: less than or equal
+  `-gt`: greater than
+  `-ge`: greater than or equal

For string comparisons, use:

+ `=`: equal
+ `!=`: not equal
+ `<`: less than (requires double brackets `[[ ]]`)
+ `>`: greater than (requires double brackets `[[ ]]`)

### Boolean Operators

Conditions can also be combined:

+ `&&`: logical AND
+ `||`: logical OR

### If Statements:

Example 1:

```bash

#!/bin/bash


OS_COMMAND=$(cat /etc/os-release | grep -q "ubuntu")

if ${OS_COMMAND}; then 

# This updates the package index, ensuring the latest information on available packages and their versions.

    sudo apt update
#This upgrades all installed packages to their latest versions, and the -y option automatically answers "yes" to prompts

    sudo apt dist-upgrade -y
# Wait for background processes to finish

    wait
fi
```

![Pasted image 20241021154556](https://github.com/user-attachments/assets/a0ceb4d7-ed92-4673-b6ed-f402b55c8297)

Example 2:

```bash

#!/bin/bash


# Bash if statement

# User input

read -p "What is your name? " name

#True if the length of the string is zero.

if [[ -z ${name} ]]; then

echo "Please enter your name!"

fi
```

![Pasted image 20241021155256](https://github.com/user-attachments/assets/33d54cde-324d-4091-beba-e8310c4b5665)


Example 3:

```bash

#!/bin/bash

num=7

if [ $num -gt 0 ] && [ $num -lt 10 ]; then

echo "Number is between 1 and 9."

fi
```

![Pasted image 20241021153033](https://github.com/user-attachments/assets/2ecd8621-deee-497c-a21c-1feda360dcf2)


### If Else Statement:

With an if-else statement, an action can be specified in case the condition in the if statement does not match. This can be combined with the conditional expressions from the previous section as follows:

Example 1:


```bash

#!/bin/bash


# Bash if statement

# User input

read -p "What is your name? " name

#True if the length of the string is zero.

if [[ -z ${name} ]]; then

echo "Please enter your name!"

else

echo "Hello, How are you $name"

fi
```


![Pasted image 20241021155728](https://github.com/user-attachments/assets/fd2e16d4-f298-456f-befd-96fb6bf615da)


Example 2:

```bash

#!/bin/bash

ADMIN_USERS=("lm3nitro" "bash" "python" "powershell")

# User input

read -p "What is your name?" username

# Checking if the username is on $ADMIN_USER list:

if [[ "${username}" == "$ADMIN_USERS" ]]; then

  echo "You're a super user!"

else
  
  echo "You are NOT a super user!"

fi
```

![Pasted image 20241021160609](https://github.com/user-attachments/assets/8ddb1982-8e22-42ce-a02c-9001c45718f3)


Example 3:

```bash

#!/bin/bash


# if statement which would check your current User ID and would not allow you to run the script as the root user:  

if (($EUID == 0 )); then

    echo "Do not run as root"

fi
```

![Pasted image 20241021161344](https://github.com/user-attachments/assets/d309bf66-4a14-4130-9f1a-c6fd0c998e37)


Example 4:

Testing multiple conditions with an if statement. In this example I want to make sure that the user is neither the admin user nor the root user to ensure the script is incapable of causing too much damage. I'll use the or operator in this example, noted by `||`. This means that either of the conditions needs to be true. If the and operator of &&  is used, then both conditions would need to be true.

```bash

#!/bin/bash


super_user=("lm3nitro" "bash" "python" "powershell")

read -p "Enter your username? " user

# Check if the username provided is the admin

if [[ "${user}" != "${super_user}" ]] && [[ $EUID != 0 ]] ;then

    echo "You are not the admin or root user, but please be safe!"

else

    echo "You are the admin user! This could be very destructive!"

fi
```


![Pasted image 20241021162022](https://github.com/user-attachments/assets/ca2a5a06-4f75-4493-97ca-9b24a25d6d4a)


###  Elif Statement:

If you have multiple conditions and scenarios, then can use elif statement with if and else statements.

Example 1:

```bash 
#!/bin/bash

read -p "Enter a number:" num

if [[ $num -gt 0 ]]; then
   echo "The number is positive"
elif [[ $num -lt 0 ]]; then
   echo "The number is negative"
else
   echo "The number is 0"
fi

```

![Pasted image 20241021220555](https://github.com/user-attachments/assets/7e842dbf-07a6-4d41-80ce-2e5e11b61d86)


### Switch Case Statements:

You can use a case statement to simplify complex conditionals when there are multiple diﬀerent choices. So rather than using a few if, and if-else statements, you could use a
single case statement.

Example 1:

```bash 

#!/bin/bash

case "$my_var" in
    pattern_a)
        echo "Executing commands for pattern_a"
        # Insert commands for pattern_a here
        ;;
    pattern_b | pattern_c | pattern_d)
        echo "Executing commands for pattern_b, pattern_c, or pattern_d"
        # Insert commands for pattern_b, pattern_c, pattern_d here
        ;;
    *)
        echo "Executing default commands"
        # Insert default commands here
        ;;
esac

```

Example 2:

```bash
#!/bin/bash

read -p "Enter the name of your food brand: " food
case $food in
    "McDonald's")#  Using double quotes allows the apostrophe to be included without needing an escape character.
        echo -en "${food} is a popular fast food chain in the USA\n."
        ;;
    Nestlé | Kraft | Coca-Cola | Pepsi)
        echo -en "${food} is a well-known food brand from Switzerland or the USA\n."
        ;;
    Sushi | Ramen | Tempura)
        echo -en "${food} represents traditional Japanese cuisine\n."
        ;;
    *)
        echo -en "${food} is an unknown food brand\n"
        ;;
esac

```

![Pasted image 20241021222900](https://github.com/user-attachments/assets/efa551e6-f47e-4890-b308-8fcde2d5f898)

### Structure of a `case` Statement:

1. **Start with `case`**: Use the `case` keyword followed by a variable or expression.
2. **Use `in`**: Follow the variable with the `in` keyword.
3. **Define Patterns**: List your patterns, ending each with `)`. You can use `|` to specify multiple patterns.
4. **Commands**: After each pattern, write the commands that should be executed if the pattern matches.
5. **Terminate Clauses**: End each pattern with `;;`.
6. **Default Case**: Use `*` for a default pattern.
7. **Close with `esac`**: Conclude the `case` statement with `esac`.
