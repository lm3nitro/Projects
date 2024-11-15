# Bash Functions

Bash functions are a great way to organize and reuse code in shell scripts:

There are two main ways to define a function:

1)Using the `function` keyword:

```bash
function my_function {
    echo "Hello from my_function!"
}
```

2)Without the `function` keyword:

```bash
my_function() {
    echo "Hello from my_function!"
}
```

Once a function is defined, it can be caclled simply by its name:

```bash
my_function
```

1. Example 1:

```bash

#!/bin/bash

# Using the function keyword:
function my_function_1 {
     echo "Hello from my first function!"
}

# Without the function keyword:
my_function_2() {
    echo "Hello from my second function!"
}
# Calling Functions:

my_function_1
echo "" 
my_function_2
```

![Pasted image 20241023171535](https://github.com/user-attachments/assets/75c4ebae-0cbd-4272-81e5-23e74f4c237b)

2. Example 2:


```bash
#!/bin/bash

server_name=$(hostname)

function memory_check() {
echo ""
echo "Memory usage on ${server_name} is: "
free -h
echo ""
}

function cpu_check() {
echo ""
echo "CPU load on ${server_name} is: "
echo ""
uptime
echo ""
}
function tcp_check() {
echo ""
echo "TCP connections on ${server_name}: "
echo ""
cat /proc/net/tcp | wc -l
echo ""
}
function kernel_check() {
echo ""
echo "Kernel version on ${server_name} is: "
echo ""
uname -r
echo ""
}
function all_checks() {
memory_check
cpu_check
tcp_check
kernel_check
}
all_checks
```

![Pasted image 20241023170237](https://github.com/user-attachments/assets/03fa3f4c-9c00-4ec2-927d-58d07665342a)

### Passing Arguments:

Once the function is defined, it can be called by its name:

```bash

#!/bin/bash

say_hello() {
    echo "Hello, $1!"
}

say_hello "lm3nitro"
```

![Pasted image 20241023171803](https://github.com/user-attachments/assets/fd8f5b40-8b5a-4310-8c72-fb9b4f002817)

### Returning Values:

Bash functions can return a status code (0 for success, non-zero for failure), but to return a value, typically `echo` is used:

```bash

#!/bin/bash

math_add() {
#$1 and $2 refer to the first and second arguments passed to the function when it is called
    echo $(($1 + $2))

}
# math_add called with 1345 and 65365 as arguments.
result=$(math_add 1345 65365)

echo "The result is $result"
```

![Pasted image 20241023172655](https://github.com/user-attachments/assets/8e49321c-5938-4639-b210-ebce63633e17)
