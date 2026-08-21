
# AWK 
AWK is a Linux command used to read, filter, search, calculate, and modify text data.

## 1. Basic AWK Syntax
```bash
awk 'pattern { action }' filename
```

Example:

```bash
awk '{print $1}' file.txt
```

* `$1` → first field
* `$2` → second field
* `$0` → complete line
* If no pattern is given, action runs for every line.

---

## 2. Print Specific Columns
Suppose `file.txt` contains:

```text
101 Rahul Developer 55000 Bangalore
102 Priya Tester 48000 Chennai
103 Aman Manager 75000 Mumbai
```

Command:

```bash
awk '{print $2,$3}' file.txt
```

Output:

```text
Rahul Developer
Priya Tester
Aman Manager
```

Use `$1`, `$2`, `$3` etc. to access different columns.

---

## 3. Print All Lines

```bash
awk '{print $0}' file.txt
```

`$0` represents the complete current line.

This is also the default action:

```bash
awk '{print}' file.txt
```

---

## 4. Print Lines Based on Condition

Print employees whose salary is greater than 50000:

```bash
awk '$4 > 50000 {print $2,$4}' file.txt
```

Output:

```text
Rahul 55000
Aman 75000
```

Here:

* `$4` → salary
* `> 50000` → condition
* `$2` → employee name

---

## 5. BEGIN and END

```bash
awk 'BEGIN{print "Employee List"}
{print $2}
END{print "Completed"}' file.txt
```

`BEGIN` runs before reading the file.

The main block runs for every record.

`END` runs after all records are processed.

---

## 6. FS — Input Field Separator

If data is separated by `:`:

```text
101:Rahul:Developer:55000
102:Priya:Tester:48000
```

Command:

```bash
awk 'BEGIN{FS=":"} {print $2,$3}' file.txt
```

Output:

```text
Rahul Developer
Priya Tester
```

`FS` tells AWK how to split input fields.

---

## 7. OFS — Output Field Separator

```bash
awk 'BEGIN{OFS="-"} {print $2,$3,$4}' file.txt
```

Output:

```text
Rahul-Developer-55000
Priya-Tester-48000
Aman-Manager-75000
```

`OFS` controls what is placed between fields in `print`.

---

## 8. NR and NF

### NR — Record Number

```bash
awk '{print NR,$2}' file.txt
```

Output:

```text
1 Rahul
2 Priya
3 Aman
```

`NR` gives the current line/record number.

### NF — Number of Fields

```bash
awk '{print NF}' file.txt
```

If every line has 5 fields:

```text
5
5
5
```

`NF` tells how many fields the current record contains.

---

## 9. Search Using Regular Expressions

Find employees whose name starts with `R` or `K`:

```bash
awk '$2 ~ /^[RK]/ {print $2}' file.txt
```

Important regex symbols:

```text
^       starts with
$       ends with
[RK]    R or K
.       any character
*       zero or more
?       optional
```

Example:

```bash
awk '/Rahul/ {print $0}' file.txt
```

Prints lines containing `Rahul`.

---

## 10. Arithmetic and Calculations

Calculate yearly salary:

```bash
awk '{print $2,$4*12}' file.txt
```

Calculate salary after 10% deduction:

```bash
awk '{print $2,$4-($4*10/100)}' file.txt
```

Common operators:

```text
+   Addition
-   Subtraction
*   Multiplication
/   Division
%   Remainder
^   Power
```

---

## 11. if-else

```bash
awk '{
    if($4 > 50000)
        print $2,"High Salary"
    else
        print $2,"Low Salary"
}' file.txt
```

Output:

```text
Rahul High Salary
Priya Low Salary
Aman High Salary
```

Use `if-else` when different actions are required for different conditions.

---

## 12. Counting and Associative Arrays

Count employees based on their role:

```bash
awk '{roles[$3]++}
END {
    for(role in roles)
        print role,roles[role]
}' file.txt
```

Example output:

```text
Developer 1
Tester 1
Manager 1
```

`roles[$3]` uses the value of `$3` as the array key.

This is useful for **counting, grouping, and summarizing data**.

---

## Useful AWK Commands to Practice

```bash
# Print first column
awk '{print $1}' file.txt

# Print second and third columns
awk '{print $2,$3}' file.txt

# Print line number
awk '{print NR,$0}' file.txt

# Print number of fields
awk '{print NF}' file.txt

# Filter by salary
awk '$4 > 50000 {print $2}' file.txt

# Filter by name
awk '$2 == "Rahul" {print $0}' file.txt

# Calculate yearly salary
awk '{print $2,$4*12}' file.txt

# Count records
awk 'END{print NR}' file.txt

# Search text
awk '/Rahul/ {print $0}' file.txt

# Change output separator
awk 'BEGIN{OFS=","} {print $1,$2,$3}' file.txt
```


