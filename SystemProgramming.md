# 시스템프로그래밍

## Basic Shell Commands

### Information

- `man` : gives information about command
- `apropos, whatis` : similar to  `man`
- `which` : shows fulll path of command
- `stat` : File and Inode information
- `uname` : print system information

### User Specific
- `useradd` : create user
- `userdel` : delete user
- `passwd` : change or create user password
- `who` : to find out who is logged in
- `whoami` : who are you

### Remote Access
- `ssh` : (secured login) is a program for logging into a remote machine and for executing commands on a remote machine.
- `scp` : (secured copy) copies files between hosts on a network.

### File System related

- `mount` : Mounting filesystem
- `find, locate` : Search for files
- `cat, less, head, tail` : used to view text files
- `touch` : create and update files
- `wc` : counts the number of lines in a file
	- `wc ~.c | awk '{ print $2}'` 
- `mkdir` : make directory
	- `mkdir -p` : 디렉토리 안의 디렉토리도 함께 만듦

### Pattern Matching
- `grep` is pattern matching tool used to search the name input file. Basically its used for lines matching a pattern

- `ls | grep *.c`

## Files

### Types

- Directory
- Block
- Character
- Soft Link
- FIFO
- Plain Text
- Socket

### Permission Modification

- `chmod` : Change file permissions
- `chown` : Change file owner
- `chown -R user:group`

## I/O Redirection

### Output

- Output redirection ( > )
- Redirecting to append ( >> )
- Redirecting the error ( 2> )

- 	`ls > /tmp/output_file`
	`ls -l >> /tmp/output_file`
	`ls 2> /tmp/output_file`
### Pipe

- A pipe is a form of redirection that is used in Linux operating systems to send the output of one program to another program for further processing.
- `ls -al /bin | less`

## Escape / Command Mode

- Search mode
	- `:/<string>`
- File mode
	- `:%s/first/sec` : Replaces the first by second every where 	in the file
	- `:%s/orange/apple/g` For all lines in a file, find string "orange" and replace with string "apple" for each instance on a line

## Makefile 

```makefile
foo: foo1.o foo2.o foo3.o foo4.o
	gcc -o foo foo1.o foo2.o foo3.o foo4.o

foo1.o: foo1.c gleep2.h gleep3.h
	gcc -c foo1.c
    
foo2.o: foo2.c gleep1.h
	gcc -c foo2.c
    
foo3.o: foo3.c gleep1.h gleep2.h
	gcc -c foo3.c
    
foo4.o: foo4.c gleep3.h
	gcc -c foo4.c
    
clean: 
	rm -f foo foo1.o foo2.o foo3.o foo4.o
    
install: foo
	mkdir -p /usr/local/bin
    	rm -f /usr/local/bin/foo
    	cp hello /usr/local/bin/foo
```

```makefile
CC = gcc
CFLAGS = -Wall -g
OBJECTS = foo1.o foo2.o foo3.o foo4.o
PREFIX = /usr/local
.SUFFIXES: .c.o
.c.o:
	$(CC) $(CFLAGS) -c $<
    
foo: $(OBJECTS)
	$(CC) $(CFLAGS) $^ -o $@
    
clean: rm -rf $(OBJECTS)

install: foo
	mkdir -p $(PREFIX)/bin
    	rm -f $(PREFIX)/bin/foo
        cp foo $(PREFIX)/bin/foo
```

### Read

```c
int fd = 0;
char line[100],  *cp = line;
struct E { char name[20]; char ssn[9]; } e;
size_t n = 15;

//using initialized variables
read(fd, cp, n);
//using constants
read(0, line, 20);
//another example with constants
read(0, (void *) &e, sizeof(struct E));
```

### Error

```c
//perror 함수를 이용한 에러메시지 출력
void perror(const char* s);
//strerror 함수를 이용한 에러메시지 출력
char *strerror(int errnum);
```

### Time

```c
#include<time.h>
#include<stdio.h>

int main() {
	time_t curr;
    struct tm *d;
    curr = time(NULL);
    d = localtime(&curr);
    d->tm_sec //초 (0-59)
    d->tm_min //분 (0-59)
    d->tm_hour //시 (0-23)
    d->tm_mday //날짜 (1-31)
    d->tm_mon //월 (0-11)
    d->tm_year //1900년 이후의 연도 수 (1900 - ing)
    d->tm_wday //day of week (0 = Sunday)
    d->tm_yday //day of year (0-365)
    d->tm_isdst //일광절약시간 범위
    d->tm_zone //타임존 이름
    d->tm_gmtoff //offset from UTC in seconds
}
```
### Character Strings

```c
#include<ctype.h>
isalpha() isspace() isuppper() islower() isprint() isdigit()
isgraph() isalnum() isascii() ispunct() iscntrl()
toupper()

#include<stdlib.h>
atol() atoi() atof() strtol() strtod()

#include<stdio.h>
Sprintf() sscanf()
```

### Memory-to-Memory Copy

```c
void *memcpy(void *dst, const void *src, size_t n);
void *memccpy(void *dst, const void *src, int c, size_t n);

int memcmp(const void *s1, const void *s2, size_t n);

void *memchr(const void *s, int c, size_t n);

void *memset(void *b, int c, size_t len);
```
