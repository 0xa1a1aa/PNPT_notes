# File print lines

Bash script that prints the lines of a file specified as argument to the script:
```bash
#!/bin/sh

while read line
do
    printf "%s\n" "$line"
done < "$1"
```

# For loop

```bash
#!/bin/bash
for i in {1..5}
do
   echo "Welcome $i times"
done
```

With steps:
```bash
for i in {0..10..2}
do 
  echo "Welcome $i times"
done
```

Infinite loop:
```bash
#!/bin/bash
for (( ; ; ))
do
   echo "infinite loops [ hit CTRL+C to stop]"
done
```

# All script arguments

**$@** contains all arguments passed to the script:
```bash
#!/bin/bash
FILES="$@"
for f in $FILES
do
    # if .bak backup file exists, read next file
	if [ -f ${f}.bak ]
	then
		echo "Skiping $f file..."
		continue  # read next file and skip the cp command
	fi
     # we are here means no backup file exists, just use cp command to copy file
	/bin/cp $f $f.bak
done
```

# If statement

**Primary expressions:**

| Primary                     | Meaning                                                                                                                                                                                                                                                             |
| :-------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [ -a FILE ]                 | True if FILE exists.                                                                                                                                                                                                                                                |
| [ -b FILE ]                 | True if FILE exists and is a block-special file.                                                                                                                                                                                                                    |
| [ -c FILE ]                 | True if FILE exists and is a character-special file.                                                                                                                                                                                                                |
| [ -d FILE ]                 | True if FILE exists and is a directory.                                                                                                                                                                                                                             |
| [ -e FILE ]                 | True if FILE exists.                                                                                                                                                                                                                                                |
| [ -f FILE ]                 | True if FILE exists and is a regular file.                                                                                                                                                                                                                          |
| [ -g FILE ]                 | True if FILE exists and its SGID bit is set.                                                                                                                                                                                                                        |
| [ -h FILE ]                 | True if FILE exists and is a symbolic link.                                                                                                                                                                                                                         |
| [ -k FILE ]                 | True if FILE exists and its sticky bit is set.                                                                                                                                                                                                                      |
| [ -p FILE ]                 | True if FILE exists and is a named pipe (FIFO).                                                                                                                                                                                                                     |
| [ -r FILE ]                 | True if FILE exists and is readable.                                                                                                                                                                                                                                |
| [ -s FILE ]                 | True if FILE exists and has a size greater than zero.                                                                                                                                                                                                               |
| [ -t FD ]                   | True if file descriptor FD is open and refers to a terminal.                                                                                                                                                                                                        |
| [ -u FILE ]                 | True if FILE exists and its SUID (set user ID) bit is set.                                                                                                                                                                                                          |
| [ -w FILE ]                 | True if FILE exists and is writable.                                                                                                                                                                                                                                |
| [ -x FILE ]                 | True if FILE exists and is executable.                                                                                                                                                                                                                              |
| [ -O FILE ]                 | True if FILE exists and is owned by the effective user ID.                                                                                                                                                                                                          |
| [ -G FILE ]                 | True if FILE exists and is owned by the effective group ID.                                                                                                                                                                                                         |
| [ -L FILE ]                 | True if FILE exists and is a symbolic link.                                                                                                                                                                                                                         |
| [ -N FILE ]                 | True if FILE exists and has been modified since it was last read.                                                                                                                                                                                                   |
| [ -S FILE ]                 | True if FILE exists and is a socket.                                                                                                                                                                                                                                |
| [ FILE1 -nt FILE2 ]         | True if FILE1 has been changed more recently than FILE2, or if FILE1 exists and FILE2 does not.                                                                                                                                                                     |
| [ FILE1 -ot FILE2 ]         | True if FILE1 is older than FILE2, or is FILE2 exists and FILE1 does not.                                                                                                                                                                                           |
| [ FILE1 -ef FILE2 ]         | True if FILE1 and FILE2 refer to the same device and inode numbers.                                                                                                                                                                                                 |
| [ -o OPTIONNAME ]           | True if shell option "OPTIONNAME" is enabled.                                                                                                                                                                                                                       |
| [ -z STRING ]               | True of the length if "STRING" is zero.                                                                                                                                                                                                                             |
| [ -n STRING ] or [ STRING ] | True if the length of "STRING" is non-zero.                                                                                                                                                                                                                         |
| [ STRING1 == STRING2 ]      | True if the strings are equal. "=" may be used instead of "==" for strict POSIX compliance.                                                                                                                                                                         |
| [ STRING1 != STRING2 ]      | True if the strings are not equal.                                                                                                                                                                                                                                  |
| [ STRING1 < STRING2 ]       | True if "STRING1" sorts before "STRING2" lexicographically in the current locale.                                                                                                                                                                                   |
| [ STRING1 > STRING2 ]       | True if "STRING1" sorts after "STRING2" lexicographically in the current locale.                                                                                                                                                                                    |
| [ ARG1 OP ARG2 ]            | "OP" is one of -eq, -ne, -lt, -le, -gt or -ge. These arithmetic binary operators return true if "ARG1" is equal to, not equal to, less than, less than or equal to, greater than, or greater than or equal to "ARG2", respectively. "ARG1" and "ARG2" are integers. |

**Combining expressions**:

|Operation|Effect|
|:--|:--|
|[ ! EXPR ]|True if **EXPR** is false.|
|[ ( EXPR ) ]|Returns the value of **EXPR**. This may be used to override the normal precedence of operators.|
|[ EXPR1 -a EXPR2 ]|True if both **EXPR1** and **EXPR2** are true.|
|[ EXPR1 -o EXPR2 ]|True if either **EXPR1** or **EXPR2** is true.|

Example check if file exists:
```bash
FILE=/etc/resolv.conf
if [ -f "$FILE" ]; then
    echo "$FILE exists."
fi
```