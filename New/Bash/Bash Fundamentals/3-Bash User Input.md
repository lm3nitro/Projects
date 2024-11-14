# Bash User Input

In Bash, users can be prompted for input using the `read` command. Here’s a basic example:

```bash
#!/bin/bash

# Prompt for user input
echo "Please enter your name:"
read name

# Output the input
echo "Hello, $name!"
```

Example:

![Pasted image 20241018222554](https://github.com/user-attachments/assets/3ebfe11a-45b3-454d-87af-45d1ca56a556)

### Additional Options:

1. It's also possible to use `-p` with `read` to combine the prompt and input into one line:

```bash
read -p "Please enter your name: " name
```

Example:

![Pasted image 20241018222921](https://github.com/user-attachments/assets/32abc919-264b-4ada-8cd6-63c7b86bfbe5)

2. Handling Multiple Inputs:

To read multiple values in one line, you can do:

```bash
read -p "Enter your first and last name: " first last
echo "First: $first, Last: $last"
```

Example:

Once I typed my name, I hit enter, and got the following output:

![Pasted image 20241018223255](https://github.com/user-attachments/assets/8b13263a-ec98-4f10-9aec-2119cd6addf2)


