# Shell Expansion or Parameter Expansion

*Linux shell expansion (often called shell expansion or parameter expansion) is a core feature of shells like Bash that transforms and interprets command-line input before execution. It allows you to write concise, dynamic commands that the shell expands into full values.*

*Think of it as a preprocessing step: the shell rewrites your command by expanding variables, patterns, and expressions.*

`echo i love linux` = prints -> i love linux.

`echo 'i -----  love -----   linux'` = prints -> i   love    linux. gap preserved on shell. Replace ----- with white spaces.

`echo "i -----  love -----   linux"` = prints -> i love linux. gap omits on shell. Replace ----- with white spaces.

## Order of Shell Expansions

*The shell processes expansions in a strict sequence:*

1. Brace expansion
2. Tilde expansion
3. Parameter & variable expansion
4. Command substitution
5. Arithmetic expansion
6. Word splitting
7. Filename (glob) expansion

### 1. Brace ({}) Expansion

*Generates multiple strings from patterns.*

* `echo file{1,3,5}.txt` = file1.txt file3.txt file5.txt. Selected expansion
* `echo file{1..3}.txt` = file1.txt file2.txt file3.txt. Range expansion
* `echo {a..e}` = a b c d e . Range expansion

### 2. Tilde (~) Expansion

* `cd ~` = Expands to your home directory (e.g., /home/sakil)
* `cd ~root` = Root user's home directory

### 3. Parameter / Variable ($) Expansion

***Example: name=Sakil***

```valueOfParameter
echo $name = prints -> sakil
echo "$name" = prints -> sakil
echo "${name}" = prints -> sakil
above 3 statements prints same result. 
```

`echo '$name'` = prints -> $name.

`echo ${#name}` = prints length of value.

### 4. Command $( ) Substitution

*Executes a command and replaces it with output.*

`echo "Today is $(date)"` = Inserts current date

### 5. Arithmetic $(( )) Expansion

`echo $((5 + 3))` = 8

```sumOfVariables
a=10
b=5
echo $((a + b)) =  15
```

### 6. Filename Expansion

* `ls *.txt` = Lists all .txt files.
* `ls *.docx` = Lists all .docx files.

```fileNameExpansion
Pattern Match
-------------
*     -> matches everything
?     -> single character
[abc] -> matches a, b, or c
```

#### Combined Example

```combinedExample
echo file_{1..3}_$(date +%Y).txt

Step-by-step:
    {1..3} -> 1 2 3
    $(date +%Y) -> current year

Final:
file_1_2026.txt file_2_2026.txt file_3_2026.txt
```

## Main Command Types in Linux

*In a Linux shell (especially Bash), command types refer to how the shell classifies and resolves a command you type. This matters because the shell searches and executes them in a specific priority order.*

### 1. Built-in Commands (Shell Builtins)

*These are implemented inside the shell itself—no external binary is needed.*

***Characteristics:***

* Faster (no process spawn)
* Always available inside the shell
* Can modify shell state (e.g., cd, export)

***`type` command checks the command type***

* `type cd` = cd is a shell builtin
* `type type` = type is a shell builtin
* `type echo` = echo is a shell builtin
* `help` = Check all builtins

### 2. External Commands (Binary Executables)

*These are actual programs stored on disk, usually in directories like:*

* /bin
* /usr/bin
* /usr/local/bin

***Examples:***

* `type ls` = ls is aliased to ls --color=auto
* `whereis ls` = ls: /usr/bin/ls /usr/share/man/man1/ls.1.gz

### 3. Shell Functions

*User-defined reusable command blocks.*

```userDefinedFunction
myFunc() {
  git pull
  npm install
}
Then run: myFunc
```

***Characteristics:***

* Stored in shell memory
* Useful for automation
* Override external commands if same name exists

### 4. Aliases

*Shortcuts for longer commands.*

* `alias myList="ls -la"` = creates custom alias named myList.
* `alias` = List aliases.

***Characteristics:***

* Simple text substitution
* Not suitable for complex logic
* Expanded before execution

### 5. Keywords (Reserved Words)

*Special syntax elements used by the shell parser.*

***Examples:***

* if
* then
* fi
* for
* while
* case

*These are not commands—you can't run them standalone.*

#### Command Resolution Order

*When you type a command, the shell checks in this order:*

* Aliases
* Keywords
* Functions
* Builtins
* External commands ($PATH)

##### How to Identify Command Type

`type ls`

***Output examples:***

* ls is /usr/bin/ls → external
* cd is a shell builtin
* ll is aliased to 'ls -la'
* myFunc is a function

## Types of Shell Variables

*Shell variables are named storage locations managed by the shell (e.g., Bash) that hold string values used during command execution, scripting, and environment configuration.*

*They're fundamental for parameterization, state management, and process inheritance in Linux systems.*

### 1. Local Variables

```definedWithinCurrent
Defined within the current shell session.
----------------------------------------
name="Sakil"
echo $name
prints 'Sakil'
```

***Characteristics:***

* Scope: current shell only
* Not inherited by child processes
* Used for scripting logic

### 2. Environment Variables

*Variables that are exported and inherited by child processes.*

`export APP_ENV=production`

***Characteristics:***

*Available to subprocesses (e.g., scripts, programs)
*Used by system tools and applications
`echo $APP_ENV` = production

### 3. Built-in Shell Variables

*Automatically maintained by the shell.*

***Examples:***

* $HOME   -> home directory
* $USER   -> current user
* $PWD    -> current working directory
* $SHELL  -> current shell path
* $?      -> last command exit status (0 means success)
* $$      -> current process ID

### 4. Positional Parameters

*Used in scripts to handle arguments.*

```PositionalParameters
$0  # script name
$1  # first argument
$2  # second argument
$@  # all arguments
$#  # number of arguments

echo "First arg: $1"
```

### 5. Readonly Variables

*Cannot be modified once set.*

`readonly PI=3.14`

#### Useful Commands

* `set` = List all variables
* `env` = List only environment variables
* `unset varName` = Remove variable

##### Summary

* Local variable → shell-only memory
* Environment variable → shared with processes
* Special variables → system-provided context

### Naming Rules of variables/identifiers

*Here's a short summary of how to write variable names in a Linux shell.*

* Use letters, numbers, underscore (_)
* Must start with a letter or (_)
* No spaces around =
* No special characters like -, @, #
* Case-sensitive (VAR ≠ var)

```variableNamingConvention
Valid Examples
--------------
user_name="sakil"
APP_ENV=production
_count=10                   # valid but discouraged to use
export userName="sakil"     # make it global

Invalid Examples
----------------
user-name="sakil"   # dash not allowed
1name="sakil"       # cannot start with number
name = "sakil"      # space not allowed

Use Variables
-------------
echo $user_name
echo $APP_ENV
echo $_count

```

## Explain built-in $PATH variable

### 1. Command itself

```explainPathVariable
echo $PATH

output:
----------
/root/.local/bin:/root/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin
```

* echo → prints output to the terminal
* $PATH → expands the PATH variable

### 2. What is PATH?

*PATH is a colon-separated list of directories that the shell searches when you run a command.*

When you type something like: `ls` . the shell does NOT magically know where `ls` is. It searches directories in `PATH` from left to right until it finds an executable named `ls`.

### 3. Output path explained