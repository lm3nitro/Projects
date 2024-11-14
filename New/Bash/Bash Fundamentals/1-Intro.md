# Introduction

### Running a script:

To run a bash script using the bash shell interpreter, the first line of the script must specify the absolute path to the bash executable:

```bash
#!/bin/bash
```

To make the script executable, run the following command:

```bash
chmod +x intro.sh
```

After that, the file can be executed using the following:

```bash
./intro.sh
```

Example:

![Pasted image 20241018193132](https://github.com/user-attachments/assets/1dd740de-4968-410b-ab01-4e33eee66b70)

The only thing that the shebang does is to instruct the operating system to run the script with the /bin/bash executable.

### Echo Command:

1. Displaying Messages:

It outputs text or variables to the terminal, which is useful for providing feedback to users or indicating the progress of a script.

```bash
echo "Starting the backup process..."
```

2. Outputting Variable Values:

Printing the values of variables.

```bash
name="lm3nitro"
echo "Hello, $name!"
```

3. Creating Files:

Writing to  text to a file.

```bash
echo "This is a new file." > my_file.txt
```

4. Appending to Files:

```bash
echo "This will be added to the file." >> my_existing_file.txt
```

5. Formatting Output:
It supports escape sequences (with the `-e` flag), which can be used for formatting. It also handle multi-line output with proper formatting.

```bash
echo -e "Hello lm3nitro\nAre you ready to learn bash scripting?"
```

Example:

![Pasted image 20241018192254](https://github.com/user-attachments/assets/cc72981f-c441-4fe0-b7be-37eb85762a13)
