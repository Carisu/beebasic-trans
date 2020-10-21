# beebasic-trans
## BBC Extended Eliminated BASIC transpiler
- BBC because it handles BBC (and probably ARM) BASIC
- Extended because it extends the types of variables, unit tests, and removes need for line numbers
- Eliminated because it eliminates unnecessary characters in variable, procedure, and function names
- Transpiler because it can will convert to working BBC BASIC, including working BBC BASIC

## New commands and constructs
### New commands
New commands supported:
- **LABEL** specifies a number or name for a label
- **INFO** defines a new extended variable
- **IMPORT** imports common closures, functions, and procedures

#### Labels
Labels identify a command to be accessed through GOTO, GOSUB, and RESTORE commands. Labels can be any numeric or strings. When labels are numeric, they can be determined through label dereference extended variabes. A standard BBC BASIC program can be converted to extended by each line number being a numeric label.
1. **LABEL** _label_ - The following command can be identified through this label
The labels can be accessed through the following commands:
1. **GOTO** _label_ - Execution jumps to command following label
1. **GOSUB** _label_ - Next command put on stack before jumping to command following label, so **RETURN** will jump to next command
1. **RESTORE** _label_ - Next **READ** statement will read from next **DATA** statement following label
The following labels are valid:
1. _number_ - Use this number for label
1. **"**_name_**"** - Use this name in string for label
1. _label deference identifier_**#** - Calculate number to use for label from other variables **(only valid in GOTO/GOSUB/RESTORE commands)** _See section on **label deferences**_

#### Info commands
Extended variables are defined using **INFO** rather than **LET**. _See section on **extended variables**_

#### Imports
Common BASIC functions and functional functions can be imported to be used as closures

### Colon operator
The Colon (:) can be used in a number of ways:
Anywhere on the line (not at the beginning):
- **:** is command separator (as normal)
- **::** indicates a unit test block _See section on **unit tests**_
- **:::** comments out everything between two blocks, or to the end of the line (not the equivalent of using REM because it can be done within a command)

At the beginning of the line:
- **:**_number_ is syntactic sugar for **LABEL** _number_
- **:"**_name_**"** is syntactic sugar for **LABEL "**_name_**"**
- **:**_variable_**#=**_value_ is syntactic sugar for **INFO** _variable_**#=**_value_
- **:::** Comments everything out (equivalent of starting the line with **REM**)

## Transpiler Process
The transpiler performs the following operations to a text (ASCII) file to generate either human-readable text files or a binary file that could be loaded on a BBC computer or emulator. Specifically, the files are in unicode format but non0standard BBC characters are reported as errors.

*Processes in italics can generate human-readable output after this stage*
1. Convert file to BBC BASIC format
   1. Report errors on non-standard characters
   1. Convert escaped strings to appropriate characters or CHR$ sequences
1. Convert line numbers to labels
   1. Check line numbers are in sequencetial order
   1. Convert line numbers to labels
   1. Extract label dereferences from GOTO, GOSUB, and RESTORE
1. *Build code syntax structure*
   1. Split each line into separate commands over : or after special commands (eg REPEAT), keeping any initial : from beginning of line and assembly intact
   1. Convert ::: comment blocks into separate REM commands to come directly before commad
   1. Convert :: unit test commands into unit test blocks
   1. Convert initial : to appropriate command
   1. Convert commands and assembly into command blocks
   1. *Decode command blocks into text*
   1. *Write each command on each line*
1. Extract identifiers (variables, functions, procedures)
  1. Identiy identifiers used in each block and convert to identifiers
  1. Identify all identifiers used through command blocks
  1. Check identifier validity
1. *Import code*
   1. Identify imports in IMPORT blocks and check availability
   1. Expand imports when defined with wildcards
   1. Remove imports when imported previously
   1. Replace IMPORT blocks with appropriate command blocks
   1. Update identifiers and check for new mismatches
   1. *Decode command blocks into text*
   1. *Write each command on each line*
1. Expand variables
1. Deference variables
   1. Extract identifier dereferences for EVAL


## Identifiers
The following identifiers are used:
- Common variable names - split into
  - Strings (end in **$**)
  - Integers (end in **%**)
  - Real numbers (no ending)
- Function names (always preceded by **FN** in code)
- Procedure names (always preceded by **PROC** in code)
- Extended variable names (end in **#**) - split into
  - Label Dereference
  - Identifier Deference
  - Object (limited to a single variable)
  - Enum (limited to a single variable)
  - Closure Function
  - Alias (allows reuse of object/enum toin another variable)

## Extended Variables
### Calculation Dereference
Define logic based on labels, variable names, and functions, so that labels can be accessed correctly and vaeiables can be evaluated correctly.

**INFO** _variable_**# =** _logic_

### Object
A variable which is divided in =to separate, names, typed, parts.

**INFO** _variable_**# =** _[**(**]_ _varaible_ _[_ **,**_variable_ _... ]_ _[**)**]_

### Enum
A variable which specifies a number of unique strings identifiers which are stored as integers

**INFO** _variable_**# =** _[**(**]_ **"**_identifier_**"** _[_ **,"**_identifier_**"** _...]_ _[**)**]_

## Clousure Function
A variable which represents a function; can specify an existing function or create a new one

1. **INFO** _variable_**# = FN(** _variable_ _[_ **,**_variable_ _... ]_ **)** _assignment_ _[_ **;**_assignment_ _... ]_
1. **INFO** _variable_**# = FN** _function_

## Alias
A variable which takes on the definition of an object, enum, or alias variable

**INFO** _variable_**# =** _variable_**#**
