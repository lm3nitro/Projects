
# Running a script:

In order to execute/run a bash script ﬁle with the bash shell interpreter, the ﬁrst line of a script ﬁle must indicate the absolute path to the bash executable:

```bash
#!/bin/bash
```


After that make the script executable by running:


```shell
chmod +x intro.sh
```

After that execute the ﬁle:

```shell
./intro.sh
```

Example:


![Pasted image 20241018193132](https://github.com/user-attachments/assets/1dd740de-4968-410b-ab01-4e33eee66b70)


All that the shebang does is to instruct the operating system to run the
script with the /bin/bash executable.


# echo command:

##### Displaying Messages:


It outputs text or variables to the terminal, which is useful for providing feedback to users or indicating the progress of a script.

```bash
echo "Starting the backup process..."

```

####**Outputting Variable Values**:

Printing the values of variables.

```bash
name="lm3nitro"
echo "Hello, $name!"

```

#### **Creating Files**:

Writing to  text to a file.

```bash
echo "This is a new file." > my_file.txt

```

#### Appending to Files:

```bash
echo "This will be added to the file." >> my_existing_file.txt

```

#### **Formatting Output**:
It supports escape sequences (with the `-e` flag), which can be used for formatting. It also handle multi-line output with proper formatting.

```bash
echo -e "Hello lm3nitro\nAre you ready to learn bash scripting?"

```

Example:
![Pasted image 20241018192254](https://github.com/user-attachments/assets/cc72981f-c441-4fe0-b7be-37eb85762a13)
