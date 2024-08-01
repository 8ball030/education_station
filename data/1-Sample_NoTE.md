---
date: 04-23-24
id: 1
modified_at: 04/23/24 23:35
path: ''
tags:
- Cool Note
title: Huh? NoTE
type: note
---

# Does this Work?

I am not sure it does....

Intro to Bash

1. From Command-Line to Bash Script
1.1 Introduction
Course Overview
Moving from single commands to full scripts
Variables and data types in Bash
Control structures (if-statements, for-loops)
Functions and cron job scheduling
Bash Overview
Stands for "Bourne Again Shell"
Developed in the 1980s
Popular default shell for Unix and Mac systems
Important for servers, ML models, data pipelines
Used in cloud company CLIs
Benefits of Scripting
Execute multiple commands with a single command
Access to programming constructs
Regex Importance
Crucial for pattern matching in Bash scripting
Practice using sites like regex101.com
Shell practice
Text file fruits.txt contains 3 lines of data:

banana
apple
carrot
grep 'a' fruit.txt: returns elements that contain 'a' (ALL)
grep 'p' fruit.txt: returns elements that contain 'p' (apple)
grep '[pc]' fruit.txt: returns elements that contain 'p' OR 'c' (apple & carrot)
sort | uniq -c: 1. sort alphabetically, 2. count
Task 1.1a: Extracting scores with shell
bash

ls start_dir/*/soccer_scores.csv  # find the correct directory containing the file
cd start_dir/second_dir  # access the correct directory
cat start_dir/*/soccer_scores.csv | grep "^1959,"  # 1. concatenate, 2. return element for 1959
Task 1.1b: Searching a book with shell
bash

cat ~/two_cities.txt | 
grep -E "Sydney Carton|Charles Darnay" | 
wc -l
-E: flag for extended regular expressions syntax
1.2 Your First Bash Script
Bash Script Anatomy
First line: Shebang (#!) + path to Bash (e.g., #!/usr/bin/bash)
Check Bash location: type which bash to check
Middle: Contains code (simple commands or advanced scripts)
Saving and Running Scripts
File extension: Usually .sh (convention, not required)
Run methods:
bash script_name.sh
./script_name.sh (if shebang is present)
Basic Script Example
bash

#!/usr/bash
echo "Hello World"
echo "Goodbye cruel World"
Run: ./eg.sh Output:

Hello World
Goodbye cruel World
Using Shell Commands in Scripts
Each line can be a shell command
Pipes can chain commands
Practical Example
animals.txt contains:

magpie, bird
kangaroo, marsupial
shark, fish
emu, bird
wallaby, marsupial
To count the number of animals in each group:
bash

#!/usr/bash 
cat animals.txt | 
cut -d " " -f 2 | 
sort | 
uniq -c
After saving the script as group.sh, run using bash group.sh Returns: 2 bird, 1 fish, 2 marsupial
Task 1.2a: A simple Bash script
To confirm environment location, use which bash cat server_log_with_todays_date.txt
Task 1.2b: Shell pipelines to Bash scripts
bash

cat soccer_scores.csv | 
cut -d "," -f 2 | 
tail -n +2 | 
sort | 
uniq -c
Task 1.2c: Extract and edit using Bash scripts
bash

cat soccer_scores.csv | 
sed 's/Cherno/Cherno City/g' | 
sed 's/Arda/Arda United/g' 
> soccer_scores_edited.csv
1.3 Standard Streams and Arguments
Standard Streams
Standard Input (STDIN): Data going into the program
Standard Output (STDOUT): Data coming out of the program
Standard Error (STDERR): Where errors and exceptions are written
Redirecting Streams
STDERR can be redirected (e.g., to /dev/null for deletion)
STDOUT can be redirected using 1>
Pipe Chain Visualisation
Output of one program becomes input for the next
Errors by default display in the terminal
STDIN Example
Using 'cat' to redirect file contents to a new file Syntax: cat file.txt 1> newfile.txt
Arguments in Bash Scripts (ARGV)
Accessed using $ notation: $1, $2, etc.
Special arguments:
$@, $*: All arguments together
$#: Number of arguments
ARGV Example
Script demonstrating use of $1, $2, $@, and $# Example execution: ./script.sh one two three four five
Running ARGV Example:
bash

bash arg.sh one two three four five
#!/usr/bash
echo $1  # returns one
echo $2  # returns two
echo $@  # returns one two three four five
echo "There are " $# "arguments"  # returns "There are 5 arguments"
Task 1.3a: Using arguments in Bash scripts
bash

cat script_name.sh  # displays script contents
bash script_name.sh Arg1 Arg2 Arg3 …  # runs script
Task 1.3b: Using arguments with HR data
bash

cat hire_data/* |  # View hire_data file *wildcard char in shell globbing
grep "$1" > "$1".csv  # takes the output from left (matching expression) and writes a new file with name
2. Variables in Bash Scripting
2.1 Basic Variables in Bash
Variable Assignment
Use equals sign to define variables: var1="Moon", reference with $: echo $var1
Use descriptive names e.g. first_name='John', last_name='Doe'
Must use $ to reference variables as without $, Bash treats as literal strings
No spaces around equals sign in assignment as spaces cause errors
Single quotes ('): Literal interpretation
Double quotes ("): Interprets $ and backticks
Backticks (`): Creates shell-within-a-shell
Variable Creation Examples
bash

now_var='NOW'
now_var_singlequote='$now_var'  # literal
now_var_doublequote="$now_var"  # interpreted
rightnow_doublequote="The date is `Date`."  # shell within a shell (older)
rightnow_parentheses="The date is $(date)."  # shell within a shell (newer)
Task 2.1a: Using Variables in Bash
Create a script that defines two variables, first_name and last_name, and then prints a greeting using these variables.
Solution:
bash
#!/bin/bash

# Define variables
first_name="John"
last_name="Doe"

# Print greeting
echo "Hello, $first_name $last_name! Welcome to Bash scripting."
Save this as greeting.sh and run it with bash greeting.sh.

2.2 Numeric Variables in Bash
Arithmetic is not natively built into Bash as opposed to R and Python which allow direct arithmetic e.g.
bash

1+4  # error message generated
expr 1+4  # expr allows for basic arithmetic calculations
# 5
expr 1+2.5  # error message as expr CANNOT handle decimals
Basic Calculator (bc)
Entered into command line to handle decimal arithmetic 'bc' can be used without opening the calculator through piping:
bash

echo "5 + 7.5" | bc
'bc' also has a scale argument:
bash

echo "10 / 3" | bc  # returns 3
echo "scale=3; 10 / 3" | bc  # returns 3.333 (3dp) ';' separates lines
Assigning Numeric Variables in Bash Scripts
Just like string variables:
bash

dog_name='Roger'
dog_age=6  # without quotes for numeric value
echo "My dog's name is $dog_name and he is $dog_age years old."
Double Bracket Notation for Arithmetic
Alternative to 'expr':
bash

expr 5+7  # returns 12 (does not work for decimals)
echo $((5 + 7))  # also returns 12
Shell-within-a-Shell for Complex Calculations
Use $() or backticks, useful for combining 'bc' with variables
bash

model1=87.65
model2=89.20
echo "The total score is $(echo "$model1 + $model2" | bc)"
echo "The average score is $(echo "($model1 + $model2) / 2" | bc)"
Task 2.2a: Converting Fahrenheit to Celsius
Create a script that converts a temperature from Fahrenheit to Celsius using the formula: C = (F - 32) * 5/9
Solution:
bash
#!/bin/bash

# Get temperature in Fahrenheit
echo "Enter temperature in Fahrenheit:"
read temp_f

# Convert to Celsius
temp_c=$(echo "scale=2; ($temp_f - 32) * 5 / 9" | bc)

# Display result
echo "$temp_f°F is equal to $temp_c°C"
Save this as temp_convert.sh and run it with bash temp_convert.sh
Task 2.2b: Extracting data from files
Create a script that reads a CSV file named scores.csv with columns for name and score, and calculates the average score.
Solution:
bash
#!/bin/bash

# Calculate average score
average=$(awk -F',' '{sum+=$2} END {print sum/NR}' scores.csv)

# Display result
echo "The average score is: $average"
Save this as calculate_average.sh and run it with bash calculate_average.sh

2.3 Arrays in Bash
Types of Arrays in Bash
Numerical-indexed arrays similar to:
Python lists: my_list = [1, 2, 3, 4]
R vectors: my_vector <- c(1,2,3,4)
bash

declare -a my_first_array  # create without adding elements
my_first_array=(1 2 3)  # create AND add elements, separate with " " NOT ","
Example:
bash

my_array=(1 3 5 2)
echo ${my_array[@]}  # returns 1 3 5 2, all elements
echo ${#my_array[@]}  # returns 4, array length
echo ${my_array[2]}  # returns 5, zero indexed
my_array[2]=7  # modifies value in index 2 to 7, NO $ notation
echo ${my_array[2]}  # returns 7
echo ${my_array[@]:N:M}  # N is starting index, M is count
echo ${my_array[@]:0:2}  # returns 1 3 
my_array+=(10)  # appends 10 to the array (parentheses important)
echo ${my_array[@]}  # returns 1 3 7 2 10
Associative arrays similar to:
Python dictionaries: my_dict = {'city name': "New York", 'population': "14000000"}
R lists: my_list = list(city_name = c('New York'), population = c(14000000)
Method 1:
bash

declare -A city_details  # declare first 
city_details=([city_name]="New York", [population]=14000000)  # add elements
echo ${city_details[city_name]}  # returns index key
Method 2:
bash

declare -A city_details=([city_name]="New York", [population]=14000000)
echo ${!city_details[@]}  # access key using '!', returns all keys
Common Errors
Using commas instead of spaces to separate elements
Not using parentheses when appending elements
Forgetting to declare associative arrays
Task 2.3a: Creating an Array
Create a script that initializes an array with the days of the week and then prints out the weekdays.
Solution:
bash
#!/bin/bash

# Initialize array with days of the week
days=(Monday Tuesday Wednesday Thursday Friday Saturday Sunday)

# Print weekdays
echo "Weekdays:"
for ((i=0; i<5; i++))
do
    echo "${days[i]}"
done
Save this as weekdays.sh and run it with bash weekdays.sh
Task 2.3b: Creating Associative Arrays
Create a script that uses an associative array to store and display information about a car.
Solution:
bash
#!/bin/bash

# Declare associative array
declare -A car_info

# Add key-value pairs
car_info[make]="Toyota"
car_info[model]="Corolla"
car_info[year]="2022"
car_info[color]="Blue"

# Display car information
echo "Car Information:"
for key in "${!car_info[@]}"
do
    echo "$key: ${car_info[$key]}"
done
Save this as car_info.sh and run it with bash car_info.sh
Task 2.3c: Climate calculations in Bash
Create a script that stores monthly average temperatures in an array and calculates the yearly average temperature.
Solution:
bash

#!/bin/bash

# Initialize array with monthly average temperatures
temperatures=(32 34 38 42 50 60 70 68 60 50 40 34)

# Calculate yearly average
sum=0
for temp in "${temperatures[@]}"
do
    sum=$((sum + temp))
done
average=$(echo "scale=2; $sum / ${#temperatures[@]}" | bc)

# Display result
echo "The yearly average temperature is: $average°F"
Save this as yearly_average_temp.sh and run it with bash yearly_average_temp.sh
3. Control Statements in Bash Scripting
3.1 IF Statements
Basic IF Statement Structure
bash

if [ CONDITION ]; then  # include spaces in brackets & semicolon after condition
# some code
else
# some more code
fi  # end, 'if' backwards
Example:
bash

x="Queen"
if [ $x == "King" ]; then  # or [$x != "King"] if x IS NOT "King", switch 
echo "$x is a King!"
else
echo "$x is not a King!"
fi  # returns "Queen is not a King!"
Option 1: Double Parentheses
bash

x=10 
if (( $x > 5 )); then  # traditional comparison symbols: >, <, >=, <=, ==, !=
echo "$x is more than 5"
fi  # returns "10 is more than 5"
Option 2: Square Brackets with Flags
bash

x=10 
if [ $x -gt 5 ]; then
echo "$x is more than 5"
fi  # returns "10 is more than 5"
Combining Conditions (AND: &&, OR: ||)
bash

x=10
if [ $x -gt 5 ] && [ $x -le 11]; then
echo "$x is > 5 AND <= 11"
fi

x=10
if [[ $x -gt 5 || $x -le 11 ]]; then
echo "$x is > 5 OR <= 11"
fi
Using Command-line Programs in Conditionals
words.txt contains 'Hello World!'
bash

if grep -q Hello words.txt; then
echo "Hello is inside!"
fi
Shell-within-a-Shell in Conditionals
bash

if $(grep -q Hello words.txt); then
echo "Hello is inside!"
fi
Task 3.1a: Sorting model results
Create a script that checks if a model's accuracy is above 90% and sorts it into a "high_accuracy" folder if it is.
Solution:
bash
#!/bin/bash

# Get model accuracy
echo "Enter model accuracy (as a percentage):"
read accuracy

# Check accuracy and sort
if [ $accuracy -gt 90 ]; then
    echo "High accuracy model detected. Moving to high_accuracy folder."
    # In a real scenario, you would move the file here
    # mv model.pkl high_accuracy/
else
    echo "Model accuracy is not above 90%. Keeping in current folder."
fi
Save this as sort_model.sh and run it with bash sort_model.sh

Task 3.1b: Moving relevant files
Create a script that moves all Python files in the current directory to a "python_scripts" folder if it exists.
Solution:
bash
#!/bin/bash

# Check if python_scripts folder exists
if [ -d "python_scripts" ]; then
    # Move Python files
    for file in *.py
    do
        if [ -f "$file" ]; then
            mv "$file" python_scripts/
            echo "Moved $file to python_scripts folder"
        fi
    done
else
    echo "python_scripts folder does not exist. Please create it first."
fi
Save this as move_python_files.sh and run it with bash move_python_files.sh

3.2 FOR loops & WHILE statements
FOR Loop Structure
Python:
python

for x in range(3):
    print x
# returns 0 1 2
R:
r

for (x in seq(3)){
    print x
}
# returns 1 2 3
Bash:
bash

for x in 1 2 3
do
    echo $x
done 
# returns 1 2 3
Brace Expansion {START..STOP..INCREMENT}
bash

{1..5..2}  # returns 1 3 5 (increment defaults to 1 if not specified)
Three Expression Syntax for Numerical Loops
bash

for ((x=2;x<=4;x+=2))
do
    echo $x
done  # returns 2 4
Glob Expansions in FOR Loops
bash

for book in books/*  # *for pattern matching
do
    echo $book
done  # returns each file in turn e.g. book/book1.txt books/book2.txt
Shell-within-a-Shell in FOR Loops $()
bash

for book in $(ls books/ |  # list all file in books/
    grep -i 'air')  # keep those with air in the name
do
    echo $book
done  # returns only files with air in name e.g. AirportBook.txt
WHILE Loop
Similar to a FOR loop except you set a condition which is tested at each iteration, iterations continue until this is no longer met.
Continues until condition is false
Can use same conditions as IF statements
Ensure loop condition eventually becomes false
&& and || can be used as in FOR
Avoid the infinite loop by adding step change (ensure loop condition can be met, update loop control variable)
bash

x=1
while [ $x -le 3 ];  # while x is <= 3
do
    echo $x  # print x
    ((x+=1))  # add 1 to x
done  # returns 1 2 3
Task 3.2a: A Simple FOR Loop
Create a script that uses a FOR loop to print the squares of numbers from 1 to 5.
Solution:
bash

#!/bin/bash

echo "Squares of numbers from 1 to 5:"
for i in {1..5}
do
    square=$((i * i))
    echo "$i squared is $square"
done
Save this as squares.sh and run it with bash squares.sh
Task 3.2b: Correcting a WHILE Statement
Fix the following WHILE loop so it correctly prints numbers from 1 to 5:
bash

#!/bin/bash

count=1
while [ $count -le 5 ]
do
    echo $count
done
Corrected Solution:
bash

#!/bin/bash

count=1
while [ $count -le 5 ]
do
    echo $count
    ((count++))  # Increment count
done
Save this as count_to_five.sh and run it with bash count_to_five.sh
Task 3.2c: Cleaning up a directory
Create a script that uses a FOR loop to delete all .tmp files in the current directory.
Solution:
bash

#!/bin/bash

echo "Deleting all .tmp files in the current directory:"
for file in *.tmp
do
    if [ -f "$file" ]; then
        rm "$file"
        echo "Deleted $file"
    fi
done
echo "Cleanup complete."
Save this as cleanup_tmp_files.sh and run it with bash cleanup_tmp_files.sh
3. Control Statements in Bash Scripting (continued)
3.3 Case Statements
CASE Statements simplify complex/nested IF statements for clearer and more efficient code.
Example of complex IF statements:
bash

if grep -q 'sydney' $1; then
    mv $1 sydney/
fi
if grep -q 'melbourne|brisbane' $1; then
    rm $1
fi
if grep -q 'canberra' $1; then
    mv $1 "IMPORTANT_$1"
fi
Basic CASE Statement Structure:
bash

case 'STRING' in
    pattern1)
        command1;;
    pattern2)
        command2;;
    *)
        default command;;
esac
Note:
STRING can be a variable or shell-within-shell construct
Use | for OR conditions
Air* starts with Air, *hat* contains hat
* is an optional default pattern
End with esac (case backwards)
Advantages over IF Statements:
More readable for multiple conditions
Efficient for pattern matching
Avoids repetitive code
Example:
bash

case $(cat $1) in
    *sydney*)
        mv $1 sydney/ ;;
    *melbourne*|*brisbane*)
        rm $1 ;;
    *canberra*)
        mv $1 "IMPORTANT_$1" ;;
    *)
        echo "No cities found";;
esac
Advanced Usage:
Can use shell-within-a-shell for variable to match against
Useful for file processing, user input handling, etc.
Key Points:
CASE statements are ideal for multiple condition checking
They improve code readability and efficiency
Useful for scenarios like data processing pipelines, user input handling
Remember to end with "esac" and use double semicolons after each pattern block
Task 3.3a: Days of the Week with CASE
Create a script that takes a number (1-7) as input and prints the corresponding day of the week using a CASE statement.
Solution:
bash

#!/bin/bash

echo "Enter a number (1-7):"
read day

case $day in
    1) echo "Monday" ;;
    2) echo "Tuesday" ;;
    3) echo "Wednesday" ;;
    4) echo "Thursday" ;;
    5) echo "Friday" ;;
    6) echo "Saturday" ;;
    7) echo "Sunday" ;;
    *) echo "Invalid input. Please enter a number between 1 and 7." ;;
esac
Save this as day_of_week.sh and run it with bash day_of_week.sh.
Task 3.3b: Moving Model Results with CASE
Create a script that moves files based on their extension using a CASE statement.
Solution:
bash

#!/bin/bash

for file in *
do
    case ${file##*.} in
        csv)
            mv "$file" csv_folder/ ;;
        txt)
            mv "$file" text_folder/ ;;
        json)
            mv "$file" json_folder/ ;;
        *)
            echo "Unknown file type: $file" ;;
    esac
done
Save this as move_files.sh and run it with bash move_files.sh.
4. Functions and Automation
4.1 Basic functions in Bash
Advantages of Functions:
Reusable code sections
Reduces repetition
Creates modular, compartmentalised code
Facilitates easy sharing and collaboration
Bash Function Syntax:
bash

function_name() {
    # function_code
    return # something
}
Alternative syntaxes:
bash

function function_name {
    # function code
    return # something
}

function print_hello (){
    echo "Hello!"
}
Key Components of Bash Functions:
Function name (should be descriptive)
Parentheses (optional in alternate structure)
Curly brackets to enclose function code
Can include loops, IF statements, shell-within-a-shell constructs, etc.
Optional return statement (different from other languages)
To call a Bash Function, simply use the function name: function_name
Example: Fahrenheit to Celsius Conversion
bash

temp_f=30
function convert_temp () {
    temp_c=$(echo "scale=2; ($temp_f - 32) * 5 / 9" | bc)
    echo $temp_c
}
convert_temp  # call, returns -1.11
Key Points:
Functions enhance code organisation and reusability
Two syntaxes available for function definition
Function names should be descriptive, calling a function is done by using its name
Functions can incorporate various Bash constructs learned previously
Task 4.1a: Uploading model results to the cloud
Create a function that simulates uploading a file to the cloud.
Solution:
bash

#!/bin/bash

upload_to_cloud() {
    local file=$1
    if [ -f "$file" ]; then
        echo "Uploading $file to the cloud..."
        sleep 2  # Simulate upload time
        echo "Upload complete: $file"
    else
        echo "Error: $file not found"
    fi
}

# Test the function
upload_to_cloud "model_results.txt"
upload_to_cloud "nonexistent_file.txt"
Save this as cloud_upload.sh and run it with bash cloud_upload.sh.
Task 4.1b: Get the current day
Create a function that returns the current day of the week.
Solution:
bash

#!/bin/bash

get_current_day() {
    echo $(date +%A)
}

echo "Today is $(get_current_day)"
Save this as current_day.sh and run it with bash current_day.sh.
4.2 Arguments, return values, and scope
Passing Arguments to Bash Functions:
Uses same syntax as ARGV in scripts ($1, $2, etc.)
Special properties: $@ (all arguments), $# (number of arguments)
Example of Passing Arguments:
bash

function print_filename {
    echo "The first file was $1"
    for file in $@
    do
        echo "This file has name $file"
    done
}
print_filename "LOTR.txt" "mod.txt" "A.py"
Scope in Programming:
Global scope: Variables accessible anywhere in the program
Local scope: Variables accessible only in certain parts of the program
Bash variables default to global scope
Use 'local' keyword to limit variable scope within a function
Return values:
'return' is used for function success (0) or failure (1-255), captured via $?
For data return:
Assign to a global variable
echo the value and capture with shell-within-a-shell
Example of Return Error:
bash

function function_2 {
    echlo  # an error of 'echo'
}
function_2
echo $?  # returns script.sh: line 2: echlo: command not found 127
Correct Method for Returning Data:
bash

function convert_temp {
    echo $(echo "scale=2; ($1 - 32) * 5 / 9" | bc)
}
converted=$(convert_temp 30)
echo "30F in Celsius is $converted C"  # returns 30F in Celsius is -1.11 C
Key Points:
Argument passing similar to script ARGV
Bash uses global scope by default, but 'local' keyword available
'return' is for status, not data
Use echo and shell-within-a-shell for returning data from functions
Be cautious of scope when accessing variables outside functions
Task 4.2a: A Percentage Calculator
Create a function that calculates the percentage of a number.
Solution:
bash

#!/bin/bash

calculate_percentage() {
    local total=$1
    local part=$2
    local percentage=$(echo "scale=2; ($part / $total) * 100" | bc)
    echo "$percentage%"
}

echo "50 is $(calculate_percentage 100 50) of 100"
echo "25 is $(calculate_percentage 200 25) of 200"
Save this as percentage_calculator.sh and run it with bash percentage_calculator.sh.
Task 4.2b: Sport Analytics Function
Create a function that calculates the win percentage for a sports team.
Solution:
bash

#!/bin/bash

win_percentage() {
    local team_name=$1
    local wins=$2
    local losses=$3
    local total_games=$((wins + losses))
    local win_percent=$(echo "scale=2; ($wins / $total_games) * 100" | bc)
    echo "$team_name has a win percentage of $win_percent%"
}

win_percentage "Tigers" 82 80
win_percentage "Lions" 60 102
Save this as sports_analytics.sh and run it with bash sports_analytics.sh.
Task 4.2c: Summing an Array
Create a function that sums the elements of an array.
Solution:
bash

#!/bin/bash

sum_array() {
    local sum=0
    for num in "$@"
    do
        sum=$((sum + num))
    done
    echo $sum
}

numbers=(1 2 3 4 5)
result=$(sum_array "${numbers[@]}")
echo "The sum of the array is: $result"
Save this as sum_array.sh and run it with bash sum_array.sh.
4.3 Scheduling your scripts with Cron
Benefits of Scheduling Scripts:
Automate regular tasks (daily, weekly, etc.)
Optimise resource usage (e.g., running during low-traffic hours)
Essential skill for modern data infrastructures
Cron, derived from Greek "chronos" (time), is a Unix-like system program since the 1970s driven by crontab file containing cronjobs.
Crontab Basics:
crontab -l: view current cronjobs
crontab -e: edit cronjobs
Cronjob format: * * * * * command_to_execute (minute hour day_of_month month day_of_week)
Cronjob Examples:
5 1 * * * bash myscript.sh: run at 5 mins past 0100 hrs every day, every month
15 14 * * 7 bash myscript.sh: run at 15 mins past 1400 hrs on Sundays
Advanced Cronjob Syntax:
15,30,45 * * * *: run at 15, 30 & 45 mins past every hour, every day
*/15 * * * *: run every 15 mins every hour, every day
Creating Your First Cronjob:
crontab -e: edit list of cronjobs
Use nano or vim editor
Add line: 30 1 * * * extract_data.sh: run extract_data 30 mins past 0100 hrs every day
Exit editor and save crontab: ctrl+o, enter, ctrl+x
crontab -l: list and verify cronjobs
Key Points:
Cron allows automated script execution on a schedule
Crontab file manages all cronjobs
Cronjob syntax includes five time fields plus the command to execute
Various options for scheduling (specific times, intervals, multiple times)
Always verify cronjobs after creation or modification
Task 4.3a: Analysing a crontab schedule
Explain what the following crontab entry does:

0 2 * * 1-5 /home/user/backup.sh
Solution: This crontab entry runs the backup.sh script at 2:00 AM (0 2) every weekday (1-5, which represents Monday to Friday). The asterisks for day of month and month mean it runs every day and every month, respectively.
Task 4.3b: Creating cronjobs
Create a cronjob that runs a script every Sunday at 8:30 PM.
Solution:
Open the crontab editor:
 
crontab -e


Add the following line:
 
30 20 * * 0 /path/to/your/script.sh


Save and exit the editor.
This will run the script every Sunday (0) at 8:30 PM (30 20).
Task 4.3c: Scheduling cronjobs with crontab
Create a cronjob that runs a script every 15 minutes during working hours (9 AM to 5 PM) on weekdays.
Solution:
Open the crontab editor:
 
crontab -e


Add the following line:
 
*/15 9-17 * * 1-5 /path/to/your/script.sh


Save and exit the editor.
This will run the script every 15 minutes (*/15) from 9 AM to 5 PM (9-17) on weekdays (1-5).