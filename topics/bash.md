# Bash

## Logical
`&&` - AND
`||` - OR

## Redirection
`>` - Redirect standard output and **overwrite**.  
`>>` - Redirect standard output and **append**.  
`<` - Redirect input to another command. Ex. `mail mike@somewhere.org < to_do.txt`

## Piping
`|` - Uses the **output** of one command as the **input** of another.

## Command Substitution
`$(PROGRAM_PATH)` - Use program output as variable. Ex. `echo "$(/bin/date) - Hi" >> logfile.log`

## grep
Only works for searching files or standard outputs.  

`grep <CRITERIA> <PATH>` - Returns all the lines containing the search criteria.
