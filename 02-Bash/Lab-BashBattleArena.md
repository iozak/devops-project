The "Bash Battle Arena" is a command-line game designed to teach and improve Bash scripting skills in a fun and interactive way. The game presents players with a series of increasingly complex challenges that they must solve by writing Bash scripts. Each challenge is structured like a level in a game, and as players progress, they learn new Bash concepts and best practices.

Objective: Players must complete various tasks or "missions" using Bash scripts. These tasks range from simple file manipulations to more complex system operations. The goal is to "defeat" each level by correctly solving the problem, with the ultimate aim of becoming a "Bash Master."

**Game Structure**
The games are level-based. You begin at level 1, and the difficulty increases as you go up the levels.

There is a Boss Battle every 5 levels designed to combine and reinforce the knowledge from the previous 5 levels.

**Level 1: The Basics**
Mission: Create a directory named Arena and then inside it, create three files: warrior.txt, mage.txt, and archer.txt. List the contents of the Arena directory.
```
mkdir Arena
```
```
cd Arena
```
```
touch warrior.txt mage.txt archer.txt
```
**Solved** - Use 'ls' to confirm

**Level 2: Variables and Loops**
Mission: Create a script that outputs the numbers 1 to 10, one number per line.
```
#!/bin/bash

number=1

while [ $number -le 10 ]
do
        echo "$number"
        ((number++))
done
```
![[BashBattleArena-Level2.png]]
**Solved** - Multiple possible solutions. Can also use 'for i in {1..10}' 

**Level 3: Conditional Statements**
Mission: Write a script that checks if a file named hero.txt exists in the Arena directory. If it does, print Hero found!; otherwise, print Hero missing!.
```
#!/bin/bash

if [[ -f Arena/hero.txt ]]; then
        echo "Hero found!"
else
        echo "Hero missing!"
fi
```
![[BashBattleArena-Level3.png]]
**Solved** - In this case we don't yet have a hero.txt file

**Level 4: File Manipulation**
Mission: Create a script that copies all .txt files from the Arena directory to a new directory called Backup.
```
#!/bin/bash

mkdir /home/zak/Arena/backup

for f in /home/zak/Arena/*.txt
do
        cp "$f" /home/zak/Arena/backup/
done
```
![[BashBattleArena-Level4.png|537]]**Solved** - Confirmed creation of new folder Backup with txt files

**Level 5: The Boss Battle - Combining Basics**
Mission: Combine what you've learned! Write a script that:
1. Creates a directory names 'Battlefield'
2. Inside Battlefield, create files named knight.txt, sorcerer.txt, and rogue.txt.
3. Check if knight.txt exists; if it does, move it to a new directory called Archive.
4. List the contents of both Battlefield and Archive.

```
#!/bin/bash

mkdir battlefield
touch battlefield/{knight,sorcerer,rogue}.txt

if [[ -f battlefield/knight.txt ]]; then
        mkdir archive
        cp battlefield/knight.txt archive/
else
        echo "knight.txt not found."
fi

echo "Battlefield Contents"
ls battlefield

echo "Archive Contents"
ls archive
```
![[BashBattleArena-Level5.png]]
**Solved**

**Level 6: Argument Parsing**
Mission: Write a script that accepts a filename as an argument and prints the number of lines in that file. If no filename is provided, display a message saying 'No file provided'.

```
#!/bin/bash

file_path="$1"
count="0"

if [ $# -eq 0 ]; then
        echo "No filename provided"
elif [ ! -f "$1" ]; then
        echo "No file found!"
else
        while IFS= read -r line; do
                ((count++))
        done < "$file_path"
echo "Number of lines in the file: $count"

fi
```
![[BashBattleArena-Level6.png]]**Solved**

**Level 7: File Sorting Script**
Mission: Write a script that sorts all .txt files in a directory by their size, from smallest to largest, and displays the sorted list.

```
#!/bin/bash

DIRECTORY="Arena"

[[ ! -d "$DIRECTORY" ]] && {
    echo "Directory does not exist."
    exit 1
}

find "$DIRECTORY" -type f -name "*.sh" -printf "%s %p\n" | sort -n
```
![[BashBattleArena-Level7.png]]

**Solved** - Used .sh in this case as files were already populated to have a size