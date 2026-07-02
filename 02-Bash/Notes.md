**Getting Started**
'vim script.sh' - to create and start building script
'#!/bin/bash' - This tells your computer to use bash
'chmod +x script.sh' - grant it executable permissions
'./script.sh' - run the script

**Writing Your First Script**
So as a first example let's create the script.sh file and populate it with below
```
#!/bin/bash
echo "Hello World!"
```
Don't forget to make the script file executable

**The 'Shebang' Line (or hashbang)**
This first line '#!/bin/bash' is required in all scripts.
It allows the system to interpret which shell to use.
For example for a python script it would be '#!/usr/bin/python3'

**Comments**
Single line comments can be placed on any line.
Using '#' this ignore everything on that line after the hash
Useful for explaining or adding notes in the script
Multi line comments. ': ' and ending with another single comma
Also useful for adding # to ignore part of a script you don't need to run

**Running Scripts from anywhere**
There are many system scripts that are used on the system
These run from anywhere and you can use 'echo $PATH' to see these locations
Now we can add our own scripts there. Such as in 'usr/local/bin'
To move anything here requires sudo
'sudo mv script.sh /usr/local/bin/greet' - this now makes the command greet
Now running 'greet' from anywhere in the shell will run the script

**Variables**
They allow you to store and manipulate data. Think of it as containers
Example 'greeting="Hello World!"'
Now when you do send 'echo $greeting' it will print Hello World!

**Parameters**
Allows you to customise the behaviour of a script
'./script.sh hello' this will pass parameter 1 to the below script
```
#!/bin/bash
echo "Paramater1: $1"
echo "Paramater2: $2"
echo "Paramater3: $3"
```

**Arithmetic Expansion**
A way to perform math calculations evaluate expressions.
'$(( xxx ))'
```
#!/bin/bash
num1=2
num2=5
result=$((num1 * num2))
echo "The sum of $num1 and $num2 is $result"
```
Incorporating this with parameters we can take step further and let user decide input
```
#!/bin/bash
num1="$1"
num2="$2"
result=$(($1 * $2))
echo "The sum of $num1 and $num2 is $result"
```
Then running './script.sh 5 10' - will multiply 5x10 and provide results

**if Statements**
Allow you to introduce decision making logic, evaluate conditions.
Always start with if and ends with fi. Example below with conditions
```
#!/bin/bash
if condition
then
	#code block to be executed
fi

eq = equals
ne = not equals to
lt = less than
gt = greater than
le = less than or equal to
ge = greater than or equal to
&& = AND
|| = OR
```
An example live script
```
#!/bin/bash
age=25
if [ $age -gt 18 ] # greater than 18
then
	echo "You meet the requirement."
fi
```

**else and elif**
The else clause give an alternative code block to execute if code results to false
```
#!/bin/bash
if condition
then
	# code block to execute
else
	# code to execute if false
fi
```
Example with only else
```
#!/bin/bash
grade=60
if [ $grade -ge 70 ] # greater than or equal to
then
	echo "You Passed!"
else
	echo "You Failed.."
fi
```
Now adding elif
```
#!/bin/bash
grade=40
if [ $grade -ge 90 ] # greater than or equal to
then
	echo "Amazing. You Passed!"
elif [ $grade -ge 80 ] # greater than or equal to
then
	echo "You Passed."
else
	echo "You failed.." # for anything else
fi
```

**Nested if Statements**
Allows us to create more complex decision making structures.
```
#!/bin/bash
age=19
grade=81

if [ $age -gt 18 ]; then
    echo "Age. Accepted" # If this passes goes onto nested if statement
    if [ $grade -ge 80 ]; then
        echo "Grade. Accepted"
        echo "Eligible for course!"
    else
        echo "Need better grade"
    fi
else
    echo "Sorry. Too young"
fi
```

**while Loops**
Allow you to repeatedly execute a block of code as long a certain condition remains true
```
#!/bin/bash
count=1

while [ $count -le 5 ]
do
    echo "Count: $count"
    ((count++))
done
```

**for Loops**
Allow you to iterate over a sequence of values and perform repetitive tasks
```
#!/bin/bash

fruits=("apple" "banana" "orange")

for fruit in "${fruits[@]}"
do
    echo "Fruits: $fruit"
done
```

**break and continue**
These statements provide additional control within for and while loops, allowing you to interrupt or skip iterations based on specific conditions. They give you the flexibility to interrupt a loop or skip iterations when necessary.
```
#!/bin/bash

count=1
while [ $count -le 5 ]
do
    if [ $count -eq 3 ]
    then
        ((count++))
        continue
    fi
    echo "Count: $count"
    ((count++))
done
```

**Basics of Functions**
Functions allow us to turn our code into modules, improve script organisation and enhance reusability so we can use functions throughout our code.
```
#!/bin/bash

hello_world() {
    echo "Hello World!"
}

hello_world
```

**Parameters**
Parameters allow us to pass data to functions, making them more versatile and adaptable.
```
#!/bin/bash

print_args() {
    echo "number of args: $#"
    echo "script name: $0"
    echo "first arg: $1"
    echo "second arg: $2"
    echo "all args: $@"
}

print_args "Alice" "Bob" "Ahmed"
```

**User Inputs**
User input allows our scripts to interact with users and make them more dynamic and responsive. Accepting user input within functions to create interactive scripts.
```
#!/bin/bash

greet_user() {
	echo "What is your name?"
	read name
	echo "Hello, $name!"
}

greet_user
```

**Handling Bad Data**
Bad data refers to invalid or unexpected user inputs that may cause errors or undesired behaviour in our scripts.

**Piping**
Piping allows us to pass the output of one command as input to another. For scripting it can also allow to connect commands and pass the output of one command as input to another.
```
#!/bin/bash

get_file_count() {
    local directory="$1"
    local file_count
    
    file_count=$(ls "$directory" | wc -l)

    echo "Number of files in $directory: $file_count"
}

get_file_count "./"
```

**Error Handling**
This is about about foreseeing where things can go wrong and taking appropriate measures to handle those situations, rather than letting the script crash.

For example checking first if the file exists
```
#!/bin/bash

FILE="/nonexist"

if [[ -f "$FILE" ]]; then
    echo "File exists"
else
    echo "File does not exit."
fi
```

In general, an exit code of 0 indicates success and any non-zero exit code like 1 indicates an error.
To check exit code of last run command
```
echo $?
```
Another example. Checking if git is installed.
```
#!/bin/bash

command -v git 2>/dev/null # devnull is used to silence error messages

if [[ $? -ne 0 ]]; then
    echo "git is not installed."
    exit 1
else
    echo "git is installed"
fi
```

**Set -e**
When we include set -e at the start of a script, the script will stop executing as soon as any command returns a non-zero exit code.
```
#!/bin/bash

set -e
echo "before the script"
nonexistcommand # switch with ls
echo "after the script"
```

**Set -u**
Forces the script to stop if it encounters an uninitialized variable. It helps you prevent scenarios where missing data could lead to incorrect results or unexpected behaviour.
```
#!/bin/bash

set -u
#W=5 # unhash and see behaviour
X=10
Y=20
Z=$((X + Y + W))
echo "Z equals: $Z"
```

**Set -x**
The set dash x option prints each command that will be executed to the terminal before it is actually executed. Useful for debugging.
```
set -x
echo "This is a test"
X=10
echo "The value of X is: $X"
```
If you want to debug only part of a script. You can use 'set +x' up until where required.
```
set -x
echo "This is a test"
set +x # this will only print above line
X=10
echo "The value of X is: $X"
```

**Set -eux**
Bash allows us to combine all 3 options.
```
#!/bin/bash

set -eux
echo "this is a test"
#X=10 #unhash to see
echo "the value of X is: $X"
nonexistcommand
```

**Set -o**
'set -o nounset' - Is equivalent to the 'set -u'. It helps catch uninitialized variables

'set -o errexit' - works the same as 'set -e'. It causes the shell to exit if any invoked command fails.

'set -o pipefail' - Useful setting that causes a pipeline to return the exit status of the last command in the pipeline that exited with a non-zero status. This can be very useful when you're piping commands together.

**Change PATH Permanently**
The path environment variable is a critical system variable that specifies the directories where the shell should look for executable files. Sometimes we might need to add our own directories to path, particularly when installing new software or when we want our own scripts or binaries to be available system-wide.
```
echo "export PATH=$PATH:~myscripts >> ~/.zshrc"
```

**Reading Environment Variables**
There are many system environments. Some known ones are HOME, USER, OSTYPE. We can also set local variables. Below example, using local variables set from environment variables.
```
#!/bin/bash

my_home="$HOME"
my_user="$USER"
my_os="$OSTYPE"

echo "Home Directory: $my_home"
echo "Current User: $my_user"
echo "OS Type: $my_os"
```
