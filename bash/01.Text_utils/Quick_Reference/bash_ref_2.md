# Bash Syntax Quick Reference

## 1. Variables & Types

```bash
# Variables (no spaces around '=')
NAME="World"
COUNT=42
READONLY_VAR="Immutable" ; readonly READONLY_VAR

# Accessing Variables
echo "$NAME"          # Good practice: always quote variables
echo "${NAME}_suffix" # Braces clarify variable boundary

# Arrays
INDEXED_ARR=("zero" "one" "two")
echo "${INDEXED_ARR[0]}"  # Access element
echo "${INDEXED_ARR[@]}"  # All elements
echo "${#INDEXED_ARR[@]}" # Array length

declare -A ASSOC_ARR      # Associative Array (Bash 4+)
ASSOC_ARR=([key1]="val1" [key2]="val2")
echo "${ASSOC_ARR[key1]}"

# Special Parameters
$0          # Script name
$1, $2...   # Positional command-line arguments
$#          # Number of command-line arguments
$@          # All arguments (as separate words when quoted)
$?          # Exit status of the last executed command
dd$$          # PID of current shell```
```

## 2. Parameter Expansion
```bash
${VAR:-default}   # If VAR is unset/empty, use default
${VAR:=default}   # If VAR is unset/empty, set VAR to default
${VAR:?error}     # Exit with 'error' if VAR is unset/empty
${#VAR}           # Length of string VAR```

### String Manipulation
```bash
${VAR#pattern}    # Remove shortest matching prefix
y ${VAR##pattern}   # Remove longest matching prefix
y ${VAR%pattern}    # Remove shortest matching suffix
y ${VAR%%pattern}   # Remove longest matching suffix
y ${VAR/search/replace}  # Replace first match
y ${VAR//search/replace} # Replace all matches```
```

## 3. Conditionals & Test Operators 
Use `[[ ... ]]` over `[ ... ]` in modern Bash for advanced pattern matching and safer handling.
```bash 
# String Comparisons 
[[ "$a" == "$b" ]]  # Equal 
[[ "$a" != "$b" ]]  # Not equal 
[[ -z "$a" ]]      # True if string is empty 
[[ -n "$a" ]]      # True if string is NOT empty 
 
# Integer Comparisons 
[[ "$a" -eq "$b" ]] // Equal (also: -ne, -lt, -le, -gt, -ge) 
ext (( a == b ))        // Alternative arithmetic comparison syntax 
 
# File Tests 
[[ -f "$file" ]]    // True if file exists and is a regular file 
[[ -d "$dir" ]]     // True if directory exists 
[[ -e "$path" ]]    // True if path exists 
[[ -r "$file" ]]    // True if readable 
[[ -w "$file" ]]    // True if writable 
[[ -x "$file" ]]    // True if executable 
 
# Boolean Operators 
[[ cond1 && cond2 ]] // AND 
[[ cond1 || cond2 ]] // OR 
d ! [[ cond ]]         // NOT```
```

# 4. Control Flow

## If / Else
**Bash**
```bash
if [[ -f "$file" ]]; then
    echo "File exists"
elif [[ -d "$file" ]]; then
    echo "It's a directory"
else
    echo "Path does not exist"
fi

## Loops

### For Loop (List)
**Bash**
for ITEM in "apple" "banana" "cherry"; do
    echo "$ITEM"
done

### C-style For Loop
**Bash**
for ((i=0; i<5; i++)); do
    echo "$i"
done

### While Loop
**Bash**
while [[ "$COUNT" -gt 0 ]]; do
    ((COUNT--))
done

### Read File Line by Line
```bash
while IFS= read -r line; do
    echo "$line"
done < input.txt

## Case Statement 
**Bash**
ecase "$VAR" in
default|boot)
        echo "Starting..."
        ;;
stop)
        echo "Stopping..."
        ;;*)
        echo "Unknown command"
        ;;
esac```

# 5. Functions & Arithmetic 
```bash
### Function Declaration
my_func() {
    local LOCAL_VAR="$1"  # Always scope function variables with 'local'
    echo "Hello, $LOCAL_VAR"
    return 0             # Returns exit status code (0-255)
}

### Invoking Function
my_func "Alice"

### Arithmetic Evaluation
RESULT=$(( 5 + 3 * 2 ))  # Integer arithmetic only
(( COUNT++ ))            # Increment
```

# 6. Redirection & Pipes
```bash
command > file.txt    # Redirect stdout (overwrite)
command >> file.txt   # Redirect stdout (append)
command 2> error.log  # Redirect stderr
command > file 2>&1   # Redirect stdout and stderr to file
command &> file.txt   # Short syntax for stdout and stderr combined
command < file.txt    # Redirect file into stdin

### Process Redirection & Piping
cmd1 | cmd2           # Pass stdout of cmd1 as stdin to cmd2
diff <(cmd1) <(cmd2)  # Process substitution (treats cmd output as file)
```

# 7. Command Execution & Subshell
```bash
### Command Substitution
OUTPUT=$(date +%Y-%m-%d) # Capture output of command

### Execution Chaining
cmd1 && cmd2   # Run cmd2 ONLY if cmd1 succeeds (exit code 0)
cmd1 || cmd2   # Run cmd2 ONLY if cmd1 fails (non-zero exit code)
cmd1 ; cmd2    # Run sequentially regardless of success

### Subshell
( cd /tmp && touch file ) # Runs in isolated subshell environment
```









