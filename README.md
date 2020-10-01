# beebasic-trans
BBC Extended Eliminated BASIC transpiler
BBC because it handles BBC (and probably ARM) BASIC
Extended because it extends the types of variables, unit tests, and removes need for line numbers
Eliminated because it eliminates unnecessary characters in variable, procedure, and function names
Transpiler because it can will convert to working BBC BASIC, including working BBC BASIC (as long as it doesn't use line numbers)

*Line numbers are not supported and have to be included through use of labels*

Four typs of variable extensions:
- Line number calculation deference
- C-style struct (limited to a single variable)
- C-style enum (limited to a single variable)
- Alias (allows struct/enum to be use for multiple types)

Two new commands supported:
- LABEL specifies a number for a label
- INFO defines a new extended variable

Colon (:) separator used in extra ways:
Anywhere on the line (not at the beginning):
- : is command separator (as normal)
- :: indicates a unit test block
- ::: comments out everything between two block, or to the end of the line (not the equivalent of using REM because it can be done within a command)

At the beginning of the line:
- :*number* is syntactic sugar for LABEL *number*
- :*variable*#=*vaue* is syntactic sugar for INFO *variable*#=*vaue*
- ::: Comments everything out (equivalent of starting the line with REM)

