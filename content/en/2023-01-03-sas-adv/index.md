---
title: SAS ADV into R
author: ''
date: '2023-01-03'
slug: sas
categories:
  - learn
tags: []
---

Chapter 10

Compare the `symput` and `symputx` difference, `symput` with `x` will give automatically removes leading zero and trailing blanks from the arguments. 
`symget` ()

- `SYMPUT` routine create or update macro variables during DATA step execution. 
   - performs automatic numeric to character conversion on any numeric value that you attempt to assign to a macro variable.

- `SYMGET` obtain the value of a macro variable during the execution of a DATA step, a PROC SQL step, or an SCL program.

- __INTO__ clause can be used in the `SELECT` statement to create or update macro variables during execution of a `PROC SQL` step.

- `SYMPUT` and `SYMPUTN` to create or update macro variables during the execution of an SCL program.


QUIZ: 
2.  to assign a literal string as a macro variable name, you enclose the literal in quotation marks. To assign a literal string as a value of the macro variable, you enclose the literal in quotation marks.
5.  
9. 


Chapter 11 

Define a Macro: 
```
   %MACRO macro-name;

            text

    %MEND <macro-name>;
```

Compile a Macro:
1. check for syntas errors.
2. write error message to SAS log 
3. stores all compiles macro language statements and constant txt in the SAS log

Call a Macro:
%MACRO

Execution a Macro
1. Search SAS catalog for the macro( Compiled macro language statement are executed)
2. Macro executive is halted at the end of the SAS step and SAS code is executed.
3. Macro exectution is resumed. 

TWO options: MPRINT AND MLOGIC 
MPRINT: Shows in the SAS log the code that results from macro
MLOGIC: Print the message that indicate macro actions that were taken during the macro executive. 

# The local Symbol Table 
The local symbol table contains the macro variables 
- created and initialized at macro parameter invocation.
- created and updated during the macro execution.
- reference everywhere within the macro.
create a local macro variables within a macro definiation:
- parameter in a maro definition.
- a %lET statement within a macro definition
- a Data step that contains a SYMPUT routine within a macro definition
- a Data step that contains a STMPUTX routine within a macro definition. 
- a SELECT SATEMENT THAT CONTAINS AN INTO clause IN proc SQL within a macro definition. 
- A %LOCAL statement. 





