# Arithmetic Operations

Bash provides several ways to perform arithmetic operations. Here are some common methods:

### Using `let`:

The `let` command allows for performing arithmetic operations:

```bash
#!/bin/bash

let "result = 5 + 3"
echo "5 + 3 = $result"
```

![Pasted image 20241018232647](https://github.com/user-attachments/assets/0d06e180-77f7-4af6-b2f5-d8ed39bbd7d2)

### Using `(( ))`:

The `(( ))` syntax is often preferred for arithmetic operations:

![Pasted image 20241018232722](https://github.com/user-attachments/assets/78873168-d9fd-4c0b-9adb-130145f7ea4f)

### Using `expr`:

The `expr` command can also be used, but it's less common:

```bash
#!/bin/bash

num1=5
num2=3
result=$(expr $num1 + $num2)
echo "$num1 + $num2 = $result"
```

![Pasted image 20241018232924](https://github.com/user-attachments/assets/6486faa4-2dfe-4f8a-9b6d-b9632191bac6)
