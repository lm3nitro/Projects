# Bash Loops

In Bash, loops are used to repeatedly execute a block of code. There are several types of loops available in Bash: `for`, `while`, and `until`.

### For Loops

1. Iterate over Array of Strings

The `for` loop iterates over a list of items and executes the block of code for each item:

```bash

#!/bin/bash

# Creating an array:
# 
fruits=("apple" "banana" "cherry")

# Iterate through an array:
for fruit in "${fruits[@]}"; do
   echo $fruit
done
```

![Pasted image 20241022133708](https://github.com/user-attachments/assets/1efd343c-3284-49a7-9795-07329c316bad)

2. Looping Over words in a string**:

```bash
#!/bin/bash

#Looping over words in a string
sentence="Cybersecurity is fun"

for s in $sentence; do
   printf "%s\n" "$s"
done
```

![Pasted image 20241022135710](https://github.com/user-attachments/assets/3c091510-5b03-432d-8d6e-36e5b3e41bfd)

3. Looping Over Characters:

C-style:

```bash
#!/bin/bash

#Looping over words in a string
name="lm3nitro"

for ((i=0; i<${#name}; i++)); do
  echo "${name:i:1}"
done
```

![Pasted image 20241022140536](https://github.com/user-attachments/assets/bae46a29-8bae-40fe-9810-6f7794fd8a8c)

> [!TIP]
>  `for` can also be used to process a series of numbers. For example here is one way to loop through from 1 to 10:

```bash

#!/bin/bash
for num in {1..10}; do
    echo ${num}
done
```

![Pasted image 20241022140912](https://github.com/user-attachments/assets/ebe8ccbd-eebb-44df-8d4b-a02340e4040b)


### While loops:

The `while` loop continues executing as long as the condition is true.

```bash
#!/bin/bash
counter=1
while [[ $counter -le 10 ]]; do
   echo "lm3nitro # $counter"
   ((counter++))
done
```

![Pasted image 20241022142251](https://github.com/user-attachments/assets/e1fd11ea-e1cf-43ff-aa63-185ca425593e)

Explanation:

I speciﬁed a counter variable and set it to 1, then inside the loop, I added counter by using this statement here: ((counter++)). That way, I make sure that the loop will run 10 times only and would not run forever. The loop will complete as soon as the counter becomes 10, as this is what we've set as the condition: while [[ $counter -le 10 ]].

```bash

#!/bin/bash

read -p "What is your name? " name
# Multi-Line Comments Using : '...'
: '
if you run the loop and just press enter without providing input,
the loop would run again and ask you for your name again and again until you actually provide some input.
'
# checks if the variable `name` is empty
while [[ -z ${name} ]]; do
    echo "Your name can not be blank. Please enter a valid name!"
    read -p "Enter your name again? " name
done

echo "Hi there ${name}"
```

![Pasted image 20241022143157](https://github.com/user-attachments/assets/7cf2fc09-6328-4a53-b991-64b0ea37ce9c)

In this example I test if the value of variable `i` is less than `5`. The body of the loop increments the value with "`i=$((i+1))`" so it executes the echo statement 0-4 times.

![Pasted image 20241022153618](https://github.com/user-attachments/assets/325fb9c8-9634-471f-99da-5ab303b9d053)

### Until Loops:

The diﬀerence between until and while loops is that the until loop will run the commands within the loop until the condition becomes true.

Structure:

```bash
until [[ your_condition ]]; do
     your_commands
done
```

```bash
#!/bin/bash

count=1
until [[ $count -gt 10 ]]; do
    echo $count
((count++))

done
```

Example:

![Pasted image 20241022153330](https://github.com/user-attachments/assets/c2bad6b9-ac83-4bd1-8ebd-884cf604e652)

### `while` vs `until`

In Bash scripting, `while` and `until` are both looping constructs, but they have opposite behaviors:`

```bash

#!/bin/bash
# while runs the loop while the condition is true.
count_1=1

while [ $count_1 -le 5 ]; do
    echo "Count is $count_1"
    ((count_1++))
done
echo "While loop is done!"
# until runs the loop until the condition is true (i.e. while the condition is false).
count_2=6
  until [ $count_2 -gt 10 ]; do
      echo "Count is $count_2"
      ((count_2++))
  done
  echo "Unil loop is done!"

```

1. Example 1:

![Pasted image 20241022150615](https://github.com/user-attachments/assets/b6fd3206-9713-4b26-81e4-ba51ac545f61)

2. Example 2:

Using while loop:

```bash
#!/bin/bash

while true; do
  echo "Exit code: $?"
  echo "sleeping"
  sleep 1
done

```

![Pasted image 20241022152455](https://github.com/user-attachments/assets/28c03826-1e5d-4969-8e0c-7ba99b324973)

Here in the output I can see the exit code of the `while loop` is 0, which means the control expression is true.

3. Using until loop:

```bash

#!/bin/bash

until ((0)); do
  echo "Exit code: $?"
  echo "sleeping"
  sleep 1
done
```

![Pasted image 20241022152656](https://github.com/user-attachments/assets/73283603-d49f-461d-87d1-139b2c020ace)

Here in the output I see the exit code is non-zero so our code will continue to execute until the condition is **false**.

4. Wait for apache2 to start (until):

```bash
#!/bin/bash

until nc -z "$1" 80; do
    sleep 1
done

echo "webserver is running"

```

With the "-z" option nc will return 0 if it sees that the port is open and will return 1 otherwise.

![Pasted image 20241022155131](https://github.com/user-attachments/assets/0ed5022a-a8de-4edf-99fd-8a1c95300b13)


5. `Wait for apache2 to start (while):`

```bash
#!/bin/bash

while ! nc -z "$1" 80; do
    sleep 1
done

echo "webserver is running"

```

Here's the equivalent with while. The only difference is how the condition was negated. The output will be the same.

![Pasted image 20241022155657](https://github.com/user-attachments/assets/bb0218c6-aa3b-4d6b-b815-cb92b70e6ca1)

### Key Differences:

Condition Logic: 
+ `while` checks if the condition is true to continue,.
+ `until` checks if the condition is false.

Usage: 
+ Use `while` when you want to repeat actions as long as a condition is met,.
+ Use`until` when you want to repeat actions until a condition is met.

### Summary:

- Use `while` for true conditions.
- Use `until` for false conditions.
