# MIPS Control code







## add



### C

```c
int main(void) {
  int x = 17;
  int y = 25;
  printf("%d\n", x + y);
  return 0;
}
```

### Simplified C

```c
int main(void) {
  
  int x, y, z;
  x = 17;
  y = 25;
  
  //seperate calulation and printf
  z = x + y;
  printf("%d", z);
  
  //seperate text to print and "\n"
  printf("\n");
  
  return 0;
}
```



### MIPS

```
main:
	li		$t0, 17			# x = 17;
	li		$t1, 25			# x = 25;
	add		$t2, t1, t0 # z = x + y;
	move	$a0, $t2		# printf("%d", z);  $a0 is for z, var to print
	li		$v0, 1			# $v0 is for syscall type, 1 stand for printf("%d")
	syscall
	li		$a0, '\n'   # $a0 is for '\n', var to print
	li		$v0, 11			# $v0 is for syscall type, 11 stand for printf("%c")
	syscall
	li		$v0, 0			# return 0 for close
	jr		$ra
```







##  While Loop 



### C

``` c
i = 0;
n = 0;
while (i < 5) {
  n = n + 1;
  i++;
}
```

### Simplified C

```c
i = 0;
n = 0;
loop:
  if (i >= 5) goto end;
    n = n + 1;
    i++;
	goto loop;
end:
```

### MIPS

```
		li		$t0, 0		# i = 0;
		li		$t1, 0	 	# n = 0;
loop:
		bge		$t0, 5, end # bge = boolean greater and equal, if(param1 >= param2) do param3
		add		$t1, $t1, $t0 # n = n + i;
		addi	$t0, $t0, 1		# i++;
		j 		loop					# jump to start of loop
end:
```









## If Condition



### C

```c
if (i < 0) {
  n = n - i;
} else {
  n = n + i;
}
```

### Simplified C

```c
		if (i >= 0) goto else1;
		n = n - i;
		goto end1;
else1:
		n = n + i;
end1:
```

### MIPS

```
		bge $t0, 0, else1
		sub $t1, $t1, $t0
		j end1
else1:
		add $t1, $t1, $t0
end1:
```











## If and &&



### C

```c
if (i < 0 && n >= 42) {
  n = n - i;
} else {
  n = n + i;
}
```

### Simplified C

```c
		if (i >= 0) goto else1;
		if (n < 42) goto else1;
		n = n - i;
		goto end1;
else1:
    n = n + i;
end1:
```

### MIPS

```
		bge $t0, 0, else1
		blt $t1, 42, else1
		sub $t1, $t1, $0
		j end1
else1:
		add $t1, $t1, $t0
end1:
```











## Odd-even



### C

```c
#include <stdio.h>

int main(void) {
  int x;
  
  printf("Enter a number: ");
  scanf("%d", &x);
  
  if ((x & 1) == 0) {
    printf("Even\n");
  } else {
    printf("Odd\n");
  }
  
  return 0;
}
```

### Simplified C

```c
#include <stdio.h>

int main(void) {
  int x, v0;
  
  printf("Enter a number: ");
  scanf("%d", &x);
  
  v0 = x & 1;
  if (v0 == 1) goto odd
    printf("Even\n");
  goto end;
odd:
    printf("Odd\n");
end:
  
  return 0;
}
```

### MIPS

```
		.data
string0:
		.asciiz "Enter a number: "
string1:
		.asciiz "Even\n"
string1:
		.asciiz "Odd\n"
		
		.text
main:
		la		$a0, string0			# get from memory
		li		$v0, 4
		syscall
		
		li		$v0, 5						# scanf("%d", x);
		syscall
		
		and		$t0, $v0, 1				# t0 = x & 1
		beq		$t0, 1, odd       # if (t0 == 1) goto odd
		
		la		$a0, string1			# get from memory
		li		$v0, 4
		syscall
		
		j 		end

odd:
		la		$a0, string2			# get from memory
		li		$v0, 4
		syscall
		
end:
		li		$v0, 0						# return 0
		jr		$ra
```









## For Loop

### C

```c
#include <stdio.h>

int main(void) {
    for (int i = 1; i <= 10; i++) {
        printf("%d\n", i);
    }
    return 0;
}
```

### Simplified C

```c
#include <stdio.h>

int main(void) {
  
  	int i;
  	i = 1;
loop:
    if (i > 10) goto end;
  			
        printf("%d\n", i);
  			printf("\n");
  
  			i++;
  
    goto loop;
end:
    return 0;
}
```

### MIPS

```
main:

		li		$t0, 1		# i = 1;
		
loop:
		bgt $t0, 10, end
		
		move $a0, $t0   # printf("d", i)
		li	 $v0, 1
		syscall
		
		li	 $a0, '\n'  # printf("%c", '\n')
		li	 $v0, 11
		syscall
		
		addi $t0, $t0, 1 # i++   add is for value in 2 register, addi add a constant
		
		j    loop

end:
		li		$v0, 0
		jr		$ra
```









## SUM OF SQUARE

### C

```c
#include <stdio.h>

int main(void) {
    int sum = 0;

    for (int i = 0; i <= 100; i++) {
        sum += i * i;
    }

    printf("%d\n", sum);

    return 0;
}
```

### Simplified C

```c
#include <stdio.h>

int main(void) {
    int i, sum, square;
  
    sum = 0;
    i = 0;
  
loop:
    if( i > 100) goto end
    square = i * i;
    sum = sum + square;    
		i = i + 1;
    goto loop;
    
end:
    printf("%d", sum);
  	printf("\n");
    return 0;
}
```

### MIPS

```
main:
		li		$t0, 0		#sum
		li		$t1, 0		#i
		
loop:
		# if( i > 100) goto end
		bgt		$t1, 100, end
		
		# square = i * i;
    mul		$t2, $t1, $t1
    
    # sum = sum + square;
    add		$t0, $t0, $t2
		
		#		i = i + 1;
		addi	$t1, $t1, 1
		
		j			loop


end: 
		# printf("%d", sum);
		move	$a0, $t0
		li		$v0, 1
		syscall

		# printf("%c");
		li		$a0, '\n'
		li		$v0, 11
		syscall
		
		# return 0
		li		$v0, 0
		jr		$ra
```

