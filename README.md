# beebasic-trans
BBC Extended Eliminated BASIC transpiler
BBC because it handles BBC (and probably ARM) BASIC
Extended because it extends the types of variables, unit tests, and removes need for line numbers
Eliminated because it eliminates unnecessary characters in variable, procedure, and function names
Transpiler because it can will convert to working BBC BASIC, including working BBC BASIC (as long as it doesn't use line numbers)

*Line numbers are not supported and have to be included through use of labels*

Four typs of variable extensions:
- Calculation deference
- Object (limited to a single variable)
- Enum (limited to a single variable)
- Closure function
- Alias (allows struct/enum to be use for multiple types)

New commands supported:
- **LABEL** specifies a number for a label
- **INFO** defines a new extended variable
- **IMPORT** imports common closures, functions, and procedures

Colon (:) separator used in extra ways:
Anywhere on the line (not at the beginning):
- **:** is command separator (as normal)
- **::** indicates a unit test block
- **:::** comments out everything between two block, or to the end of the line (not the equivalent of using REM because it can be done within a command)

At the beginning of the line:
- **:**_number_ is syntactic sugar for **LABEL** _number_
- **:**_variable_**#=**_value_ is syntactic sugar for **INFO** _variable_**#=**_value_
- **:::** Comments everything out (equivalent of starting the line with **REM**)

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
