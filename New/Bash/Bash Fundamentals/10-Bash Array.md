# Bash Array

The main thing to know is that unlike variables, arrays can hold several values under one name. An array can be initialized by assigning values divided by space and enclosed in (). 

Example:

```bash
my_array=("value 1" "value 2" "value 3" "value 4")
```

To access the elements in the array, it will need to be referenced by their numeric index.

> [!IMPORTANT]  
>  Keep in mind that curly brackets need to be used!

### Accessing Array Elements:

1. Access a single element, this would output: value 2

```bash
echo ${my_array[1]}
```

Example:

![Pasted image 20241019121154](https://github.com/user-attachments/assets/8a1feed0-d43b-4d72-9fe4-00b7a4c3fc3f)

2. This would return the last element: value 4

```bash
echo ${my_array[-1]}
```

3. As with command line arguments using @ will return all elements in the array, as follows:

```bash
#!/bin/bash


#Creating an array:

my_array=("lm3nitro" "bash" "python" "powershell")

#Printing my_array:

echo "${my_array[@]}"
```

![Pasted image 20241019120627](https://github.com/user-attachments/assets/2ce750ed-5d9a-4786-8a33-4e3660eb9f12)

### Length of the Array:

Assuming the array is preceded by a hash sign (#), it would output the total number of elements in the array, in my case it is 4:

``` bash
#!/bin/bash

#Creating an array:

my_array=("lm3nitro" "bash" "python" "powershell" "php" "java")

#Printing my_array:

echo "${#my_array[@]}"
```

![Pasted image 20241019120805](https://github.com/user-attachments/assets/7cfa2055-921b-4ee4-af46-8d8b645b1b09)

### Adding Elements:

It's possible to add elements to an existing array:

```bash
#!/bin/bash

#Creating an array:

my_array=("lm3nitro" "bash" "python" "powershell" "php" "java")

#Add a new element

my_array+=(golang)

#Printing my_array:

#Looping Through my_array:

for array in "${my_array[@]}";do

echo $array

done

```

Example:

![Pasted image 20241019121834](https://github.com/user-attachments/assets/e884f892-01d7-4f07-bbba-5e834818ab67)

### Array Slicing:

While Bash doesn't support true array slicing, similar results can be achieced using a combination of array indexing and string slicing:

```bash
#!/bin/bash
array=("A" "B" "C" "D" "E")
# Print entire array
echo "${array[@]}" # Output: A B C D E
# Access a single element
echo "${array[1]}" # Output: B
# Print a range of elements (requires Bash 4.0+)
echo "${array[@]:1:3}" # Output: B C D
# Print from an index to the end
echo "${array[@]:3}" # Output: D E
```

Example:

![Pasted image 20241019122741](https://github.com/user-attachments/assets/cb3a877f-798b-4083-94bb-7b359b436fe3)

When working with arrays, always use [@] to refer to all elements, and enclose the parameter expansion in quotes to preserve spaces in array elements.

### String Slicing:

In Bash, portions of a string can be extracted using slicing. The basic syntax is:

```bash
${string:start:length}
```

Where start is the starting index (0-based), and length is the maximum number of characters to extract:

```bash
#!/bin/bash

text="LM3NITRO"

# Extract from index 0, maximum 2 characters

echo "${text:0:2}"

# Extract from index 3 to the end

echo "${text:3}"

# Extract 3 characters starting from index 1

echo "${text:1:3}"

# If length exceeds remaining characters, it stops at the end

echo "${text:3:3}"
```

Example:

![Pasted image 20241019124506](https://github.com/user-attachments/assets/0c971388-bcfc-4016-ad02-95a1a2c07a1f)

> [!NOTE]  
> The second number in the slice notation represents the maximum length of the extracted substring, not the ending index. This differs from some other programming languages like Python. In Bash, if a length that exceeds the end of the string is specified, it will simply stop at the string's end without raising an error.

For example:

```bash
text="Hello, lm3nitro!"

# Extract 5 characters starting from index 7

echo "${text:7:8}"

# Attempt to extract 10 characters starting from index 7

# (even though only 8 characters remain)

echo "${text:7:10}"
```

![Pasted image 20241019125005](https://github.com/user-attachments/assets/ee33cef8-d2f5-48f9-9724-3b735ab6b807)


Even though we asked for 10 characters, Bash
only returns the 8 available characters from index 7 to the end of the
string. This behavior can be particularly useful when you're not sure of
the exact length of the string you're working with.
