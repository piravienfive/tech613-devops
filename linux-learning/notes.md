
# Linux Beginner Notes – File Ownership, Permissions and Streams

These notes cover:
- File ownership
- File permissions
- Permission numbers
- Changing permissions
- Linux streams
- Redirection and appending

---

# Part 1 – File Ownership

## 1. Why is managing file ownership important?

File ownership keeps files safe.

It controls who can:
- Read files
- Change files
- Run files

It helps stop people from accessing files they shouldn't.

## 2. What command shows file ownership?

```bash
ls -l
```

This shows the file owner, group and permissions.

Example:

```text
-rw-r--r-- 1 ubuntu ubuntu 100 test.txt
```

The first `ubuntu` is the owner.

The second `ubuntu` is the group.

## 3. What permissions does a new file or directory have?

Usually, if the `umask` is `022`:

### New file: 644

- Owner = Read + Write
- Group = Read
- Other = Read

### New directory: 755

- Owner = Read + Write + Execute
- Group = Read + Execute
- Other = Read + Execute

The file normally belongs to the user who created it.

The group is normally the user's main group.

To check the default permission setting:

```bash
umask
```

## 4. Why does a new file not have execute (X) permission?

For security reasons.

Not every file is a program.

Linux does not automatically make new files executable.

## 5. What command changes the owner of a file?

```bash
chown
```

Example:

```bash
sudo chown john testfile.txt
```

This changes the owner to `john`.

To change a folder and everything inside:

```bash
sudo chown -R john myfolder
```

---

# Part 2 – Understanding File Permissions

## 1. Does owning a file mean you have full permissions?

**No.**

Being the owner does not automatically mean you can do everything.

For example:

```text
-r--r--r--
```

The owner can only read this file.

They cannot write to it or execute it using its current permissions.

However, the owner can normally change the permissions using `chmod`.

## 2. What does User permission mean?

**User (u)** means the owner of the file.

Example:

```bash
chmod u+r testfile.txt
```

This gives the owner read permission.

## 3. What does Group permission mean?

**Group (g)** means users who belong to the file's group.

Example:

```bash
chmod g+w testfile.txt
```

This gives the group write permission.

## 4. What does Other permission mean?

**Other (o)** means everyone else.

These are users who are not the file owner and are not covered by the file's group permissions.

Example:

```bash
chmod o+r testfile.txt
```

This gives other users read permission.

## 5. If User has read-only, Group has read/write, and Other has full permissions, what can the owner do?

**Answer: Read only.**

Example:

```text
-r--rw-rwx
```

Permissions:

- User = Read only
- Group = Read + Write
- Other = Read + Write + Execute

If you own the file, Linux uses the **User permissions**.

Even if you belong to the group, you still use the owner's permissions.

So you can only read the file under its current permissions.

You can normally use `chmod` to change those permissions because you own the file.

## 6. How do you understand an `ls -l` result?

Example:

```text
-rw-r--r-- 1 ubuntu ubuntu 100 test.txt
```

Explanation:

| Part | Meaning |
|------|---------|
| First `-` | Regular file |
| `rw-` | Owner can read and write |
| `r--` | Group can read |
| `r--` | Other users can read |
| `1` | Number of hard links |
| First `ubuntu` | File owner |
| Second `ubuntu` | File group |
| `100` | File size in bytes |
| `test.txt` | File name |

If the first character is `d`, it means directory.

Example:

```text
drwxr-xr-x
```

This is a directory with permissions `755`.

---

# Part 3 – Permission Numbers

## 1. What numbers are used for permissions?

| Permission | Number |
|------------|--------|
| Read (r) | 4 |
| Write (w) | 2 |
| Execute (x) | 1 |
| No permission (-) | 0 |

## 2. What number means read + write?

**Answer: 6**

4 + 2 = 6

## 3. What number means read + write + execute?

**Answer: 7**

4 + 2 + 1 = 7

## 4. What number means read + execute?

**Answer: 5**

4 + 1 = 5

## 5. What does 644 mean?

```text
644 = rw-r--r--
```

- 6 = User can read and write
- 4 = Group can read
- 4 = Other can read

Remember:

First number = User

Second number = Group

Third number = Other

---

# Part 4 – Changing File Permissions

## 1. What command changes permissions?

```bash
chmod
```

`chmod` means change mode.

## 2. Who can change file permissions?

Two answers:

1. The file owner.
2. The root user (administrator).

A user with `sudo` access can run the command as root.

## 3. Create a file called testfile.txt

```bash
touch testfile.txt
```

## 4. Give User read, Group read/write/execute, and Other read/write

Using numbers:

```bash
chmod 476 testfile.txt
```

Using letters:

```bash
chmod u=r,g=rwx,o=rw testfile.txt
```

Meaning:

- User = Read (4)
- Group = Read + Write + Execute (7)
- Other = Read + Write (6)

## 5. Add execute permission to everyone

```bash
chmod a+x testfile.txt
```

- `a` = All
- `+` = Add
- `x` = Execute

## 6. Remove write permission from Group

```bash
chmod g-w testfile.txt
```

- `g` = Group
- `-` = Remove
- `w` = Write

## 7. Give User read/write, Group read, and Other no access

```bash
chmod 640 testfile.txt
```

Meaning:

- User = Read + Write (6)
- Group = Read (4)
- Other = Nothing (0)

---

# Part 5 – Linux Streams

## 1. What is a stream in Linux?

A stream is a way that information moves into or out of a command.

For example:
- You type something into a command.
- A command prints a result.
- A command prints an error.

Linux uses streams to handle this information.

## 2. What are the 3 data streams?

| Stream | Number | Meaning |
|--------|--------|---------|
| stdin | 0 | Standard input |
| stdout | 1 | Normal output |
| stderr | 2 | Error output |

### Easy way to remember

- **0 = Input**
- **1 = Normal output**
- **2 = Error output**

## 3. Create a file and test the streams

First, create a file:

```bash
touch new.txt
```

Now run:

```bash
ls missing_directory new.txt
```

Make sure `missing_directory` does not exist.

You should see something similar to:

```text
ls: cannot access 'missing_directory': No such file or directory
new.txt
```

What happened?

- `new.txt` goes to stdout (1).
- The error about `missing_directory` goes to stderr (2).

Both normally appear in your terminal.

## 4. How do you send normal output to a file?

Use:

```bash
ls missing_directory new.txt > output.txt
```

This sends normal output (stdout) to `output.txt`.

The error still appears in the terminal.

You can also write:

```bash
ls missing_directory new.txt 1> output.txt
```

The `1` means stdout.

## 5. How do you send errors to a file?

Use:

```bash
ls missing_directory new.txt 2> errors.txt
```

This sends errors (stderr) to `errors.txt`.

Normal output still appears in the terminal.

The `2` means stderr.

## 6. How do you send both outputs to one file?

Use:

```bash
ls missing_directory new.txt > all.txt 2>&1
```

This sends normal output and errors to `all.txt`.

A shorter way in Bash is:

```bash
ls missing_directory new.txt &> all.txt
```

Both do the same thing.

## 7. Which stream goes to a file by default?

**Standard output (stdout), stream 1.**

When you use `>` without a stream number, Linux redirects stdout.

Example:

```bash
ls > output.txt
```

Only normal output goes to the file.

Errors still appear in the terminal.

---

# Part 6 – Redirection and Appending

## 1. What does > do?

`>` sends normal output into a file.

It **replaces** the old content of the file.

Example:

```bash
echo "Hello" > message.txt
```

The file now contains:

```text
Hello
```

If you run:

```bash
echo "Goodbye" > message.txt
```

The file now contains:

```text
Goodbye
```

The word `Hello` is gone because `>` overwrote it.

If the file does not exist, `>` normally creates it.

## 2. What is appending?

Appending means **adding new information to the end of a file**.

It does not delete the old content.

We use:

```text
>>
```

## 3. Example of appending to a file

First:

```bash
echo "Hello" > message.txt
```

Then:

```bash
echo "World" >> message.txt
```

The file now contains:

```text
Hello
World
```

Both lines are saved.

## 4. Example using ls

```bash
ls >> msg.txt
```

This adds the output of `ls` to the end of `msg.txt`.

It keeps any text already in the file.

## 5. What is the difference between > and >>?

| Symbol | Meaning |
|--------|---------|
| `>` | Replace the file content |
| `>>` | Add to the end of the file |
| `2>` | Send errors to a file, replacing its content |
| `2>>` | Add errors to the end of a file |
| `&>` | Send normal output and errors to one file (Bash) |

---

# Part 7 – Simple Practice Commands

Try these commands on your Linux VM.

## Practice 1 – Check your username

```bash
whoami
```

Shows your current username.

## Practice 2 – Create a file

```bash
touch testfile.txt
```

## Practice 3 – Check permissions

```bash
ls -l testfile.txt
```

## Practice 4 – Change permissions

```bash
chmod 644 testfile.txt
```

## Practice 5 – Check again

```bash
ls -l testfile.txt
```

You should see:

```text
-rw-r--r--
```

## Practice 6 – Add execute permission

```bash
chmod a+x testfile.txt
```

## Practice 7 – Check again

```bash
ls -l testfile.txt
```

You should see:

```text
-rwxr-xr-x
```

## Practice 8 – Test output redirection

```bash
echo "Hello Linux" > practice.txt
```

Check the file:

```bash
cat practice.txt
```

## Practice 9 – Test appending

```bash
echo "I am learning Linux" >> practice.txt
```

Check again:

```bash
cat practice.txt
```

Expected output:

```text
Hello Linux
I am learning Linux
```

## Practice 10 – Test error redirection

```bash
ls missing_directory 2> errors.txt
```

Read the errors:

```bash
cat errors.txt
```

You should see an error saying the directory does not exist.

---

# Quick Revision Cheat Sheet

## File Ownership and Permissions

| Command | Meaning |
|---------|---------|
| `ls -l` | Show file permissions and ownership |
| `ls -ld folder` | Show folder permissions and ownership |
| `chown` | Change file owner |
| `chmod` | Change file permissions |
| `umask` | Show default permission restrictions |
| `touch` | Create an empty file |
| `mkdir` | Create a folder |
| `whoami` | Show current username |

## Permission Numbers

| Number | Meaning |
|--------|---------|
| 0 | No permission |
| 1 | Execute |
| 2 | Write |
| 4 | Read |
| 5 | Read + Execute |
| 6 | Read + Write |
| 7 | Read + Write + Execute |

## Permission Letters

| Letter | Meaning |
|--------|---------|
| u | User (owner) |
| g | Group |
| o | Other |
| a | All |
| r | Read |
| w | Write |
| x | Execute |
| + | Add permission |
| - | Remove permission |
| = | Set exact permissions |

## Streams and Redirection

| Symbol | Meaning |
|--------|---------|
| 0 | Standard input |
| 1 | Standard output |
| 2 | Standard error |
| `>` | Overwrite a file with normal output |
| `>>` | Append normal output |
| `2>` | Redirect errors |
| `2>>` | Append errors |
| `2>&1` | Send errors to the same place as normal output |
| `&>` | Redirect both outputs in Bash |

---

# Most Important Things to Remember

1. `ls -l` shows ownership and permissions.
2. `chown` changes who owns a file.
3. `chmod` changes file permissions.
4. Read = 4, Write = 2, Execute = 1.
5. `644` means owner read/write, group read, other read.
6. Being the owner does not always mean having full permissions.
7. stdin (0) is input.
8. stdout (1) is normal output.
9. stderr (2) is error output.
10. `>` replaces file content.
11. `>>` adds to the end of a file.

---

# Final Summary

Linux uses ownership and permissions to keep files safe.

The three permission groups are:

- User = File owner
- Group = Users in the file's group
- Other = Everyone else

Linux also uses three streams:

- stdin = Input
- stdout = Normal output
- stderr = Error output

Remember these commands:

```bash
ls -l
chmod 644 testfile.txt
chown john testfile.txt
echo "Hello" > message.txt
echo "World" >> message.txt
```

These are the basics of Linux file ownership, permissions, streams, redirection and appending.


---

# Quick Revision Cheat Sheet

| Command | What it does |
|---------|--------------|
| `ls -l` | Shows file ownership and permissions |
| `ls -ld folder` | Shows a folder's ownership and permissions |
| `touch testfile.txt` | Creates an empty file |
| `chmod` | Changes permissions |
| `chown` | Changes ownership |
| `umask` | Shows default permission restrictions |
| `chmod 644 file` | Owner read/write, everyone else read |
| `chmod 777 file` | Everyone has full permissions (unsafe in most cases) |
| `chmod 640 file` | Owner read/write, group read, others none |
| `chmod a+x file` | Adds execute permission for everyone |
| `chmod g-w file` | Removes write permission from group |

---

# Easy Way to Remember

## Permission Numbers

- **4 = Read** (View a file)
- **2 = Write** (Change a file)
- **1 = Execute** (Run a file)

## Add the Numbers Together

- `4 + 2 = 6` → Read + Write
- `4 + 1 = 5` → Read + Execute
- `4 + 2 + 1 = 7` → Read + Write + Execute

---

# Most Important Commands to Remember

- `chmod` = Change permissions
- `chown` = Change owner
- `ls -l` = Check ownership and permissions
- `umask` = Check default permission restrictions

**Remember:**

`644` = Owner can read/write, everyone else can only read.



# Linux Beginner Notes – Processes

## What is a Process?

A process is a program or command that is running.

Every process has a **PID (Process ID)**, which is its unique number.

---

## Viewing Processes

| Command | Purpose |
|---------|---------|
| `ps` | Show processes in your terminal |
| `ps --help simple` | Show help for ps |
| `ps -A` | Show all running processes |
| `ps aux` | Show processes with CPU and memory usage |
| `ps -ef` | Show all processes, including parent IDs |
| `top` | Watch running processes live |
| `htop` | Easier-to-read process viewer |

Press `q` to exit `top` or `htop`.

---

## The sleep Command

`sleep` makes a process wait for a set number of seconds.

```bash
sleep 3
```

Waits 3 seconds.

```bash
sleep 5000
```

Waits 5000 seconds.

Press `Ctrl + C` to stop the command early.

---

## Running Processes in the Background

Adding `&` runs a command in the background.

```bash
sleep 6000 &
```

This allows you to continue using your terminal.

Check background jobs:

```bash
jobs
```

Show jobs with their Process IDs:

```bash
jobs -l
```

---

## Stopping Processes

The `kill` command sends a signal to a process.

| Command | Purpose |
|---------|---------|
| `Ctrl + C` | Interrupt a running command |
| `kill PID` | Ask a process to stop normally |
| `kill -1 PID` | Send a hangup signal (SIGHUP) |
| `kill -9 PID` | Force a process to stop |

**PID** means the process number.

Example:

```bash
kill 1105
```

Stops process `1105` if it responds to the signal.

**Important:** Replace `1105` with your own PID.

---

## Practice – Creating and Killing Processes

### 1. Start a background process

```bash
sleep 6000 &
```

### 2. Check running jobs

```bash
jobs
```

### 3. Find the PID

```bash
jobs -l
```

### 4. Stop the process

```bash
kill PID
```

Replace `PID` with the actual number.

### 5. Check it stopped

```bash
jobs
```

---

## Practice – Killing Two Processes

### 1. Create two processes

```bash
sleep 7000 &
sleep 8000 &
```

### 2. Find their PIDs

```bash
jobs -l
```

### 3. Stop the first process normally

```bash
kill PID1
```

### 4. Force stop the second process

```bash
kill -9 PID2
```

### 5. Check both stopped

```bash
jobs
```

Replace `PID1` and `PID2` with your actual process numbers.

---

## Quick Cheat Sheet

| Command | Meaning |
|---------|---------|
| `ps` | View processes |
| `ps -A` | View all processes |
| `ps aux` | View detailed processes |
| `ps -ef` | View processes and parent IDs |
| `top` | Live process monitor |
| `htop` | Easier live process monitor |
| `sleep 3` | Wait 3 seconds |
| `&` | Run in background |
| `jobs` | View background jobs |
| `jobs -l` | View jobs and PIDs |
| `Ctrl + C` | Interrupt a command |
| `kill PID` | Stop a process normally |
| `kill -1 PID` | Send hangup signal |
| `kill -9 PID` | Force stop a process |

---

## Key Things to Remember

- **Process** = Running program
- **PID** = Process ID number
- **ps** = View processes
- **top / htop** = Monitor processes
- **sleep** = Make a process wait
- **&** = Run in background
- **jobs** = View background jobs
- **kill** = Send a signal to a process
- **Ctrl + C** = Interrupt a running command
