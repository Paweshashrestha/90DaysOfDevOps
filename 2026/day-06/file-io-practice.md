# Day 06 – File Read/Write Practice

Practice note for basic file I/O using redirection, `cat`, `head`, `tail`, and `tee`.

---

## File created: `notes.txt`

Created in this folder with the following flow.

---

## Commands run

### 1. Create empty file
```bash
touch notes.txt
```
Creates `notes.txt` if it doesn’t exist; updates its timestamp if it does.

### 2. Write first line (overwrite)
```bash
echo "Line 1" > notes.txt
```
`>` redirects stdout to the file and **replaces** existing content. After this, the file contains only `Line 1`.

### 3. Append second line
```bash
echo "Line 2" >> notes.txt
```
`>>` appends to the file. File now has Line 1 and Line 2.

### 4. Append and display (tee)
```bash
echo "Line 3" | tee -a notes.txt
```
`tee -a` writes stdin to the file (**-a** = append) and also prints it. File has three lines; "Line 3" appears on the terminal.

### Command:

```
echo "Line 3" | tee -a notes.txt
```

## What does **`tee`** mean?

`tee` is a command that:

> **Takes input and sends it to two places at the same time.**

1. Shows it on the screen
2. Writes it to a file

---

### Why is it called `tee`?

Because it works like a **T-shaped pipe**:

```
        → Screen
Input ──┤
        → File
```

---

## In your example:

```
echo "Line 3" | tee -a notes.txt
```

Step by step:

1. `echo "Line 3"` → creates text
2. `|` → sends it to `tee`
3. `tee -a notes.txt`:

   * Shows `Line 3` on terminal
   * Appends (`-a`) it to `notes.txt`

---

## What does `-a` mean?

`-a` = **append**

Without `-a`:

* It **overwrites** the file

With `-a`:

* It **adds to the end** of the file

---

### Simple Summary

`tee` = **display + save at the same time**

### 5. Read full file
```bash
cat notes.txt
```
Prints the entire file. Output:
```
Line 1
Line 2
Line 3
```

### 6. Read first 2 lines
```bash
head -n 2 notes.txt
```
Prints only the first 2 lines. Useful for peeking at the top of logs or configs.

### 7. Read last 2 lines
```bash
tail -n 2 notes.txt
```
Prints only the last 2 lines. Commonly used for recent log entries (e.g. `tail -f` to follow a log).

---

## Summary

| Redirection / command | Effect |
|-----------------------|--------|
| `>` | Overwrite file with output |
| `>>` | Append output to file |
| `tee -a` | Append to file and show output |
| `cat` | Read and print full file |
| `head -n N` | First N lines |
| `tail -n N` | Last N lines |

Run these in `2026/day-06/` so that `notes.txt` is in the same directory.
