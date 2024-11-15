# Continue

Tells your bash script to stop the current iteration of the loop and start the next iteration.

```bash
#!/bin/bash

for i in {1..10}; do
    if [[ $i -eq 2 ]]; then
        echo "skipping number 2"
        continue
    fi
    echo "i is equal to $i"
done
```

![Pasted image 20241022194609](https://github.com/user-attachments/assets/5ca16bf6-4ec3-4e69-9188-b7f6a364c09b)


# Break

Tells the bash script to end the loop straight away.

```bash

#!/bin/bash

num=1

while [[ $num -lt 10 ]]; do
    if [[ $num -eq 5 ]]; then
       echo "$num"
       break
    fi
    ((num++))
done
echo "Loop completed"

```

![Pasted image 20241022195525](https://github.com/user-attachments/assets/e2854ecc-7a96-4c6c-95a2-72b6fa4155ea)

### `Break command with multiple loops`

To exit out of current working loop whether inner or outer loop, I simply use break but if I' am  in an inner loop and I want to exit out of outer loop, I can use break 2.

```bash

#!/bin/bash

for (( a = 1; a < 10; a++ )); do
    echo "outer loop: $a"
    for (( b = 1; b < 100; b++ )); do
        if [[ $b -gt 5 ]]; then
        break 2
        fi
    echo "Inner loop: $b "
    done
done

```

The bash script will begin with `a=1` and will move to inner loop and when it reaches `b=5`, it will break the outer loop. I can use break only instead of break 2, to break inner loop and see how it aﬀects the output.

Example:
![Pasted image 20241022200538](https://github.com/user-attachments/assets/f153f5c8-2f83-40f0-956d-07a401f3a4f5)
