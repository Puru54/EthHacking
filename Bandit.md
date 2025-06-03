
```
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

---
# Bandit Level 0

The goal of this level is for you to log into the game using SSH. The host to which you need to connect is **bandit.labs.overthewire.org**, on port 2220. The username is **bandit0** and the password is **bandit0**. Once logged in, go to the [Level 1](https://overthewire.org/wargames/bandit/bandit1.html) page to find out how to beat Level 1.

### **Goal:**
Log into the game using SSH.

### **Command:**
```sh
ssh bandit0@bandit.labs.overthewire.org -p 2220
```


![[Pasted image 20250211121506.png]]

Level password: 
```
ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```



---
# Bandit Level 1 → Level 2

## **Commands you may need to solve this level

[ls](https://manpages.ubuntu.com/manpages/noble/man1/ls.1.html) , [cd](https://manpages.ubuntu.com/manpages/noble/man1/cd.1posix.html) , [cat](https://manpages.ubuntu.com/manpages/noble/man1/cat.1.html) , [file](https://manpages.ubuntu.com/manpages/noble/man1/file.1.html) , [du](https://manpages.ubuntu.com/manpages/noble/man1/du.1.html) , [find](https://manpages.ubuntu.com/manpages/noble/man1/find.1.html)

## **Helpful Reading Material

- [Google Search for “dashed filename”](https://www.google.com/search?q=dashed+filename)
- [Advanced Bash-scripting Guide - Chapter 3 - Special Characters](https://linux.die.net/abs-guide/special-chars.html)

### **Goal:**
The password for the next level is stored in a file called `-` located in the home directory.

### **Commands Used:**
```sh
ls -la
cat ./-
```

### **Level Password:**
```
ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```

![[Pasted image 20250211122148.png]]

---
# Bandit Level 2 → Level 3

## **Commands you may need to solve this level

[ls](https://manpages.ubuntu.com/manpages/noble/man1/ls.1.html) , [cd](https://manpages.ubuntu.com/manpages/noble/man1/cd.1posix.html) , [cat](https://manpages.ubuntu.com/manpages/noble/man1/cat.1.html) , [file](https://manpages.ubuntu.com/manpages/noble/man1/file.1.html) , [du](https://manpages.ubuntu.com/manpages/noble/man1/du.1.html) , [find](https://manpages.ubuntu.com/manpages/noble/man1/find.1.html)

## **Helpful Reading Material

- [Google Search for “spaces in filename”](https://www.google.com/search?q=spaces+in+filename)

### **Goal:**
The password for the next level is stored in a file called `spaces in this filename` located in the home directory.

### **Commands Used:**
```sh
ls -la
cat "spaces in this filename"
```

### **Level Password:**
```
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```

![[Pasted image 20250211122505.png]]


---
# Bandit Level 3 → Level 4


## **Commands you may need to solve this level

[ls](https://manpages.ubuntu.com/manpages/noble/man1/ls.1.html) , [cd](https://manpages.ubuntu.com/manpages/noble/man1/cd.1posix.html) , [cat](https://manpages.ubuntu.com/manpages/noble/man1/cat.1.html) , [file](https://manpages.ubuntu.com/manpages/noble/man1/file.1.html) , [du](https://manpages.ubuntu.com/manpages/noble/man1/du.1.html) , [find](https://manpages.ubuntu.com/manpages/noble/man1/find.1.html)

### **Goal:**
The password for the next level is stored in a **hidden file** in the `inhere` directory.

### **Commands Used:**
```sh
ls -la inhere
cat inhere/.hidden
```

### **Level Password:**
```
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```

![[Pasted image 20250211122744.png]]

---

# Bandit Level 4 → Level 5


## **Commands you may need to solve this level

[ls](https://manpages.ubuntu.com/manpages/noble/man1/ls.1.html) , [cd](https://manpages.ubuntu.com/manpages/noble/man1/cd.1posix.html) , [cat](https://manpages.ubuntu.com/manpages/noble/man1/cat.1.html) , [file](https://manpages.ubuntu.com/manpages/noble/man1/file.1.html) , [du](https://manpages.ubuntu.com/manpages/noble/man1/du.1.html) , [find](https://manpages.ubuntu.com/manpages/noble/man1/find.1.html)

### **Goal:**
The password for the next level is stored in the **only human-readable** file in the `inhere` directory.

### **Commands Used:**
```sh
cd inhere
ls inhere
file ./*
cat ./-file07
```

### **Level Password:**
```
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
```


![[Pasted image 20250211123052.png]]

---

# Bandit Level 5 → Level 6


## **Commands you may need to solve this level

[ls](https://manpages.ubuntu.com/manpages/noble/man1/ls.1.html) , [cd](https://manpages.ubuntu.com/manpages/noble/man1/cd.1posix.html) , [cat](https://manpages.ubuntu.com/manpages/noble/man1/cat.1.html) , [file](https://manpages.ubuntu.com/manpages/noble/man1/file.1.html) , [du](https://manpages.ubuntu.com/manpages/noble/man1/du.1.html) , [find](https://manpages.ubuntu.com/manpages/noble/man1/find.1.html)

### **Goal:**
The password for the next level is stored in a file **somewhere under the `inhere` directory** and has the following properties:
- **Human-readable**
- **1033 bytes in size**
- **Not executable**

### **Commands Used:**
```sh
find . -type f -size 1033c ! -executable
cat ./maybehere07/.file2
```

### **Level Password:**
```
HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
```

![[Pasted image 20250211123500.png]]

---

# Bandit Level 6 → Level 7


## **Commands you may need to solve this level

[ls](https://manpages.ubuntu.com/manpages/noble/man1/ls.1.html) , [cd](https://manpages.ubuntu.com/manpages/noble/man1/cd.1posix.html) , [cat](https://manpages.ubuntu.com/manpages/noble/man1/cat.1.html) , [file](https://manpages.ubuntu.com/manpages/noble/man1/file.1.html) , [du](https://manpages.ubuntu.com/manpages/noble/man1/du.1.html) , [find](https://manpages.ubuntu.com/manpages/noble/man1/find.1.html) , [grep](https://manpages.ubuntu.com/manpages/noble/man1/grep.1.html)

### **Goal:**
The password for the next level is stored **somewhere on the server** and has the following properties:
- **Owned by user `bandit7`**
- **Owned by group `bandit6`**
- **33 bytes in size**

### **Commands Used:**
```sh
find /var -user bandit7 -group bandit6 -size 33c 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password

```

### **Level Password:**
```
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj

```
 ![[Pasted image 20250224111300.png]]


# Bandit Level 7 → Level 8

### **Goal:**
The password for the next level is stored in a file called `data.txt`, but the file is very large.

### **Solution:**
Checking the file size of `data.txt`, we can see it is huge:
```sh
bandit7@bandit:~$ du -b data.txt 
4184396 data.txt
```
Since the password is in the same line as the word **'millionth'**, we use `grep` to find it quickly:
```sh
bandit7@bandit:~$ cat data.txt | grep millionth
millionth       cvX2JJa4CFALtqS87jk27qwqGhBM9plV
```

### **Level Password:**
```
dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

![[Pasted image 20250224112117.png]]



---

# Bandit Level 8 → Level 9

### **Goal:**
The password for the next level is stored in the file `data.txt` and is the only line of text that occurs **only once**.

### **Solution:**
To find the unique line, we first sort the lines and then filter for the unique one:
```sh
bandit8@bandit:~$ sort data.txt | uniq -u
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
```

### **Level Password:**
```
4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```

![[Pasted image 20250224112338.png]]



---

# Bandit Level 9 → Level 10

### **Goal:**
The password for the next level is stored in the file `data.txt` in one of the few human-readable strings, preceded by several `=` characters.

### **Solution:**
The `strings` command finds human-readable text in files. We use it here to filter out the password:
```sh
strings data.txt | grep ===

```

### **Level Password:**
```
FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```

![[Pasted image 20250224112809.png]]

---

# Bandit Level 10 → Level 11

### **Goal:**
The password for the next level is stored in the file `data.txt`, which contains base64 encoded data.

### **Solution:**
Base64 is a binary-to-text encoding scheme. We can decode the file using the `base64` command:
```sh
cat data.txt
base64 -d data.txt

```

### **Level Password:**
```
dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```
![[Pasted image 20250224113108.png]]
